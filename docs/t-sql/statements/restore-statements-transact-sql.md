---
title: RESTORE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- RESTORE DATABASE
- RESTORE_TSQL
- RESTORE_DATABASE_TSQL
- RESTORE
- RESTORE_LOG_TSQL
- RESTORE LOG
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE DATABASE, see RESTORE statement
- full backups [SQL Server]
- RECOVERY option
- database snapshots [SQL Server], reverting to
- STOPAT syntax
- differential backups, RESTORE statement
- point in time recovery [SQL Server]
- restoring [SQL Server]
- NORECOVERY option
- online restores [SQL Server], RESTORE statement
- moving databases
- RESTORE statement, syntax
- cross-platform restores
- restoring databases [SQL Server], options
- RESTORE statement
- snapshots [SQL Server database snapshots], reverting to
- reverting database snapshots
- transaction log backups [SQL Server], RESTORE statement
- RESTORE LOG, see RESTORE statement
ms.assetid: 877ecd57-3f2e-4237-890a-08f16e944ef1
caps.latest.revision: 248
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1edf0ff22f56446faf5fa316723c4b2a525534cb
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="restore-statements-transact-sql"></a>Instructions RESTORE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]

  Restaure les sauvegardes réalisées à l'aide de la commande BACKUP. Cette commande vous permet d'effectuer les scénarios de restauration suivants :  
  
-   Restaurer une base de données complète à partir d'une sauvegarde de base de données complète (restauration complète)  
  
-   Restaurer une partie d'une base de données (restauration partielle)  
  
-   Restaurer des fichiers ou des groupes de fichiers spécifiques dans une base de données (restauration de fichiers)  
  
-   Restaurer des pages spécifiques dans une base de données (restauration de pages)  
  
-   Restaurer un journal des transactions dans une base de données (restauration de journal des transactions)  
  
-   Rétablir une base de données au point d'instantané de base de données.  
  
[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

 Pour plus d’informations sur les scénarios de restauration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  Pour plus d’informations sur les descriptions des arguments, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).   Lors de la restauration d’une base de données d’une autre instance, tenez compte des informations de la page [Gérer les métadonnées lors de la mise à disposition d’une base de données sur une autre instance de serveur (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).
  
> [REMARQUE :](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) Pour plus d’informations sur la restauration à partir du service de stockage Microsoft Blob Azure, consultez **Sauvegarde et restauration SQL Server avec le service de stockage Microsoft Blob Azure**.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
--To Restore an Entire Database from a Full database backup (a Complete Restore):  
RESTORE DATABASE { database_name | @database_name_var }   
 [ FROM <backup_device> [ ,...n ] ]  
 [ WITH   
   {  
    [ RECOVERY | NORECOVERY | STANDBY =   
        {standby_file_name | @standby_file_name_var }   
       ]  
   | ,  <general_WITH_options> [ ,...n ]  
   | , <replication_WITH_option>  
   | , <change_data_capture_WITH_option>  
   | , <FILESTREAM_WITH_option>  
   | , <service_broker_WITH options>   
   | , \<point_in_time_WITH_options—RESTORE_DATABASE>   
   } [ ,...n ]  
 ]  
[;]  
  
--To perform the first step of the initial restore sequence  
-- of a piecemeal restore:  
RESTORE DATABASE { database_name | @database_name_var }   
   <files_or_filegroups> [ ,...n ]  
 [ FROM <backup_device> [ ,...n ] ]   
   WITH   
      PARTIAL, NORECOVERY   
      [  , <general_WITH_options> [ ,...n ]   
       | , \<point_in_time_WITH_options—RESTORE_DATABASE>   
      ] [ ,...n ]   
[;]  
  
--To Restore Specific Files or Filegroups:   
RESTORE DATABASE { database_name | @database_name_var }   
   <file_or_filegroup> [ ,...n ]  
 [ FROM <backup_device> [ ,...n ] ]   
   WITH   
   {  
      [ RECOVERY | NORECOVERY ]  
      [ , <general_WITH_options> [ ,...n ] ]  
   } [ ,...n ]   
[;]  
  
--To Restore Specific Pages:   
RESTORE DATABASE { database_name | @database_name_var }   
   PAGE = 'file:page [ ,...n ]'   
 [ , <file_or_filegroups> ] [ ,...n ]  
 [ FROM <backup_device> [ ,...n ] ]   
   WITH   
       NORECOVERY     
      [ , <general_WITH_options> [ ,...n ] ]  
[;]  
  
--To Restore a Transaction Log:  
RESTORE LOG { database_name | @database_name_var }  -- Does not apply to SQL Database Managed Instance 
 [ <file_or_filegroup_or_pages> [ ,...n ] ]  
 [ FROM <backup_device> [ ,...n ] ]   
 [ WITH   
   {  
     [ RECOVERY | NORECOVERY | STANDBY =   
        {standby_file_name | @standby_file_name_var }   
       ]  
    | ,  <general_WITH_options> [ ,...n ]  
    | , <replication_WITH_option>  
    | , \<point_in_time_WITH_options—RESTORE_LOG>   
   } [ ,...n ]  
 ]   
[;]  
  
--To Revert a Database to a Database Snapshot:     
RESTORE DATABASE { database_name | @database_name_var }   
FROM DATABASE_SNAPSHOT = database_snapshot_name   
  
<backup_device>::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
 | { DISK    -- Does not apply to SQL Database Managed Instance
     | TAPE  -- Does not apply to SQL Database Managed Instance
     | URL   -- Applies to SQL Server and SQL Database Managed Instance
   } = { 'physical_backup_device_name' |  
      @physical_backup_device_name_var }   
}   
Note: URL is the format used to specify the location and the file name for the Windows Azure Blob. Although Windows Azure storage is a service, the implementation is similar to disk and tape to allow for a consistent and seemless restore experince for all the three devices.  
<files_or_filegroups>::=   
{   
   FILE = { logical_file_name_in_backup | @logical_file_name_in_backup_var }   
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }   
 | READ_WRITE_FILEGROUPS  
}   
  
<general_WITH_options> [ ,...n ]::=   
--Restore Operation Options  
   MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'   
          [ ,...n ]   
 | REPLACE   
 | RESTART   
 | RESTRICTED_USER  | CREDENTIAL  
  
--Backup Set Options  
 | FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }   
 | BLOCKSIZE = { blocksize | @blocksize_variable }   
  
--Data Transfer Options  
 | BUFFERCOUNT = { buffercount | @buffercount_variable }   
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }   
  
--Monitoring Options  
 | STATS [ = percentage ]   
  
--Tape Options. Does not apply to SQL Database Managed Instance
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }   
  
<replication_WITH_option>::=  
 | KEEP_REPLICATION   
  
<change_data_capture_WITH_option>::=  
 | KEEP_CDC  
  
<FILESTREAM_WITH_option>::=  
 | FILESTREAM ( DIRECTORY_NAME = directory_name )  
  
<service_broker_WITH_options>::=   
 | ENABLE_BROKER   
 | ERROR_BROKER_CONVERSATIONS   
 | NEW_BROKER  
  
\<point_in_time_WITH_options—RESTORE_DATABASE>::=   
 | {  
   STOPAT = { 'datetime'| @datetime_var }   
 | STOPATMARK = 'lsn:lsn_number'  
                 [ AFTER 'datetime']   
 | STOPBEFOREMARK = 'lsn:lsn_number'  
                 [ AFTER 'datetime']   
   }   
  
\<point_in_time_WITH_options—RESTORE_LOG>::=   
 | {  
   STOPAT = { 'datetime'| @datetime_var }   
 | STOPATMARK = { 'mark_name' | 'lsn:lsn_number' }  
                 [ AFTER 'datetime']   
 | STOPBEFOREMARK = { 'mark_name' | 'lsn:lsn_number' }  
                 [ AFTER 'datetime']   
   }  
  
```  
  
## <a name="arguments"></a>Arguments  
 Pour une description des arguments, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="about-restore-scenarios"></a>À propos des scénarios de restauration  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un large éventail de scénarios de restauration :  
  
-   restauration de base de données complète  
  
     Restaure l'ensemble de la base de données ; commence par une sauvegarde de base de données complète, qui peut être suivie par la restauration d'une sauvegarde de base de données différentielle (et des sauvegardes du journal). Pour plus d’informations, consultez [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) ou [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).  
  
-   Restauration de fichiers  
  
     Restaure un fichier ou un groupe de fichiers dans une base de données de plusieurs groupes de fichiers. Notez qu'en mode de récupération simple, le fichier doit appartenir à un groupe de fichiers en lecture seule. Une fois que la restauration complète des fichiers a été effectuée, une sauvegarde différentielle des fichiers peut être restaurée. Pour plus d’informations, consultez [Restaurations de fichiers &#40; Mode de récupération complète&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md) et [Restaurations de fichiers &#40;Mode de récupération simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md).  
  
-   Restauration de pages  
  
     Restaure des pages individuelles. La restauration de pages n'est disponible qu'en mode de récupération complète et en mode de récupération utilisant les journaux de transactions. Pour plus d’informations, consultez [Restaurer des pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).  
  
-   Restauration fragmentaire  
  
     Restaure la base de données par étape ; commence avec le groupe de fichiers primaire et se poursuit avec un ou plusieurs des groupes de fichiers secondaires. Une restauration fragmentaire commence par une instruction RESTORE DATABASE utilisant l'option PARTIAL et spécifiant un ou plusieurs groupes de fichiers à restaurer. Pour plus d’informations, consultez [Restaurations fragmentaires &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
-   Récupération uniquement  
  
     Récupère les données déjà cohérentes avec la base de données et qui doivent être disponibles. Pour plus d’informations, consultez [Récupérer une base de données sans restauration des données &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).  
  
-   Restauration du journal des transactions  
  
     En mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, la restauration des sauvegardes du journal est nécessaire pour atteindre le point de récupération souhaité. Pour plus d’informations sur la restauration des sauvegardes de journaux, consultez [Appliquer les sauvegardes du journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
-   Préparer une base de données de disponibilité pour un groupe de disponibilité AlwaysOn  
  
     Pour plus d’informations, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
-   Préparer une base de données miroir pour la mise en miroir de bases de données  
  
     Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
-   Restauration en ligne  
  
    > **REMARQUE :** La restauration en ligne est autorisée uniquement dans l’édition Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Si la restauration en ligne est prise en charge et si la base de données est en ligne, les restaurations de fichiers et de pages s'effectuent automatiquement en ligne, ainsi que les restaurations du groupe de fichiers secondaire une fois la phase initiale de restauration fragmentaire effectuée.  
  
    > **REMARQUE :** Les restaurations en ligne peuvent impliquer des [transactions différées](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).  
  
     Pour plus d’informations, consultez [Restauration en ligne &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
## <a name="additional-considerations-about-restore-options"></a>Considérations supplémentaires à propos des options RESTORE  
  
### <a name="discontinued-restore-keywords"></a>Mots clés RESTORE supprimés  
 Les mots clés suivants ont été supprimés dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] :  
  
|Mot clé supprimé|Remplacé par…|Exemple de mot clé de remplacement|  
|--------------------------|------------------|------------------------------------|  
|LOAD|RESTORE|`RESTORE DATABASE`|  
|TRANSACTION|LOG|`RESTORE LOG`|  
|DBO_ONLY|RESTRICTED_USER|`RESTORE DATABASE ... WITH RESTRICTED_USER`|  
  
### <a name="restore-log"></a>RESTORE LOG  
 RESTORE LOG peut contenir une liste de fichiers pour permettre la création des fichiers lors de la restauration par progression. Elle est utilisée lorsque la sauvegarde du journal contient des enregistrements qui ont été écrits lorsqu'un fichier a été ajouté à la base de données.  
  
> **REMARQUE :** Pour une base de données en mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, vous devez dans la plupart des cas effectuer une sauvegarde de la fin du journal avant de restaurer la base de données. Restaurer une base de données sans effectuer une sauvegarde de la fin du journal au préalable entraîne une erreur, sauf si l'instruction RESTORE DATABASE contient la clause WITH REPLACE ou WITH STOPAT, qui doit spécifier une heure ou une transaction qui s'est produite après la fin de la sauvegarde des données. Pour plus d’informations sur les sauvegardes de la fin du journal, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
### <a name="comparison-of-recovery-and-norecovery"></a>Comparaison entre RECOVERY et NORECOVERY  
 La restauration est contrôlée par l'instruction RESTORE et les options [ RECOVERY | NORECOVERY ] :  
  
-   NORECOVERY indique que la restauration ne s'effectue pas. Cela permet la poursuite de la restauration par progression avec l'instruction suivante de la séquence.  
  
     Dans ce cas, la séquence de restauration restaure d'autres sauvegardes, puis les restaure par progression.  
  
-   RECOVERY (valeur par défaut) indique que la restauration doit être effectuée après la restauration par progression de la sauvegarde actuelle.  
  
     Pour récupérer la base de données, le jeu de données faisant l’objet de la restauration (*jeu de restauration par progression*) doit être entièrement cohérent par rapport à la base de données. Si la restauration par progression du jeu de restauration par progression n'est pas allée assez loin pour être cohérente par rapport à la base de données et si l'option RECOVERY est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur.  
  
## <a name="compatibility-support"></a>Prise en charge de la compatibilité  
 Les sauvegardes des bases de données **master**, **model** et **msdb** créées avec une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées par [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
> **REMARQUE :** Aucune sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut restaurée dans une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à celle à partir de laquelle la sauvegarde a été créée.  
  
 Chaque version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un chemin d'accès par défaut différent de celui des versions antérieures. Par conséquent, pour restaurer une base de données créée à l'emplacement par défaut des sauvegardes de version antérieure, vous devez utiliser l'option MOVE. Pour plus d’informations sur le nouveau chemin par défaut, consultez [Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md).  
  
 Après avoir restauré une base de données de version antérieure dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], la base de données est automatiquement mise à niveau. En général, la base de données est immédiatement disponible. Toutefois, si une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] comprend des index de recherche en texte intégral, le processus de mise à niveau les importe, les réinitialise ou les reconstruit, selon la valeur de la propriété de serveur  **upgrade_option** . Si l’option de mise à niveau a la valeur Importer (**upgrade_option** = 2) ou Reconstruire (**upgrade_option** = 0), les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que lorsque l'option de mise à niveau est Importer, les index de recherche en texte intégral associés sont reconstruits si aucun catalogue de texte intégral n'est disponible. Pour modifier le paramètre de la propriété de serveur **upgrade_option** , utilisez [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).  
  
 Lorsqu'une base de données est attachée ou restaurée pour la première fois à une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une copie de la clé principale de la base de données (chiffrée par la clé principale du service) n'est pas encore stockée sur le serveur. Vous devez utiliser l’instruction **OPEN MASTER KEY** pour déchiffrer la clé principale de la base de données. Une fois la clé principale de la base de données déchiffrée, vous avez la possibilité d’activer le déchiffrement automatique dans le futur en exécutant l’instruction **ALTER MASTER KEY REGENERATE** pour fournir au serveur une copie de la clé principale de la base de données chiffrée avec la clé principale du service. Lorsqu'une base de données a été mise à niveau à partir d'une version antérieure, la clé DMK doit être régénérée de façon à utiliser le nouvel algorithme AES. Pour plus d’informations sur la régénération de la clé DMK, consultez [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md). La durée nécessaire pour régénérer la clé DMK à mettre à niveau vers AES dépend du nombre d'objets protégés par la clé DMK. La régénération de la clé DMK à mettre à niveau vers AES est nécessaire une seule fois et n'a aucune incidence sur les régénérations ultérieures effectuées dans le cadre d'une stratégie de rotation de clés.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Lors d'une restauration hors ligne, si la base de données spécifiée est en cours d'utilisation, RESTORE force la déconnexion des utilisateurs après un court délai. Pour la restauration en ligne d'un groupe de fichiers non primaire, la base de données peut rester active, sauf si le groupe de fichiers restaurés est hors ligne. Toutes les données que la base contient sont remplacées par les données restaurées.  
  
 Pour plus d’informations sur la récupération de base de données, consultez [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
 Les opérations de restauration inter-plateformes, impliquant éventuellement des types de processeurs différents, peuvent être réalisées tant que le classement de la base de données est pris en charge par le système d'exploitation.  
  
 La commande RESTORE peut être redémarrée après une erreur. En outre, vous pouvez contraindre la commande RESTORE à poursuivre son traitement en dépit des erreurs. Elle restaure ainsi le plus de données possible (voir l'option CONTINUE_AFTER_ERROR).  
  
 La commande RESTORE n'est pas autorisée dans une transaction explicite ou implicite.  
  
 La restauration d’une base de données **master** endommagée s’effectue selon une procédure spéciale. Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
 La restauration d'une base de données efface le cache de plan pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. Pour chaque mémoire cache effacée dans le cache de plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d'information suivant : « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a rencontré %d occurrence(s) de vidages de mémoire cache pour la mémoire cache '%s' (partie du cache du plan) en raison d'opérations de maintenance ou de reconfiguration de base de données ». Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.  
  
 Pour restaurer une base de données de disponibilité, restaurez d'abord la base de données en tant qu'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis ajoutez la base de données au groupe de disponibilité.  

## <a name="general-remarks---sql-database-managed-instance"></a>Remarques générales sur SQL Database Managed Instance

Dans le cas d’une restauration asynchrone, la restauration continue même si la connexion cliente s’arrête. Si votre connexion est supprimée, vous pouvez afficher la vue [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) pour vérifier l’état d’une opération de restauration (et d’une opération CREATE ou DROP sur une base de données). 

Les options de base de données suivantes sont définies/remplacées et ne peuvent pas être modifiées ultérieurement :

- NEW_BROKER (si le service Broker n’est pas activé dans le fichier .bak)
- ENABLE_BROKER (si le service Broker n’est pas activé dans le fichier .bak)
- AUTO_CLOSE=OFF (si une base de données dans le fichier .bak est définie avec AUTO_CLOSE=ON)
- RECOVERY FULL (si une base de données dans le fichier .bak est définie avec le mode de récupération SIMPLE ou BULK_LOGGED)
- Un groupe de fichiers à mémoire optimisée, appelé XTP, est ajouté au fichier source .bak s’il n’y est pas déjà. Chaque groupe de fichiers à mémoire optimisée existant est renommé XTP
- Les options SINGLE_USER et RESTRICTED_USER sont remplacées par l’option MULTI_USER

## <a name="limitations---sql-database-managed-instance"></a>Limitations pour SQL Database Managed Instance
Les limitations suivantes s’appliquent :

- Les fichiers .bak contenant plusieurs jeux de sauvegarde ne peuvent pas être restaurés.
- Les fichiers .bak contenant plusieurs fichiers journaux ne peuvent pas être restaurés.
- La restauration échoue si le fichier .bak contient des données FILESTREAM.
- Les sauvegardes contenant des bases de données avec des objets en mémoire active ne peuvent pas être restaurées.
- Les sauvegardes contenant des bases de données avec des objets en mémoire à certains points ne peuvent pas être restaurées.
- Les sauvegardes contenant des bases de données en mode lecture seule ne peuvent pas être restaurées. Cette limitation sera supprimée prochainement.

Pour plus d’informations, consultez [Managed Instance](/azure/sql-database/sql-database-managed-instance)

## <a name="interoperability"></a>Interopérabilité  
  
### <a name="database-settings-and-restoring"></a>Paramètres de bases de données et restauration  
 Lors d'une restauration, les valeurs de la plupart des options de base de données pouvant être définies avec ALTER DATABASE et en vigueur à la fin de la sauvegarde sont rétablies.  
  
 Cependant, l'utilisation de WITH RESTRICTED_USER annule ce comportement pour le paramètre de l'option d'accès utilisateur. Ce paramètre est toujours défini suivant une instruction RESTORE qui inclut l'option WITH RESTRICTED_USER.  
  
### <a name="restoring-an-encrypted-database"></a>Restauration d'une base de données chiffrée  
 Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique qui a servi à chiffrer la base de données. Sans le certificat et la clé asymétrique, la base de données ne peut pas être restaurée. En conséquence, le certificat utilisé pour chiffrer la clé de chiffrement de base de données doit être conservé tant que la sauvegarde est utile. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
### <a name="restoring-a-database-enabled-for-vardecimal-storage"></a>Restauration d'une base de données activée pour le stockage vardecimal  
 Les sauvegardes et les restaurations fonctionnent correctement avec le format de stockage **vardecimal**. Pour plus d’informations sur le format de stockage **vardecimal**, consultez [sp_db_vardecimal_storage_format &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).  
  
### <a name="restore-full-text-data"></a>Restauration de données de texte intégral  
 Les données de texte intégral sont restaurées avec d'autres données de base de données lors d'une restauration complète. La syntaxe régulière `RESTORE DATABASE database_name FROM backup_device` permet de restaurer les fichiers de texte intégral comme des éléments faisant partie intégrante de la restauration de fichiers de base de données.  
  
 L'instruction RESTORE permet également d'effectuer des restaurations dans d'autres emplacements, des restaurations différentielles, des restaurations de fichiers et de groupes de fichiers, ainsi que des restaurations de fichiers et groupes de fichiers différentielles de données de texte intégral. En outre, cette instruction permet de restaurer des fichiers de texte intégral uniquement, ainsi que des données de base de données.  
  
> **REMARQUE :** Les catalogues de texte intégral importés à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sont toujours traités comme fichiers de base de données. Pour ceux-ci, la procédure [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] de sauvegarde des catalogues de texte intégral reste applicable, mais la suspension et la reprise en cours de sauvegarde ne sont plus nécessaires. Pour plus d’informations, consultez [Sauvegarde et restauration de catalogues de texte intégral](http://go.microsoft.com/fwlink/?LinkId=107381).  
  
## <a name="metadata"></a>Métadonnées  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intègre des tables d'historique de sauvegarde et de restauration qui assurent le suivi des activités de sauvegarde et de restauration pour chaque instance de serveur. Lorsqu'une restauration est effectuée, les tables d'historique de sauvegarde sont également modifiées. Pour plus d’informations sur ces tables, consultez [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md).  
  
##  <a name="REPLACEoption"></a> Impact de l’option REPLACE  
 L'option REPLACE ne doit être utilisée que rarement et après un examen attentif. Généralement, la restauration empêche le remplacement accidentel d'une base de données par une autre. Si la base de données nommée dans l'instruction RESTORE existe déjà sur le serveur actif et que le GUID de famille de la base de données spécifié ne correspond pas à celui qui est enregistré dans le jeu de sauvegarde, la base de données n'est pas restaurée. Cette mesure de sécurité est très importante,  
  
 L'option REPLACE annule d'importants contrôles de sécurité normalement effectués lors d'une restauration. Ces contrôles ignorés sont les suivants :  
  
-   Restauration par remplacement d'une base de données existante par une sauvegarde d'une autre base de données.  
  
     Avec l'option REPLACE, la restauration vous permet de remplacer une base de données existante par la base de données figurant dans le jeu de sauvegarde, même si le nom de base de données spécifié diffère de celui enregistré dans le jeu en question. Cela peut entraîner un remplacement accidentel d'une base de données par une autre.  
  
-   Restauration par remplacement d'une base de données à l'aide du mode de restauration complète ou du mode de récupération utilisant les journaux de transactions quand une sauvegarde de la fin du journal n'a pas été réalisée et quand l'option STOPAT n'est pas employée.  
  
     Avec l'option REPLACE, vous pouvez perdre des transactions déjà validées du fait que le dernier journal écrit n'a pas été sauvegardé.  
  
-   Remplacement des fichiers existants  
  
     Par exemple, une erreur peut entraîner le remplacement des fichiers existants d'un type incorrect (par exemple, fichiers .xls) ou de fichiers actuellement utilisés par une autre base de données qui n'est pas en ligne. Une perte de données arbitraire peut avoir lieu si des fichiers existants sont remplacés, même si la base de données restaurée est complète.  
  
## <a name="redoing-a-restore"></a>Rétablir une restauration  
 Il n'est pas possible d'annuler les effets d'une restauration. Cependant, vous pouvez annuler les effets de la copie et de la restauration par progression des données en recommençant fichier par fichier. Pour recommencer, restaurez le fichier approprié et réeffectuez la restauration par progression. Par exemple, si vous avez accidentellement restauré trop de sauvegardes du journal et dépassé le point d'arrêt voulu, redémarrez la séquence.  
  
 Une séquence de restauration peut être annulée et redémarrée en restaurant l'ensemble du contenu des fichiers concernés.  
  
## <a name="reverting-a-database-to-a-database-snapshot"></a>Restauration d'une base de données vers un instantané de base de données  
 Une *opération de rétablissement de base de données* (spécifiée avec l’option DATABASE_SNAPSHOT) ramène une base de données source complète à un moment donné dans le temps, en rétablissant l’instantané de base de données à cet instant-là, c’est-à-dire en remplaçant la base de données source par les données correspondant à une date et heure précises dans l’instantané de base de données spécifié. Seul l'instantané vers lequel vous effectuez le rétablissement peut exister. Cette opération reconstruit ensuite le journal (par conséquent, vous ne pouvez pas effectuer une restauration par progression d'une base de données rétablie au point d'erreur de l'utilisateur).  
  
 La perte de données est limitée aux mises à jour de la base de données depuis la création de l'instantané. Les métadonnées d'une base de données rétablie sont identiques aux métadonnées au moment de la création de l'instantané. Toutefois, le retour à un instantané supprime tous les catalogues de texte intégral.  
  
 Le rétablissement à partir d'un instantané de base de données n'est pas destiné à la récupération des supports. Contrairement à un jeu de sauvegarde classique, l'instantané de base de données est une copie incomplète des fichiers de la base de données. Si la base de données ou l'instantané de base de données est endommagé, le rétablissement à partir d'un instantané sera probablement impossible. En outre, même lorsque le rétablissement est possible, il est peu probable qu'il permette de corriger le problème en cas de dommages.  
  
### <a name="restrictions-on-reverting"></a>Restrictions liées au rétablissement  
 Le rétablissement n'est pas pris en charge dans les conditions suivantes :  
  
-   La base de données source contient des groupes de fichiers en lecture seule ou compressés.  
  
-   Des fichiers sont hors connexion alors qu'ils étaient en ligne lors de la création de l'instantané.  
  
-   Il existe plus d'un instantané de la base de données.  
  
 Pour plus d’informations, consultez [Rétablir une base de données dans l’état d’un instantané de base de données](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).  
  
## <a name="security"></a>Sécurité  
 Une opération de sauvegarde peut éventuellement spécifier des mots de passe pour un support de sauvegarde, un jeu de sauvegarde ou les deux. Lorsqu'un mot de passe a été défini sur un support de sauvegarde ou un jeu de sauvegarde, vous devez entrer le ou les mots de passe corrects dans l'instruction RESTORE. Ces mots de passe empêchent les opérations non autorisées de restauration et d'ajouts de jeux de sauvegarde au support à l'aide d'outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cependant, les supports protégés par un mot de passe peuvent être remplacés par l'option FORMAT de l'instruction BACKUP.  
  
> [!IMPORTANT]  
>  La protection assurée par ce mot de passe est plutôt faible. Son but est d'éviter que des utilisateurs autorisés ou non autorisés effectuent une restauration incorrecte à l'aide des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En aucun cas, elle n'empêche la lecture des données de la sauvegarde par d'autres moyens ou le remplacement du mot de passe. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]La bonne pratique en matière de protection des sauvegardes consiste à stocker les bandes de sauvegarde dans un emplacement sûr ou à sauvegarder les fichiers disque protégés par une liste de contrôle d’accès (ACL). La liste de contrôle d'accès doit être définie à la racine du répertoire dans lequel les sauvegardes sont effectuées.  
>   
>  Pour obtenir des informations spécifiques sur la sauvegarde et la restauration SQL Server avec le service de stockage Microsoft Blob Azure, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Microsoft Blob Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
### <a name="permissions"></a>Autorisations  
 Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations CREATE DATABASE afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe **sysadmin** et **dbcreator** et au propriétaire (**dbo**) de la base de données (pour l’option FROM DATABASE_SNAPSHOT, la base de données existe toujours).  
  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="examples"></a> Exemples  
 Tous les exemples partent du principe qu'une sauvegarde complète de la base de données a été effectuée.  
  
 Parmi les exemples d'instruction RESTORE, citons :  
  
-   A. [Restauration d’une base de données complète](#restoring_full_db)  
  
-   B. [Restauration de sauvegardes complètes et différentielles d’une base de données](#restoring_full_n_differential_db_backups)  
  
-   C. [Restauration d’une base de données en utilisant la syntaxe RESTART](#restoring_db_using_RESTART)  
  
-   D. [Restauration d’une base de données et déplacement des fichiers](#restoring_db_n_move_files)  
  
-   E. [Copie d’une base de données en utilisant BACKUP et RESTORE](#copying_db_using_bnr)  
  
-   F. [Restauration jusqu'à une date et heure en utilisant STOPAT](#restoring_to_pit_using_STOPAT)  
  
-   G. [Restauration du journal des transactions jusqu’à une marque](#restoring_transaction_log_to_mark)  
  
-   H. [Restauration en utilisant la syntaxe TAPE](#restoring_using_TAPE)  
  
-   I. [Restauration en utilisant la syntaxe FILE et FILEGROUP](#restoring_using_FILE_n_FG)  
  
-   J. [Rétablissement à partir d’un instantané de base de données](#reverting_from_db_snapshot)  
  
-   K. [Restauration à partir du service de stockage Microsoft Blob Azure](#Azure_Blob)  
  
> **REMARQUE :** Pour obtenir d’autres exemples, consultez les rubriques de procédures de restauration répertoriées dans [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
###  <a name="restoring_full_db"></a> A. Restauration d’une base de données complète  
 L'exemple suivant restaure une sauvegarde de base de données complète à partir de l'unité de sauvegarde logique `AdventureWorksBackups`. Pour obtenir un exemple de création de cette unité, consultez [Unités de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
```  
RESTORE DATABASE AdventureWorks2012   
   FROM AdventureWorks2012Backups;  
```  
  
> **REMARQUE :** Pour une base de données employant le mode de récupération complète ou utilisant les journaux de transactions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exige dans la plupart des cas que vous sauvegardiez la fin du journal avant de restaurer la base de données. Pour plus d’informations, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="restoring_full_n_differential_db_backups"></a> B. Restauration de sauvegardes complètes et différentielles d'une base de données  
 L'exemple suivant restaure une sauvegarde de base de données complète, puis une sauvegarde différentielle, à partir de l'unité de sauvegarde `Z:\SQLServerBackups\AdventureWorks2012.bak` qui contient les deux sauvegardes. La sauvegarde de base de données complète à restaurer correspond au sixième jeu de sauvegarde se trouvant sur l'unité (`FILE = 6`), tandis que la sauvegarde de base de données différentielle correspond au neuvième jeu de sauvegarde se trouvant sur l'unité (`FILE = 9`). Une fois la sauvegarde différentielle récupérée, la récupération de la base de données est terminée.  
  
```  
RESTORE DATABASE AdventureWorks2012  
   FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'  
   WITH FILE = 6  
      NORECOVERY;  
RESTORE DATABASE AdventureWorks2012  
   FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'  
   WITH FILE = 9  
      RECOVERY;  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="restoring_db_using_RESTART"></a> C. Restauration d'une base de données en utilisant la syntaxe RESTART  
 Dans l'exemple suivant, l'option `RESTART` est utilisée pour redémarrer une opération `RESTORE` interrompue par une coupure de courant sur le serveur.  
  
```  
-- This database RESTORE halted prematurely due to power failure.  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorksBackups;  
-- Here is the RESTORE RESTART operation.  
RESTORE DATABASE AdventureWorks2012   
   FROM AdventureWorksBackups WITH RESTART;  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="restoring_db_n_move_files"></a> D. Restauration d'une base de données et déplacement des fichiers  
 L'exemple suivant restaure l'intégralité d'une base de données et de son journal des transactions, puis déplace la base de données restaurée vers le répertoire `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`.  
  
```  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorksBackups  
   WITH NORECOVERY,   
      MOVE 'AdventureWorks2012_Data' TO   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.mdf',   
      MOVE 'AdventureWorks2012_Log'   
TO 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.ldf';  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorksBackups  
   WITH RECOVERY;  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="copying_db_using_bnr"></a> E. Copie d'une base de données en utilisant BACKUP et RESTORE  
 L'exemple suivant utilise à la fois les instructions `BACKUP` et `RESTORE` pour effectuer une copie de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. L'instruction `MOVE` entraîne la restauration des données et du fichier journal aux emplacements spécifiés. L'instruction `RESTORE FILELISTONLY` permet de déterminer le nombre et le nom des fichiers de la base de données en cours de restauration. La nouvelle copie de la base de données se nomme `TestDB`. Pour plus d’informations, consultez [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
```  
BACKUP DATABASE AdventureWorks2012   
   TO AdventureWorksBackups ;  
  
RESTORE FILELISTONLY   
   FROM AdventureWorksBackups ;  
  
RESTORE DATABASE TestDB   
   FROM AdventureWorksBackups   
   WITH MOVE 'AdventureWorks2012_Data' TO 'C:\MySQLServer\testdb.mdf',  
   MOVE 'AdventureWorks2012_Log' TO 'C:\MySQLServer\testdb.ldf';  
GO  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="restoring_to_pit_using_STOPAT"></a> F. Restauration jusqu'à une date et heure en utilisant STOPAT  
 L'exemple suivant restaure une base de données dans l'état où elle se trouvait le `12:00 AM` à `April 15, 2020` et décrit une opération de restauration impliquant plusieurs sauvegardes de fichiers journaux. Sur l'unité de sauvegarde `AdventureWorksBackups`, la sauvegarde de base de données complète à restaurer correspond au troisième jeu de sauvegarde (`FILE = 3`), la première sauvegarde de fichier journal correspond au quatrième jeu de sauvegarde (`FILE = 4`) et la seconde sauvegarde de fichier journal correspond au cinquième jeu de sauvegarde (`FILE = 5`).  
  
```  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorksBackups  
   WITH FILE=3, NORECOVERY;  
  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorksBackups  
   WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorksBackups  
   WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
RESTORE DATABASE AdventureWorks2012 WITH RECOVERY;  
  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="restoring_transaction_log_to_mark"></a> G. Restauration du journal des transactions jusqu'à une marque  
 L'exemple suivant restaure le journal des transactions jusqu'à la marque dans la transaction marquée nommée `ListPriceUpdate`.  
  
```  
USE AdventureWorks2012  
GO  
BEGIN TRANSACTION ListPriceUpdate  
   WITH MARK 'UPDATE Product list prices';  
GO  
  
UPDATE Production.Product  
   SET ListPrice = ListPrice * 1.10  
   WHERE ProductNumber LIKE 'BK-%';  
GO  
  
COMMIT TRANSACTION ListPriceUpdate;  
GO  
  
-- Time passes. Regular database   
-- and log backups are taken.  
-- An error occurs in the database.  
USE master;  
GO  
  
RESTORE DATABASE AdventureWorks2012  
FROM AdventureWorksBackups  
WITH FILE = 3, NORECOVERY;  
GO  
  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorksBackups   
   WITH FILE = 4,  
   RECOVERY,   
   STOPATMARK = 'UPDATE Product list prices';  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="restoring_using_TAPE"></a> H. Restauration en utilisant la syntaxe TAPE  
 L'exemple suivant restaure une sauvegarde de base de données complète à partir d'une unité de sauvegarde `TAPE`.  
  
```  
RESTORE DATABASE AdventureWorks2012   
   FROM TAPE = '\\.\tape0';  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="restoring_using_FILE_n_FG"></a> I. Restauration en utilisant la syntaxe FILE et FILEGROUP  
 L'exemple suivant restaure une base de données nommée `MyDatabase` qui est composée de deux fichiers, d'un groupe de fichiers secondaire et d'un journal des transactions. La base de données utilise le mode de récupération complète.  
  
 La sauvegarde de la base de données est le neuvième jeu de sauvegarde dans le support de sauvegarde sur une unité de sauvegarde logique nommée `MyDatabaseBackups`. Trois sauvegardes de fichier journal, qui se trouvent dans les trois jeux de sauvegarde suivants (`10`, `11` et `12`) sur l'unité `MyDatabaseBackups` sont ensuite restaurés à l'aide de `WITH NORECOVERY`. Une fois restaurée la dernière sauvegarde de fichier journal, la base de données est récupérée.  
  
> **REMARQUE :** La récupération est réalisée séparément afin d’éviter qu’elle n’ait lieu trop tôt, c’est-à-dire avant que toutes les sauvegardes de fichier journal n’aient été restaurées.  
  
 Dans l'instruction `RESTORE DATABASE`, il existe deux types d'options `FILE`. Les options `FILE` qui précèdent le nom de l'unité de sauvegarde spécifient les noms de fichiers logiques des fichiers de base de données à restaurer à partir du jeu de sauvegarde, par exemple `FILE = 'MyDatabase_data_1'`. Ce jeu de sauvegarde n'est pas la première sauvegarde de base de données dans le support de sauvegarde. Par conséquent, sa position dans le support de sauvegarde est indiquée via l'option `FILE` dans la clause `WITH`, en l'occurrence `FILE=9`.  
  
```  
RESTORE DATABASE MyDatabase  
   FILE = 'MyDatabase_data_1',  
   FILE = 'MyDatabase_data_2',  
   FILEGROUP = 'new_customers'  
   FROM MyDatabaseBackups  
   WITH   
      FILE = 9,  
      NORECOVERY;  
GO  
-- Restore the log backups.  
RESTORE LOG MyDatabase  
   FROM MyDatabaseBackups  
   WITH FILE = 10,   
      NORECOVERY;  
GO  
RESTORE LOG MyDatabase  
   FROM MyDatabaseBackups  
   WITH FILE = 11,   
      NORECOVERY;  
GO  
RESTORE LOG MyDatabase  
   FROM MyDatabaseBackups  
   WITH FILE = 12,   
      NORECOVERY;  
GO  
--Recover the database:  
RESTORE DATABASE MyDatabase WITH RECOVERY;  
GO  
```  
  
 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="reverting_from_db_snapshot"></a> J. Rétablissement à partir d'un instantané de base de données  
 L'exemple suivant rétablit un instantané de base de données. Il part du principe qu'un seul instantané existe actuellement dans la base de données. Pour obtenir un exemple de la façon de créer cet instantané de base de données, consultez [Créer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).  
  
> **REMARQUE :** Le rétablissement d’un instantané a pour effet de supprimer tous les catalogues de texte intégral.  
  
```  
USE master;    
RESTORE DATABASE AdventureWorks2012 FROM DATABASE_SNAPSHOT = 'AdventureWorks_dbss1800';  
GO  
```  
 Pour plus d’informations, consultez [Rétablir une base de données dans l’état d’un instantané de base de données](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).  

 [&#91;Début des exemples&#93;](#examples)  
  
###  <a name="Azure_Blob"></a> K. Restauration à partir du service de stockage Microsoft Blob Azure  
Les trois exemples ci-dessous impliquent l’utilisation du service de stockage Microsoft Azure.  Le nom du compte de stockage est `mystorageaccount`.  Le conteneur des fichiers de données est appelé `myfirstcontainer`.  Le conteneur des fichiers de sauvegarde est appelé `mysecondcontainer`.  Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture, suppression et liste pour chaque conteneur.  Des informations d’identification SQL Server ont été créées en utilisant des signatures d’accès partagé associées aux stratégies d’accès stockées.  Pour obtenir des informations spécifiques sur la sauvegarde et la restauration SQL Server avec le service de stockage Microsoft Blob Azure, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Microsoft Blob Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  

**K1.  Restaurer une sauvegarde complète de base de données à partir du service de stockage Microsoft Azure**  
Une sauvegarde complète de base de données située dans le conteneur `mysecondcontainer` de `Sales` sera restaurée dans `myfirstcontainer`.  `Sales` n’existe actuellement pas sur le serveur. 
```
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'   
  WITH  MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf', 
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf', 
  STATS = 10;
```

**K2. Restaurer une sauvegarde complète de base de données du service de stockage Microsoft Azure vers un stockage local**  
Une sauvegarde complète de base de données située dans le conteneur `mysecondcontainer` de `Sales` sera restaurée dans le stockage local.  `Sales` n’existe actuellement pas sur le serveur.
```
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'   
  WITH  MOVE 'Sales_Data1' to 'H:\DATA\Sales_Data1.mdf', 
  MOVE 'Sales_log' to 'O:\LOG\Sales_log.ldf', 
  STATS = 10;
```
  
**K3. Restaurer une sauvegarde complète de base de données du stockage local vers le service de stockage Microsoft Azure**  
```
RESTORE DATABASE Sales
  FROM DISK = 'E:\BAK\Sales.bak'
  WITH  MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf', 
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf', 
  STATS = 10;
```  
  

  
 [&#91;Début des exemples&#93;](#examples)  
  
## <a name="much-more-information"></a>Informations complémentaires  
 - [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) 
- [Sauvegarder et restaurer des bases de données système (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md) 
 - [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)
 - [Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 - [Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 - [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 - [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 - [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 - [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 - [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)  
 - [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
 - [Historique de sauvegarde et informations d’en-tête &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
