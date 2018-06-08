---
title: Options SET d’ALTER DATABASE (Transact-SQL) | Microsoft Docs
description: Découvrez comment définir des options de base de données telles que l’optimisation automatique, le chiffrement et le magasin des requêtes dans SQL Server et Azure SQL Database.
ms.custom: ''
ms.date: 12/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- online database state [SQL Server]
- database options [SQL Server]
- emergency database state [SQL Server]
- databases [SQL Server], options
- read-only databases
- recovery models [SQL Server], switching
- ALTER DATABASE statement, SET options
- offline database state [SQL Server]
- snapshot isolation framework option
- checksums [SQL Server]
- automatic tuning
- SQL plan regression correction
- auto_create_statistics
- auto_update_statistics
ms.assetid: f76fbd84-df59-4404-806b-8ecb4497c9cc
caps.latest.revision: 159
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ea637d3853d3e63cbab6806022000c04d95e3e92
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-database-set-options-transact-sql"></a>Options SET d'ALTER DATABASE (Transact-SQL) 
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Cette rubrique contient la syntaxe ALTER DATABASE en rapport avec la définition d'options de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir des informations sur une autre syntaxe ALTER DATABASE, consultez les rubriques suivantes.  
  
-   [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  

-   [ALTER DATABASE &#40;Azure SQL Database&#41;](alter-database-azure-sql-database.md) 

-   [ALTER DATABASE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/alter-database-azure-sql-data-warehouse.md)  
  
-   [ALTER DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/alter-database-parallel-data-warehouse.md)  
  
La mise en miroir de bases de données, [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], et les niveaux de compatibilité sont des options `SET` mais sont décrits dans des rubriques distinctes en raison de leur longueur. Pour plus d’informations, consultez [Mise en miroir de bases de données ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md), [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md) et [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
> [!NOTE]  
> La plupart des options SET de base de données peuvent être configurées pour la session active à l’aide des [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md) et sont souvent configurées par des applications au moment de la connexion. Les options SET de niveau session remplacent les valeurs **ALTER DATABASE SET** . Les options de base de données décrites ci-après sont des valeurs qui peuvent être définies pour les sessions qui ne fournissent pas explicitement d'autres valeurs d'option SET.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ALTER DATABASE { database_name  | CURRENT }  
SET   
{  
    <optionspec> [ ,...n ] [ WITH <termination> ]   
}  
  
<optionspec> ::=   
{  
    <auto_option>   
  | <automatic_tuning_option>   
  | <change_tracking_option>   
  | <containment_option>   
  | <cursor_option>   
  | <database_mirroring_option>  
  | <date_correlation_optimization_option>  
  | <db_encryption_option>  
  | <db_state_option>  
  | <db_update_option>   
  | <db_user_access_option>   
  | <delayed_durability_option>  
  | <external_access_option>  
  | FILESTREAM ( <FILESTREAM_option> )  
  | <HADR_options>  
  | <mixed_page_allocation_option>  
  | <parameterization_option>  
  | <query_store_options>  
  | <recovery_option>  
  | <remote_data_archive_option>  
  | <service_broker_option>  
  | <snapshot_option>  
  | <sql_option>   
  | <target_recovery_time_option>   
  | <termination>  
}  
  
<auto_option> ::=   
{  
    AUTO_CLOSE { ON | OFF }   
  | AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }   
  | AUTO_SHRINK { ON | OFF }   
  | AUTO_UPDATE_STATISTICS { ON | OFF }   
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }  
}  
  
<automatic_tuning_option> ::=  
{  
  AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}  

<change_tracking_option> ::=  
{  
  CHANGE_TRACKING   
   {   
       = OFF  
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]   
     | ( <change_tracking_option_list> [,...n] )  
   }  
}  
  
   <change_tracking_option_list> ::=  
   {  
       AUTO_CLEANUP = { ON | OFF }   
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }  
   }  
  
<containment_option> ::=   
   CONTAINMENT = { NONE | PARTIAL }  
  
<cursor_option> ::=   
{  
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }   
  | CURSOR_DEFAULT { LOCAL | GLOBAL }   
}  
  
<database_mirroring_option>  
  ALTER DATABASE Database Mirroring  
  
<date_correlation_optimization_option> ::=  
    DATE_CORRELATION_OPTIMIZATION { ON | OFF }  
  
<db_encryption_option> ::=  
    ENCRYPTION { ON | OFF }  
  
<db_state_option> ::=  
    { ONLINE | OFFLINE | EMERGENCY }  
  
<db_update_option> ::=  
    { READ_ONLY | READ_WRITE }  
  
<db_user_access_option> ::=  
    { SINGLE_USER | RESTRICTED_USER | MULTI_USER }  
  
<delayed_durability_option> ::=  
    DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }  
  
<external_access_option> ::=  
{  
    DB_CHAINING { ON | OFF }  
  | TRUSTWORTHY { ON | OFF }  
  | DEFAULT_FULLTEXT_LANGUAGE = { <lcid> | <language name> | <language alias> }  
  | DEFAULT_LANGUAGE = { <lcid> | <language name> | <language alias> }  
  | NESTED_TRIGGERS = { OFF | ON }  
  | TRANSFORM_NOISE_WORDS = { OFF | ON }  
  | TWO_DIGIT_YEAR_CUTOFF = { 1753, ..., 2049, ..., 9999 }  
}  
<FILESTREAM_option> ::=  
{  
    NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL   
  | DIRECTORY_NAME = <directory_name>  
}  
<HADR_options> ::=  
    ALTER DATABASE SET HADR  
  
<mixed_page_allocation_option> ::=  
    MIXED_PAGE_ALLOCATION { OFF | ON }  
  
<parameterization_option> ::=  
    PARAMETERIZATION { SIMPLE | FORCED }  
  
<query_store_options> ::=  
{  
    QUERY_STORE   
    {  
          = OFF   
        | = ON [ ( <query_store_option_list> [,...n] ) ]  
        | ( < query_store_option_list> [,...n] )  
        | CLEAR [ ALL ]  
    }  
}   
  
<query_store_option_list> ::=  
{  
      OPERATION_MODE = { READ_WRITE | READ_ONLY }   
    | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )  
    | DATA_FLUSH_INTERVAL_SECONDS = number   
    | MAX_STORAGE_SIZE_MB = number   
    | INTERVAL_LENGTH_MINUTES = number   
    | SIZE_BASED_CLEANUP_MODE = [ AUTO | OFF ]  
    | QUERY_CAPTURE_MODE = [ ALL | AUTO | NONE ]  
    | MAX_PLANS_PER_QUERY = number
    | WAIT_STATS_CAPTURE_MODE = [ ON | OFF ]
}  
  
<recovery_option> ::=   
{  
    RECOVERY { FULL | BULK_LOGGED | SIMPLE }   
  | TORN_PAGE_DETECTION { ON | OFF }  
  | PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }  
}  
  
<remote_data_archive_option> ::=  
{  
    REMOTE_DATA_ARCHIVE =  
    {  
        ON ( SERVER = <server_name> ,   
                  {   CREDENTIAL = <db_scoped_credential_name>  
                     | FEDERATED_SERVICE_ACCOUNT =  ON | OFF   
                  }  
               )  
      | OFF  
    }  
}  
  
<service_broker_option> ::=  
{  
    ENABLE_BROKER  
  | DISABLE_BROKER  
  | NEW_BROKER  
  | ERROR_BROKER_CONVERSATIONS  
  | HONOR_BROKER_PRIORITY { ON | OFF}  
}  
  
<snapshot_option> ::=  
{  
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }  
  | READ_COMMITTED_SNAPSHOT {ON | OFF }  
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = {ON | OFF }  
}  
<sql_option> ::=   
{  
    ANSI_NULL_DEFAULT { ON | OFF }   
  | ANSI_NULLS { ON | OFF }   
  | ANSI_PADDING { ON | OFF }   
  | ANSI_WARNINGS { ON | OFF }   
  | ARITHABORT { ON | OFF }   
  | COMPATIBILITY_LEVEL = { 90 | 100 | 110 | 120 | 130 | 140 }  
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }   
  | NUMERIC_ROUNDABORT { ON | OFF }   
  | QUOTED_IDENTIFIER { ON | OFF }   
  | RECURSIVE_TRIGGERS { ON | OFF }   
}  
  
<target_recovery_time_option> ::=  
    TARGET_RECOVERY_TIME = target_recovery_time { SECONDS | MINUTES }  
  
<termination>  ::=   
{  
    ROLLBACK AFTER integer [ SECONDS ]   
  | ROLLBACK IMMEDIATE   
  | NO_WAIT  
}  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données à modifier.  
  
 CURRENT  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 `CURRENT` effectue l’action dans la base de données active. `CURRENT` n’est pas pris en charge pour toutes les options dans tous les contextes. Si `CURRENT` échoue, fournissez le nom de la base de données.  
  
 **\<auto_option> ::=**  
  
 Contrôle les options automatiques.  
 <a name="auto_close"></a> AUTO_CLOSE { ON | OFF }  
 ON  
 La base de données est arrêtée correctement et ses ressources sont libérées dès que le dernier utilisateur l'a quittée.  
  
 La base de données est rouverte automatiquement lorsqu'un utilisateur tente de la réutiliser. Par exemple, en exécutant une instruction `USE database_name`. Si la base de données est fermée correctement et que AUTO_CLOSE a la valeur ON, elle ne se rouvre qu'au moment où un utilisateur tente de l'utiliser au redémarrage suivant du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 OFF  
 La base de données reste ouverte après que le dernier utilisateur l'a quittée.  
  
 L'option AUTO_CLOSE est utile pour les bases de données bureautiques, puisqu'elle permet aux fichiers de base de données d'être gérés comme des fichiers normaux. Ceux-ci peuvent être déplacés, copiés en vue d'une sauvegarde ou même transmis par messagerie électronique à d'autres utilisateurs. Le processus AUTO_CLOSE est asynchrone ; l'ouverture et la fermeture répétées de la base de données n'ont aucune incidence sur les performances.  
  
> [!NOTE]  
>  L’option AUTO_CLOSE n’est pas disponible dans une base de données à relation contenant-contenu, ni sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_auto_close_on de l'affichage catalogue sys.databases ou la propriété IsAutoClose de la fonction DATABASEPROPERTYEX.  
  
> [!NOTE]  
>  Quand AUTO_CLOSE a la valeur ON, certaines colonnes de l’affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) et la fonction DATABASEPROPERTYEX retournent la valeur NULL, car la base de données est inaccessible et qu’aucune donnée ne peut être extraite. Pour résoudre ce problème, exécutez une instruction USE pour ouvrir la base de données.  
  
> [!NOTE]  
>  La mise en miroir de bases de données exige AUTO_CLOSE OFF.  
  
 Si la base de données a la valeur AUTOCLOSE = ON, une opération qui initialise un arrêt de la base de données automatique efface le cache du plan pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 et ultérieur, pour chaque mémoire cache effacée dans le cache de plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d'information suivant : « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a rencontré %d occurrence(s) de vidages de mémoire cache pour la mémoire cache '%s' (partie du cache du plan) en raison d'opérations de maintenance ou de reconfiguration de base de données ». Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.  
 
 <a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }  
 ON  
 L'optimiseur de requête crée des statistiques sur les colonnes uniques des prédicats de requête, en fonction des besoins, afin d'améliorer les plans de requête et les performances des requêtes. Ces statistiques de colonnes uniques sont créées lorsque l'optimiseur de requête compile les requêtes. Les statistiques de colonnes uniques sont créées uniquement sur les colonnes qui ne constituent pas déjà la première colonne d'un objet de statistiques existant.  
  
 La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.  
  
 OFF  
 L'optimiseur de requête ne crée pas de statistiques sur les colonnes uniques des prédicats de requête lorsqu'il est en train de compiler des requêtes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_auto_create_stats_on de l'affichage catalogue sys.databases ou la propriété IsAutoCreateStatistics de la fonction DATABASEPROPERTYEX.  
  
 Pour plus d’informations, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans [Statistiques](../../relational-databases/statistics/statistics.md).  
  
 INCREMENTAL = ON | OFF  
 Lorsque AUTO_CREATE_STATISTICS et INCREMENTAL ont la valeur ON, les statistiques sont créées automatiquement comme incrémentielles chaque fois que les statistiques incrémentielles sont prises en charge. La valeur par défaut est OFF. Pour plus d’informations, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 <a name="auto_shrink"></a> AUTO_SHRINK { ON | OFF }  
 ON  
 Les fichiers de base de données peuvent faire l'objet d'une réduction périodique.  
  
 Les fichiers de données et les fichiers journaux peuvent être automatiquement réduits. AUTO_SHRINK ne réduit la taille du journal des transactions que si le mode de récupération SIMPLE est défini pour la base de données ou si le journal est sauvegardé. Si la valeur OFF est définie, les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l'espace inutilisé.  
  
 L'option AUTO_SHRINK provoque un compactage dès qu'un fichier comprend plus de 25 % d'espace inutilisé. Le fichier est compacté à une taille laissant 25 % d'espace inutilisé ou à sa taille initiale au moment de sa création, selon la valeur la plus élevée.  
  
 Vous ne pouvez pas compacter une base de données en lecture seule.  
  
 OFF  
 Les fichiers de base de données ne sont pas réduits automatiquement lors des vérifications périodiques de l'espace inutilisé.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_auto_shrink_on de l'affichage catalogue sys.databases ou la propriété IsAutoShrink de la fonction DATABASEPROPERTYEX.  
  
> [!NOTE]  
> L'option AUTO_SHRINK n'est pas disponible dans une base de données à relation contenant-contenu.  
  
 <a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { ON | OFF }  
 ON  
 Spécifie que l'optimiseur de requête met à jour les statistiques lorsqu'elles sont utilisées par une requête et lorsqu'elles sont peut-être obsolètes. Les statistiques deviennent obsolètes après que des opérations d'insertion, de mise à jour, de suppression ou de fusion ont modifié la distribution des données dans la table ou la vue indexée. L'optimiseur de requête détermine si les statistiques sont obsolètes en comptant le nombre de modifications de données depuis la dernière mise à jour des statistiques et en comparant le nombre de modifications à un seuil. Ce seuil est basé sur le nombre de lignes contenues dans la table ou la vue indexée.  
  
 L'optimiseur de requête vérifie s'il existe des statistiques obsolètes avant de compiler une requête et avant d'exécuter un plan de requête mis en cache. Avant de compiler une requête, l'optimiseur de requête utilise les colonnes, les tables et les vues indexées du prédicat de requête pour identifier les statistiques susceptibles d'être obsolètes. Avant d'exécuter un plan de requête mis en cache, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie que le plan de requête fait référence à des statistiques à jour.  
  
 L'option AUTO_UPDATE_STATISTICS s'applique aux statistiques créées pour les index, aux colonnes uniques contenues dans les prédicats de requête et aux statistiques créées à l'aide de l'instruction CREATE STATISTICS. Cette option s'applique également aux statistiques filtrées.  
  
 La valeur par défaut est ON. Nous vous recommandons d'utiliser le paramètre par défaut pour la plupart des bases de données.  
  
 Utilisez l'option AUTO_UPDATE_STATISTICS_ASYNC pour spécifier si les statistiques doivent être mises à jour en mode synchrone ou asynchrone.  
  
 OFF  
 Spécifie que l'optimiseur de requête ne met pas à jour les statistiques lorsqu'elles sont utilisées par une requête et lorsqu'elles sont peut-être obsolètes. Si cette option a la valeur OFF, il peut en résulter des plans de requête non optimisés et une dégradation des performances des requêtes.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_auto_update_stats_on de l'affichage catalogue sys.databases ou la propriété IsAutoUpdateStatistics de la fonction DATABASEPROPERTYEX.  
  
 Pour plus d’informations, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans [Statistiques](../../relational-databases/statistics/statistics.md).  
  
 <a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }  
 ON  
 Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont asynchrones. L'optimiseur de requête n'attend pas la fin des mises à jour des statistiques pour compiler les requêtes.  
  
 Affecter la valeur ON à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.  
  
 Par défaut, l'option AUTO_UPDATE_STATISTICS_ASYNC a la valeur OFF ; l'optimiseur de requête met à jour les statistiques en mode synchrone.  
  
 OFF  
 Spécifie que les mises à jour des statistiques pour l'option AUTO_UPDATE_STATISTICS sont synchrones. L'optimiseur de requête attend la fin des mises à jour des statistiques pour compiler les requêtes.  
  
 Affecter la valeur OFF à cette option n'a aucun effet à moins que AUTO_UPDATE_STATISTICS n'ait également la valeur ON.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_auto_update_stats_async_on de l'affichage catalogue sys.databases.  
  
 Pour plus d’informations sur l’utilisation des mises à jour de statistiques synchrones ou asynchrones, consultez la section « Utilisation des options de statistiques à l’échelle de la base de données » dans [Statistiques](../../relational-databases/statistics/statistics.md).  
  
 <a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**  
 **S'applique à**: [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)].  

 Active ou désactive l’option d’`FORCE_LAST_GOOD_PLAN` [optimisation automatique](../../relational-databases/automatic-tuning/automatic-tuning.md).  
  
 FORCE_LAST_GOOD_PLAN = { ON | OFF }  
 ON  
 Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] force automatiquement le dernier plan correct connu sur les requêtes [!INCLUDE[tsql_md](../../includes/tsql_md.md)] où le nouveau plan SQL provoque des régressions des performances. Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] surveille en permanence les performances de la requête [!INCLUDE[tsql_md](../../includes/tsql_md.md)] avec le plan forcé. S’il existe des gains de performances, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] continue à utiliser le dernier plan correct connu. Si aucun gain de performances n’est détecté, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] génère un nouveau plan SQL. L’instruction échoue si le magasin de requêtes n’est pas activé ou s’il n’est pas en mode *lecture-écriture*.   

 OFF  
 Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] signale les régressions des performances de requêtes potentielles dues à des changements de plan SQL dans la vue [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md). Toutefois, ces recommandations ne sont pas appliquées automatiquement. L’utilisateur peut surveiller les recommandations actives et résoudre les problèmes identifiés en appliquant les scripts [!INCLUDE[tsql_md](../../includes/tsql_md.md)] qui sont affichés dans la vue. Il s'agit de la valeur par défaut.

 **\<change_tracking_option> ::=**  
  
 **S’applique à**  : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSFull](../../includes/sssds-md.md)].  
  
 Contrôle les options de suivi des modifications. Vous pouvez activer le suivi des modifications, définir des options, modifier des options et désactiver le suivi des modifications. Pour consulter des exemples, reportez-vous à la section Exemples plus loin dans cette rubrique.  
  
 ON  
 Active le suivi des modifications pour la base de données Lorsque vous activez le suivi des modifications, vous pouvez également définir les options AUTO CLEANUP et CHANGE RETENTION.  
  
 AUTO_CLEANUP = { ON | OFF }  
 ON  
 Les informations de suivi des modifications sont supprimées automatiquement à l'issue de la période de rétention spécifiée.  
  
 OFF  
 Les données de suivi des modifications ne sont pas supprimées de la base de données.  
  
 CHANGE_RETENTION =*retention_period* { DAYS | HOURS | MINUTES }  
 Spécifie la période minimale de conservation des informations de suivi des modifications dans la base de données. Les données sont supprimées uniquement lorsque AUTO_CLEANUP a la valeur ON.  
  
 *retention_period* est un entier qui spécifie la composante numérique de la période de rétention.  
  
 La période de rétention par défaut est 2 jours. La période de rétention minimale est 1 minute. Le type de rétention par défaut est DAYS.  
  
 OFF  
 Désactive le suivi des modifications pour la base de données. Vous devez désactiver le suivi des modifications sur toutes les tables avant de le désactiver sur la base de données.  
  
 **\<containment_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Contrôle des options de la relation contenant-contenu de la base de données.  
  
 CONTAINMENT = { NONE | PARTIAL}  
 Aucune  
 La base de données n'est pas une base de données à relation contenant-contenu.  
  
 PARTIAL  
 La base de données est une base de données à relation contenant-contenu. La définition de la relation contenant-contenu de base de données sur la valeur partielle échouera si l'option de réplication, de capture des données modifiées ou de suivi des modifications est activée. La vérification des erreurs prend fin après un échec. Pour plus d'informations sur les bases de données à relation contenant-contenu, consultez [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md).  
  
> [!NOTE]  
> La relation contenant-contenu ne peut pas être configurée dans [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]. La relation contenant-contenu n’est pas désignée explicitement, mais [!INCLUDE[ssSDS](../../includes/sssds-md.md)] peut utiliser des fonctionnalités contenues telles que les utilisateurs de base de données à relation contenant-contenu.  
  
 **\<cursor_option> ::=**  
  
 Contrôle les options de curseur.  
  
 CURSOR_CLOSE_ON_COMMIT { ON | OFF }  
 ON  
 Tout curseur ouvert au moment où une transaction est validée ou restaurée est fermé.  
  
 OFF  
 Les curseurs restent ouverts lorsqu'une transaction est validée. La restauration d'une transaction ferme tous les curseurs à l'exception de ceux définis avec la valeur INSENSITIVE ou STATIC.  
  
 Les paramètres de niveau connexion définis à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de CURSOR_CLOSE_ON_COMMIT. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui désactive l'option CURSOR_CLOSE_ON_COMMIT pour la session (valeur OFF), lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SET CURSOR_CLOSE_ON_COMMIT &#40;Transact-SQL&#41;](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_cursor_close_on_commit_on de l'affichage catalogue sys.databases ou la propriété IsCloseCursorsOnCommitEnabled de la fonction DATABASEPROPERTYEX.  
  
 CURSOR_DEFAULT { LOCAL | GLOBAL }  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Détermine si l'étendue du curseur utilise LOCAL ou GLOBAL.  
  
 LOCAL  
 Si LOCAL est spécifié et qu'aucun curseur n'est défini comme GLOBAL lors de sa création, le curseur a une étendue locale pour le traitement, la procédure stockée ou le déclencheur dans lequel il a été créé. Le nom du curseur n'est valide que dans cette étendue. Le curseur peut être référencé par des variables de curseur locales dans le traitement, la procédure stockée ou le déclencheur, ou bien par un paramètre OUTPUT d'une procédure stockée. Le curseur est libéré implicitement à la fin du lot, de la procédure stockée ou du déclencheur, à moins d'avoir été retourné dans un paramètre OUTPUT. S'il a été retourné dans un paramètre OUTPUT, le curseur est libéré lorsque la dernière variable qui y fait référence est libérée, ou est hors de portée.  
  
 GLOBAL  
 Si GLOBAL est spécifié et qu'aucun curseur n'est défini comme LOCAL lors de sa création, le curseur est d'étendue globale pour la connexion. Toute procédure stockée ou tout lot exécuté par la connexion peut faire référence au nom du curseur.  
  
 Le curseur n'est libéré implicitement qu'au moment de la déconnexion. Pour plus d’informations, consultez [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md).  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_local_cursor_default de l'affichage catalogue sys.databases ou la propriété IsLocalCursorsDefault de la fonction DATABASEPROPERTYEX.  
  
 **\<database_mirroring>**  
  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Pour obtenir une description des arguments, consultez [Mise en miroir de bases de données ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md).  
  
 **\<date_correlation_optimization_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Contrôle l'option date_correlation_optimization.  
  
 DATE_CORRELATION_OPTIMIZATION { ON | OFF }  
 ON  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve les statistiques de corrélation entre deux tables quelconques de la base de données qui sont liées par une contrainte FOREIGN KEY et qui ont des colonnes **datetime**.  
  
 OFF  
 Les statistiques de corrélation ne sont pas conservées.  
  
 Pour affecter la valeur ON à DATE_CORRELATION_OPTIMIZATION, il ne doit exister aucune connexion active à la base de données, à l'exception de celle qui exécute l'instruction ALTER DATABASE. Ensuite, différentes connexions peuvent être prises en charge.  
  
 Vous pouvez déterminer la valeur actuelle de cette option en consultant la colonne is_date_correlation_on de l'affichage catalogue sys.databases.  
  
 **\<db_encryption_option> ::=**  
  
 Contrôle l'état de chiffrement de la base de données.  
  
 ENCRYPTION {ON | OFF}  
 Spécifie si la base de données doit être chiffrée (ON) ou non chiffrée (OFF). Pour plus d’informations sur le chiffrement de base de données, consultez [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md) et [Chiffrement transparent des données avec Azure SQL Database](../../relational-databases/security/encryption/transparent-data-encryption-azure-sql.md).  
  
 Lorsque le chiffrement est activé au niveau de la base de données, tous les groupes de fichiers seront chiffrés. Tous les nouveaux groupes de fichiers hériteront de la propriété chiffrée. Si des groupes de fichiers dans la base de données ont la valeur **READ ONLY**, l'opération de chiffrement de la base de données échouera.  
  
 Vous pouvez voir l’état de chiffrement de la base de données en utilisant la vue de gestion dynamique [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).  
  
 **\<db_state_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Contrôle l'état de la base de données.  
  
 OFFLINE  
 La base de données est fermée et arrêtée correctement, puis marquée comme étant déconnectée. Tant que la base de données est hors connexion, elle ne peut pas être modifiée.  
  
 ONLINE  
 La base de données est ouverte et peut être utilisée.  
  
 EMERGENCY  
 La base de données est marquée READ_ONLY, la journalisation est désactivée et l'accès est restreint aux membres du rôle serveur fixe sysadmin. EMERGENCY est principalement utilisé à des fins de dépannage. Par exemple, une base de données marquée comme suspecte en raison d'un fichier journal corrompu peut se voir affecté l'état EMERGENCY. L'administrateur système peut alors accéder en lecture seule à la base de données. Seuls les membres du rôle serveur fixe sysadmin peuvent définir l'état EMERGENCY pour une base de données.  
  
> [!NOTE]  
> **Autorisations :** L’autorisation ALTER DATABASE pour la base de données d’objet est nécessaire pour faire passer une base de données à l’état hors connexion ou urgence. L'autorisation ALTER ANY DATABASE au niveau serveur est requise pour faire passer en ligne une base de données hors connexion.  
  
 Vous pouvez déterminer l’état de cette option en consultant les colonnes state et state_desc de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété Status de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Pour plus d'informations, consultez [Database States](../../relational-databases/databases/database-states.md).  
  
 Une base de données marquée RESTORING ne peut pas se voir affecté la valeur OFFLINE, ONLINE ou EMERGENCY. Une base de données peut être à l'état RESTORING durant une opération de restauration active, ou lorsqu'une opération de restauration d'un fichier de base de données ou d'un fichier journal échoue car un fichier de sauvegarde est corrompu.  
  
 **\<db_update_option> ::=**  
  
 Contrôle si des mises à jour sont autorisées dans la base de données.  
  
 READ_ONLY  
 Les utilisateurs peuvent lire des données dans la base de données mais ils n'ont pas le droit de les modifier.  
  
> [!NOTE]  
>  Pour améliorer les performances des requêtes, mettez à jour les statistiques avant de définir une base de données à READ_ONLY. Si des statistiques supplémentaires sont nécessaires après qu'une base de données est définie à READ_ONLY, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des statistiques dans tempdb. Pour plus d’informations sur les statistiques pour une base de données en lecture seule, consultez [Statistiques](../../relational-databases/statistics/statistics.md).  
  
 READ_WRITE  
 La base de données est accessible aux opérations de lecture et d'écriture.  
  
 Pour modifier cet état, vous devez bénéficier d'un accès exclusif à la base de données. Pour plus d'informations, consultez la clause SINGLE_USER.  
  
> [!NOTE]  
> Dans les bases de données fédérées [!INCLUDE[ssSDS](../../includes/sssds-md.md)], SET {READ_ONLY | READ_WRITE} est désactivé.  
  
 **\<db_user_access_option> ::=**  
  
 Contrôle l'accès utilisateur à la base de données.  
  
 SINGLE_USER  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Indique que l'accès à la base de données n'est autorisé qu'à un seul utilisateur à la fois. Si SINGLE_USER est spécifié et que d'autres utilisateurs sont connectés à la base de données, l'instruction ALTER DATABASE est bloquée jusqu'à ce que tous les autres utilisateurs se déconnectent de cette base de données. Pour remplacer ce comportement, examinez la clause WITH \<termination>.  
  
 La base de données demeure en mode SINGLE_USER même si l'utilisateur qui a défini l'option se déconnecte. À ce stade, un autre utilisateur (et un seul) peut se connecter à la base de données.  
  
 Avant d'affecter la valeur SINGLE_USER à la base de données, vérifiez que l'option AUTO_UPDATE_STATISTICS_ASYNC a la valeur OFF. Si la valeur est ON, le thread d'arrière-plan utilisé pour mettre à jour les statistiques se connecte à la base de données et vous ne pourrez pas accéder à celle-ci en mode mono-utilisateur. Pour afficher l’état de cette option, exécutez une requête sur la colonne is_auto_update_stats_async_on dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Si l'option a la valeur ON, effectuez les tâches suivantes :  
  
1.  Affectez la valeur OFF à AUTO_UPDATE_STATISTICS_ASYNC.  
  
2.  Recherchez les travaux des statistiques asynchrones actifs en interrogeant la vue de gestion dynamique [sys.dm_exec_background_job_queue](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md).  
  
 Si des travaux sont actifs, laissez ces travaux se terminer ou arrêtez-les manuellement à l’aide de [KILL STATS JOB](../../t-sql/language-elements/kill-stats-job-transact-sql.md).  
  
RESTRICTED_USER  
 RESTRICTED_USER permet uniquement aux membres du rôle de base de données fixe db_owner et aux rôles serveurs fixes dbcreator et sysadmin de se connecter à la base de données, mais il n'en limite pas le nombre. Toutes les connexions à la base de données sont déconnectées dans la plage de temps spécifiée par la clause d'arrêt de l'instruction ALTER DATABASE. Après que la base est passée à l'état RESTRICTED_USER, toute tentative de connexion par des utilisateurs non qualifiés est refusée.  
  
MULTI_USER  
 Tous les utilisateurs qui bénéficient des autorisations appropriées peuvent se connecter à la base de données.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne user_access de l'affichage catalogue sys.databases ou la propriété UserAccess de la fonction DATABASEPROPERTYEX.  
  
 **\<delayed_durability_option> ::=**  
  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Contrôle si les transactions sont validées de manière entièrement durable ou durable différée.  
  
 DISABLED  
 Toutes les transactions suivant SET DISABLED sont entièrement durables. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.  
  
 ALLOWED  
 Toutes les transactions suivant SET ALLOWED sont soit entièrement durables, soit durables différées, en fonction de l'option de durabilité définie dans l'instruction de validation ou de bloc atomique.  
  
 FORCED  
 Toutes les transactions suivant SET FORCED sont durables différées. Toutes les options de durabilité définies dans une instruction de validation ou de bloc atomique sont ignorées.  
  
 **\<external_access_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Contrôle si des ressources externes, par exemple des objets d'une autre base de données, peuvent accéder à la base de données.  
  
 DB_CHAINING { ON | OFF }  
 ON  
 La base de données peut être la source ou la cible d'une chaîne de propriétés des bases de données croisées.  
  
 OFF  
 La base de données ne peut prendre part à un chaînage des propriétés des bases de données croisées.  
  
> [!IMPORTANT]  
> L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconnaît ce paramètre lorsque l'option de serveur cross db ownership chaining a la valeur 0 (OFF). Lorsque cross db ownership chaining a la valeur 1 (ON), toutes les bases de données utilisateur peuvent participer aux chaînages des propriétés des bases de données croisées, quelle que soit la valeur de cette option. Cette option est configurée à l’aide de [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 Pour définir cette option, l’autorisation CONTROL SERVER est nécessaire sur la base de données.  
  
 L'option DB_CHAINING ne peut pas être définie sur les bases de données système suivantes : master, model et tempdb.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_db_chaining_on de l'affichage catalogue sys.databases.  
  
 TRUSTWORTHY { ON | OFF }  
 ON  
 Les modules de base de données (par exemple, les procédures stockées ou les fonctions définies par l'utilisateur) qui utilisent un contexte d'emprunt d'identité peuvent accéder à des ressources en dehors de la base de données.  
  
 OFF  
 Les modules de base de données qui utilisent l'emprunt d'identité ne peuvent pas accéder à des ressources externes à la base de données.  
  
 TRUSTWORTHY prend la valeur OFF chaque fois que la base de données est attachée.  
  
 Par défaut, pour toutes les bases de données système, sauf pour la base msdb, l'option TRUSTWORTHY est définie à OFF (désactivé). La valeur ne peut pas être modifiée pour les bases de données model et tempdb. Nous vous recommandons de ne jamais définir l'option TRUSTWORTHY à ON (activé) pour la base de données master.  
  
 Pour définir cette option, l’autorisation CONTROL SERVER est nécessaire sur la base de données.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_trustworthy_on de l'affichage catalogue sys.databases.  
  
 DEFAULT_FULLTEXT_LANGUAGE  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie la valeur de langue par défaut pour les colonnes indexées de texte intégral.  
  
> [!IMPORTANT]  
>  Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.  
  
 DEFAULT_LANGUAGE  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie la langue par défaut de toutes les nouvelles connexions. La langue peut être spécifiée en fournissant son ID local (lcid), son nom ou son alias. Pour obtenir une liste de noms et d’alias de langue acceptables, consultez [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.  
  
 NESTED_TRIGGERS  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie si un déclencheur AFTER peut s'exécuter en cascade et, par conséquent, réaliser une action qui initialise un autre déclencheur, lequel initialise un autre déclencheur, etc. Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.  
  
 TRANSFORM_NOISE_WORDS  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Utilisé pour supprimer un message d'erreur si des mots parasites ou des mots vides provoquent l'échec d'une opération booléenne sur une requête de texte intégral. Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.  
  
 TWO_DIGIT_YEAR_CUTOFF  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie un entier compris entre 1 753 et 9 999 qui représente l'année de coupure permettant d'interpréter les années à deux chiffres comme des années à quatre chiffres. Cette option est autorisée uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.  
  
 **\<FILESTREAM_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Contrôle les paramètres des FileTables.  
  
 NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }  
 OFF  
 L'accès non transactionnel aux données FileTable est désactivé.  
  
 READ_ONLY  
 Les données FILESTREAM dans les FileTables de cette base de données peuvent être lues par des processus non transactionnels.  
  
 FULL  
 L'accès non transactionnel complet aux données FILESTREAM dans les FileTables est activé.  
  
 DIRECTORY_NAME = *\<directory_name>*  
 Nom de répertoire compatible avec Windows. Ce nom doit être unique parmi tous les noms de répertoire au niveau de la base de données dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comparaison d'unicité n'est pas sensible à la casse, indépendamment des paramètres de classement. Cette option doit être définie avant de créer un FileTable dans cette base de données.  
  
 **\<HADR_options> ::=**  
  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Voir [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md).  
  
 **\<mixed_page_allocation_option> ::=**  
  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)). Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 MIXED_PAGE_ALLOCATION { OFF | ON } contrôle si la base de données peut créer des pages initiales à l’aide d’une extension mixte pour les huit premières pages d’une table ou d’un index.  
  
 OFF  
 La base de données crée toujours les pages initiales à l’aide d’extensions uniformes. Il s'agit de la valeur par défaut.  
  
 ON  
 La base de données peut créer des pages initiales à l’aide d’extensions mixtes.  
  
 Ce paramètre est ON pour toutes les bases de données système. **tempdb** est la seule base de données système qui prend en charge la valeur OFF.  
  
 **\<PARAMETERIZATION_option> ::=**  
  
 Contrôle l'option de paramétrage.  
  
 PARAMETERIZATION { SIMPLE | FORCED }  
 SIMPLE  
 Les requêtes sont paramétrables en fonction du comportement par défaut de la base de données.  
  
 FORCED  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] paramètre toutes les requêtes de la base de données.  
  
 Vous pouvez déterminer la valeur actuelle de cette option en consultant la colonne is_parameterization_forced de l'affichage catalogue sys.databases.  
  
 **\<query_store_options> ::=**  
  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 ON | OFF | CLEAR [ ALL ]  
 Contrôle si le magasin de requête est activé dans la base de données, ainsi que la suppression du contenu du magasin de requête.  
  
ON  
 Active le magasin de requêtes.  
  
OFF  
 Désactive le magasin de requêtes.  Il s'agit de la valeur par défaut.   
  
CLEAR  
 Supprime le contenu du magasin de requêtes.  
  
OPERATION_MODE  
 Décrit le mode de fonctionnement du magasin de requête. Les valeurs valides sont READ_ONLY et READ_WRITE. En mode READ_WRITE, le magasin de requête collecte et conserve les informations sur le plan de requête et les statistiques d'exécution. En mode READ_ONLY, les informations peuvent être lues à partir du magasin de requête, mais les nouvelles informations ne sont pas ajoutées. Si la valeur maximale de l'espace du magasin  de requête allouée est épuisée, le mode d’opération du magasin de requête passe en READ_ONLY.  
  
 CLEANUP_POLICY  
 Décrit la stratégie de rétention des données du magasin de requête. STALE_QUERY_THRESHOLD_DAYS détermine le nombre de jours pendant lesquels les informations d’une requête sont conservées dans le magasin de requêtes. STALE_QUERY_THRESHOLD_DAYS est de type **bigint**.  
  
 DATA_FLUSH_INTERVAL_SECONDS  
 Détermine la fréquence à laquelle les données écrites sur le magasin de requête magasin est conservée sur le disque. Pour optimiser les performances, les données collectées par le magasin de requête sont écrites de façon asynchrone sur le disque. La fréquence à laquelle ce transfert asynchrone se produit est configurée à l'aide de l'argument DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS est de type **bigint**.  
  
 MAX_STORAGE_SIZE_MB  
 Détermine l'espace alloué au magasin de requête. MAX_STORAGE_SIZE_MB est de type **bigint**.  
  
 INTERVAL_LENGTH_MINUTES  
 Détermine l'intervalle de temps à laquelle les données des statistiques d'exécution du runtime sont agrégées dans le magasin de requête. Pour optimiser l'espace, les statistiques d'exécution du runtime du magasin de statistiques du runtime sont agrégées sur une période fixe. Cette fenêtre de temps fixe est configurée à l'aide de l'argument INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES est de type **bigint**.  
  
 SIZE_BASED_CLEANUP_MODE  
 Contrôle si le nettoyage est activé automatiquement quand la quantité totale de données approche de la taille maximale :  
  
 OFF  
 Le nettoyage basé sur la taille n’est pas activé automatiquement. 
  
 AUTO  
 Le nettoyage basé sur la taille est activé automatiquement quand la taille sur le disque atteint 90 % de **max_storage_size_mb**. Le nettoyage basée sur la taille supprime les requêtes les moins coûteuses et les plus anciennes en premier. Il s’arrête à environ 80 % de **max_storage_size_mb**.  Il s’agit de la valeur de configuration par défaut.  
  
 SIZE_BASED_CLEANUP_MODE est de type **nvarchar**.  
  
 QUERY_CAPTURE_MODE  
 Désigne le mode de capture de requête actif actuellement :  
  
 ALL : Toutes les requêtes sont capturées. Il s’agit de la valeur de configuration par défaut.  Il s’agit de la valeur de configuration par défaut pour [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)].
  
 AUTO : Capturer les requêtes pertinentes en fonction du nombre d’exécutions et de la consommation de ressources.  Il s’agit de la valeur de configuration par défaut pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
  
 NONE : Cesser la capture des nouvelles requêtes. Le magasin de requêtes continuera à recueillir des statistiques de compilation et d’exécution pour les requêtes qui ont déjà été capturées. Utilisez cette configuration avec précaution, car vous risquez de manquer la capture de requêtes importantes.  
  
 QUERY_CAPTURE_MODE est de type **nvarchar**.  
  
 max_plans_per_query  
 Entier représentant le nombre maximal de plans gérés pour chaque requête. La valeur par défaut est 200.  
  
 **\<recovery_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Contrôle les options de récupération de base de données et la vérification des erreurs d'E/S disque.  
  
 FULL  
 Assure une récupération complète après la défaillance d'un support à l'aide des sauvegardes de journaux des transactions. Si un fichier de données est endommagé, la récupération des supports peut restaurer toutes les transactions validées. Pour plus d’informations, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 BULK_LOGGED  
 Fournit la récupération après la défaillance d'un support en associant des performances optimales et une utilisation minimale de l'espace réservé aux fichiers journaux pour certaines opérations en bloc ou de grande échelle. Pour plus d’informations sur les opérations pouvant faire l’objet d’une journalisation minimale, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md). Avec le mode de récupération BULK_LOGGED, ces opérations font l'objet d'une journalisation minimale. Pour plus d’informations, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 SIMPLE  
 Une stratégie de sauvegarde simple utilisant un espace de journalisation minimal est fournie. L'espace réservé aux fichiers journaux peut être automatiquement réutilisé lorsqu'il n'est plus utilisé pour la récupération des défaillances serveur. Pour plus d’informations, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
> [!IMPORTANT]  
> Le mode de récupération simple est plus facile à gérer que les deux autres modes, mais le risque de perte de données est plus élevé lorsqu'un fichier de données est endommagé. Toutes les modifications apportées depuis la dernière sauvegarde de la base de données ou de la sauvegarde différentielle de la base de données sont perdues et doivent être réintroduites manuellement.  
  
 Le mode de récupération par défaut dépend du mode de récupération de la base de données **model** . Pour plus d’informations sur la sélection du mode de récupération le plus approprié, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 Vous pouvez déterminer l’état de cette option en consultant les colonnes **recovery_model** et **recovery_model_desc** de la vue de catalogue sys.databases ou la propriété Recovery de la fonction DATABASEPROPERTYEX.  
  
 TORN_PAGE_DETECTION { ON | OFF }  
 ON  
 Les pages incomplètes peuvent être détectées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 OFF  
 Les pages incomplètes ne peuvent pas être détectées par le [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
> [!IMPORTANT]  
> La structure syntaxique TORN_PAGE_DETECTION ON | OFF sera supprimée dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette structure syntaxique dans le développement de nouvelles applications et envisagez de modifier celles qui l'utilisent actuellement. Utilisez l'option PAGE_VERIFY à la place.  
  
<a name="page_verify"></a> PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }  
 Détecte les pages de base de données endommagées résultant d'erreurs de chemin d'E/S disque. Les erreurs de chemin d'E/S disque peuvent endommager la base de données et résultent généralement d'une défaillance matérielle des disques ou de pannes d'alimentation survenant au moment de l'écriture de la page sur le disque.  
  
 CHECKSUM  
 Calcule une somme de contrôle du contenu d'une page entière et stocke la valeur dans l'en-tête de page lorsque celle-ci est écrite sur le disque. Lorsque la page est ensuite lue à partir du disque, la somme de contrôle est recalculée et le résultat est comparé à la valeur préalablement stockée dans l'en-tête de la page. Si les valeurs diffèrent, le message d'erreur 824 (indiquant l'échec d'une somme de contrôle) est signalé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le journal des événements Windows. Un échec de somme de contrôle indique un problème de chemin d'E/S. Pour en déterminer la cause, vous devez examiner le matériel, les pilotes de microprogrammes, le BIOS, les pilotes de filtre (par exemple un logiciel antivirus) et d'autres composants de chemin d'E/S.  
  
 TORN_PAGE_DETECTION  
 Enregistre un modèle spécifique de 2 bits pour chaque secteur de 512 octets dans la page de base de données de 8 kilo-octets (Ko) et le stocke dans l'en-tête de page de base de données au moment où la page est écrite sur le disque. Lorsque la page est ensuite lue à partir du disque, les bits endommagés stockés dans l'en-tête de la page sont comparés aux informations réelles du secteur concerné. Lorsque les valeurs ne concordent pas, cela signifie que seule une partie de la page a été écrite sur le disque. Dans un tel cas, le message d'erreur 824 (indiquant une erreur de page endommagée) est signalé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le journal des événements Windows. Les pages endommagées sont généralement détectées par la récupération de base de données s'il s'agit réellement d'une écriture de page incomplète. Toutefois, les échecs de chemin d'E/S peuvent donner lieu à tout moment à une page endommagée.  
  
 Aucune  
 Les écritures de page de base de données ne génèrent pas de valeur CHECKSUM ni TORN_PAGE_DETECTION. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne vérifie pas une somme de contrôle ou une page endommagée au cours d'une lecture, même si l'en-tête de page comporte une valeur CHECKSUM ou TORN_PAGE_DETECTION.  
  
 Prenez en considération les points suivants lorsque vous utilisez l'option PAGE_VERIFY :  
  
-   La valeur par défaut est CHECKSUM.  
  
-   Lorsqu'une base de données utilisateur ou système est mise à niveau vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou une version ultérieure, la valeur PAGE_VERIFY (NONE ou TORN_PAGE_DETECTION) est conservée. Nous vous recommandons d'utiliser CHECKSUM.  
  
    > [!NOTE]  
    > Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'option de base de données PAGE_VERIFY est définie par la valeur NONE pour la base de données tempdb et ne peut pas être modifiée. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, la valeur par défaut pour la base de données tempdb est CHECKSUM pour les nouvelles installations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quand vous mettez à niveau une installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la valeur par défaut reste NONE. L'option peut être modifiée. Nous vous recommandons d'utiliser CHECKSUM pour la base de données tempdb.  
  
-   Même si la valeur TORN_PAGE_DETECTION utilise moins de ressources, elle ne fournit qu'un sous-ensemble limité de la protection offerte par CHECKSUM.  
  
-   Il est possible de définir PAGE_VERIFY sans mettre la base de données hors connexion, la verrouiller ni empêcher d'une quelconque façon la concurrence pour cette base de données.  
  
-   CHECKSUM et TORN_PAGE_DETECTION s'excluent mutuellement. Les deux options ne peuvent pas être activées en même temps.  
  
 Lors de la détection d'une page endommagée ou d'un échec de somme de contrôle, vous pouvez procéder à une récupération en restaurant les données ou en reconstruisant éventuellement l'index si la défaillance se limite à des pages d'index. En présence d'une erreur de somme de contrôle, pour déterminer le type de la ou des pages de données affectées, exécutez DBCC CHECKDB. Pour plus d’informations sur les options de restauration, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Bien que la restauration des données permette de résoudre le problème d'endommagement des données, la cause première, par exemple une défaillance matérielle du disque, doit être identifiée et corrigée le plus rapidement possible pour éviter que ces erreurs persistent.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procède à quatre nouvelles tentatives pour une lecture qui échoue avec une erreur de somme de contrôle, de page endommagée ou d'E/S disque. Si la lecture réussit lors d'une de ces tentatives, un message est écrit dans le journal des erreurs et l'exécution de la commande qui a déclenché la lecture se poursuit. Si les tentatives de lecture échouent, la commande échoue elle aussi avec le message d'erreur 824.  
  
 Pour plus d’informations sur les messages d’erreur 823, 824 et 825, consultez [Comment faire pour dépanner une erreur Msg 823 dans SQL Server](http://support.microsoft.com/help/2015755), [How to troubleshoot Msg 824 in SQL Server (Guide pratique pour résoudre une erreur Msg 824 dans SQL Server)](http://support.microsoft.com/help/2015756) et [How to troubleshoot Msg 825 &#40;read retry&#41; in SQL Server (Guide pratique pour résoudre une erreur Msg 825 (nouvelle tentative de lecture) dans SQL Server)](http://support.microsoft.com/help/2015757).
  
 Vous pouvez déterminer l’état de cette option en consultant la colonne *page_verify_option* de la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété *IsTornPageDetectionEnabled* de la fonction [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).  
  
**\<remote_data_archive_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Active ou désactive Stretch Database pour la base de données. Pour plus d'informations, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
REMOTE_DATA_ARCHIVE = { ON ( SERVER = \<server_name> , { CREDENTIAL = \<db_scoped_credential_name> | FEDERATED_SERVICE_ACCOUNT =  ON | OFF } )| OFF ON  
 Active Stretch Database pour la base de données. Pour plus d’informations, notamment sur les prérequis supplémentaires, consultez [Activer Stretch Database pour une base de données](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).  
  
 **Les autorisations**. L’activation de Stretch Database pour une table ou une base de données nécessite les autorisations db_owner. L’activation de Stretch Database pour une base de données nécessite également les autorisations CONTROL DATABASE.  
  
SERVER = \<server_name>  
 Spécifie l’adresse du serveur Azure. Incluez la partie `.database.windows.net` du nom. Par exemple, `MyStretchDatabaseServer.database.windows.net`.  
  
CREDENTIAL = \<db_scoped_credential_name>  
 Spécifie les informations d’identification délimitées à la base de données utilisées par l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter au serveur Azure. Vérifiez que les informations d’identification existent avant d’exécuter cette commande. Pour plus d’informations, consultez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).  
  
FEDERATED_SERVICE_ACCOUNT =  ON | OFF  
 Vous pouvez utiliser un compte de service fédéré pour que le serveur SQL Server local communique avec le serveur Azure distant quand les conditions suivantes sont toutes remplies.  
  
-   Le compte de service sous lequel l’instance de SQL Server s’exécute est un compte de domaine.  
  
-   Le compte de domaine appartient à un domaine dont Active Directory est fédéré avec Azure Active Directory.  
  
-   Le serveur Azure distant est configuré pour prendre en charge l’authentification Azure Active Directory.  
  
-   Le compte de service sous lequel s’exécute l’instance de SQL Server doit être configuré comme un compte dbmanager ou sysadmin sur le serveur Azure distant.  
  
 Si vous spécifiez ON, vous ne pouvez pas spécifier également l’argument CREDENTIAL. Si vous spécifiez OFF, vous devez fournir l’argument CREDENTIAL.  
  
 OFF  
 Désactive Stretch Database pour la base de données. Pour plus d’informations, consultez [Désactiver Stretch Database et récupérer les données distantes](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).  
  
 Vous pouvez uniquement désactiver Stretch Database pour une base de données une fois que celle-ci ne contient plus de tables qui sont activées pour Stretch Database. Après la désactivation de Stretch Database, la migration des données s’arrête et les résultats de la requête n’incluent plus les résultats des tables distantes.  
  
 La désactivation de Stretch ne supprime pas la base de données distante. Si vous souhaitez supprimer la base de données distante, vous devez la supprimer à l'aide du portail de gestion Azure.  
  
**\<service_broker_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Contrôle les options [!INCLUDE[ssSB](../../includes/sssb-md.md)] suivantes : active ou désactive la remise de messages, définit un nouvel identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] ou définit les priorités de conversation sur ON ou OFF.  
  
 ENABLE_BROKER  
 Spécifie que [!INCLUDE[ssSB](../../includes/sssb-md.md)] est activé pour la base de données spécifiée. La remise des messages est démarrée et l'indicateur is_broker_enabled a la valeur true dans l'affichage catalogue sys.databases. La base de données conserve l'identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant. Service Broker ne peut pas être activé lorsque la base de données est la base de données principale dans une configuration de mise en miroir de bases de données.  
  
> [!NOTE]  
>  ENABLE_BROKER requiert un verrou de base de données exclusif. Si d'autres sessions ont verrouillé des ressources dans la base de données, ENABLE_BROKER attend jusqu'à ce que les autres sessions libèrent leurs verrous. Pour activer [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans une base de données utilisateur, vérifiez qu'aucune autre session n'utilise la base de données avant d'exécuter l'instruction ALTER DATABASE SET ENABLE_BROKER, comme le fait de mettre la base de données en mode mono-utilisateur. Pour activer [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans la base de données msdb, arrêtez d'abord l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], afin que [!INCLUDE[ssSB](../../includes/sssb-md.md)] puisse obtenir le verrou nécessaire.  
  
 DISABLE_BROKER  
 Spécifie que [!INCLUDE[ssSB](../../includes/sssb-md.md)] est désactivé pour la base de données spécifiée. La remise des messages est arrêtée et l'indicateur is_broker_enabled a la valeur false dans l'affichage catalogue sys.databases. La base de données conserve l'identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant.  
  
 NEW_BROKER  
 Spécifie que la base de données doit recevoir un nouvel identificateur Service Broker. Dans la mesure où la base de données est considérée comme un nouveau Service Broker, toutes les conversations existantes dans la base de données sont immédiatement supprimées sans générer de message de fin de dialogue. Tout itinéraire qui fait référence à l'ancien identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit être recréé avec le nouvel identificateur.  
  
 ERROR_BROKER_CONVERSATIONS  
 Spécifie que la remise de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] est activée. Cela préserve l’identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant de la base de données. [!INCLUDE[ssSB](../../includes/sssb-md.md)] met fin à toutes les conversations de la base de données avec une erreur. De cette façon, les applications peuvent effectuer un nettoyage régulier des conversations existantes.  
  
 HONOR_BROKER_PRIORITY {ON | OFF}  
 ON  
 Les opérations d'envoi prennent en considération les niveaux de priorité assignés aux conversations. Les messages issus de conversations dont les niveaux de priorité sont élevés sont envoyés avant ceux issus de conversations dont les niveaux de priorité sont faibles.  
  
 OFF  
 Les opérations d'envoi s'exécutent comme si toutes les conversations étaient dotées du niveau de priorité par défaut.  
  
 Les modifications apportées à l'option HONOR_BROKER_PRIORITY prennent effet immédiatement pour les nouveaux dialogues ou les dialogues qui n'ont pas de messages en attente d'envoi. Les dialogues qui ont des messages attendant d'être envoyés lorsque l'instruction ALTER DATABASE est exécutée ne récupéreront pas le nouveau paramètre tant que certains messages pour le dialogue n'auront pas été envoyés. La durée nécessaire pour que tous les dialogues commencent à utiliser le nouveau paramètre peut varier considérablement.  
  
 Le paramètre actuel de cette propriété est signalé dans la colonne is_broker_priority_honored dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 **\<snapshot_option> ::=**  
  
 Détermine le niveau d'isolation de la transaction.  
  
 ALLOW_SNAPSHOT_ISOLATION { ON | OFF }  
 ON  
 Active l'option Instantané au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n'utilise l'isolement de capture instantanée. Une fois que cette option est activée, les transactions peuvent spécifier le niveau d'isolement des transactions SNAPSHOT. Quand une transaction s'exécute au niveau d'isolement SNAPSHOT, toutes les instructions voient un instantané des données tel qu'il existe au début de la transaction. Si une transaction exécutée au niveau d'isolement SNAPSHOT accède à des données dans plusieurs bases de données, l'option ALLOW_SNAPSHOT_ISOLATION doit avoir la valeur ON dans toutes les bases de données ou chaque instruction de la transaction doit utiliser des indicateurs de verrouillage sur toute référence d'une clause FROM à une table de la base de données dont l'option ALLOW_SNAPSHOT_ISOLATION a la valeur OFF.  
  
 OFF  
 Désactive l'option Instantané au niveau de la base de données. Les transactions ne peuvent pas spécifier le niveau d'isolation de la transaction SNAPSHOT.  
  
 Lorsque vous modifiez l'état de ALLOW_SNAPSHOT_ISOLATION (de ON à OFF ou inversement), ALTER DATABASE ne retourne pas le contrôle à l'appelant tant que toutes les transactions existantes dans la base de données n'ont pas été validées. Si la base de données présente déjà l'état spécifié dans l'instruction ALTER DATABASE, le contrôle est immédiatement retourné à l'appelant. Si l’instruction ALTER DATABASE n’est pas retournée rapidement, utilisez [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) pour déterminer si certaines transactions sont de longue durée. Si l'instruction ALTER DATABASE est annulée, la base de données conserve l'état qu'elle présentait au démarrage de ALTER DATABASE. la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indique l’état des transactions d’isolement d’instantané dans la base de données. Si **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF marque une pause de six secondes puis réessaie d’exécuter l’opération.  
  
 Vous ne pouvez pas modifier l'état de ALLOW_SNAPSHOT_ISOLATION si la base de données est hors connexion (OFFLINE).  
  
 Si vous configurez ALLOW_SNAPSHOT_ISOLATION dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé si la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).  
  
 Vous pouvez modifier les paramètres de ALLOW_SNAPSHOT_ISOLATION pour les bases de données master, model, msdb et tempdb. Si vous changez le paramètre pour la base de données tempdb, il est conservé à chaque arrêt et redémarrage de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Si vous modifiez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées à l'exception de tempdb.  
  
 Cette option a la valeur ON par défaut pour les bases de données master et msdb.  
  
 Vous pouvez déterminer la valeur actuelle de cette option en consultant la colonne snapshot_isolation_state de l'affichage catalogue sys.databases.  
  
 READ_COMMITTED_SNAPSHOT { ON | OFF }  
 ON  
 Active l'option d'instantané de lecture validée au niveau de la base de données. Lorsque cette option est activée, les instructions DML commencent à générer des versions de ligne même quand aucune transaction n'utilise l'isolement de capture instantanée. Une fois que cette option est activée, les transactions qui définissent le niveau d'isolement de lecture validée utilisent le contrôle de version de ligne au lieu du verrouillage. Lorsqu'une transaction est exécutée au niveau d'isolation de lecture validée, toutes les instructions voient un instantané des données telles qu'elles se présentent au début de l'instruction.  
  
 OFF  
 Désactive l'option d'instantané de lecture validée au niveau de la base de données. Les transactions spécifiant le niveau d'isolation READ COMMITTED utilisent le verrouillage.  
  
 Pour activer (ON) ou désactiver (OFF) READ_COMMITTED_SNAPSHOT, il ne doit exister aucune connexion active à la base de données, à l'exception de la connexion exécutant la commande ALTER DATABASE. Toutefois, il n'est pas nécessaire que la base de données soit en mode mono-utilisateur. Vous ne pouvez pas modifier l'état de cette option si la base de données est hors connexion (OFFLINE).  
  
 Si vous configurez READ_COMMITTED_SNAPSHOT dans une base de données en lecture seule (READ_ONLY), le paramètre est conservé lorsque la base de données devient par la suite accessible en lecture et en écriture (READ_WRITE).  
  
 Il n'est pas possible d'affecter la valeur ON à READ_COMMITTED_SNAPSHOT pour les bases de données système master, tempdb ou msdb. Si vous changez le paramètre pour la base de données model, il devient le paramètre par défaut pour toutes les nouvelles bases de données créées, à l’exception de tempdb.  
  
 Vous pouvez déterminer la valeur actuelle de cette option en consultant la colonne is_read_committed_snapshot_on de l'affichage catalogue sys.databases.  
  
> [!WARNING]  
>  Quand une table est créée avec **DURABILITY = SCHEMA_ONLY** et que **READ_COMMITTED_SNAPSHOT** est changé par la suite à l’aide d’**ALTER DATABASE**, les données de la table sont perdues.  
  
 MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | OFF }  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 ON  
 Lorsque le niveau d'isolation de la transaction est défini sur un niveau inférieur à SNAPSHOT (par exemple, READ COMMITTED ou READ UNCOMMITTED), toutes les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables mémoire optimisées sont effectuées avec l'isolation SNAPSHOT. C'est le cas même si le niveau d'isolation de la transaction est défini explicitement sur le niveau de la session, ou si la valeur par défaut est utilisée implicitement.  
  
 OFF  
 N'élève pas le niveau d'isolation pour les opérations en [!INCLUDE[tsql](../../includes/tsql-md.md)] interprété sur les tables mémoire optimisées.  
  
 Vous ne pouvez pas modifier l'état de MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT si la base de données est hors connexion (OFFLINE).  
  
 L'option est désactivée (OFF) par défaut.  
  
 Vous pouvez déterminer la valeur actuelle de cette option en consultant la colonne **is_memory_optimized_elevate_to_snapshot_on** de la vue de catalogue [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 **\<sql_option> ::=**  
  
 Contrôle les options de conformité ANSI au niveau de la base de données.  
  
 ANSI_NULL_DEFAULT { ON | OFF }  
 Détermine la valeur par défaut, NULL ou NOT NULL, d’une colonne, d’un [type CLR défini par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) dont le paramètre d’acceptation des valeurs NULL n’est pas défini de façon explicite dans les instructions CREATE TABLE ou ALTER TABLE. Les colonnes définies avec des contraintes respectent les règles de contrainte, quelle que soit la valeur de ce paramètre.  
  
 ON  
 La valeur par défaut est NULL.  
  
 OFF  
 La valeur par défaut est NOT NULL.  
  
 Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre défini pour ANSI_NULL_DEFAULT au niveau de la base de données par défaut. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULL_DEFAULT pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SET ANSI_NULL_DFLT_ON &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).  
  
 Pour garantir la compatibilité ANSI, l'activation (ON) de l'option de base de données ANSI_NULL_DEFAULT entraîne la définition de NULL comme valeur par défaut de la base de données.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_ansi_null_default_on de l'affichage catalogue sys.databases ou la propriété IsAnsiNullDefault de la fonction DATABASEPROPERTYEX.  
  
 ANSI_NULLS { ON | OFF }  
 ON  
 Toutes les comparaisons avec une valeur Null produisent le résultat UNKNOWN (inconnu).  
  
 OFF  
 Les comparaisons de valeurs non-UNICODE avec une valeur Null génèrent la valeur TRUE si les deux valeurs sont Null.  
  
> [!IMPORTANT]  
>  Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
 Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut défini pour ANSI_NULLS au niveau de la base de données. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_NULLS pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md).  
  
 SET ANSI_NULLS doit également avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_ansi_nulls_on de l'affichage catalogue sys.databases ou la propriété IsAnsiNullsEnabled de la fonction DATABASEPROPERTYEX.  
  
 ANSI_PADDING { ON | OFF }  
 ON  
 Les chaînes sont complétées pour avoir la même longueur avant leur conversion ou insertion dans un type de données **varchar** ou **nvarchar**.  
  
 Les espaces à droite dans les valeurs de type caractère insérées dans des colonnes **varchar** ou **nvarchar** et les zéros à droite dans les valeurs binaires insérées dans des colonnes **varbinary** ne sont pas supprimés. Les valeurs ne sont pas complétées à concurrence de la longueur de la colonne.  
  
 OFF  
 Les espaces à droite pour **varchar** ou **nvarchar** et les zéros pour **varbinary** sont supprimés.  
  
 Lorsque cette option a la valeur OFF, elle affecte uniquement la définition des nouvelles colonnes.  
  
> [!IMPORTANT]  
>  Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Il est recommandé de toujours affecter la valeur ON à l'option ANSI_PADDING. Par ailleurs, ANSI_PADDING doit avoir la valeur ON lorsque vous créez ou manipulez des index dans des colonnes calculées ou des vues indexées.  
  
 Les colonnes **char(*n*)** et **binary(*n*)** qui acceptent les valeurs NULL sont complétées à concurrence de la longueur de la colonne quand ANSI_PADDING a la valeur ON, mais les espaces et les zéros à droite sont supprimés quand ANSI_PADDING a la valeur OFF. Les colonnes **char(*n*)** et **binary(*n*)** qui n’acceptent pas les valeurs NULL sont toujours complétées à concurrence de la longueur de la colonne.  
  
 Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre de ANSI_PADDING au niveau de la base de données par défaut. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_PADDING pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_ansi_padding_on de l'affichage catalogue sys.databases ou la propriété IsAnsiPaddingEnabled de la fonction DATABASEPROPERTYEX.  
  
 ANSI_WARNINGS { ON | OFF }  
 ON  
 Des erreurs ou des avertissements sont générés en présence de certaines conditions, telles qu'une division par zéro, ou lorsque des valeurs Null apparaissent dans des fonctions d'agrégation.  
  
 OFF  
 Aucun avertissement n'est généré et des valeurs Null sont retournées lorsque des conditions telles qu'une division par zéro se manifestent.  
  
 SET ANSI_WARNINGS doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.  
  
 Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de ANSI_NULLS défini au niveau de la base de données. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à ANSI_WARNINGS pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md).  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_ansi_warnings_on de l'affichage catalogue sys.databases ou la propriété IsAnsiWarningsEnabled de la fonction DATABASEPROPERTYEX.  
  
 ARITHABORT { ON | OFF }  
 ON  
 Arrête une requête lorsqu'un dépassement de capacité ou une division par zéro se produit durant son exécution.  
  
 OFF  
 Un message d'avertissement s'affiche si l'une de ces erreurs se produit, mais le traitement de la requête, du lot ou de la transaction se poursuit, comme s'il n'y avait pas d'erreur.  
  
 SET ARITHABORT doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_arithabort_on de l'affichage catalogue sys.databases ou la propriété IsArithmeticAbortEnabled de la fonction DATABASEPROPERTYEX.  
  
 COMPATIBILITY_LEVEL = { 90 | 100 | 110 | 120 | 130 | 140 }  
 Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
 CONCAT_NULL_YIELDS_NULL { ON | OFF }  
 ON  
 Le résultat d'une concaténation est NULL lorsque l'un des deux opérandes est NULL. Par exemple, la concaténation de la chaîne de caractères « Ceci est » et NULL donne la valeur NULL et non la valeur « Ceci est ».  
  
 OFF  
 La valeur Null est considérée comme une chaîne de caractères vide.  
  
 CONCAT_NULL_YIELDS_NULL doit avoir la valeur ON lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.  
  
> [!IMPORTANT]  
>  Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], CONCAT_NULL_YIELDS_NULL aura toujours la valeur ON et toute application qui définira explicitement l'option à OFF produira une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
 Les paramètres définis à l'aide de l'instruction SET au niveau de la connexion se substituent au paramètre par défaut de CONCAT_NULL_YIELDS_NULL défini au niveau de la base de données. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à CONCAT_NULL_YIELDS_NULL pour la session lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_concat_null_yields_null_on de l'affichage catalogue sys.databases ou la propriété IsNullConcat de la fonction DATABASEPROPERTYEX.  
  
 QUOTED_IDENTIFIER { ON | OFF }  
 ON  
 Des guillemets doubles peuvent être utilisés pour entourer des identificateurs délimités.  
  
 Toutes les chaînes délimitées par des guillemets doubles sont considérées comme des identificateurs d'objet. Les identificateurs entre guillemets n'ont pas à respecter les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] propres aux identificateurs. Ils peuvent être des mots clés et contenir des caractères généralement interdits dans les identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si un guillemet simple (') fait partie de la chaîne littérale, il pourra être représenté par un guillemet double ('').  
  
 OFF  
 Les identificateurs ne peuvent figurer entre guillemets et doivent respecter toutes les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] en matière d'identificateurs. Les chaînes littérales peuvent être délimitées par des guillemets simples ou doubles.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet également de délimiter les identificateurs par des crochets ([ ]). Les identificateurs entre crochets peuvent toujours être utilisés, quel que soit le paramétrage de QUOTED_IDENTIFIER. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
 Lors de la création d'une table, l'option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table, même si elle a la valeur OFF au moment de sa création.  
  
 Les paramètres définis au niveau de la connexion à l'aide de l'instruction SET se substituent au paramètre de base de données par défaut de QUOTED_IDENTIFIER. Par défaut, les clients ODBC et OLE DB génèrent une instruction SET de niveau connexion qui affecte la valeur ON à QUOTED_IDENTIFIER, lors de la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_quoted_identifier_on de l'affichage catalogue sys.databases ou la propriété IsQuotedIdentifiersEnabled de la fonction DATABASEPROPERTYEX.  
  
 NUMERIC_ROUNDABORT { ON | OFF }  
 ON  
 Une erreur est générée lors d'une perte de précision dans une expression.  
  
 OFF  
 Les pertes de précision ne génèrent pas de messages d'erreur et le résultat est arrondi en fonction de la précision de la colonne ou de la variable contenant le résultat.  
  
 NUMERIC_ROUNDABORT doit avoir la valeur OFF lorsque vous créez ou modifiez des index dans des colonnes calculées ou des vues indexées.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_numeric_roundabort_on de l'affichage catalogue sys.databases ou la propriété IsNumericRoundAbortEnabled de la fonction DATABASEPROPERTYEX.  
  
 RECURSIVE_TRIGGERS { ON | OFF }  
 ON  
 L'activation récursive des déclencheurs AFTER est autorisée.  
  
 OFF  
 Seule l'activation récursive directe des déclencheurs AFTER n'est pas autorisée. Pour désactiver également la récursivité indirecte des déclencheurs AFTER, affectez la valeur **0** à l’option serveur nested triggers à l’aide de **sp_configure**.  
  
> [!NOTE]  
>  Seule la récursivité directe est désactivée lorsque RECURSIVE_TRIGGERS a la valeur OFF. Pour désactiver la récursivité indirecte, vous devez aussi affecter la valeur 0 à l'option serveur nested triggers.  
  
 Vous pouvez déterminer l'état de cette option en consultant la colonne is_recursive_triggers_on de l'affichage catalogue sys.databases ou la propriété IsRecursiveTriggersEnabled de la fonction DATABASEPROPERTYEX.  
  
 **\<target_recovery_time_option> ::=**  
  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Non disponible dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Spécifie la fréquence des points de contrôle indirects en fonction de chaque base de données. Depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la valeur par défaut pour les nouvelles bases de données est une minute, ce qui signifie que la base de données utilise les points de contrôle indirects. Pour les versions antérieures, la valeur par défaut est 0, ce qui indique que la base de données utilise les points de contrôle automatiques, dont la fréquence dépend du paramètre d’intervalle de récupération de l’instance de serveur. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande une valeur d’une minute pour la plupart des systèmes.  
  
 TARGET_RECOVERY_TIME **=***target_recovery_time* { SECONDS | MINUTES }  
 *target_recovery_time*  
 Spécifie la limite maximale de durée de récupération de la base de données spécifiée en cas de sinistre.  
  
 SECONDS  
 Indique que *target_recovery_time* correspond au nombre de secondes.  
  
 MINUTES  
 Indique que *target_recovery_time* correspond au nombre de minutes.  
  
 Pour plus d’informations sur les points de contrôle indirects, consultez [Points de contrôle de base de données &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
 **WITH \<termination> ::=**  
  
 Spécifie le(s) cas où une transaction incomplète doit être restaurée lors d'un changement d'état de la base de données. Lorsque la clause de fin est omise, l'instruction ALTER DATABASE attend indéfiniment s'il existe un verrou quelconque sur la base de données. Une seule clause de fin peut être spécifiée, à la suite des clauses SET.  
  
> [!NOTE]  
>  Toutes les options de base de données n’utilisent pas la clause WITH \<termination>. Pour plus d'informations, consultez le tableau du paragraphe «[Configuration des options](#SettingOptions) » de la section « Notes » de cette rubrique.  
  
 ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE  
 Indique si la restauration intervient après le nombre de secondes spécifié ou immédiatement.  
  
 NO_WAIT  
 Indique que la modification souhaitée de l'option ou de l'état de la base de données échoue si sa réalisation immédiate suppose la validation ou la restauration des transactions de leur propre fait.  
  
##  <a name="SettingOptions"></a> Configuration des options  
 Pour récupérer les paramètres actuels des options de base de données, utilisez la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).  
  
 Lorsque vous définissez une option de base de données, la modification prend effet immédiatement.  
  
 Pour modifier les valeurs par défaut de l'une des options de base de données afin qu'elles s'appliquent à toutes les nouvelles bases de données créées, modifiez l'option de base de données appropriée dans la base de données model.  
  
 Toutes les options de base de données n’utilisent pas la clause WITH \<termination> et ne peuvent pas être combinées avec d’autres options. Le tableau suivant répertorie ces options ainsi que l'état de l'option et d'arrêt.  
  
|Catégorie d'options|Peut être spécifiée avec d'autres options|Peut utiliser la clause WITH \<termination>|  
|----------------------|-----------------------------------------|---------------------------------------------|  
|\<db_state_option>|Oui|Oui|  
|\<db_user_access_option>|Oui|Oui|  
|\<db_update_option>|Oui|Oui|  
|\<delayed_durability_option>|Oui|Oui|  
|\<external_access_option>|Oui|non|  
|\<cursor_option>|Oui|non|  
|\<auto_option>|Oui|non|  
|\<sql_option>|Oui|non|  
|\<recovery_option>|Oui|non|  
|\<target_recovery_time_option>|non|Oui|  
|\<database_mirroring_option>|non|non|  
|ALLOW_SNAPSHOT_ISOLATION|non|non|  
|READ_COMMITTED_SNAPSHOT|non|Oui|  
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Oui|Oui|  
|\<service_broker_option>|Oui|non|  
|DATE_CORRELATION_OPTIMIZATION|Oui|Oui|  
|\<parameterization_option>|Oui|Oui|  
|\<change_tracking_option>|Oui|Oui|  
|\<db_encryption>|Oui|non|  
  
 Le cache de plan pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est effacé par la configuration d'une des options suivantes :  
  
|||  
|-|-|  
|OFFLINE|READ_WRITE|  
|ONLINE|MODIFY FILEGROUP DEFAULT|  
|MODIFY_NAME|MODIFY FILEGROUP READ_WRITE|  
|COLLATE|MODIFY FILEGROUP READ_ONLY|  
|READ_ONLY||  
  
 Le cache de procédures est également vidé dans les scénarios suivants.  
  
-   L'option de base de données AUTO_CLOSE est activée (ON). Lorsqu'aucune connexion utilisateur ne fait référence ou n'utilise la base de données, la tâche en arrière-plan essaie de fermer et d'arrêter la base de données automatiquement.  
  
-   Vous exécutez plusieurs requêtes sur une base de données dont les options par défaut sont activées. Puis, la base de données est supprimée.  
  
-   Un instantané de base de données pour une base de données source est supprimé.  
  
-   Vous reconstruisez avec succès le journal des transactions d'une base de données.  
  
-   Vous restaurez une sauvegarde de base de données.  
  
-   Vous détachez une base de données.  
  
 Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. Pour chaque mémoire cache effacée dans le cache de plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d'information suivant : « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a rencontré %d occurrence(s) de vidages de mémoire cache pour la mémoire cache '%s' (partie du cache du plan) en raison d'opérations de maintenance ou de reconfiguration de base de données ». Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-options-on-a-database"></a>A. Configuration des options d'une base de données  
 L'exemple suivant définit les options de mode de récupération et de vérification de pages de données pour l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL PAGE_VERIFY CHECKSUM;  
GO  
  
```  
  
### <a name="b-setting-the-database-to-readonly"></a>B. Paramétrage de la base de données avec READ_ONLY  
 Pour modifier l'état d'une base de données ou d'un groupe de fichiers en spécifiant READ_ONLY ou READ_WRITE, vous avez besoin d'un accès exclusif à la base de données. L'exemple ci-dessous illustre le basculement de la base de données en mode `SINGLE_USER` pour obtenir l'accès exclusif. L'exemple affecte ensuite à la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] l'état `READ_ONLY` et rend à tous les utilisateurs l'accès à la base de données.  
  
> [!NOTE]  
>  Cet exemple utilise l'option de fin `WITH ROLLBACK IMMEDIATE` dans la première instruction `ALTER DATABASE` . Toutes les transactions incomplètes seront restaurées et les autres connexions à la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] immédiatement déconnectées.  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
SET SINGLE_USER  
WITH ROLLBACK IMMEDIATE;  
GO  
ALTER DATABASE AdventureWorks2012  
SET READ_ONLY  
GO  
ALTER DATABASE AdventureWorks2012  
SET MULTI_USER;  
GO  
  
```  
  
### <a name="c-enabling-snapshot-isolation-on-a-database"></a>C. Activation de l'isolement d'instantané sur une base de données  
 L'exemple ci-dessous illustre l'activation de l'option d'infrastructure d'isolement d'instantané pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
-- Check the state of the snapshot_isolation_framework  
-- in the database.  
SELECT name, snapshot_isolation_state,   
    snapshot_isolation_state_desc AS description  
FROM sys.databases  
WHERE name = N'AdventureWorks2012';  
GO  
  
```  
  
 Le jeu de résultats montre que l'infrastructure d'isolement d'instantané est activée.  
  
 |NAME |snapshot_isolation_state |description|  
 |-------------------- |------------------------  |----------|  
 |AdventureWorks2012   | 1                        | ON |  
  
### <a name="d-enabling-modifying-and-disabling-change-tracking"></a>D. Activation, modification et désactivation du suivi des modifications  
 L'exemple ci-dessous illustre l'activation du suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] et la définition d'une période de rétention de `2` jours.  
  
```  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = ON  
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);  
```  
  
 L'exemple suivant illustre comment modifier la période de rétention en spécifiant `3` jours.  
  
```  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);  
```  
  
 L'exemple ci-dessous illustre comment désactiver le suivi des modifications pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = OFF;  
```  
  
### <a name="e-enabling-the-query-store"></a>E. Activation du magasin de requête  
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 L'exemple suivant active le magasin de requête et configure les paramètres de stockage des requêtes.  
  
```  
ALTER DATABASE AdventureWorks2012  
SET QUERY_STORE = ON   
    (  
      OPERATION_MODE = READ_WRITE   
    , CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 )  
    , DATA_FLUSH_INTERVAL_SECONDS = 900   
    , MAX_STORAGE_SIZE_MB = 1024   
    , INTERVAL_LENGTH_MINUTES = 60   
    );  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
 [Mise en miroir de bases de données ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [Activer et désactiver le suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)   
 [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
 [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [Bonnes pratiques relatives au Magasin des requêtes](../../relational-databases/performance/best-practice-with-the-query-store.md) 
  
