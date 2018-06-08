---
title: jeu de sauvegarde (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- backupset
- backupset_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backupset system table
- backup media [SQL Server], backupset system table
- backup sets [SQL Server]
ms.assetid: 6ff79bbf-4acf-4f75-926f-38637ca8a943
caps.latest.revision: 70
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1089a1a30e50b389b05c79cf9af2443f7dc55d6e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="backupset-transact-sql"></a>backupset (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-pdw-md.md)]

  Contient une ligne pour chaque jeu de sauvegarde. Un *jeu de sauvegarde* contient la sauvegarde issue d’une opération de sauvegarde réussie unique. Les instructions RESTORE, RESTORE FILELISTONLY, RESTORE HEADERONLY et RESTORE VERIFYONLY fonctionnent sur un jeu de sauvegarde unique dans le jeu de médias sur la ou les unités de sauvegarde spécifiées.  
  
 Cette table est stockée dans le **msdb** base de données.  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**backup_set_id**|**int**|Numéro d'identification unique du jeu de sauvegardes Identité, clé primaire.|  
|**backup_set_uuid**|**uniqueidentifier**|Numéro d'identification unique du jeu de sauvegardes|  
|**media_set_id**|**int**|Numéro d'identification unique du support de sauvegarde contenant le jeu de sauvegardes. Références **backupmediaset (media_set_id)**.|  
|**first_family_number**|**tinyint**|Numéro de famille du support qui est le premier du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**first_media_number**|**smallint**|Numéro du support qui est le premier du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**last_family_number**|**tinyint**|Numéro de famille du support qui est le dernier du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**last_media_number**|**smallint**|Numéro du support qui est le dernier du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**catalog_family_number**|**tinyint**|Numéro de famille du support contenant le début du répertoire du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**catalog_media_number**|**smallint**|Numéro du support de sauvegardes contenant le début du répertoire du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**position**|**int**|Position du jeu de sauvegardes utilisée lors de la restauration pour localiser les fichiers et le jeu de sauvegardes appropriés. Sa valeur peut être NULL. Pour plus d’informations, consultez le fichier dans [sauvegarde &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).|  
|**expiration_date**|**datetime**|Date et heure d'expiration du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**software_vendor_id**|**int**|Numéro d'identification du fournisseur du logiciel qui a écrit l'en-tête de support de sauvegardes Sa valeur peut être NULL.|  
|**nom**|**nvarchar(128)**|Nom du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**description**|**nvarchar(255)**|Description du jeu de sauvegardes. Sa valeur peut être NULL.|  
|**user_name**|**nvarchar(128)**|Nom de l'utilisateur effectuant la sauvegarde Sa valeur peut être NULL.|  
|**software_major_version**|**tinyint**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Numéro de version principale. Sa valeur peut être NULL.|  
|**software_minor_version**|**tinyint**|Numéro de version secondaire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sa valeur peut être NULL.|  
|**software_build_version**|**smallint**|Numéro de build de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sa valeur peut être NULL.|  
|**time_zone**|**smallint**|Différence entre l'heure locale (lieu où l'opération de sauvegarde se déroule) et le temps universel UTC, exprimée en intervalles de 15 minutes. Les valeurs peuvent être comprises entre - 48 et + 48 incluses. La valeur 127 signifie inconnu. Par exemple, -20 correspond à l'heure de l'Est (USA) soit 5 heures après l'heure universelle UTC. Sa valeur peut être NULL.|  
|**mtf_minor_version**|**tinyint**|Numéro de la version mineure de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Tape Format Sa valeur peut être NULL.|  
|**first_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant au premier enregistrement ou à l'enregistrement le plus ancien du journal dans le jeu de sauvegardes Sa valeur peut être NULL.|  
|**last_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant à l'enregistrement du journal suivant après le jeu de sauvegarde. Sa valeur peut être NULL.|  
|**checkpoint_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant à l'enregistrement à partir duquel le rétablissement doit démarrer. Sa valeur peut être NULL.|  
|**database_backup_lsn**|**numeric(25,0)**|Numéro séquentiel dans le journal correspondant à la sauvegarde complète la plus récente de la base de données. Sa valeur peut être NULL.<br /><br /> **database_backup_lsn** est le « début du point de contrôle » déclenché lors du démarrage de la sauvegarde. Ce LSN coïncide avec **first_lsn** si la sauvegarde est effectuée lorsque la base de données est inactive et aucune réplication n’est configurée.|  
|**database_creation_date**|**datetime**|Date et heure de création de la base de données. Sa valeur peut être NULL.|  
|**backup_start_date**|**datetime**|Date et heure de début de la sauvegarde Sa valeur peut être NULL.|  
|**backup_finish_date**|**datetime**|Date et heure de fin de la sauvegarde Sa valeur peut être NULL.|  
|**type**|**char(1)**|Type de sauvegarde. Valeurs possibles :<br /><br /> D = Base de données<br /><br /> I = Base de données différentielle<br /><br /> L = Journal<br /><br /> F = Fichier ou groupe de fichiers<br /><br /> G =Fichier différentiel<br /><br /> P = Partiel<br /><br /> Q = Partielle différentielle<br /><br /> Sa valeur peut être NULL.|  
|**sort_order**|**smallint**|Ordre de tri utilisé par le serveur effectuant la sauvegarde. Sa valeur peut être NULL. Pour plus d’informations sur les ordres de tri et les classements, consultez [prise en charge Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**code_page**|**smallint**|Page de codes du serveur effectuant la sauvegarde. Sa valeur peut être NULL. Pour plus d’informations sur les pages de codes, consultez [prise en charge Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).|  
|**compatibility_level**|**tinyint**|Paramètres définissant le niveau de compatibilité de la base de données. Valeurs possibles :<br /><br /> 90 = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> 100 = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]<br /><br /> 110 = [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]<br /><br /> 120 = [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]<br /><br /> Sa valeur peut être NULL.<br /><br /> Pour plus d’informations sur les niveaux de compatibilité, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|  
|**database_version**|**int**|Numéro de version de la base de données. Sa valeur peut être NULL.|  
|**backup_size**|**numeric(20,0)**|Taille du jeu de sauvegardes en octets. Sa valeur peut être NULL. Pour les sauvegardes VSS, backup_size est une valeur estimée.|  
|**database_name**|**nvarchar(128)**|Nom de la base de données impliquée dans la sauvegarde. Sa valeur peut être NULL.|  
|**server_name**|**nvarchar(128)**|Nom du serveur exécutant la sauvegarde de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sa valeur peut être NULL.|  
|**machine_name**|**nvarchar(128)**|Nom de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]est exécuté. Sa valeur peut être NULL.|  
|**flags**|**int**|Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le **indicateurs** colonne a été déconseillée et est remplacée par les colonnes de bits suivantes :<br /><br /> **has_bulk_logged_data** <br /> **is_snapshot** <br /> **is_readonly** <br /> **is_single_user** <br /> **has_backup_checksums** <br /> **is_damaged** <br /> **begins_log_chain** <br /> **has_incomplete_metadata** <br /> **is_force_offline** <br /> **is_copy_only**<br /><br /> Sa valeur peut être NULL.<br /><br /> Dans des jeux de sauvegarde à partir de versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], bits d'indicateur :<br />1 = La sauvegarde contient des données consignées de façon minimale. <br />2 = WITH SNAPSHOT a été utilisé. <br />4 = La base de données était accessible en lecture seule au moment de la sauvegarde.<br />8 = La base de données était en mode mono-utilisateur au moment de la sauvegarde.|  
|**unicode_locale**|**int**|Paramètres régionaux Unicode. Sa valeur peut être NULL.|  
|**unicode_compare_style**|**int**|Style de comparaison Unicode. Sa valeur peut être NULL.|  
|**collation_name**|**nvarchar(128)**|Nom du classement. Sa valeur peut être NULL.|  
|**Is_password_protected**|**bit**|Jeu de sauvegardes<br /><br /> protégé par mot de passe :<br /><br /> 0 = Non protégé<br /><br /> 1 = Protégé|  
|**recovery_model**|**nvarchar(60)**|Mode de récupération de la base de données :<br /><br /> FULL<br /><br /> BULK-LOGGED<br /><br /> SIMPLE|  
|**has_bulk_logged_data**|**bit**|1 = La sauvegarde contient des données journalisées en bloc.|  
|**is_snapshot**|**bit**|1 = La sauvegarde a été réalisée à l'aide de l'option SNAPSHOT.|  
|**is_readonly**|**bit**|1 = La base de données était accessible en lecture seule au moment de la sauvegarde.|  
|**is_single_user**|**bit**|1 = La base de données était en mode mono-utilisateur lors de la sauvegarde.|  
|**has_backup_checksums**|**bit**|1 = La sauvegarde contient des sommes de contrôle de sauvegarde.|  
|**is_damaged**|**bit**|1 = Des dommages ont été détectés pour la base de données lors de la création de cette sauvegarde. L'opération de sauvegarde a dû continuer malgré les erreurs.|  
|**begins_log_chain**|**bit**|1 = Il s'agit de la première d'une chaîne continue de sauvegardes journalisées. Une séquence de journaux démarre par la première sauvegarde journalisée effectuée après la création de la base de données, ou lorsqu'elle passe du mode de récupération simple à complète ou utilisant les journaux de transactions.|  
|**has_incomplete_metadata**|**bit**|1 = Sauvegarde de la fin du journal avec des métadonnées incomplètes. Pour plus d’informations, consultez [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).|  
|**is_force_offline**|**bit**|1 = La base de données est passée en mode hors connexion à l'aide de l'option NORECOVERY lors de la sauvegarde.|  
|**is_copy_only**|**bit**|1 = Sauvegarde de copie unique. Pour plus d’informations, consultez [Sauvegardes de copie uniquement &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).|  
|**first_recovery_fork_guid**|**uniqueidentifier**|ID du branchement de récupération de début. Cela correspond à **FirstRecoveryForkID** de RESTORE HEADERONLY.<br /><br /> Pour les sauvegardes de données, **first_recovery_fork_guid** est égal à **last_recovery_fork_guid**.|  
|**last_recovery_fork_guid**|**uniqueidentifier**|ID du branchement de récupération de fin. Cela correspond à **RecoveryForkID** de RESTORE HEADERONLY.<br /><br /> Pour les sauvegardes de données, **first_recovery_fork_guid** est égal à **last_recovery_fork_guid**.|  
|**fork_point_lsn**|**numeric(25,0)**|Si **first_recovery_fork_guid** n’est pas égal à **last_recovery_fork_guid**, il s’agit du numéro de séquence de journal du point du branchement. Dans le cas contraire, la valeur est NULL.|  
|**database_guid**|**uniqueidentifier**|ID unique de la base de données. Cela correspond à **BindingID** de RESTORE HEADERONLY. Lors de la restauration de la base de données, une nouvelle valeur est attribuée.|  
|**family_guid**|**uniqueidentifier**|ID unique de la base de données d'origine lors de sa création. Cette valeur demeure identique lors de la restauration de la base de données, même sous un nom différent.|  
|**differential_base_lsn**|**numeric(25,0)**|Numéro de séquence d'enregistrement de base pour les sauvegardes différentielles. Pour une sauvegarde différentielle unique ; modifications avec un LSN supérieurs ou égales à **differential_base_lsn** sont inclus dans la sauvegarde différentielle.<br /><br /> Pour une sauvegarde différentielle multiple, la valeur est NULL et la base de LSN doit être déterminée au niveau du fichier (voir [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)).<br /><br /> Pour les types de sauvegarde non différentiels, la valeur est toujours NULL.|  
|**differential_base_guid**|**uniqueidentifier**|Pour une sauvegarde différentielle unique, cette valeur constitue l'identificateur unique de la base différentielle.<br /><br /> Pour les sauvegardes différentielles multiples, cette valeur est NULL, tandis que la base différentielle doit être déterminée au niveau du fichier.<br /><br /> Pour les types de sauvegarde non différentiels, la valeur est NULL.|  
|**compressed_backup_size**|**Numeric(20,0)**|Nombre total d'octets de la sauvegarde stocké sur le disque.<br /><br /> Pour calculer le taux de compression, utilisez **compressed_backup_size** et **backup_size**.<br /><br /> Pendant une **msdb** mise à niveau, cette valeur est définie sur NULL. ce qui indique une sauvegarde non compressée.|  
|**key_algorithm**|**nvarchar(32)**|Algorithme de chiffrement utilisé pour chiffrer la sauvegarde. La valeur NO_Encryption indique que la sauvegarde n'est pas chiffrée.|  
|**encryptor_thumbprint**|**varbinary(20)**|Empreinte numérique du chiffreur pouvant être utilisé pour rechercher un certificat ou la clé asymétrique dans la base de données. Si la sauvegarde n'est pas chiffrée, cette valeur est NULL.|  
|**encryptor_type**|**nvarchar(32)**|Type de chiffreur utilisé : certificat ou clé asymétrique . Si la sauvegarde n'est pas chiffrée, cette valeur est NULL.|  
  
## <a name="remarks"></a>Notes  
 RESTORE VERIFYONLY FROM *backup_device* WITH LOADHISTORY remplit la colonne de la **backupmediaset** table avec les valeurs appropriées de l’en-tête du support de sauvegarde.  
  
 Pour réduire le nombre de lignes dans cette table et dans d’autres tables d’historique de sauvegarde et, exécutez le [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) procédure stockée.  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde et restauration des Tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [Sauvegarde et restauration des Tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)  
  
  
