---
title: Alimenter des index de recherche en texte intégral | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.component: search
ms.reviewer: ''
ms.suite: sql
ms.technology: search
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- index populations [full-text search]
- incremental populations [full-text search]
- populations [full-text search]
- full-text search [SQL Server], populations
- crawls [full-text search]
- automatic full-text index updates
- change tracking-based populations [full-text search]
- manual updates [full-text search]
- manual populations [full-text search]
- auto populations [full-text search]
- full-text indexes [SQL Server], key column
- full populations [full-text search]
- full-text indexes [SQL Server], populations
ms.assetid: 76767b20-ef55-49ce-8dc4-e77cb8ff618a
caps.latest.revision: 78
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 15619488c2b8d9f71423af9a0ca853b74ed5b12b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="populate-full-text-indexes"></a>Alimenter des index de recherche en texte intégral
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La création et la maintenance d’un index de recherche en texte intégral impliquent le remplissage de l’index à l’aide d’un processus appelé *alimentation* (également appelé *analyse*).  
  
##  <a name="types"></a> Types d’alimentation  
Un index de recherche en texte intégral prend en charge les types d’alimentation suivants :
-   Alimentation **complète**
-   Alimentation automatique ou manuelle basée sur le **suivi des modifications**
-   Alimentation incrémentielle basée sur un **horodatage**
  
## <a name="full-population"></a>Alimentation complète  
 Au cours d'une alimentation complète, les entrées d'index sont créées pour toutes les lignes d'une table ou d'une vue indexée. Une alimentation complète d'un index de recherche en texte intégral crée des entrées d'index pour toutes les lignes de la table de base ou de la vue indexée.  
  
Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alimente complètement un nouvel index de recherche en texte intégral dès que celui-ci est créé.
-   D’une part, une alimentation complète peut consommer beaucoup de ressources. Par conséquent, si vous créez un index de recherche en texte intégral pendant une période de pointe, il est recommandé de reporter l’alimentation complète à une période creuse, en particulier si la table de base d’un index de recherche en texte intégral est volumineuse.
-   D’autre part, le catalogue de texte intégral auquel l’index appartient n’est pas utilisable tant que tous ses index de recherche en texte intégral ne sont pas alimentés.

Pour créer un index de recherche en texte intégral sans le remplir immédiatement, spécifiez la clause `CHANGE_TRACKING OFF, NO POPULATION` dans l’instruction `CREATE FULLTEXT INDEX`. Si vous spécifiez `CHANGE_TRACKING MANUAL`, le moteur d’indexation et de recherche en texte intégral ne remplit pas le nouvel index de recherche en texte intégral tant que vous n’exécutez pas une instruction `ALTER FULLTEXT INDEX` à l’aide de la clause `START FULL POPULATION` ou `START INCREMENTAL POPULATION`. 

### <a name="example---create-a-full-text-index-without-running-a-full-population"></a>Exemple - Créer un index de recherche en texte intégral sans exécuter une alimentation complète  
 L'exemple ci-après crée un index de recherche en texte intégral sur la table `Production.Document` de l'exemple de base de données `AdventureWorks` . Cet exemple utilise `WITH CHANGE_TRACKING OFF, NO POPULATION` pour différer l’alimentation complète initiale.  
  
```sql
CREATE UNIQUE INDEX ui_ukDoc ON Production.Document(DocumentID);  
CREATE FULLTEXT CATALOG AW_Production_FTCat;  
CREATE FULLTEXT INDEX ON Production.Document  
(  
    Document                         --Full-text index column name   
        TYPE COLUMN FileExtension    --Name of column that contains file type information  
        Language 1033                 --1033 is LCID for the English language  
)  
    KEY INDEX ui_ukDoc  
    ON AW_Production_FTCat  
    WITH CHANGE_TRACKING OFF, NO POPULATION;  
GO  
  
```  
  
### <a name="example---run-a-full-population-on-a-table"></a>Exemple - Exécuter une alimentation complète sur une table  
 L'exemple ci-après exécute une alimentation complète sur la table `Production.Document` de l'exemple de base de données `AdventureWorks` .  
  
```sql
ALTER FULLTEXT INDEX ON Production.Document  
   START FULL POPULATION;  
```  
   
## <a name="population-based-on-change-tracking"></a>Alimentation basée sur le suivi des modifications
 Vous pouvez éventuellement utiliser le suivi des modifications pour procéder à la gestion d'un index de recherche en texte intégral après son alimentation complète initiale. La surcharge associée au suivi des modifications est réduite, car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère une table dans laquelle il suit les modifications apportées à la table de base depuis la dernière alimentation. Quand vous utilisez le suivi des modifications, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve un enregistrement des lignes de la table de base ou de la vue indexée ayant été modifiées par des opérations de mise à jour, de suppression ou d’insertion. Les modifications apportées aux données à l’aide de WRITETEXT et d’UPDATETEXT ne sont pas répercutées dans l’index de recherche en texte intégral, et ne sont pas prises en compte par le suivi des modifications.  
  
> [!NOTE]  
>  Pour les tables qui contiennent une colonne **timestamp**, vous pouvez utiliser des alimentations incrémentielles plutôt que le suivi des modifications.  
  
 Quand vous activez le suivi des modifications pendant la création d’index, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alimente complètement le nouvel index de recherche en texte intégral immédiatement après sa création. Ensuite, les modifications font l'objet d'un suivi et propagées à l'index de recherche en texte intégral.

### <a name="enable-change-tracking"></a>Activer le suivi des modifications
Il existe deux types de suivi des modifications :
-   Automatique (option`CHANGE_TRACKING AUTO`). Le suivi des modifications automatique est le comportement par défaut.
-   Manuel (option `CHANGE_TRACKING MANUAL`).   
  
 Le type de suivi des modifications détermine la façon dont l'index de recherche en texte intégral est alimenté, comme expliqué ci-après.  
  
-   **Alimentation automatique**  
  
     Par défaut, ou si vous spécifiez `CHANGE_TRACKING AUTO`, le moteur d’indexation et de recherche en texte intégral utilise l’alimentation automatique sur l’index de recherche en texte intégral. Une fois l'alimentation complète initiale terminée, les données modifiées dans la table de base font l'objet d'un suivi, et ces modifications sont propagées automatiquement. L'index de recherche en texte intégral étant toutefois mis à jour en arrière-plan, il se peut que les modifications propagées ne soient pas répercutées immédiatement dans l'index.  
  
     **Pour commencer le suivi des modifications avec alimentation automatique**  
  
    -   [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md) … WITH CHANGE_TRACKING AUTO  
  
    -   [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) … SET CHANGE_TRACKING AUTO  
  
    **Exemple - Modifier un index de recherche en texte intégral pour qu’il utilise le suivi des modifications automatique**  
    L'exemple ci-après modifie l'index de recherche en texte intégral de la table `HumanResources.JobCandidate` de l'exemple de base de données `AdventureWorks` pour qu'il utilise le suivi des modifications avec alimentation automatique.  
  
    ```sql  
    USE AdventureWorks;  
    GO  
    ALTER FULLTEXT INDEX ON HumanResources.JobCandidate SET CHANGE_TRACKING AUTO;  
    GO   
    ```  
  
-   **Alimentation manuelle**  
  
     Si vous spécifiez CHANGE_TRACKING MANUAL, le moteur d'indexation et de recherche en texte intégral utilise le remplissage manuel sur l'index de recherche en texte intégral. Une fois le remplissage complet initial terminé, les données modifiées dans la table de base font l'objet d'un suivi. Toutefois, elles ne sont pas propagées à l’index de recherche en texte intégral tant que vous n’exécutez pas une instruction ALTER FULLTEXT INDEX … START UPDATE POPULATION . Vous pouvez utiliser l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour appeler régulièrement cette instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
     **Pour commencer le suivi des modifications avec remplissage manuel**  
  
    -   [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md) … WITH CHANGE_TRACKING MANUAL  
  
    -   [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) … SET CHANGE_TRACKING MANUAL  
  
    **Exemple - Créer un index de recherche en texte intégral avec le suivi des modifications manuel**  
    L'exemple ci-après crée un index de recherche en texte intégral qui utilisera le suivi des modifications avec alimentation manuelle sur la table `HumanResources.JobCandidate` de l'exemple de base de données `AdventureWorks` .  
  
    ```sql
    USE AdventureWorks;  
    GO  
    CREATE UNIQUE INDEX ui_ukJobCand ON HumanResources.JobCandidate(JobCandidateID);  
    CREATE FULLTEXT CATALOG ft AS DEFAULT;  
    CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume)   
       KEY INDEX ui_ukJobCand   
       WITH CHANGE_TRACKING=MANUAL;  
    GO  
    ```  
  
    **Exemple - Exécuter une alimentation manuelle**  
    L'exemple ci-après exécute une alimentation manuelle sur l'index de recherche en texte intégral faisant l'objet d'un suivi des modifications, sur la table `HumanResources.JobCandidate` de l'exemple de base de données `AdventureWorks` .  
  
    ```sql 
    USE AdventureWorks;  
    GO  
    ALTER FULLTEXT INDEX ON HumanResources.JobCandidate START UPDATE POPULATION;  
    GO  
    ```
   
### <a name="disable-change-tracking"></a>Désactiver le suivi des modifications 
  
-   [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md) … WITH CHANGE_TRACKING OFF  
  
-   [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) … SET CHANGE_TRACKING OFF  
   
  
## <a name="incremental-population-based-on-a-timestamp"></a>Alimentation incrémentielle basée sur un horodatage  
 L'alimentation incrémentielle est un autre mécanisme permettant d'alimenter manuellement un index de recherche en texte intégral. Si une table fait l'objet d'un nombre important d'insertions, l'alimentation incrémentielle peut s'avérer plus efficace que l'alimentation manuelle.
 
 Vous pouvez exécuter une alimentation incrémentielle pour un index de recherche en texte intégral pour lequel CHANGE_TRACKING a la valeur MANUAL ou OFF. 
  
 Pour permettre une alimentation incrémentielle, la table indexée doit disposer d'une colonne dont le type de données est **timestamp** . S'il n'existe pas de colonne de type **timestamp** , l'alimentation incrémentielle ne peut s'effectuer.   

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la colonne **timestamp** pour identifier des lignes qui ont été modifiées depuis la dernière alimentation. L'alimentation incrémentielle met alors à jour l'index de recherche en texte intégral avec les lignes ajoutées, supprimées ou modifiées après la dernière alimentation ou pendant l'exécution de cette dernière. À la fin d’une alimentation, le moteur d’indexation et de recherche en texte intégral enregistre une nouvelle valeur **timestamp** . Il s’agit de la plus grande valeur **timestamp** trouvée par l’utilitaire de rassemblement SQL. Cette valeur sera utilisée au démarrage de la prochaine alimentation incrémentielle.  
 
Dans certains cas, la demande d’une alimentation incrémentielle entraîne une alimentation complète.
-   Une demande d'alimentation incrémentielle dans une table sans colonne de type **timestamp** entraîne une alimentation complète.
-   Si la première alimentation d'un index de recherche en texte intégral est une alimentation incrémentielle, elle indexe toutes les lignes, ce qui équivaut à une alimentation complète. 
-   Si des métadonnées concernant l’index de recherche en texte intégral de la table ont changé depuis la dernière alimentation, les demandes d’alimentation incrémentielle sont implémentées comme des alimentations complètes. Cela concerne les modifications de métadonnées provoquées par des modifications de définitions de colonne, d'index ou d'index de recherche en texte intégral. 

### <a name="run-an-incremental-population"></a>Exécuter une alimentation incrémentielle
  
 Pour exécuter une alimentation incrémentielle, exécutez une instruction `ALTER FULLTEXT INDEX` à l’aide de la clause `START INCREMENTAL POPULATION`.  
  
###  <a name="create"></a> Créer ou modifier une planification pour l’alimentation incrémentielle   
  
1.  Dans Management Studio, dans l’Explorateur d’objets, développez le serveur.  
  
2.  Développez **Bases de données**, puis la base de données qui contient l’index de recherche en texte intégral.  
  
3.  Développez **Tables**.  
  
    Cliquez avec le bouton droit sur la table sur laquelle l’index de recherche en texte intégral est défini, sélectionnez **Index de recherche en texte intégral**et, dans le menu contextuel **Index de recherche en texte intégral** , cliquez sur **Propriétés**. La boîte de dialogue **Propriétés d’index de recherche en texte intégral** s’affiche.  

    > [!IMPORTANT]  
    >  Si la table ou la vue de base ne contient pas une colonne du type de données **timestamp**, une alimentation incrémentielle n’est pas possible.
      
1.  Dans le volet **Sélectionner une page**, sélectionnez **Planifications**.  
  
     Utilisez cette page pour créer ou gérer des planifications pour un travail de l'Agent SQL Server qui démarre une alimentation de table incrémentielle sur la table de base ou la vue indexée de l'index de recherche en texte intégral.  

     Les options disponibles sont les suivantes :  
  
    -   Pour **créer** une planification, cliquez sur **Nouveau**.  
  
        La boîte de dialogue **Nouvelle planification de la table d’indexation de texte intégral** s’affiche pour vous permettre de créer une planification. Pour enregistrer la planification, cliquez sur **OK**.  
  
        > [!IMPORTANT]  
        >  Un travail SQL Server Agent (démarrage de l’alimentation de table incrémentielle sur *nom_base_de_données*.*nom_table*) est associé à une nouvelle planification une fois que vous avez fermé la boîte de dialogue **Propriétés d’index de texte intégral** . Si vous créez plusieurs planifications pour le même index de recherche en texte intégral, elles utilisent toutes le même travail.  
  
    -   Pour **changer** une planification existante, sélectionnez-la, puis cliquez sur **Modifier**.  
  
         La boîte de dialogue **Nouvelle planification de la table d’indexation de texte intégral** s’affiche pour vous permettre de modifier la planification.  
  
        > [!NOTE]  
        >  Pour plus d’informations sur la modification d’un travail SQL Server Agent, consultez [Modifier un travail](http://msdn.microsoft.com/library/dd5e5f20-20c4-4ab9-a19a-db87577dcd43).  
  
    -   Pour **supprimer** une planification existante, sélectionnez-la, puis cliquez sur **Supprimer**.  
  
2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]   
  
##  <a name="crawl"></a> Résoudre les erreurs dans une alimentation de texte intégral (analyse)  
Lorsqu'une erreur se produit durant une analyse, la fonction d'analyse de la recherche en texte intégral crée et conserve un journal de l'analyse sous forme de fichier texte. Chaque journal de l'analyse correspond à un catalogue de texte intégral particulier. Par défaut, les journaux d’analyse pour une instance donnée (dans cet exemple, l’instance par défaut) figurent dans le dossier `%ProgramFiles%\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\LOG`.
 
Le fichier journal de l'analyse respecte le modèle de dénomination suivant :  
  
`SQLFT<DatabaseID><FullTextCatalogID>.LOG[<n>]`
  
Les parties variables du nom de fichier du journal d’analyse sont les suivantes.
-   <**DatabaseID**> - ID d’une base de données. <**dbid**> est un nombre à 5 chiffres commençant par des zéros non significatifs.  
-   <**FullTextCatalogID**> - ID du catalogue de texte intégral. <**catid**> est un nombre à 5 chiffres commençant par des zéros non significatifs.  
-   <**n**> - Entier qui indique qu’il existe un ou plusieurs journaux d’analyse du même catalogue de texte intégral.  
  
 Par exemple, `SQLFT0000500008.2` est le fichier journal d’analyse pour une base de données ayant un ID de base de données = 5 et un ID de catalogue de texte intégral = 8. Le 2 à la fin du nom de fichier indique qu'il existe deux fichiers journaux d'analyse pour cette combinaison base de données/catalogue.  

## <a name="see-also"></a> Voir aussi  
 [sys.dm_fts_index_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)   
 [Commencer à utiliser la recherche en texte intégral](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
