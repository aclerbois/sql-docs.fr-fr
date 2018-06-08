---
title: Évaluation de la cardinalité (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- cardinality estimator
- CE (cardinality estimator)
- estimating cardinality
ms.assetid: baa8a304-5713-4cfe-a699-345e819ce6df
caps.latest.revision: 11
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 197e68e1aaaacbcb99410698551bc94352ad6df0
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="cardinality-estimation-sql-server"></a>Évaluation de la cardinalité (SQL Server)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Cet article explique comment évaluer et choisir la meilleure configuration d’estimation de la cardinalité pour votre système SQL. La plupart des systèmes bénéficient de la dernière version de l’estimation de la cardinalité, car il s’agit de la plus précise. L’estimation de la cardinalité prédit le nombre de lignes que votre requête est susceptible de renvoyer. La prédiction de la cardinalité est utilisée par l’optimiseur de requête pour générer un plan de requête optimal. Avec des estimations plus précises, l’optimiseur de requête est généralement en mesure de produire un plan de requête plus optimal.  
  
Le système d’applications peut contenir une requête importante dont le plan est remplacé par un plan plus lent en raison de la nouvelle estimation de la cardinalité. Voici quelques exemples d’une telle requête :  
  
- Une requête OLTP (traitement transactionnel en ligne) qui s’exécute si souvent que plusieurs instances de cette requête s’exécutent simultanément.  
- Une instruction SELECT avec une agrégation importante qui s’exécute pendant vos heures de travail OLTP.  
  
Vous disposez de techniques pour identifier une requête qui s’exécute plus lentement avec la nouvelle estimation de cardinalité. Vous disposez d’options pour résoudre les problèmes de performances.     
  
## <a name="versions-of-the-ce"></a>Versions de l’estimation de cardinalité  
En 1998, une mise à jour majeure de l’estimation de cardinalité a été intégrée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, pour lequel le niveau de compatibilité était de 70. Cette version du modèle CE est fondée sur quatre hypothèses de base :

-  **Indépendance :** Les distributions de données sur différentes colonnes sont supposées être indépendantes les unes des autres, à moins que des informations de corrélation soient disponibles et utilisables.
-  **Homogénéité :** Les valeurs distinctes sont espacées de manière égale et ont toutes la même fréquence. Plus précisément, dans chaque étape d’[histogramme](../../relational-databases/statistics/statistics.md#histogram), les valeurs distinctes sont réparties uniformément et chaque valeur a la même fréquence. 
-  **Autonomie (simple) :** Les utilisateurs interrogent des données qui existent. Par exemple, pour une jointure d’égalité entre deux tables, prendre en compte la sélectivité des prédicats <sup>1</sup> dans chaque histogramme d’entrée, avant de joindre les histogrammes pour estimer la sélectivité de jointure. 
-  **Inclusion :** Pour les prédicats de filtres où `Column = Constant`, la constante est en fait supposée exister pour la colonne associée. Si une étape d’histogramme correspondante n’est pas vide, l’une des valeurs distinctes de l’étape est supposée correspondre à la valeur du prédicat.

  <sup>1</sup> Nombre de lignes satisfaisant au prédicat.

Les mises à jour suivantes ont commencé avec [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], ce qui signifie que les niveaux de compatibilité sont de 120 et au-delà. Les mises à jour de l’estimation de cardinalité pour les niveaux 120 et au-delà comprennent des hypothèses et des algorithmes mis à jour qui fonctionnent bien sur l’entreposage moderne de données et sur les charges de travail OLTP. À partir des hypothèses CE 70, les hypothèses de modèle suivantes ont été changées à compter de CE 120 :

-  **Indépendance** devient **Corrélation :** La combinaison des valeurs de différentes colonnes n’est pas nécessairement indépendante. Cela peut ressembler davantage à une interrogation de données réelles.
-  **Autonomie simple** devient **Autonomie de base :** Les utilisateurs peuvent interroger des données qui n’existent pas. Par exemple, pour une jointure d’égalité entre deux tables, nous utilisons les histogrammes des tables de base pour estimer la sélectivité de jointure, puis nous prenons en compte la sélectivité des prédicats.
  
**Niveau de compatibilité** : Vous pouvez garantir le niveau de votre base de données en utilisant le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant pour [COMPATIBILITY_LEVEL](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

```sql  
SELECT ServerProperty('ProductVersion');  
GO  
  
ALTER DATABASE <yourDatabase>  
SET COMPATIBILITY_LEVEL = 130;  
GO  
  
SELECT d.name, d.compatibility_level  
FROM sys.databases AS d  
WHERE d.name = 'yourDatabase';  
GO  
```  
  
Pour une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définie au niveau de compatibilité 120 ou plus, l’activation de l’[indicateur de trace 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) force le système à utiliser la version 70 de l’estimation de la cardinalité.  
  
**Estimation de cardinalité héritée** : Pour une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définie au niveau de compatibilité 120 et plus, la version 70 de l’estimation de la cardinalité peut être activée à l’aide de l’instruction [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION 
SET LEGACY_CARDINALITY_ESTIMATION = ON;  
GO  
  
SELECT name, value  
FROM sys.database_scoped_configurations  
WHERE name = 'LEGACY_CARDINALITY_ESTIMATION';  
GO
```  
 
Ou, à partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1, à l’aide de l’[indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md#use_hint) `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')`.
 
 ```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01'; 
OPTION (USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION'));  
```
 
**Magasin des requêtes** : Si vous utilisez [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], le Magasin des requêtes est un outil pratique pour examiner les performances de vos requêtes. Dans [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)], dans l’**Explorateur d’objets** situé sous le nœud de votre base de données, le nœud **Magasin des requêtes** s’affiche quand le Magasin des requêtes est activé.  
  
```sql  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE = ON;  
GO  
  
SELECT q.actual_state_desc AS [actual_state_desc_of_QueryStore],  
        q.desired_state_desc,  
        q.query_capture_mode_desc  
FROM sys.database_query_store_options AS q;  
GO  
  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE CLEAR;  
```  
  
> [!TIP] 
> Nous vous recommandons d’installer la dernière version de [Management Studio](http://msdn.microsoft.com/library/mt238290.aspx) et de la mettre souvent à jour.  
  
Pour effectuer le suivi du processus de l’estimation de la cardinalité, vous pouvez utiliser l’événement étendu nommé **query_optimizer_estimate_cardinality**. L’exemple de code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant s’exécute sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il écrit un fichier .xel dans `C:\Temp\` (vous pouvez changer ce chemin). Lorsque vous ouvrez le fichier .xel dans [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)], vous voyez ses informations détaillées dans un affichage convivial.  
  
```sql  
DROP EVENT SESSION Test_the_CE_qoec_1 ON SERVER;  
go  
  
CREATE EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
ADD EVENT sqlserver.query_optimizer_estimate_cardinality  
    (  
        ACTION (sqlserver.sql_text)  
            WHERE (  
                sql_text LIKE '%yourTable%'  
                and sql_text LIKE '%SUM(%'  
            )  
    )  
ADD TARGET package0.asynchronous_file_target   
        (SET  
            filename = 'c:\temp\xe_qoec_1.xel',  
            metadatafile = 'c:\temp\xe_qoec_1.xem'  
        );  
GO  
  
ALTER EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
STATE = START;  --STOP;  
GO  
```  
  
Pour plus d’informations sur les événements étendus adaptés à [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consultez [Événements étendus dans la base de données SQL](http://azure.microsoft.com/documentation/articles/sql-database-xevent-db-diff-from-svr/).  
  
## <a name="steps-to-assess-the-ce-version"></a>Étapes d’évaluation de la version d’estimation de la cardinalité  
  
Les étapes suivantes permettent d’évaluer si l’une de vos requêtes importantes est moins performante avec la dernière version de l’estimation de cardinalité. Certaines étapes sont effectuées en exécutant l’exemple de code présenté dans la section précédente.  
  
1.  Ouvrir [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. Vérifiez que le niveau de compatibilité de votre base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré sur la valeur maximale.  
  
2.  Effectuez les étapes préliminaires suivantes :  
  
    1.  Ouvrir [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].  
  
    2.  Exécutez le code T-SQL pour vérifier que le niveau de compatibilité de votre base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré sur la valeur maximale.  
  
    3.  Vérifiez dans votre base de données que la configuration `LEGACY_CARDINALITY_ESTIMATION` est désactivée.  
  
    4.  Supprimez le contenu de votre magasin de requêtes. Vérifiez bien entendu que votre magasin de requêtes est activé.  
  
    5.  Exécutez l’instruction suivante : `SET NOCOUNT OFF;`  
  
3.  Exécutez l’instruction suivante : `SET STATISTICS XML ON;`  
  
4.  Exécutez votre requête importante.  
  
5.  Dans le volet de résultats, sous l’onglet **Messages** , notez le nombre de lignes affectées.  
  
6.  Dans le volet de résultats de l’onglet **Résultats** , double-cliquez sur la cellule qui contient les statistiques au format XML. Un plan de requête graphique s’affiche.  
  
7.  Cliquez avec le bouton droit sur la première zone du plan de requête graphique, puis cliquez sur **Propriétés**.  
  
8.  Notez les valeurs des propriétés suivantes en vue de les comparer ultérieurement avec une configuration différente :  
  
    -   **CardinalityEstimationModelVersion**.  
  
    -   **Nombre de lignes estimé**.  
  
    -   **Coût E/S estimé**, et plusieurs propriétés *estimées* similaires qui impliquent des performances réelles plutôt que des prédictions de nombre de lignes.  
  
    -   **Opération logique** et **Opération physique**. *Parallélisme* est une bonne valeur.  
  
    -   **Mode d’exécution réel**. *Lot* est une bonne valeur, meilleure que *Ligne*.  
  
9. Comparez le nombre estimé de lignes au nombre réel de lignes. Le pourcentage d’inexactitude de l’estimation de la cardinalité est-il de 1 % (haut ou bas) ou de 10 % ?  
  
10. Exécutez l’instruction suivante : `SET STATISTICS XML OFF;`  
  
11. Exécutez le code T-SQL pour baisser d’un niveau le niveau de compatibilité de votre base de données (par exemple, de 130 à 120).  
  
12. Exécutez à nouveau toutes les étapes non préliminaires.  
  
13. Comparez les valeurs des propriétés de l’estimation de la cardinalité des deux exécutions.  
  
    - Le pourcentage d’inexactitude est-il moins élevé avec la nouvelle estimation de la cardinalité ?  
  
14. Enfin, comparez les différentes valeurs de propriétés de performances des deux exécutions.  
  
    -   Votre requête a-t-elle utilisé un plan différent pour les deux estimations ?  
  
    -   Votre requête s’est-elle exécutée plus lentement sous la dernière version de l’estimation ?  
  
    -   À moins que votre requête ne s’exécute mieux et avec un autre plan sous l’ancienne version de l’estimation de la cardinalité, il est préférable d’utiliser la dernière version.  
  
    -   Toutefois, si votre requête s’exécute avec un plan plus rapide sous l’ancienne version de l’estimation de la cardinalité, envisagez de forcer le système à utiliser le plan plus rapide et à ignorer l’estimation de la cardinalité. Ainsi, vous pourrez disposer de la dernière version de l’estimation de la cardinalité, tout en gardant le plan plus rapide pour les cas exceptionnels.  
  
## <a name="how-to-activate-the-best-query-plan"></a>Comment activer le meilleur plan de requête  
  
Supposez qu’avec l’estimation de la cardinalité 120 ou plus, un plan de requête moins efficace est généré pour votre requête. Voici quelques options permettant d’activer le meilleur plan :  
  
1. Vous pouvez définir le niveau de compatibilité sur une valeur inférieure à la dernière disponible pour l’ensemble de votre base de données.  
  
   - Par exemple, définir le niveau de compatibilité à une valeur inférieure ou égale à 110 active l’estimation de la cardinalité 70, mais rend toutes les requêtes soumises au modèle d’estimation de la cardinalité précédent.  
  
   - De plus, si vous définissez un niveau de compatibilité inférieur, vous ne bénéficiez pas d’un certain nombre d’améliorations dans l’optimiseur de requête pour les versions les plus récentes.  
  
2. L’option de base de données `LEGACY_CARDINALITY_ESTIMATION` vous permet d’utiliser l’ancienne version de l’estimation de la cardinalité pour l’ensemble de la base de données, tout en profitant des autres améliorations de l’optimiseur de requête.   

3. Vous pouvez utiliser l’indicateur de requête `LEGACY_CARDINALITY_ESTIMATION` pour faire en sorte qu’une requête spécifique utilise l’ancienne estimation de la cardinalité, tout en conservant les autres améliorations de l’optimiseur de requête.  
  
Pour un contrôle accru, vous pouvez *forcer* le système à utiliser le plan généré avec l’estimation de la cardinalité 70 durant vos tests. Après avoir *épinglé* votre plan par défaut, vous pouvez configurer votre base de données dans son intégralité pour qu’elle utilise le niveau de compatibilité et l’estimation de cardinalité les plus récents. L’utilisation de cette option est développée plus loin dans cette rubrique.  
  
### <a name="how-to-force-a-particular-query-plan"></a>Comment forcer un plan de requête  
  
Le magasin de requêtes vous propose différentes façons de forcer le système à utiliser un plan de requête :  
  
- Exécutez **sp_query_store_force_plan**.  
  
- Dans [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)], développez le nœud de votre **Magasin de requêtes**, cliquez avec le bouton droit sur **Principaux nœuds consommateurs de ressources**, puis cliquez sur **Afficher les principaux nœuds consommateurs de ressources**. L’affichage montre les boutons **Forcer le plan** et **Annuler l’obligation d’utiliser le plan**.  
  
Pour plus d’informations sur le magasin de requêtes, consultez [Analyse des performances à l’aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="examples-of-ce-improvements"></a>Exemples d’améliorations apportées à l’estimation de la cardinalité  
  
Cette section contient des exemples de requêtes qui tirent parti des améliorations implémentées dans les versions récentes de l’estimation de la cardinalité. Il s’agit là d’informations générales qui ne nécessitent pas d’intervention de votre part.  
  
### <a name="example-a-ce-understands-maximum-value-might-be-higher-than-when-statistics-were-last-gathered"></a>Exemple A. L’estimation de la cardinalité comprend que la valeur maximale peut être plus élevée que lors de la dernière collecte de statistiques  
  
Supposez que la dernière collecte de statistiques pour `OrderTable` a eu lieu le `2016-04-30`, quand la valeur maximale `OrderAddedDate` était `2016-04-30`. L’estimation de la cardinalité 120 (et versions ultérieures) comprend que les colonnes dans `OrderTable` ayant des données *croissantes* peuvent avoir des valeurs supérieures aux valeurs maximales enregistrées par les statistiques. Cette compréhension permet d’améliorer le plan de requête pour les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT telles que la suivante.  
  
```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01';  
```  
  
### <a name="example-b-ce-understands-that-filtered-predicates-on-the-same-table-are-often-correlated"></a>Exemple B. L’estimation de la cardinalité comprend que les prédicats filtrés sur la même table sont souvent corrélés  
  
Dans l’instruction SELECT suivante, nous voyons les prédicats filtrés sur `Model` et `ModelVariant`. Nous comprenons intuitivement que quand `Model` est « Xbox », il est possible que `ModelVariant` soit « One », étant donné que Xbox a une variante appelée One.  
  
À compter de l’estimation de la cardinalité avec le niveau 120, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprend qu’il peut y avoir une corrélation entre les deux colonnes de la même table (`Model` et `ModelVariant`). L’estimation de la cardinalité fait une estimation plus précise du nombre de lignes retournées par la requête, et l’[optimiseur de requête](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements) génère un plan plus optimal.  
  
```sql  
SELECT Model, Purchase_Price  
FROM dbo.Hardware  
WHERE Model = 'Xbox' AND  
      ModelVariant = 'One';  
```  
  
### <a name="example-c-ce-no-longer-assumes-any-correlation-between-filtered-predicates-from-different-tables"></a>Exemple C. L’estimation de la cardinalité ne suppose plus aucune corrélation entre les prédicats filtrés issus de tables différentes 
Grâce à la nouvelle recherche étendue sur les charges de travail modernes, les données métier ont révélé que les filtres de prédicat issus de différentes tables ne sont généralement pas corrélés. Dans la requête suivante, l’estimation de la cardinalité suppose qu’il n’existe aucune corrélation entre `s.type` et `r.date`. Par conséquent, l’estimation de la cardinalité obtient une estimation plus basse que le nombre de lignes retournées.  
  
```sql  
SELECT s.ticket, s.customer, r.store  
FROM dbo.Sales    AS s  
CROSS JOIN dbo.Returns  AS r  
WHERE s.ticket = r.ticket AND  
      s.type = 'toy' AND  
      r.date = '2016-05-11';  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](http://msdn.microsoft.com/library/dn673537.aspx) (Optimisation de vos plans de requête avec l’estimateur de cardinalité SQL Server 2014)  
 [Indicateurs de requête](../../t-sql/queries/hints-transact-sql-query.md) [Indicateurs de requête USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint)     
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)    
 [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md)   
