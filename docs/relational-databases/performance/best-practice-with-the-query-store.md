---
title: Bonnes pratiques relatives au magasin de requêtes | Microsoft Docs
ms.custom: ''
ms.date: 11/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Query Store, best practices
ms.assetid: 5b13b5ac-1e4c-45e7-bda7-ebebe2784551
caps.latest.revision: 24
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c6b667bf8be87561461c1ceec2543dcce051b93e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="best-practice-with-the-query-store"></a>Bonnes pratiques relatives au magasin de requêtes
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Cette rubrique décrit les bonnes pratiques concernant l’utilisation du magasin de requêtes avec votre charge de travail.  
  
##  <a name="SSMS"></a>Utiliser la dernière version de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] propose un ensemble d’interfaces utilisateur conçu pour configurer le magasin de requêtes et consommer les données collectées relatives à votre charge de travail.  
Téléchargez la dernière version de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] [ici](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms).  
  
 Pour obtenir une description rapide de l’utilisation du Magasin des requêtes dans des scénarios de résolution des problèmes, consultez les informations relatives au [Magasin des requêtes @Azure Blogs](https://azure.microsoft.com/en-us/blog/query-store-a-flight-data-recorder-for-your-database/).  
  
##  <a name="Insight"></a> Utiliser Query Performance Insight dans Azure SQL Database  
 Si vous exécutez le magasin de requêtes dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , vous pouvez utiliser **Query Performance Insight** pour analyser la consommation DTU dans le temps.  
Même si vous pouvez utiliser [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour obtenir des détails sur la consommation en ressources de toutes vos requêtes (processeur, mémoire, E/S, etc.), Query Performance Insight vous offre un moyen rapide et efficace de déterminer leur impact sur la consommation DTU globale de votre base de données.  
Pour plus d’informations, consultez [Query Performance Insight pour base de données SQL Azure](https://azure.microsoft.com/documentation/articles/sql-database-query-performance/).    

##  <a name="using-query-store-with-elastic-pool-databases"></a>Utilisation du magasin de requêtes avec des bases de données de pool élastique
Vous pouvez utiliser le magasin de requêtes dans toutes les bases de données sans le moindre problème, même dans les pools très denses. Tous les problèmes liés à l’utilisation excessive de ressources qui peuvent s’être produits lors de l’activation du magasin de requêtes pour le grand nombre de base de données des pools élastiques ont été résolus.

##  <a name="Configure"></a> Veiller à l’adéquation entre le magasin de requêtes et votre charge de travail  
 Configurez le magasin de requêtes en fonction de votre charge de travail et selon vos besoins en matière de résolution des problèmes de performances.   
Si les paramètres par défaut sont adaptés pour un démarrage rapide, vous devez cependant surveiller le comportement du magasin de requêtes dans le temps et ajuster sa configuration en conséquence :  
  
 ![propriétés du magasin de requêtes](../../relational-databases/performance/media/query-store-properties.png "propriétés du magasin de requêtes")  
  
 Voici les recommandations à suivre pour définir les valeurs des paramètres :  
  
 **Taille maximale (Mo) :** indique la limite de l’espace de données qu’occupera le magasin de requêtes à l’intérieur de votre base de données.  Paramètre le plus important, il affecte directement le mode d’opération du magasin de requêtes.  
  
 Pendant que le magasin de requêtes collecte requêtes, plans d’exécution et autres statistiques, sa taille dans la base de données croît jusqu’à ce que cette limite soit atteinte. Quand cela se produit, le magasin de requêtes change automatiquement de mode d’opération pour passer en lecture seule et cesse de collecter les nouvelles données, ce qui signifie que l’analyse de vos performances n’est plus précise.  
  
 La valeur par défaut (100 Mo) peut ne pas suffire si votre charge de travail génère un grand nombre de requêtes et de plans différents ou si vous souhaitez conserver l’historique de requêtes sur une plus longue période. Suivez l’utilisation d’espace actuelle et augmentez la taille maximale (Mo) pour empêcher le magasin de requêtes de passer en mode lecture seule.  Utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou exécutez le script suivant pour obtenir les dernières informations concernant la taille du magasin de requêtes :  
  
```sql 
USE [QueryStoreDB];  
GO  
  
SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,   
    max_storage_size_mb, readonly_reason  
FROM sys.database_query_store_options;  
```  
  
 Le script suivant permet de définir une nouvelle taille maximale (Mo) :  
  
```sql  
ALTER DATABASE [QueryStoreDB]  
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);  
```  
  
 **Intervalle de collecte des statistiques :** définit le niveau de granularité des statistiques d’exécution collectées (la valeur par défaut est 1 heure). Envisagez d’utiliser une valeur inférieure si vous avez besoin d’une granularité plus fine ou une durée inférieure pour détecter et atténuer les problèmes, mais gardez à l’esprit que cela a un effet direct sur la taille des données du magasin de requêtes. Pour attribuer une valeur différente au paramètre Intervalle de collecte des statistiques, utilisez SSMS ou Transact-SQL :  
  
```sql  
ALTER DATABASE [QueryStoreDB] SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 60);  
```  
  
 **Seuil de requêtes périmées (jours) :** stratégie de nettoyage basé sur la durée qui permet de contrôler la période de rétention des statistiques d’exécution persistantes et des requêtes inactives.  
Par défaut, le magasin de requêtes est configuré pour conserver les données pendant 30 jours, ce qui est peut-être inutilement long pour votre scénario.  
  
 Évitez de conserver les données d’historique que vous ne prévoyez pas d’utiliser. Cela réduira les passages à l’état lecture seule. La taille des données du magasin de requêtes et le temps de détection et d’atténuation des problèmes seront plus prévisibles. Utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou le script suivant pour configurer la stratégie de nettoyage basée sur la durée :  
  
```sql  
ALTER DATABASE [QueryStoreDB]   
SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 90));  
```  
  
 **Mode de nettoyage basé sur la taille :** indique si le nettoyage de données automatique aura lieu dès que la taille des données du magasin de requêtes s’approchera de la limite.  
  
 Il est vivement recommandé d’activer le nettoyage basée sur la taille de sorte que le magasin de requêtes s’exécute toujours en mode lecture-écriture et collecte les données les plus récentes.  
  
```sql  
ALTER DATABASE [QueryStoreDB]   
SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);  
```  
  
 **Mode de capture du magasin de requêtes :** spécifie la stratégie de capture de requêtes du magasin de requêtes.  
  
-   **All** : capture toutes les requêtes. Il s'agit de l'option par défaut.  
  
-   **Auto** : les requêtes peu fréquentes et les requêtes dont la durée de compilation et d’exécution n’est pas significative sont ignorées. Les seuils concernant le nombre d’exécutions et la durée de compilation et d’exécution sont déterminés en interne.  
  
-   **None** : le magasin de requêtes cesse de capturer les nouvelles requêtes.  
  
 Le script suivant permet de définir le mode de capture de requête sur Auto :  
  
```sql  
ALTER DATABASE [QueryStoreDB]   
SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);  
```  
  
## <a name="how-to-start-with-query-performance-troubleshooting"></a>Comment appréhender la résolution des problèmes de performances des requêtes  
 La résolution des problèmes liés au magasin de requêtes est un flux de travail simple, comme l’illustre le diagramme suivant :  
  
 ![résolution des problèmes du magasin de requêtes](../../relational-databases/performance/media/query-store-troubleshooting.png "résolution des problèmes du magasin de requêtes")  
  
 Activez le magasin de requêtes à l’aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , comme décrit dans la section précédente, ou exécutez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
```sql  
ALTER DATABASE [DatabaseOne] SET QUERY_STORE = ON;  
```  
  
 Le magasin de requêtes a besoin d’un certain temps avant de collecter le jeu de données qui représente avec précision votre charge de travail. En générale, un jour suffit, même pour les charges de travail très complexes. Néanmoins, vous pouvez commencer à explorer les données et à identifier les requêtes qui demandent votre attention de suite après avoir activé la fonctionnalité.   
Accédez au sous-dossier Magasin de requêtes sous le nœud Bases de données de l’Explorateur d’objets de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour ouvrir les vues de résolution de problèmes de scénarios spécifiques.   
Les vues du magasin de requêtes de[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] fonctionnent avec l’ensemble de métriques d’exécution, chacune exprimée comme étant l’une des fonctions statistiques suivantes :  
  
|Version [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Métrique d’exécution|Fonction statistique|  
|----------------------|----------------------|------------------------|  
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|Temps processeur, Durée, Nombre d’exécutions, Lectures logiques, Écritures logiques, Consommation de mémoire, Lectures physiques, Durée du CLR, Degré de parallélisme et Nombre de lignes|Moyenne, Maximum, Minimum, Écart type, Total|
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|Temps processeur, Durée, Nombre d’exécutions, Lectures logiques, Écritures logiques, Consommation de mémoire, Lectures physiques, Durée du CLR, Degré de parallélisme, Nombre de lignes, Mémoire utilisée par la journalisation, Délai d’attente|Moyenne, Maximum, Minimum, Écart type, Total|
  
 Le graphique suivant montre comment trouver les vues du magasin de requêtes :  
  
 ![vues du magasin de requêtes](../../relational-databases/performance/media/query-store-views.png "vues du magasin de requêtes")  
  
 Le tableau suivant explique quand utiliser chaque vue du magasin de requêtes :  
  
|Vue SSMS|Scénario|  
|---------------|--------------|  
|Requêtes régressées|Identifie les requêtes dont les métriques d’exécution ont récemment régressé (c’est-à-dire, dont l’état s’est aggravé). <br />Utilisez cette vue pour mettre en corrélation les problèmes de performances observés dans votre application avec les requêtes réelles qui ont besoin d’être corrigées ou améliorées.|  
|Consommation globale des ressources|Analyse la consommation totale de ressources pour la base de données par rapport à l’une des métriques d’exécution.<br />Utilisez cette vue pour identifier des modèles de ressources (charges de travail diurnes/nocturnes) et optimiser la consommation globale pour votre base de données.|  
|Principales requêtes consommatrices de ressources|Choisissez une mesure d’exécution présentant un intérêt et identifiez les requêtes qui ont enregistré les valeurs les plus extrêmes sur un intervalle de temps donné. <br />Utilisez cette vue pour concentrer votre attention sur les requêtes les plus pertinentes, qui ont le plus fort impact sur la consommation en ressources de base de données.|  
|Requêtes avec des plans forcés|Liste les plans forcés à l’aide du Magasin des requêtes. <br />Utilisez cette vue pour accéder rapidement à tous les plans forcés.|  
|Requêtes avec variation forte|Analysez les requêtes ayant une forte variation d’exécution en lien avec les dimensions disponibles, notamment la durée, le temps processeur, les E/S et l’utilisation de la mémoire dans l’intervalle de temps souhaité.<br />Utilisez cette vue pour identifier les requêtes avec des performances extrêmement variables qui peuvent impacter l’expérience utilisateur dans vos applications.|  
|Requêtes suivies|Suit l’exécution des requêtes les plus importantes en temps réel. En règle générale, vous utilisez cette vue quand certaines de vos requêtes sont soumises à des plans forcés et que vous voulez vérifier que les performances des requêtes sont stables.|
  
> [!TIP]  
>  Pour savoir comment identifier les principales requêtes consommatrices de ressources et corriger celles qui ont régressé en raison d’un changement de plan à l’aide de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], consultez les blogs @Azure sur le [magasin de requêtes](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).  
  
 Quand vous identifiez une requête dont les performances ne sont pas optimales, votre action dépend de la nature du problème.  
  
-   Si la requête a été exécutée avec plusieurs plans et que le dernier est nettement plus mauvais que le précédent, vous pouvez utiliser le mécanisme de forçage de plan pour forcer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à toujours utiliser le plan optimal pour les exécutions futures.  
  
     ![magasin de requêtes-forcer le plan](../../relational-databases/performance/media/query-store-force-plan.png "magasin de requêtes-forcer le plan")  

> [!NOTE]  
> Le graphique ci-dessus peut présenter des formes différentes pour des plans de requête spécifiques, avec les significations suivantes pour chaque état possible :<br />  
> |Graphique à base de formes|Signification|  
> |-------------------|-------------|
> |Cercle|Requête effectuée (exécution normale achevée correctement)|
> |Carré|Annulée (le client est à l’origine de l’abandon de l’exécution)|
> |Triangle|Échec (abandon d’exécution lié à une exception)|
> De plus, la taille de la forme reflète le nombre d’exécutions des requêtes dans l’intervalle de temps spécifié. Elle augmente en fonction du nombre d’exécutions.  

-   Vous pouvez en déduire qu’il manque un index à votre requête pour qu’elle s’exécute de façon optimale. Ces informations apparaissent dans le plan d’exécution de requête. Créez l’index manquant et vérifiez les performances de requête en utilisant le magasin de requêtes.  
  
     ![magasin de requêtes-afficher le plan](../../relational-databases/performance/media/query-store-show-plan.png "magasin de requêtes-afficher le plan")  
  
     Si vous exécutez votre charge de travail sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)], inscrivez-vous à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Index Advisor pour recevoir automatiquement des recommandations d’index.  
  
-   Dans certains cas, vous pouvez imposer une recompilation des statistiques si vous constatez une grande différence entre le nombre estimé et le nombre réel de lignes dans le plan d’exécution.  
  
-   Réécrivez les requêtes problématiques, par exemple, pour profiter du paramétrage des requêtes ou pour implémenter une logique plus optimale.  
  
##  <a name="Verify"></a> Vérifier que le magasin des requêtes collecte les données des requêtes en continu  
 Le magasin de requêtes peut modifier discrètement le mode d’opération. Vous avez donc tout intérêt à surveiller régulièrement l’état du magasin de requêtes pour vérifier qu’il fonctionne bien et prendre des mesures pour éviter des défaillances dont les causes étaient évitables. Exécutez la requête suivante pour déterminer le mode d’opération et afficher les paramètres les plus pertinents :  
  
```sql
USE [QueryStoreDB];  
GO  
  
SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,   
    max_storage_size_mb, readonly_reason, interval_length_minutes,   
    stale_query_threshold_days, size_based_cleanup_mode_desc,   
    query_capture_mode_desc  
FROM sys.database_query_store_options;  
```  
  
 La différence entre `actual_state_desc` et `desired_state_desc` indique que le mode d’opération a changé automatiquement. Le changement le plus courant est celui qui concerne le magasin de requêtes, qui bascule discrètement en mode lecture seule. Dans certaines circonstances très rares, le magasin de requêtes peut finir à l’état ERREUR en raison d’erreurs internes.  
  
 Quand l’état effectif est Lecture seule, consultez la colonne **readonly_reason** pour déterminer la cause première. En règle générale, le magasin de requêtes passe en mode lecture seule quand le quota de taille est dépassé. Dans ce cas, **readonly_reason** a la valeur 65536. Pour d’autres raisons, consultez [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md).  
  
 Pour faire basculer le magasin de requêtes en mode lecture-écriture et activer la collecte de données, envisagez les mesures suivantes :  
  
-   Augmentez la taille maximale de stockage en utilisant l’option **MAX_STORAGE_SIZE_MB** de la commande **ALTER DATABASE**.  
  
-   Nettoyez les données du magasin de requêtes à l’aide de l’instruction suivante :  
  
    ```sql  
    ALTER DATABASE [QueryStoreDB] SET QUERY_STORE CLEAR;  
    ```  
  
Vous pouvez appliquer une ou deux de ces mesures en exécutant l’instruction suivante qui remet explicitement le mode d’opération en lecture-écriture :  
  
```sql  
ALTER DATABASE [QueryStoreDB]   
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);  
```  
  
 À titre préventif, prenez les mesures suivantes :  
  
-   Vous pouvez prévenir les changements discrets du mode d’opération en appliquant les bonnes pratiques. Si vous vérifiez que la taille du magasin de requêtes est toujours inférieure à la valeur maximale autorisée, vous réduirez considérablement les chances de passer en mode lecture seule. Activez la stratégie basée sur la taille comme indiqué dans la section [Configurer le magasin de requêtes](#Configure) pour que le magasin de requêtes nettoie automatiquement les données quand la taille s’approche de la limite.  
  
-   Pour faire en sorte que les données les plus récentes soient conservées, configurez la stratégie basée sur la durée pour supprimer régulièrement les informations obsolètes.  
  
-   Enfin, envisagez de définir le mode de capture de requête sur Auto pour éliminer les requêtes les moins pertinentes par rapport à votre charge de travail.  
  
### <a name="error-state"></a>État d’erreur  
 Pour récupérer le magasin de requêtes, essayez de définir explicitement le mode lecture-écriture et vérifiez à nouveau l’état réel.  
  
```sql  
ALTER DATABASE [QueryStoreDB]   
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);    
GO  
  
SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,   
    max_storage_size_mb, readonly_reason, interval_length_minutes,   
    stale_query_threshold_days, size_based_cleanup_mode_desc,   
    query_capture_mode_desc  
FROM sys.database_query_store_options;  
```  
  
 Si le problème persiste, cela signifie que les données du magasin des requêtes sont endommagées sur le disque.
 
 Le Magasin des requêtes a pu être récupéré via l’exécution de la procédure stockée **sp_query_store_consistency_check** dans la base de données affectée.
 
 Si cela n’a pas résolu le problème, vous pouvez essayer d’effacer le magasin des requêtes avant de demander le mode lecture/écriture.  
  
```sql  
ALTER DATABASE [QueryStoreDB]   
SET QUERY_STORE CLEAR;  
GO  
  
ALTER DATABASE [QueryStoreDB]   
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);    
GO  
  
SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,   
    max_storage_size_mb, readonly_reason, interval_length_minutes,   
    stale_query_threshold_days, size_based_cleanup_mode_desc,   
    query_capture_mode_desc  
FROM sys.database_query_store_options;  
```  
  
## <a name="set-the-optimal-query-capture-mode"></a>Définir le mode de capture de requête optimal  
 Conservez les données les plus pertinentes dans le magasin de requêtes. Le tableau suivant décrit des scénarios types pour chaque mode de capture de requête :  
  
|Mode de capture de requête|Scénario|  
|------------------------|--------------|  
|All|Analysez minutieusement votre charge de travail, c’est-à-dire toutes les formes de requêtes, leur fréquence d’exécution et les autres statistiques.<br /><br /> Identifiez les nouvelles requêtes dans votre charge de travail.<br /><br /> Déterminez si des requêtes ad hoc sont utilisées pour identifier les possibilités de paramétrage utilisateur ou automatique.|  
|Auto|Concentrez-vous sur les requêtes pertinentes et exploitables, c’est-à-dire les requêtes qui s’exécutent régulièrement ou qui consomment beaucoup de ressources.|  
|None|Vous avez déjà capturé le jeu de requêtes que vous vouliez surveiller dans le runtime et souhaitez éliminer les confusions que pourraient provoquer les autres requêtes.<br /><br /> L’option Aucun(e) est adaptée aux environnements de test et d’évaluation.<br /><br /> Elle est aussi appropriée pour les éditeurs de logiciels qui proposent le magasin de requêtes avec une configuration destinée à surveiller la charge de travail de leur application.<br /><br /> Cette option doit être utilisée avec précaution, car vous risquez de ne pas pouvoir suivre et optimiser les nouvelles requêtes importantes. Évitez d’utiliser l’option Aucun(e) sauf si l’un de vos scénarios l’exige.|  
  
## <a name="keep-the-most-relevant-data-in-query-store"></a>Conserver les données les plus pertinentes dans le magasin des requêtes  
 Configurez le magasin de requêtes de sorte qu’il ne contienne que les données pertinentes. Ainsi, il s’exécutera toujours en offrant une excellente expérience de résolution des problèmes tout en ayant un impact minime sur votre charge de travail normale.  
Le tableau suivant décrit les bonnes pratiques :  
  
|Bonne pratique|Paramètre|  
|-------------------|-------------|  
|Limiter la conservation des données d’historique.|Configurez la stratégie basée sur la durée pour activer le nettoyage automatique.|  
|Filtrer les requêtes non pertinentes.|Configurez le mode de capture de requête sur Auto.|  
|Supprimer les requêtes les moins pertinentes dès que la taille maximale est atteinte.|Activez la stratégie de nettoyage basée sur la taille.|  
  
##  <a name="Parameterize"></a> Éviter l’utilisation de requêtes non paramétrées  
 Il est déconseillé d’utiliser des requêtes non paramétrées quand cela n’est pas indispensable (par exemple, dans le cas d’une analyse ad hoc).  Les plans mis en cache ne peuvent pas être réutilisés, ce qui force l’optimiseur de requête à compiler les requêtes pour chaque texte de requête unique. Pour plus d’informations à ce sujet, consultez [Principes d’utilisation du paramétrage forcé](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide).  
  De plus, le magasin de requêtes peut rapidement dépasser le quota de taille du fait du nombre potentiellement élevé de textes de requête différents et, partant, du nombre élevé de plans d’exécution différents de forme similaire.  
Par conséquent, votre charge de travail ne fonctionnera pas de façon optimale et le magasin de requêtes risque de basculer en mode lecture seule ou de supprimer constamment les données, essayant de suivre le rythme des requêtes entrantes.  
  
 Envisagez les possibilités suivantes :  

  -   Paramétrez des requêtes, le cas échéant, par exemple en les incluant dans un wrapper (procédure stockée ou sp_executesql). Pour plus d’informations à ce sujet, consultez [Réutilisation des paramètres et des plans d’exécution](../../relational-databases/query-processing-architecture-guide.md#PlanReuse).    
  
-   Utilisez l’option [**Optimiser pour les charges de travail ad hoc**](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) si votre charge de travail contient de nombreux lots ad hoc à usage unique avec des plans de requête différents.  
  
    -   Comparez le nombre de valeurs query_hash distinctes au nombre total d’entrées dans sys.query_store_query. Si le rapport est proche de 1, cela signifie que votre charge de travail ad hoc génère des requêtes différentes.  
  
-   Appliquez le [**paramétrage forcé**](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) pour la base de données ou une partie des requêtes si le nombre de plans de requête différents est modéré.  
  
    -   Utilisez le [repère de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md) pour forcer le paramétrage uniquement pour la requête sélectionnée.  
  
    -   Configurez le paramétrage forcé à l’aide de l’option de base de données [Parameterization](../../relational-databases/databases/database-properties-options-page.md#miscellaneous) si votre charge de travail ne contient qu’un petit nombre de plans de requête différents (quand le rapport entre le nombre de valeurs query_hash distinctes et le nombre total d’entrées dans sys.query_store_query est nettement inférieur à 1).  
  
-   Affectez la valeur AUTO au **Mode de capture de requête** pour éliminer automatiquement les requêtes ad hoc qui consomment peu de ressources.  
  
##  <a name="Drop"></a> Éviter un modèle DROP et CREATE en cas de conservation d’objets conteneurs pour les requêtes  
 Le magasin de requêtes associe une entrée de requête à un objet conteneur (procédure stockée, fonction et déclencheur).  Quand vous recréez un objet conteneur, une nouvelle entrée de requête est générée pour le même texte de requête. Cela vous empêche de suivre les statistiques de performances de cette requête dans le temps et d’utiliser le mécanisme de forçage de plan. Pour éviter cela, utilisez le processus `ALTER <object>` pour modifier la définition d’un objet conteneur quand cela est possible.  
  
##  <a name="CheckForced"></a> Vérifier régulièrement l’état des plans forcés  

 Le forçage de plan est un mécanisme pratique qui permet de corriger les problèmes de performances des requêtes importantes et de les rendre plus prévisibles. Or, comme pour les indicateurs de plan et les repères de plan, forcer un plan n’est pas la garantie qu’il sera utilisé dans les exécutions futures. En règle générale, quand le schéma de base de données change au point que les objets référencés par le plan d’exécution sont modifiés ou supprimés, le forçage de plan échoue. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a recours à la recompilation des requêtes et la raison réelle de l’échec du forçage apparaît dans [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). La requête suivante retourne des informations sur les plans forcés :  
  
```sql  
USE [QueryStoreDB];  
GO  
  
SELECT p.plan_id, p.query_id, q.object_id as containing_object_id,  
    force_failure_count, last_force_failure_reason_desc  
FROM sys.query_store_plan AS p  
JOIN sys.query_store_query AS q on p.query_id = q.query_id  
WHERE is_forced_plan = 1;  
```  
  
 Pour obtenir une liste complète des raisons, consultez [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). Vous pouvez aussi utiliser le XEvent **query_store_plan_forcing_failed** pour suivre et résoudre les échecs de forçage de plan.  
  
##  <a name="Renaming"></a> Éviter de renommer les bases de données en présence de requêtes associées à des plans forcés  

 Dans les plans d’exécution, les objets sont référencés avec des noms en trois parties (`database.schema.object`).   

Si vous renommez une base de données, le forçage de plan échoue, ce qui entraîne une recompilation dans toutes les exécutions de requête suivantes.  

##  <a name="Recovery"></a>Utiliser des indicateurs de trace sur les serveurs critiques pour améliorer la récupération d’urgence
 
  Vous pouvez utiliser les indicateurs de trace globaux 7745 et 7752 pour améliorer les performances du Magasin des requêtes dans les scénarios de haute disponibilité et de récupération d’urgence. Pour plus d'informations, consultez [Indicateurs de trace](../..//t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
  
  L’indicateur de trace 7745 empêche le comportement par défaut où le Magasin des requêtes écrit des données sur le disque avant que SQL Server ne puisse être arrêté.
  
  L’indicateur de trace 7752 permet le chargement asynchrone du Magasin des requêtes et permet également à SQL Server d’exécuter des requêtes avant le chargement complet du Magasin des requêtes. Le comportement du Magasin des requêtes par défaut empêche les requêtes de s’exécuter avant la récupération du Magasin des requêtes.

## <a name="see-also"></a> Voir aussi  
 [Vues de catalogue du magasin de requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [Utilisation du Magasin des requêtes avec l’OLTP en mémoire](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md)  
  
