---
title: Analyse des performances à l’aide du Magasin des requêtes | Microsoft Docs
ms.custom: ''
ms.date: 10/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Query Store
- Query Store, described
ms.assetid: e06344a4-22a5-4c67-b6c6-a7060deb5de6
caps.latest.revision: 38
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f30d87526729100a99336408778f2d2df4406475
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="monitoring-performance-by-using-the-query-store"></a>Surveillance des performances à l’aide du magasin de requêtes
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  La fonctionnalité de magasin de requêtes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous fournit des informations sur le choix de plan de requête et sur les performances. Elle simplifie la résolution des problèmes de performances en vous permettant de trouver rapidement les différences de performances provoquées par des changements de plan de requête. Le magasin de requête capture automatiquement l'historique des requêtes, des plans et des statistiques d'exécution et les conserve à des fins de révision. Elle sépare les données en périodes, ce qui vous permet de voir les modèles d'utilisation de base de données et de comprendre à quel moment les changements de plan de requête ont eu lieu sur le serveur. Vous pouvez configurer le magasin de requêtes à l’aide de l’option [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) . 
  
 Pour plus d’informations sur l’utilisation du Magasin des requêtes dans Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consultez [Utilisation du Magasin des requêtes dans Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-operate-query-store/).  
  
##  <a name="Enabling"></a> Activation du magasin de requêtes  
 Le magasin de requêtes n'est pas actif par défaut pour les nouvelles bases de données.  
  
#### <a name="use-the-query-store-page-in-includessmanstudiofullincludesssmanstudiofull-mdmd"></a>Utiliser la page Magasin des requêtes dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur une base de données, puis sur **Propriétés**.  
  
    > [!NOTE]  
    > Nécessite au moins la version 16 de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Dans la boîte de dialogue **Propriétés de la base de données** , sélectionnez la page **Magasin de requêtes** .  
  
3.  Dans la zone **Mode d’opération (demandé)** , sélectionnez **Activé**.  
  
#### <a name="use-transact-sql-statements"></a>Utilisation d’instructions Transact-SQL  
  
Utilisez l’instruction **ALTER DATABASE** pour activer le magasin de requêtes. Exemple :  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET QUERY_STORE = ON;  
```  
  
Pour obtenir d’autres options de syntaxe relatives au Magasin des requêtes, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
> [!NOTE]  
>  Vous ne pouvez pas activer le Magasin des requêtes pour la base de données **master** ou **tempdb**.  
 
## <a name="About"></a> Informations sur le Magasin des requêtes  
 Les plans d’exécution d’une requête spécifique dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] évoluent généralement au fil du temps pour un certain nombre de raisons, telles que les modifications des statistiques, les modifications de schémas, la création/suppression d’index, etc. Le cache de procédures (où sont stockés les plans de requête mis en cache) stocke uniquement le dernier plan d'exécution. Les plans sont également supprimés du cache du plan en raison de la sollicitation de la mémoire. Par conséquent, les régressions des performances de requête provoquées par des modifications du plan d'exécution peuvent être significatives et longues à résoudre.  
  
 Comme le magasin de requêtes conserve plusieurs plans d'exécution par requête, il peut appliquer des stratégies pour indiquer au processeur de requêtes d'utiliser un plan d'exécution spécifique pour une requête. On parle alors de forçage de plan. Le forçage de plan dans un magasin de requêtes est fourni à l'aide d'un mécanisme semblable à l’indicateur de requête [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md) , mais il ne nécessite pas d’apporter des modifications dans les applications utilisateur. Le forçage de plan peut résoudre une régression des performances de requête provoquée par une modification du plan dans un délai très court.  

 Les **statistiques d’attente** sont une autre source d’informations qui permet de résoudre les problèmes de performances dans SQL Server. Pendant longtemps, les statistiques d’attente étaient disponibles uniquement au niveau de l’instance, ce qui rendait difficile leur rétroaction sur la requête réelle. Dans SQL Server 2017 et Azure SQL Database, nous avons ajouté une autre dimension au Magasin des requêtes qui effectue le suivi des statistiques d’attente. 

 Voici des scénarios courants pour l'utilisation de la fonctionnalité de magasin de requêtes :  
  
-   Recherchez et corrigez rapidement une régression des performances du plan en forçant l’application du plan de requête précédent. Résolvez les requêtes qui ont récemment rencontré une régression des performances suite à la modification du plan d'exécution.  
-   Déterminez le nombre de fois où une requête a été exécutée dans une fenêtre de temps donnée, en aidant un administrateur de base de données à résoudre les problèmes liés aux ressources de performances.  
-   Identifiez les *n* requêtes les plus importantes (en termes de temps d’exécution, de consommation de mémoire, etc.) au cours des *x* dernières heures.  
-   Auditez l'historique des plans de requête pour une requête donnée.  
-   Analysez les ressources (UC, E/S et mémoire) des modèles d'utilisation pour une base de données spécifique.  
-   Identifiez les n premières requêtes qui attendent des ressources. 
-   Comprenez la nature de l’attente d’une requête ou d’un plan en particulier.
  
Le Magasin des requêtes contient trois magasins :
- Un **magasin de plans** pour rendre persistantes les informations du plan d’exécution.     
- Un **magasin de statistiques de runtime** pour rendre persistantes les informations des statistiques d’exécution.    
- Un **magasin de statistiques d’attente** pour rendre persistantes les informations des statistiques d’attente.     
 
Le nombre de plans uniques pouvant être stockés pour une requête dans le magasin de plans est limité par l’option de configuration **max_plans_per_query** . Pour améliorer les performances, les informations sont écrites dans les magasins de façon asynchrone. Pour optimiser l'espace, les statistiques d'exécution du runtime du magasin de statistiques du runtime sont agrégées sur une période fixe. Les informations contenues dans ces magasins sont visibles en interrogeant les affichages de catalogue du magasin de requêtes.  
  
La requête suivante retourne des informations sur les requêtes et plans du magasin de requêtes.  
  
```sql  
SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*  
FROM sys.query_store_plan AS Pl  
INNER JOIN sys.query_store_query AS Qry  
    ON Pl.query_id = Qry.query_id  
INNER JOIN sys.query_store_query_text AS Txt  
    ON Qry.query_text_id = Txt.query_text_id ;  
```  
 
##  <a name="Regressed"></a> Utiliser la fonctionnalité des requêtes régressées  
 Après avoir activé le Magasin des requêtes, actualisez la partie de la base de données du volet de l'Explorateur d'objets pour ajouter la section **Magasin des requêtes** .  
  
 ![Arborescence du Magasin des requêtes de l’Explorateur d’objets](../../relational-databases/performance/media/objectexplorerquerystore.PNG "Arborescence du Magasin des requêtes de l’Explorateur d’objets")  
  
 Sélectionnez **Requêtes régressées** pour ouvrir le volet **Requêtes régressées** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Le volet Requêtes régressées affiche les requêtes et les plans du magasin de requêtes. Utilisez les listes déroulantes situées en haut du volet vous permettent de sélectionner les requêtes selon différents critères. Sélectionnez un plan pour afficher le plan de requête sous forme graphique. Des boutons permettent d'afficher la requête source, de forcer un plan de requête et d’annuler son application forcée, ainsi que d'actualiser l'affichage.  
  
 ![Requêtes de régression de l’Explorateur d’objets](../../relational-databases/performance/media/objectexplorerregressedqueries.PNG "Requêtes de régression de l’Explorateur d’objets")  
  
 Pour forcer un plan, sélectionnez une requête et un plan, puis cliquez sur **Forcer le plan.** Vous pouvez uniquement forcer des plans qui ont été enregistrés par la fonctionnalité de plan de requête et sont toujours conservés dans le cache du plan de requête.  
##  <a name="Waiting"></a> Recherche de requêtes d’attente

À partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CTP 2.0 et [!INCLUDE[ssSDS](../../includes/sssds-md.md)], des statistiques d’attente par requête sur la durée sont disponibles dans le Magasin des requêtes. Dans le Magasin des requêtes, les types d’attente sont combinés en **catégories d’attente**. Vous trouverez dans [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md#wait-categories-mapping-table) une correspondance entre les catégories d’attente et les types d’attente.

Les **catégories d’attente** combinent différents types d’attente dans des compartiments similaires par nature. Différentes catégories d’attente nécessitent une analyse de suivi différente pour résoudre le problème, mais les types d’attente d’une même catégorie entraînent des expériences de résolution de problèmes très similaires à condition que la requête affectée au dessus des attentes soit l’élément manquant de la plupart de ces expériences.

Voici quelques exemples vous permettant d’obtenir plus d’insights sur votre charge de travail avant et après l’introduction des catégories d’attente dans le Magasin des requêtes :

|||| 
|-|-|-|  
|Expérience précédente|Nouvelle expérience|Action|
|Attentes élevées de RESOURCE_SEMAPHORE par base de données|Attentes élevées de mémoire dans le Magasin des requêtes pour des requêtes spécifiques|Recherchez les requêtes les plus consommatrices de mémoire dans le Magasin des requêtes. Ces requêtes retardent probablement davantage la progression des requêtes concernées. Utilisez l’indicateur de requête MAX_GRANT_PERCENT pour ces requêtes ou pour les requêtes concernées.|
|Attentes élevées de LCK_M_X par base de données|Attentes élevées de verrouillage dans le Magasin des requêtes pour des requêtes spécifiques|Vérifiez les textes de requête des requêtes affectées et identifiez les entités cibles. Recherchez dans le Magasin des requêtes d’autres requêtes modifiant la même entité, qui sont fréquemment exécutées et/ou ont une durée importante. Après avoir identifié ces requêtes, modifiez la logique d’application pour améliorer l’accès concurrentiel ou utilisez un niveau d’isolation moins restrictif.|
|Attentes élevées de PAGEIOLATCH_SH par base de données|Attentes élevées d’E/S de mémoire tampon dans le Magasin des requêtes pour des requêtes spécifiques|Recherchez les requêtes avec un grand nombre de lectures physiques dans le Magasin des requêtes. Si elles correspondent aux requêtes avec des attentes élevées d’E/S, introduisez un index sur l’entité sous-jacente pour faire des recherches au lieu d’analyses et ainsi réduire la surcharge d’E/S des requêtes.|
|Attentes élevées de SOS_SCHEDULER_YIELD par base de données|Attentes élevées du processeur dans le Magasin des requêtes pour des requêtes spécifiques|Recherchez les requêtes les plus consommatrices de processeur dans le Magasin des requêtes. Parmi elles, identifiez celles pour lesquelles la tendance de processeur élevé correspond aux attentes élevées de processeur pour les requêtes concernées. Concentrez-vous sur l’optimisation de ces requêtes : il peut y avoir une régression de plan ou peut-être un index manquant.|

##  <a name="Options"></a> Options de configuration 

Les options suivantes sont disponibles pour configurer les paramètres du magasin de requête.

*OPERATION_MODE*  
Peut être défini avec la valeur **READ_WRITE** (par défaut) ou READ_ONLY.  
  
*CLEANUP_POLICY (STALE_QUERY_THRESHOLD_DAYS)*  
Configurez l’argument `STALE_QUERY_THRESHOLD_DAYS` pour spécifier le nombre de jours de conservation des données dans le magasin de requêtes. La valeur par défaut est 30. Pour l’édition [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] De base, la valeur par défaut est **7** jours.
  
*DATA_FLUSH_INTERVAL_SECONDS*  
Détermine la fréquence à laquelle les données écrites sur le magasin de requête magasin est conservée sur le disque. Pour optimiser les performances, les données collectées par le magasin de requête sont écrites de façon asynchrone sur le disque. La fréquence à laquelle ce transfert asynchrone se produit est configurée par le biais de `DATA_FLUSH_INTERVAL_SECONDS`. La valeur par défaut est **900** (15 minutes).  
  
*MAX_STORAGE_SIZE_MB*  
Configure la taille maximale du magasin de requêtes. Si la taille des données du magasin de requêtes atteint la limite `MAX_STORAGE_SIZE_MB`, le magasin de requêtes fait passer automatiquement l’état de Lecture-écriture à Lecture seule et arrête la collecte de nouvelles données.  La valeur par défaut est 100 Mo. Pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition Premium, la valeur par défaut est de **1 Go** et pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] édition De base, elle est de **10 Mo**.
  
*INTERVAL_LENGTH_MINUTES*  
Détermine l'intervalle de temps à laquelle les données des statistiques d'exécution du runtime sont agrégées dans le magasin de requête. Pour optimiser l'espace, les statistiques d'exécution du runtime du magasin de statistiques du runtime sont agrégées sur une période fixe. Cette période fixe est configurée via `INTERVAL_LENGTH_MINUTES`. La valeur par défaut est **60**. 
  
*SIZE_BASED_CLEANUP_MODE*  
Contrôle si le processus de nettoyage est automatiquement activé lorsque la quantité totale de données approche de la taille maximale. Peut être défini avec la valeur **AUTO** (par défaut) or OFF.  
  
*QUERY_CAPTURE_MODE*  
Indique si le magasin de requête capture toutes les requêtes ou des requêtes appropriées en fonction du nombre d’exécutions et de la consommation de ressources ou arrête l’ajout de nouvelles requêtes et effectue uniquement un suivi des requêtes en cours. Peut être défini avec la valeur **ALL** (capturer toutes les requêtes), AUTO (ignorer les requêtes peu fréquentes et celles dont la durée de compilation et d’exécution n’est pas significative) ou NONE (arrêter la capture des nouvelles requêtes). La valeur par défaut est ALL sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), et AUTO sur Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
  
*MAX_PLANS_PER_QUERY*  
Entier représentant le nombre maximal de plans gérés pour chaque requête. La valeur par défaut est **200**.  
 
*WAIT_STATS_CAPTURE_MODE*  
Contrôle si le Magasin des requêtes capture les informations statistiques d’attente. Peut avoir la valeur OFF ou **ON** (par défaut).  
 
Interrogez l’affichage **sys.database_query_store_options** pour déterminer les options actuelles du magasin de requêtes. Pour plus d’informations sur les valeurs, consultez [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md).  
  
Pour plus d'informations sur la définition des options à l'aide d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] , consultez [Gestion des options](#OptionMgmt).  
  
## <a name="Related"></a> Vues, fonctions et procédures associées  
 Affichez et gérez le Magasin des requêtes par le biais de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou à l’aide des vues et les procédures suivantes.  

### <a name="query-store-functions"></a>Fonctions du Magasin des requêtes  
 Les fonctions facilitent les opérations avec le Magasin des requêtes. 
 
||| 
|-|-|  
|[sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)|| 
  
### <a name="query-store-catalog-views"></a>Affichages catalogue de magasin de requête  
 Les affichages catalogue présentent des informations sur le magasin de requêtes.  

||| 
|-|-|  
|[sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)|[sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)|  
|[sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)|[sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)|  
|[sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)|[sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)|  
|[sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)|[sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)|  
  
### <a name="query-store-stored-procedures"></a>Procédures stockées du magasin de requêtes  
 Les procédures stockées configurent le magasin de requêtes.  

||| 
|-|-|  
|[sp_query_store_flush_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)|[sp_query_store_reset_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)|  
|[sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)|[sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)|  
|[sp_query_store_remove_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)|[sp_query_store_remove_query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)|  
 
##  <a name="Scenarios"></a> Principaux scénarios d’utilisation  
  
###  <a name="OptionMgmt"></a> Gestion des options  
 Cette section fournit des instructions sur la gestion de la fonctionnalité de magasin de requête proprement dite.  
  
 **Le magasin de requêtes est-il actuellement actif ?**  
  
 Le magasin de requêtes stocke ses données dans la base de données utilisateur. C’est pourquoi sa taille est limitée (`MAX_STORAGE_SIZE_MB`). Si les données du magasin de requêtes atteignent cette limite, le magasin de requêtes fait passer automatiquement l'état de Lecture-écriture à Lecture seule et arrête la collecte de nouvelles données.  
  
 Interrogez [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) pour déterminer si le magasin de requêtes est actif et s’il collecte des statistiques d’exécution.  
  
```sql  
SELECT actual_state, actual_state_desc, readonly_reason,   
    current_storage_size_mb, max_storage_size_mb  
FROM sys.database_query_store_options;  
```  
  
 L’état du magasin de requêtes est déterminé par la colonne actual_state. S’il diffère de l’état souhaité, la colonne `readonly_reason` peut vous donner plus d’informations.   
Lorsque la taille du magasin de requête dépasse le quota, la fonctionnalité passe en mode readon_only.  
  
 **Accès aux options du magasin de requêtes**  
  
 Pour trouver des informations détaillées sur l'état du magasin de requêtes, exécutez ce qui suit dans une base de données utilisateur.  
  
```sql  
SELECT * FROM sys.database_query_store_options;  
```  
  
 **Définition de l’intervalle du magasin de requêtes**  
  
 Vous pouvez remplacer l'intervalle d'agrégation des statistiques d'exécution de requête (la valeur par défaut est 60 minutes).  
  
```sql  
ALTER DATABASE <database_name>   
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 15);  
```  
  
 > [!NOTE]
 > Les valeurs arbitraires ne sont pas autorisées pour `INTERVAL_LENGTH_MINUTES`. Utilisez l’une des valeurs suivantes : 1, 5, 10, 15, 30, 60 ou 1440 minutes.  
  
 La nouvelle valeur de l’intervalle est exposée par le biais de l’affichage **sys.database_query_store_options** .  
  
 **Utilisation de l’espace du magasin de requêtes**  
  
 Pour vérifier la taille et la limite actuelles du magasin de requête, exécutez l’instruction suivante dans la base de données utilisateur.  
  
```sql  
SELECT current_storage_size_mb, max_storage_size_mb   
FROM sys.database_query_store_options;  
```  
  
 Si le stockage du magasin de requêtes est saturé, utilisez l'instruction suivante pour l’étendre.  
  
```sql  
ALTER DATABASE <database_name>   
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <new_size>);  
```  
  
 **Définition de toutes les options du magasin de requêtes**  
  
 Vous pouvez définir simultanément plusieurs options de magasin de requêtes avec l'instruction ALTER DATABASE.  
  
```sql  
ALTER DATABASE <database name>   
SET QUERY_STORE (  
    OPERATION_MODE = READ_WRITE,  
    CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30),  
    DATA_FLUSH_INTERVAL_SECONDS = 3000,  
    MAX_STORAGE_SIZE_MB = 500,  
    INTERVAL_LENGTH_MINUTES = 15,  
    SIZE_BASED_CLEANUP_MODE = AUTO,  
    QUERY_CAPTURE_MODE = AUTO,  
    MAX_PLANS_PER_QUERY = 1000,
    WAIT_STATS_CAPTURE_MODE = ON 
);  
```  
  
 **Nettoyage de l’espace**  
  
 Les tables internes du magasin de requêtes sont créées dans le groupe de fichiers PRIMARY lors de la création de la base de données. Cette configuration ne peut pas être modifiée ultérieurement. Si vous manquez d'espace, vous pouvez effacer les anciennes données du magasin de requêtes à l'aide de l'instruction suivante.  
  
```sql  
ALTER DATABASE <db_name> SET QUERY_STORE CLEAR;  
```  
  
 Vous pouvez également effacer uniquement les données de requête ad hoc, car elles sont moins pertinentes pour les optimisations de requête et l'analyse du plan, mais utilisent autant d'espace.  
  
 **Supprimer des requêtes ad-hoc** Cette opération supprime les requêtes qui ont été exécutées une seule fois et qui datent de plus de 24 heures.  
  
```sql  
DECLARE @id int  
DECLARE adhoc_queries_cursor CURSOR   
FOR   
SELECT q.query_id  
FROM sys.query_store_query_text AS qt  
JOIN sys.query_store_query AS q   
    ON q.query_text_id = qt.query_text_id  
JOIN sys.query_store_plan AS p   
    ON p.query_id = q.query_id  
JOIN sys.query_store_runtime_stats AS rs   
    ON rs.plan_id = p.plan_id  
GROUP BY q.query_id  
HAVING SUM(rs.count_executions) < 2   
AND MAX(rs.last_execution_time) < DATEADD (hour, -24, GETUTCDATE())  
ORDER BY q.query_id ;  
  
OPEN adhoc_queries_cursor ;  
FETCH NEXT FROM adhoc_queries_cursor INTO @id;  
WHILE @@fetch_status = 0  
    BEGIN   
        PRINT @id  
        EXEC sp_query_store_remove_query @id  
        FETCH NEXT FROM adhoc_queries_cursor INTO @id  
    END   
CLOSE adhoc_queries_cursor ;  
DEALLOCATE adhoc_queries_cursor;  
```  
  
 Vous pouvez définir votre propre procédure avec une logique différente pour effacer les données dont vous n’avez plus besoin.  
  
 L’exemple ci-dessus utilise la procédure stockée étendue **sp_query_store_remove_query** pour supprimer les données inutiles. Vous pouvez également utiliser :  
  
-   **sp_query_store_reset_exec_stats** pour effacer les statistiques d’exécution pour un plan donné.  
-   **sp_query_store_remove_plan** pour supprimer un plan unique.  
 
  
###  <a name="Peformance"></a> Audit et résolution des problèmes de performances  
 Le magasin de requêtes conserve un historique des métriques de compilation et de runtime pour des exécutions de requêtes, ce qui vous permet de poser des questions sur votre charge de travail.  
  
 **Les *n* dernières requêtes exécutées sur la base de données ?**  
  
```sql  
SELECT TOP 10 qt.query_sql_text, q.query_id,   
    qt.query_text_id, p.plan_id, rs.last_execution_time  
FROM sys.query_store_query_text AS qt   
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_text_id   
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.query_id   
JOIN sys.query_store_runtime_stats AS rs   
    ON p.plan_id = rs.plan_id  
ORDER BY rs.last_execution_time DESC;  
```  
  
 **Nombre d’exécutions de chaque requête ?**  
  
```sql  
SELECT q.query_id, qt.query_text_id, qt.query_sql_text,   
    SUM(rs.count_executions) AS total_execution_count  
FROM sys.query_store_query_text AS qt   
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_text_id   
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.query_id   
JOIN sys.query_store_runtime_stats AS rs   
    ON p.plan_id = rs.plan_id  
GROUP BY q.query_id, qt.query_text_id, qt.query_sql_text  
ORDER BY total_execution_count DESC;  
```  
  
 **Nombre de requêtes avec la durée moyenne d’exécution la plus longue au cours de la dernière heure ?**  
  
```sql  
SELECT TOP 10 rs.avg_duration, qt.query_sql_text, q.query_id,  
    qt.query_text_id, p.plan_id, GETUTCDATE() AS CurrentUTCTime,   
    rs.last_execution_time   
FROM sys.query_store_query_text AS qt   
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_text_id   
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.query_id   
JOIN sys.query_store_runtime_stats AS rs   
    ON p.plan_id = rs.plan_id  
WHERE rs.last_execution_time > DATEADD(hour, -1, GETUTCDATE())  
ORDER BY rs.avg_duration DESC;  
```  
  
 **Nombre de requêtes ayant la plus grande moyenne de lectures d’E/S physiques au cours des dernières 24 heures, avec le nombre moyen de lignes et d’exécutions correspondant ?**  
  
```sql  
SELECT TOP 10 rs.avg_physical_io_reads, qt.query_sql_text,   
    q.query_id, qt.query_text_id, p.plan_id, rs.runtime_stats_id,   
    rsi.start_time, rsi.end_time, rs.avg_rowcount, rs.count_executions  
FROM sys.query_store_query_text AS qt   
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_text_id   
JOIN sys.query_store_plan AS p   
    ON q.query_id = p.query_id   
JOIN sys.query_store_runtime_stats AS rs   
    ON p.plan_id = rs.plan_id   
JOIN sys.query_store_runtime_stats_interval AS rsi   
    ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id  
WHERE rsi.start_time >= DATEADD(hour, -24, GETUTCDATE())   
ORDER BY rs.avg_physical_io_reads DESC;  
```  
  
 **Requêtes avec plusieurs plans ?** Ces requêtes sont particulièrement intéressantes, car elles sont adaptées aux régressions dues à un changement de plan. La requête suivante identifie ces requêtes, ainsi que tous les plans :  
  
```sql  
WITH Query_MultPlans  
AS  
(  
SELECT COUNT(*) AS cnt, q.query_id   
FROM sys.query_store_query_text AS qt  
JOIN sys.query_store_query AS q  
    ON qt.query_text_id = q.query_text_id  
JOIN sys.query_store_plan AS p  
    ON p.query_id = q.query_id  
GROUP BY q.query_id  
HAVING COUNT(distinct plan_id) > 1  
)  
  
SELECT q.query_id, object_name(object_id) AS ContainingObject,   
    query_sql_text, plan_id, p.query_plan AS plan_xml,  
    p.last_compile_start_time, p.last_execution_time  
FROM Query_MultPlans AS qm  
JOIN sys.query_store_query AS q  
    ON qm.query_id = q.query_id  
JOIN sys.query_store_plan AS p  
    ON q.query_id = p.query_id  
JOIN sys.query_store_query_text qt   
    ON qt.query_text_id = q.query_text_id  
ORDER BY query_id, plan_id;  
```  
  
 **Requêtes ayant récemment régressé en termes de performances (en comparant avec d’autres points dans le temps) ?** L'exemple de requête suivant retourne toutes les requêtes dont le temps d'exécution a doublé au cours des dernières 48 heures suite à un changement de plan. La requête compare tous les intervalles de statistiques d'exécution côte à côte.  
  
```sql  
SELECT   
    qt.query_sql_text,   
    q.query_id,   
    qt.query_text_id,   
    rs1.runtime_stats_id AS runtime_stats_id_1,  
    rsi1.start_time AS interval_1,   
    p1.plan_id AS plan_1,   
    rs1.avg_duration AS avg_duration_1,   
    rs2.avg_duration AS avg_duration_2,  
    p2.plan_id AS plan_2,   
    rsi2.start_time AS interval_2,   
    rs2.runtime_stats_id AS runtime_stats_id_2  
FROM sys.query_store_query_text AS qt   
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_text_id   
JOIN sys.query_store_plan AS p1   
    ON q.query_id = p1.query_id   
JOIN sys.query_store_runtime_stats AS rs1   
    ON p1.plan_id = rs1.plan_id   
JOIN sys.query_store_runtime_stats_interval AS rsi1   
    ON rsi1.runtime_stats_interval_id = rs1.runtime_stats_interval_id   
JOIN sys.query_store_plan AS p2   
    ON q.query_id = p2.query_id   
JOIN sys.query_store_runtime_stats AS rs2   
    ON p2.plan_id = rs2.plan_id   
JOIN sys.query_store_runtime_stats_interval AS rsi2   
    ON rsi2.runtime_stats_interval_id = rs2.runtime_stats_interval_id  
WHERE rsi1.start_time > DATEADD(hour, -48, GETUTCDATE())   
    AND rsi2.start_time > rsi1.start_time   
    AND p1.plan_id <> p2.plan_id  
    AND rs2.avg_duration > 2*rs1.avg_duration  
ORDER BY q.query_id, rsi1.start_time, rsi2.start_time;  
```  
  
 Si vous souhaitez voir toutes les régressions de performances (pas uniquement celles liées au changement de plan), supprimez simplement la condition `AND p1.plan_id <> p2.plan_id` de la requête précédente.  

 **Quelles sont les requêtes qui attendent le plus ?**
Cette requête retourne les 10 premières requêtes qui attendent le plus. 
 
 ```sql 
  SELECT TOP 10
    qt.query_text_id,
    q.query_id,
    p.plan_id,
    sum(total_query_wait_time_ms) AS sum_total_wait_ms
FROM sys.query_store_wait_stats ws
JOIN sys.query_store_plan p ON ws.plan_id = p.plan_id
JOIN sys.query_store_query q ON p.query_id = q.query_id
JOIN sys.query_store_query_text qt ON q.query_text_id = qt.query_text_id
GROUP BY qt.query_text_id, q.query_id, p.plan_id
ORDER BY sum_total_wait_ms DESC
 ```
 
 **Requêtes ayant récemment régressé en termes de performances (en comparant les exécutions récentes à l’historique) ?** La requête suivante compare l'exécution des requête en fonction des périodes d'exécution. Dans cet exemple spécifique, la requête compare l’exécution pendant une période récente (1 heure) et l’exécution pendant une période historique (la veille), puis identifie celle qui a introduit `additional_duration_workload`. Cette mesure est calculée comme suit : différence entre la moyenne des exécutions récentes et la moyenne des exécutions historiques, multipliée par le nombre d'exécutions récentes. Elle représente en fait la durée supplémentaire introduite dans les exécutions récentes en comparaison avec les exécutions historiques :  
  
```sql  
--- "Recent" workload - last 1 hour  
DECLARE @recent_start_time datetimeoffset;  
DECLARE @recent_end_time datetimeoffset;  
SET @recent_start_time = DATEADD(hour, -1, SYSUTCDATETIME());  
SET @recent_end_time = SYSUTCDATETIME();  
  
--- "History" workload  
DECLARE @history_start_time datetimeoffset;  
DECLARE @history_end_time datetimeoffset;  
SET @history_start_time = DATEADD(hour, -24, SYSUTCDATETIME());  
SET @history_end_time = SYSUTCDATETIME();  
  
WITH  
hist AS  
(  
    SELECT   
        p.query_id query_id,   
        CONVERT(float, SUM(rs.avg_duration*rs.count_executions)) total_duration,   
        SUM(rs.count_executions) count_executions,  
        COUNT(distinct p.plan_id) num_plans   
     FROM sys.query_store_runtime_stats AS rs  
        JOIN sys.query_store_plan p ON p.plan_id = rs.plan_id  
    WHERE  (rs.first_execution_time >= @history_start_time   
               AND rs.last_execution_time < @history_end_time)  
        OR (rs.first_execution_time \<= @history_start_time   
               AND rs.last_execution_time > @history_start_time)  
        OR (rs.first_execution_time \<= @history_end_time   
               AND rs.last_execution_time > @history_end_time)  
    GROUP BY p.query_id  
),  
recent AS  
(  
    SELECT   
        p.query_id query_id,   
        CONVERT(float, SUM(rs.avg_duration*rs.count_executions)) total_duration,   
        SUM(rs.count_executions) count_executions,  
        COUNT(distinct p.plan_id) num_plans   
    FROM sys.query_store_runtime_stats AS rs  
        JOIN sys.query_store_plan p ON p.plan_id = rs.plan_id  
    WHERE  (rs.first_execution_time >= @recent_start_time   
               AND rs.last_execution_time < @recent_end_time)  
        OR (rs.first_execution_time \<= @recent_start_time   
               AND rs.last_execution_time > @recent_start_time)  
        OR (rs.first_execution_time \<= @recent_end_time   
               AND rs.last_execution_time > @recent_end_time)  
    GROUP BY p.query_id  
)  
SELECT   
    results.query_id query_id,  
    results.query_text query_text,  
    results.additional_duration_workload additional_duration_workload,  
    results.total_duration_recent total_duration_recent,  
    results.total_duration_hist total_duration_hist,  
    ISNULL(results.count_executions_recent, 0) count_executions_recent,  
    ISNULL(results.count_executions_hist, 0) count_executions_hist   
FROM  
(  
    SELECT  
        hist.query_id query_id,  
        qt.query_sql_text query_text,  
        ROUND(CONVERT(float, recent.total_duration/  
                   recent.count_executions-hist.total_duration/hist.count_executions)  
               *(recent.count_executions), 2) AS additional_duration_workload,  
        ROUND(recent.total_duration, 2) total_duration_recent,   
        ROUND(hist.total_duration, 2) total_duration_hist,  
        recent.count_executions count_executions_recent,  
        hist.count_executions count_executions_hist     
    FROM hist   
        JOIN recent   
            ON hist.query_id = recent.query_id   
        JOIN sys.query_store_query AS q   
            ON q.query_id = hist.query_id  
        JOIN sys.query_store_query_text AS qt   
            ON q.query_text_id = qt.query_text_id      
) AS results  
WHERE additional_duration_workload > 0  
ORDER BY additional_duration_workload DESC  
OPTION (MERGE JOIN);  
```  
 
###  <a name="Stability"></a> Maintien de la stabilité des performances des requêtes  
 Pour les requêtes exécutées plusieurs fois, vous pouvez remarquer que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise différents plans, ce qui entraîne une utilisation des ressources et une durée différentes. Le magasin de requêtes permet de détecter le moment où les performances des requêtes ont régressé et de déterminer le plan optimal dans un délai donné. Vous pouvez ensuite forcer l'application de ce plan optimal pour l'exécution de requêtes ultérieures.  
  
 Vous pouvez également identifier les performances de requêtes incohérentes avec des paramètres (définis manuellement ou automatiquement). Parmi les différents plans, vous pouvez identifier le plan qui est suffisamment rapide et optimal pour la totalité ou la plupart des valeurs de paramètre et forcer ce plan, en maintenant ainsi des performances prévisibles pour un ensemble plus large de scénarios utilisateur.  
  
 **Forcer un plan pour une requête (appliquer une stratégie de forçage).** Lorsqu'un plan est forcé pour une requête donnée, chaque fois qu'une requête est exécutée, elle l'est avec le plan forcé.  
  
```sql  
EXEC sp_query_store_force_plan @query_id = 48, @plan_id = 49;  
```  
  
 Quand vous utilisez **sp_query_store_force_plan** , vous pouvez uniquement forcer des plans qui ont été enregistrés par le magasin de requêtes en tant que plan pour cette requête. En d'autres termes, les plans disponibles pour une requête sont uniquement ceux qui ont déjà été utilisés pour exécuter cette requête lorsque le magasin de requêtes était actif.  
  
 **Supprimer l’application forcée du plan pour une requête.** Pour vous appuyer à nouveau sur l’optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour calculer le plan de requête optimal, utilisez **sp_query_store_unforce_plan** pour annuler l’application forcée du plan qui était sélectionné pour la requête.  
  
```sql  
EXEC sp_query_store_unforce_plan @query_id = 48, @plan_id = 49;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Bonnes pratiques relatives au magasin de requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md)   
 [Utilisation du Magasin des requêtes avec l’OLTP en mémoire](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)   
 [Scénarios d’utilisation du Magasin des requêtes](../../relational-databases/performance/query-store-usage-scenarios.md)   
 [Comment le Magasin des requêtes collecte les données](../../relational-databases/performance/how-query-store-collects-data.md)   
 [Procédures stockées du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [Affichages catalogue du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [Outils de surveillance et d’optimisation des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)   
 [Ouvrir le Moniteur d’activité &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)   
 [Statistiques des requêtes dynamiques](../../relational-databases/performance/live-query-statistics.md)   
 [Moniteur d’activité](../../relational-databases/performance-monitor/activity-monitor.md)   
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)  
 [Utilisation du Magasin des requêtes dans Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-operate-query-store/) 
  
