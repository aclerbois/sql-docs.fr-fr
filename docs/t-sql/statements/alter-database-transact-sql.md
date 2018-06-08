---
title: ALTER DATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/20/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER_DATABASE_TSQL
- ALTER DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- databases [SQL Server], modifying
- ALTER DATABASE statement
- databases [SQL Server], renaming
- renaming databases
- database modifications [SQL Server]
- ALTER DATABASE statement, syntax described
- database names [SQL Server], ALTER DATABASE
- modifying databases
- collations [SQL Server], modifying
- database mirroring [SQL Server], Transact-SQL
ms.assetid: 15f8affd-8f39-4021-b092-0379fc6983da
caps.latest.revision: 282
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 5e0e79fb6a74373268e6077971662b323623cc54
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-database-transact-sql"></a>ALTER DATABASE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie une base de données, ou les fichiers et groupes de fichiers associés à la base de données. Ajoute ou supprime des fichiers et des groupes de fichiers d'une base de données, modifie ses attributs ou ses fichiers et groupes de fichiers, modifie le classement de la base de données et définit les options de la base de données. Les instantanés de base de données ne peuvent pas être modifiés. Pour modifier les options de base de données associées à la réplication, utilisez [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
   
 En raison de sa longueur, la syntaxe d'ALTER DATABASE est divisée en plusieurs rubriques :  
  
 ALTER DATABASE  
 La rubrique actuelle fournit la syntaxe à utiliser pour renommer une base de données et en modifier le classement.  
  
 [Options de fichiers et de groupes de fichiers ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
 Fournit la syntaxe à utiliser pour ajouter et supprimer des fichiers et groupes de fichiers d'une base de données, et pour modifier les attributs des fichiers et groupes de fichiers.  
  
 [Options ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
 Fournit la syntaxe à utiliser pour modifier les attributs d'une base de données à l'aide des options SET d'ALTER DATABASE.  
  
 [Mise en miroir de bases de données ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)  
 Fournit la syntaxe à utiliser pour les options SET d'ALTER DATABASE relatives à la mise en miroir de bases de données.  
  
 [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)  
 Fournit la syntaxe des options [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] d’ALTER DATABASE en vue de la configuration d’une base de données secondaire sur un réplica secondaire d’un groupe de disponibilité Always On.  
  
 [Niveau de compatibilité ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
 Fournit la syntaxe à utiliser pour les options SET d'ALTER DATABASE relatives aux niveaux de compatibilité de bases de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
Pour Azure SQL Database, consultez [ALTER DATABASE &#40;Azure SQL Database&#41;](../../t-sql/statements/alter-database-azure-sql-database.md)  
Pour Azure SQL Data Warehouse, consultez [ALTER DATABASE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/alter-database-azure-sql-data-warehouse.md).  
Pour Parallel Data Warehouse, consultez [ALTER DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/alter-database-azure-sql-data-warehouse.md).
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- SQL Server Syntax  
ALTER DATABASE { database_name  | CURRENT }  
{  
    MODIFY NAME = new_database_name   
  | COLLATE collation_name  
  | <file_and_filegroup_options>  
  | <set_database_options>  
}  
[;]  
  
<file_and_filegroup_options >::=  
  <add_or_modify_files>::=  
  <filespec>::=   
  <add_or_modify_filegroups>::=  
  <filegroup_updatability_option>::=  
  
<set_database_options>::=  
  <optionspec>::=   
  <auto_option> ::=   
  <change_tracking_option> ::=  
  <cursor_option> ::=   
  <database_mirroring_option> ::=   
  <date_correlation_optimization_option> ::=  
  <db_encryption_option> ::=  
  <db_state_option> ::=  
  <db_update_option> ::=  
  <db_user_access_option> ::=  <delayed_durability_option> ::=  <external_access_option> ::=  
  <FILESTREAM_options> ::=  
  <HADR_options> ::=    
  <parameterization_option> ::=  
  <query_store_options> ::=  
  <recovery_option> ::=   
  <service_broker_option> ::=  
  <snapshot_option> ::=  
  <sql_option> ::=   
  <termination> ::=  
  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données à modifier.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données à relation contenant-contenu.  
  
 CURRENT  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Indique que la base de données actuelle en cours d'utilisation doit être modifiée.  
  
 MODIFY NAME **=***new_database_name*  
 Renomme la base de données avec le nom spécifié *nouveau_nom_base_de_données*.  
  
 COLLATE *collation_name*  
 Spécifie le classement par défaut de la base de données. *collation_name* peut être un nom de classement Windows ou SQL. S'il n'est pas spécifié, le classement par défaut de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sera appliqué à la base de données.  
  
 Lors de la création de bases de données autrement qu'avec le classement par défaut, les données dans la base de données respectent toujours le classement spécifié. Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quand vous créez une base de données à relation contenant-contenu, les informations de catalogue interne sont conservées à l’aide du classement par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **Latin1_General_100_CI_AS_WS_KS_SC**.  
  
 Pour plus d’informations sur les noms de classements Windows et SQL, consultez [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 **\<delayed_durability_option> ::=**  
 **S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Pour plus d’informations, consultez[Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) et [Contrôler la durabilité d’une transaction](../../relational-databases/logs/control-transaction-durability.md).  
  
 **\<file_and_filegroup_options>::=**  
 Pour plus d’informations, consultez [Options de fichiers et de groupes de fichiers ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
## <a name="remarks"></a>Notes   
 Pour supprimer une base de données, utilisez [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).  
  
 Pour diminuer la taille d'une base de données, utilisez [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).  
  
 L'instruction ALTER DATABASE doit être exécutée en mode de validation automatique (mode de gestion des transactions par défaut) et n'est pas autorisée dans une transaction explicite ou implicite.  
  
 L'état d'un fichier de base de données (par exemple, en ligne ou hors connexion) est préservé indépendamment de l'état de la base de données. Pour plus d’informations, consultez [États des fichiers](../../relational-databases/databases/file-states.md). L'état des fichiers dans un groupe de fichiers détermine la disponibilité de tout le groupe de fichiers. Pour qu'un groupe de fichiers soit disponible, tous ses fichiers doivent être en ligne. Si un groupe de fichiers est hors connexion, toute tentative d'accès au groupe par une instruction SQL échoue avec une erreur. Lorsque vous créez des plans de requête pour les instructions SELECT, l'optimiseur de requête évite les index non cluster et les vues indexées qui résident dans les groupes de fichiers hors connexion. Cela permet aux instructions de s'exécuter correctement. Cependant, si le groupe de fichiers hors connexion contient le segment ou l'index cluster d'une table cible, les instructions SELECT échouent. De plus, toute instruction INSERT, UPDATE ou DELETE modifiant une table assortie d'un index dans un groupe de fichiers hors connexion ne peut être exécutée.  
  
 Lorsque l'état d'une base de données est RESTORING, les instructions ALTER DATABASE, pour la plupart, échouent. La définition des options de mise en miroir de bases de données fait exception. Une base de données peut être à l'état RESTORING durant une opération de restauration active, ou lorsqu'une opération de restauration d'un fichier de base de données ou d'un fichier journal échoue car un fichier de sauvegarde est corrompu.  
  
 Le cache du plan pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est effacé par la définition de l'une des options suivantes :  
  
|||  
|-|-|  
|OFFLINE|READ_WRITE|  
|ONLINE|MODIFY FILEGROUP DEFAULT|  
|MODIFY_NAME|MODIFY FILEGROUP READ_WRITE|  
|COLLATE|MODIFY FILEGROUP READ_ONLY|  
|READ_ONLY|PAGE_VERIFY|  
  
 Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. Pour chaque mémoire cache effacée dans le cache de plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d'information suivant : « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a rencontré %d occurrence(s) de vidages de mémoire cache pour la mémoire cache '%s' (partie du cache du plan) en raison d'opérations de maintenance ou de reconfiguration de base de données ». Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.  
  
 Le cache de procédures est également vidé dans les scénarios suivants :  
  
-   L'option de base de données AUTO_CLOSE est activée (ON). Lorsqu'aucune connexion utilisateur ne fait référence ou n'utilise la base de données, la tâche en arrière-plan essaie de fermer et d'arrêter la base de données automatiquement.  
  
-   Vous exécutez plusieurs requêtes sur une base de données dont les options par défaut sont activées. Puis, la base de données est supprimée.  
  
-   Un instantané de base de données pour une base de données source est supprimé.  
  
-   Vous reconstruisez avec succès le journal des transactions d'une base de données.  
  
-   Vous restaurez une sauvegarde de base de données.  
  
-   Vous détachez une base de données.  
  
## <a name="changing-the-database-collation"></a>Modification du classement de la base de données  
 Avant d'appliquer un autre classement à une base de données, veillez à ce que les conditions suivantes soient remplies :  
  
-   Vous êtes actuellement le seul à utiliser la base de données.  
  
-   Aucun objet lié à un schéma ne dépend du classement de la base de données.  
  
     Si les objets suivants, qui dépendent du classement de base de données, existent dans la base de données, l’instruction ALTER DATABASE*database_name*COLLATE échoue. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d'erreur pour chaque objet bloquant l'action ALTER :  
  
    -   Fonctions et vues définies par l'utilisateur créées avec SCHEMABINDING  
  
    -   Colonnes calculées  
  
    -   Contraintes CHECK  
  
    -   Fonctions table qui retournent des tables comportant des colonnes de type caractère avec des classements hérités du classement par défaut de la base de données  
  
     Les informations de dépendance des entités non liées au schéma sont mises à jour automatiquement lorsque le classement de la base de données est modifié.  
  
 La modification du classement de la base de données ne crée pas de doublons parmi les noms système des objets de la base de données. Si cette modification entraîne la duplication de noms, les espaces de noms suivants peuvent faire échouer une modification du classement de la base de données :  
  
-   Noms d'objets tels qu'une procédure, une table, un déclencheur ou une vue  
  
-   Noms de schémas.  
  
-   Principaux, tels qu'un groupe, un rôle ou un utilisateur  
  
-   Noms de types scalaires, comme les types système ou définis par l'utilisateur  
  
-   Noms de catalogues de texte intégral  
  
-   Noms de colonnes ou de paramètres dans un objet  
  
-   Noms d'index dans une table  
  
Les noms en double qui résultent du nouveau classement entraînent l'échec de l'action de modification et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d'erreur spécifiant l'espace de noms en cause.  
  
## <a name="viewing-database-information"></a>Affichage des informations de bases de données  
 Vous pouvez utiliser les affichages catalogue, les fonctions système et les procédures stockées du système pour retourner des informations sur les bases de données, les fichiers et les groupes de fichiers.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-name-of-a-database"></a>A. Modification du nom d'une base de données  
 L'exemple suivant modifie le nom de la base de données `AdventureWorks2012` en `Northwind`.  
  
```  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
Modify Name = Northwind ;  
GO  
```  
  
### <a name="b-changing-the-collation-of-a-database"></a>B. Modification du classement d'une base de données  
 L'exemple suivant crée une base de données nommée `testdb` qui utilise le classement `SQL_Latin1_General_CP1_CI_A`S, puis modifie le classement de la base de données `testdb` en `COLLATE French_CI_AI`.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
USE master;  
GO  
  
CREATE DATABASE testdb  
COLLATE SQL_Latin1_General_CP1_CI_AS ;  
GO  
  
ALTER DATABASE testDB  
COLLATE French_CI_AI ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
- [ALTER DATABASE &#40;Azure SQL Database&#41;](alter-database-azure-sql-database.md)  
- [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
- [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md)   
- [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
- [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
- [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
- [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
- [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
- [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
- [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
- [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
- [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
- [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
- [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
- [Bases de données système](../../relational-databases/databases/system-databases.md)  
  
