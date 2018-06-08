---
title: BACKUP (Transact-SQL) | Microsoft Docs
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
- BACKUP_TSQL
- BACKUP
- BACKUP_DATABASE_TSQL
- BACKUP_LOG_TSQL
- BACKUP LOG
- BACKUP DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], BACKUP statement
- backing up filegroups [SQL Server]
- backup file formats [SQL Server]
- backups [SQL Server]
- database backups [SQL Server], BACKUP statement
- multifamily media sets [SQL Server]
- mirrored media sets [SQL Server]
- backing up files [SQL Server]
- backup sets [SQL Server], BACKUP statement
- backup compression [SQL Server], BACKUP statement
- BACKUP statement
- backups [SQL Server], BACKUP statement
- backup history tables [SQL Server]
- transaction log backups [SQL Server], BACKUP LOG statement
- READ_WRITE_FILEGROUPS option
- BACKUP DATABASE statement
- concurrency [SQL Server], backups
- backing up databases [SQL Server]
- passwords [SQL Server], backups
- security [SQL Server], backups
- media families [SQL Server]
- BACKUP LOG statement
- backing up transaction logs [SQL Server]
- stripe sets [SQL Server]
- cross-platform backups
ms.assetid: 89a4658a-62f1-4289-8982-f072229720a1
caps.latest.revision: 275
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3c7d97d9c8ee56af89807f07cd335b16c50fbcc1
ms.sourcegitcommit: 02c889a1544b0859c8049827878d66b2301315f8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="backup-transact-sql"></a>BACKUP (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Sauvegarde une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] complète pour créer une sauvegarde de la base de données, ou un ou plusieurs fichiers ou groupes de fichiers de la base de données pour créer une sauvegarde de fichiers (BACKUP DATABASE). De plus, en mode de restauration complète ou en mode de récupération utilisant les journaux de transactions, sauvegarde le journal des transactions de la base de données afin de créer une sauvegarde de journal (BACKUP LOG). 

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
``` 
Backing Up a Whole Database   
BACKUP DATABASE { database_name | @database_name_var }   
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { DIFFERENTIAL -- Not supporterd in SQL Database Managed Instance
           | <general_WITH_options> [ ,...n ] } ]  
[;]  
  
Backing Up Specific Files or Filegroups  
BACKUP DATABASE { database_name | @database_name_var }   
 <file_or_filegroup> [ ,...n ]   
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]  
[;]  
  
Creating a Partial Backup  
BACKUP DATABASE { database_name | @database_name_var }   
 READ_WRITE_FILEGROUPS [ , <read_only_filegroup> [ ,...n ] ]  
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]  
[;]  
  
Backing Up the Transaction Log (full and bulk-logged recovery models)  
BACKUP LOG -- Not supported in SQL Database Managed Instance
  { database_name | @database_name_var }  
  TO <backup_device> [ ,...n ]   
  [ <MIRROR TO clause> ] [ next-mirror-to ]  
  [ WITH { <general_WITH_options> | \<log-specific_optionspec> } [ ,...n ] ]  
[;]  
  
<backup_device>::=   
 {  
   { logical_device_name | @logical_device_name_var }   
 | {   DISK -- Not supported in SQL Database Managed Instance
     | TAPE -- Not supported in SQL Database Managed Instance
     | URL } =   
     { 'physical_device_name' | @physical_device_name_var | 'NUL' }  
 }   
  
<MIRROR TO clause>::=  
 MIRROR TO <backup_device> [ ,...n ]  
  
<file_or_filegroup>::=  
 {  
   FILE = { logical_file_name | @logical_file_name_var }   
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }  
 }   
  
<read_only_filegroup>::=  
FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }  
  
<general_WITH_options> [ ,...n ]::=   
--Backup Set Options  
   COPY_ONLY -- Only backup set option supported by SQL Database Managed Instance  
 | { COMPRESSION | NO_COMPRESSION }   
 | DESCRIPTION = { 'text' | @text_variable }   
 | NAME = { backup_set_name | @backup_set_name_var }   
 | CREDENTIAL  
 | ENCRYPTION  
 | FILE_SNAPSHOT  --Not supported in SQL Database Managed Instance
 | { EXPIREDATE = { 'date' | @date_var }   
        | RETAINDAYS = { days | @days_var } }   
  
--Media Set Options  
   { NOINIT | INIT }   
 | { NOSKIP | SKIP }   
 | { NOFORMAT | FORMAT }   
 | MEDIADESCRIPTION = { 'text' | @text_variable }   
 | MEDIANAME = { media_name | @media_name_variable }   
 | BLOCKSIZE = { blocksize | @blocksize_variable }   
  
--Data Transfer Options  
   BUFFERCOUNT = { buffercount | @buffercount_variable }   
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }  
  
--Error Management Options  
   { NO_CHECKSUM | CHECKSUM }  
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Compatibility Options  
   RESTART   
  
--Monitoring Options  
   STATS [ = percentage ]   
  
--Tape Options. These are not supported in SQL Database Managed Instance
   { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }   
  
--Log-specific Options. These are not supported in SQL Database Managed Instance 
   { NORECOVERY | STANDBY = undo_file_name }  
 | NO_TRUNCATE  
  
--Encryption Options  
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=   
   SERVER CERTIFICATE = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name   
```  
  
## <a name="arguments"></a>Arguments  

DATABASE  
 Spécifie une sauvegarde complète de la base de données. Si une liste de fichiers et de groupes de fichiers est spécifiée, seuls ceux-ci sont sauvegardés. Au cours d'une sauvegarde de base de données complète ou différentielle, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sauvegarde une portion suffisante du journal des transactions afin d'assurer la cohérence de la base de données lors de la restauration de la sauvegarde.  
  
 Lorsque vous restaurez une sauvegarde créée par BACKUP DATABASE (une *sauvegarde de données*), l’ensemble de la sauvegarde est restauré. Seule une sauvegarde du fichier journal peut être restaurée à un moment ou une transaction spécifique au sein de la sauvegarde.  
  
> [!NOTE]  
> Seule une sauvegarde complète peut être effectuée sur la base de données **master**.  
  
LOG **S’applique à** : SQL Server

 Indique que la sauvegarde ne doit porter que sur le journal des transactions. Le journal est sauvegardé à partir de la dernière sauvegarde réussie du fichier journal et jusqu'à sa fin actuelle. Avant de pouvoir créer la première sauvegarde du fichier journal, vous devez créer une sauvegarde complète.  
  
 Vous pouvez restaurer une sauvegarde du fichier journal jusqu’à une date et heure précises ou une transaction spécifique en spécifiant `WITH STOPAT`,`STOPATMARK` ou `STOPBEFOREMARK` dans votre instruction [RESTORE LOG](../../t-sql/statements/restore-statements-transact-sql.md).  
  
> [!NOTE]  
>  Après une sauvegarde de fichier journal standard, certains enregistrements du journal des transactions deviennent inactifs, sauf si vous spécifiez `WITH NO_TRUNCATE` ou `COPY_ONLY`. Le journal est tronqué une fois que tous les enregistrements d'un ou de plusieurs fichiers journaux virtuels sont devenus inactifs. Si le journal n'est pas tronqué après des sauvegardes normales du journal, il se peut que quelque chose retarde la troncation du journal. Pour plus d’informations, consultez [Facteurs pouvant retarder la troncation du journal](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation).  
  
 { *database_name* | **@***database_name_var* } Correspond à la base de données à partir de laquelle va être opérée la sauvegarde du journal des transactions, la sauvegarde complète ou partielle. S’il est fourni en tant que variable (**@***database_name_var*), ce nom peut être spécifié comme constante de chaîne (**@***database_name_var***=***database name*) ou comme variable de type chaîne de caractères, sauf pour les types de données  **ntext** ou **text**.  
  
> [!NOTE]  
> La base de données miroir d'un partenariat de mise en miroir de bases de données ne peut pas être sauvegardée.  
  
\<file_or_filegroup> [ **,**...*n* ]  
 Utilisé uniquement avec BACKUP DATABASE, cet argument spécifie un fichier ou groupe de fichiers de base de données à inclure dans une sauvegarde de fichiers ou spécifie un fichier ou groupe de fichiers en lecture seule à inclure dans une sauvegarde partielle.  
  
 FILE **=** { *logical_file_name* | **@***logical_file_name_var* }  
 Nom logique d'un fichier ou variable dont la valeur correspond au nom logique d'un fichier à inclure dans la sauvegarde.  
  
 FILEGROUP **=** { *logical_filegroup_name* | **@***logical_filegroup_name_var* }  
 Nom logique d'un groupe de fichiers ou variable dont la valeur correspond au nom logique d'un groupe de fichiers à inclure dans la sauvegarde. En mode de récupération simple, la sauvegarde d'un groupe de fichiers n'est autorisée que pour un groupe de fichiers en lecture seule.  
  
> [!NOTE]  
> Pensez à utiliser des sauvegardes de fichiers lorsque la taille de la base de données et les exigences en matière de performances rendent la sauvegarde de la base de données totalement inadaptée. L’appareil NULL peut être utilisé pour tester les performances des sauvegardes, mais il ne doit pas être utilisé dans les environnements de production.
  
 *n*  
 Espace réservé indiquant qu'il est possible de spécifier plusieurs fichiers et groupes de fichiers dans une liste séparée par des virgules. Le nombre est illimité. 
  
 Pour plus d’informations, consultez [Sauvegardes de fichiers complètes &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md) et [Sauvegarder des fichiers et des groupes de fichiers &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md).  
  
 READ_WRITE_FILEGROUPS [ **,** FILEGROUP = { *logical_filegroup_name* | **@***logical_filegroup_name_var* } [ **,**...* n* ] ]  
 Spécifie une sauvegarde partielle. Une sauvegarde partielle inclut tous les fichiers en lecture/écriture dans une base de données : le groupe de fichiers primaire, tous les groupes de fichiers secondaires en lecture/écriture, ainsi que les fichiers ou groupes de fichiers en lecture seule qui ont été spécifiés.  
  
 READ_WRITE_FILEGROUPS  
 Spécifie que tous les groupes de fichiers en lecture/écriture doivent être sauvegardés dans la sauvegarde partielle. Si la base de données est en lecture seule, READ_WRITE_FILEGROUPS inclut uniquement le groupe de fichiers primaire.  
  
> [!IMPORTANT]  
> Si, au lieu d'utiliser READ_WRITE_FILEGROUPS, vous listez de manière explicite les groupes de fichiers en lecture/écriture en utilisant FILEGROUP, vous allez créer une sauvegarde de fichiers.  
  
 FILEGROUP = { *logical_filegroup_name* | **@***logical_filegroup_name_var* }  
Nom logique d'un groupe de fichiers en lecture seule ou variable dont la valeur correspond au nom logique d'un groupe de fichiers en lecture seule à inclure dans la sauvegarde partielle. Pour plus d’informations, reportez-vous à « \<file_or_filegroup> », plus haut dans cette rubrique.
  
 *n*  
 Espace réservé indiquant qu'il est possible de spécifier plusieurs groupes de fichiers en lecture seule dans une liste séparée par des virgules.  
  
 Pour plus d’informations sur les sauvegardes partielles, consultez [Sauvegardes partielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/partial-backups-sql-server.md).  
  
TO \<backup_device> [ **,**...*n* ] Indique que le jeu des [unités de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md) associé est soit un support de sauvegarde non miroir, soit le premier miroir d’un support de sauvegarde miroir (pour lequel une ou plusieurs clauses MIRROR TO sont déclarées).  
  
\<backup_device> **S’applique à** : SQL Server Spécifie une unité de sauvegarde logique ou physique à utiliser pour l’opération de sauvegarde.  
  
 { *logical_device_name* | **@***logical_device_name_var* } **S’applique à** : SQL Server Nom logique de l’unité de sauvegarde dans laquelle la base de données est sauvegardée. Le nom logique doit se conformer aux règles en vigueur pour les identificateurs. Fourni comme variable (@* logical_device_name_var *), le nom de l’unité de sauvegarde peut être spécifié sous la forme d’une constante de chaîne (@* logical_device_name_var***=** nom logique de l’unité de sauvegarde) ou d’une variable de type chaîne de caractères, sauf pour les types de données **ntext** et **text**.  
  
 { DISK | TAPE | URL} **=** { **'***physical_device_name***'** | **@***physical_device_name_var* | 'NUL' } **S’applique à** : DISK, TAPE et URL s’appliquent à SQL Server. Seul URL s’applique à SQL Database Managed Instance Spécifie un fichier sur disque ou un périphérique à bandes, ou un service de stockage Blob Microsoft Azure. Le format d’URL est utilisé pour créer des sauvegardes dans le service de stockage Microsoft Azure. Pour plus d’informations et d’exemples, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). Pour un tutoriel, consultez [Tutoriel : Sauvegarde et restauration SQL Server dans le service de stockage Microsoft Azure](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md). 

> [!NOTE] 
> L’unité de disque NUL supprime toutes les informations qui lui sont envoyées et ne devrait être utilisée qu’à des fins de test. À ne pas utiliser en production.
  
> [!IMPORTANT]  
> De [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], vous ne pouvez sauvegarder que sur une seule unité de disque lorsque vous effectuez une sauvegarde vers une URL. Pour effectuer une sauvegarde sur plusieurs unités lorsque vous sauvegardez vers une URL, vous devez utiliser [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] ou [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], ainsi que des jetons de signature d’accès partagé. Pour obtenir des exemples de signatures d’accès partagé, consultez [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md) et [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with PowerShell](http://blogs.msdn.com/b/sqlcat/archive/2015/03/21/simplifying-creation-sql-credentials-with-shared-access-signature-sas-keys-on-azure-storage-containers-with-powershell.aspx).  
  
**URL s’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]) et SQL Database Managed Instance.  
  
 Une unité de disque n'est pas tenue d'exister pour pouvoir être spécifiée dans une instruction BACKUP. Si l'unité physique existe et si l'option INIT n'est pas spécifiée dans l'instruction BACKUP, la sauvegarde est ajoutée à l'unité.  
 
> [!NOTE] 
> L’unité NUL va supprimer toutes les entrées envoyées à ce fichier. Toutefois, la sauvegarde continue de marquer toutes les pages comme étant sauvegardées.
  
 Pour plus d’informations, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
> [!NOTE]  
> L'option TAPE sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
 *n*  
 Espace réservé indiquant qu'il est possible de spécifier jusqu'à 64 unités de sauvegarde dans une liste séparée par des virgules.  
  
MIRROR TO \<backup_device> [ **,**...*n* ] Spécifie un jeu constitué au maximum de trois unités de sauvegarde, dont chacune est le miroir des unités de sauvegarde spécifiées dans la clause TO. La clause MIRROR TO doit spécifier le même type et le même nombre d’unités de sauvegarde que la clause TO. Vous pouvez spécifier jusqu'à trois clauses MIRROR TO.  
  
 Cette option est disponible uniquement dans l'édition Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> Pour MIRROR TO = DISK, BACKUP détermine automatiquement la taille de bloc appropriée des unités de disques. Pour plus d'informations sur la taille de bloc, consultez « BLOCKSIZE » ultérieurement dans ce tableau.  
  
\<backup_device>Consultez « \<backup_device> », plus haut dans cette section.
  
 *n*  
 Espace réservé indiquant qu'il est possible de spécifier jusqu'à 64 unités de sauvegarde dans une liste séparée par des virgules. Le nombre d'unités spécifiées dans la clause MIRROR TO doit être égal au nombre d'unités spécifiées dans la clause TO.  
  
 Pour plus d’informations, consultez « Familles de supports de sauvegarde miroirs » dans la section [Remarques](#general-remarks), plus loin dans cette rubrique.  
  
 [ *next-mirror-to* ]  
 Espace réservé indiquant qu'une instruction BACKUP peut contenir jusqu'à trois clauses MIRROR TO, en plus de la clause TO.  
  
### <a name="with-options"></a>Options WITH  
 Spécifie les options à utiliser avec une opération de sauvegarde.  
  
 CREDENTIAL  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]) et SQL Database Managed Instance.  
 S'utilise uniquement lors de la création d'une sauvegarde dans le service de stockage d'objets blob Windows Azure.  
  
 FILE_SNAPSHOT **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).

 Utilisé pour créer un instantané Azure des fichiers de base de données lorsque tous les fichiers de base de données SQL Server sont stockés à l’aide du service Azure Blob Storage. Pour plus d’informations, consultez [Fichiers de données SQL Server dans Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). La sauvegarde d’instantanés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des instantanés Azure des fichiers de base de données (données et fichiers journaux) dont l’état est cohérent. Un ensemble cohérent d’instantanés Azure constitue une sauvegarde, qui est enregistrée dans le fichier de sauvegarde. La seule différence entre `BACKUP DATABASE TO URL WITH FILE_SNAPSHOT` et `BACKUP LOG TO URL WITH FILE_SNAPSHOT` est que ce dernier tronque le journal des transactions. Avec la sauvegarde d’instantanés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une fois effectuée la sauvegarde complète initiale dont a besoin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour établir la chaîne de sauvegarde, seule une sauvegarde de fichier journal est nécessaire pour restaurer une base de données vers le point dans le temps correspondant à la sauvegarde de fichier journal. En outre, seules deux sauvegardes de fichier journal sont nécessaires pour restaurer une base de données vers un point dans le temps situé entre les deux sauvegardes de fichier journal.  
    
 DIFFERENTIAL  
**S’applique à** : SQL Server Utilisé uniquement avec BACKUP DATABASE, ce paramètre spécifie que la sauvegarde de base de données ou de fichier ne doit porter que sur les parties de la base de données ou du fichier qui ont été modifiées depuis la dernière sauvegarde complète. Une sauvegarde différentielle occupe en général moins d'espace qu'une sauvegarde complète. Utilisez cette option de façon à ne pas avoir à appliquer toutes les sauvegardes successives du journal effectuées depuis la dernière sauvegarde complète.  
  
> [!NOTE]  
> Par défaut, `BACKUP DATABASE` crée une sauvegarde complète.  
  
 Pour plus d’informations, consultez [Sauvegardes différentielles &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
 ENCRYPTION  
 Utilisé pour spécifier le chiffrement d'une sauvegarde. Vous pouvez spécifier un algorithme de chiffrement pour chiffrer la sauvegarde ou spécifier `NO_ENCRYPTION` pour ne pas chiffrer la sauvegarde. Il est recommandé d'utiliser le chiffrement pour sécuriser les fichiers de sauvegarde. Liste des algorithmes possibles :  
  
-   `AES_128`  
-   `AES_192`  
-   `AES_256`  
-   `TRIPLE_DES_3KEY`  
-   `NO_ENCRYPTION`    

Si vous choisissez de chiffrer, vous devez également spécifier le chiffreur à l'aide des options de chiffreur :  
  
-   SERVER CERTIFICATE = Encryptor_Name  
-   SERVER ASYMMETRIC KEY = Encryptor_Name  
  
> [!WARNING]  
> Lorsque le chiffrement est utilisé conjointement à l’argument `FILE_SNAPSHOT`, le fichier de métadonnées est chiffré à l’aide de l’algorithme de chiffrement spécifié, et le système vérifie qu’un [chiffrement TDE (Transparent Data Encryption)](../../relational-databases/security/encryption/transparent-data-encryption.md) a été effectué pour la base de données. Aucun autre chiffrement n’est effectué pour les données. La sauvegarde échoue si la base de données n’a pas été chiffrée ou si le chiffrement n’a pas été exécuté avant l’émission de l’instruction BACKUP.  
  
**Options du jeu de sauvegarde**  
  
Ces options s'appliquent au jeu de sauvegarde qui est créé par cette opération de sauvegarde.  
  
> [!NOTE]  
> Pour spécifier un jeu de sauvegarde pour une opération de restauration, utilisez l’option `FILE = <backup_set_file_number>`. Pour plus d’informations sur la façon de spécifier un jeu de sauvegarde, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).
  
 COPY_ONLY **S’applique à** : SQL Server et SQL Database Managed Instance Spécifie que la sauvegarde est une *sauvegarde en copie seule* qui n’a aucun impact sur la séquence normale des sauvegardes. Une sauvegarde en copie seule est créée indépendamment de vos sauvegardes régulières standard. Ce type de sauvegarde n'a aucun effet sur les procédures globales de sauvegarde et de restauration de la base de données.  
  
 Les sauvegardes en copie seule doivent être utilisées dans les cas où une sauvegarde est effectuée dans un but particulier, par exemple pour sauvegarder le journal avant une restauration de fichiers en ligne. En règle générale, une sauvegarde de fichier journal en copie seule est utilisée une fois, puis supprimée.  
  
-   Lorsqu’elle est utilisée avec `BACKUP DATABASE`, l’option `COPY_ONLY` crée une sauvegarde complète qui ne peut pas servir de base différentielle. La bitmap différentielle n'est pas mise à jour et les sauvegardes différentielles se comportent comme si la sauvegarde en copie seule n'existait pas. Les sauvegardes différentielles ultérieures utilisent la dernière sauvegarde complète standard en tant que base.  
  
    > [!IMPORTANT]  
    > Si `DIFFERENTIAL` et `COPY_ONLY` sont utilisés ensemble, `COPY_ONLY` est ignoré, et une sauvegarde différentielle est créée.  
  
-   Lorsqu’elle est utilisée avec `BACKUP LOG`, l’option `COPY_ONLY` crée une *sauvegarde de fichier journal en copie seule* qui ne tronque pas le journal des transactions. La sauvegarde de journal en copie seule n'a aucun effet sur la séquence de journaux de transactions consécutifs et les autres sauvegardes de journal se comportent comme si la sauvegarde en copie seule n'existait pas.  
  
Pour plus d’informations, consultez [Sauvegardes de copie uniquement &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).  
  
{ COMPRESSION | NO_COMPRESSION }  
Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et les versions ultérieures uniquement, spécifie si la [compression de sauvegarde](../../relational-databases/backup-restore/backup-compression-sql-server.md) est effectuée sur cette sauvegarde, remplaçant la valeur par défaut au niveau du serveur.  
  
Lors de l'installation, le comportement par défaut exclut toute compression des sauvegardes. Toutefois, ce paramétrage par défaut peut être modifié en définissant l’option de configuration du serveur [Compression par défaut des sauvegardes](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Pour plus d’informations sur l’affichage de la valeur actuelle de cette option, consultez [Afficher ou modifier des propriétés de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).  

Pour plus d’informations sur l’utilisation de la compression de sauvegarde avec des bases de données où [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) est activé, consultez la section [Remarques](#general-remarks).
  
COMPRESSION  
Active explicitement la compression des sauvegardes.  
  
NO_COMPRESSION  
Désactive explicitement la compression des sauvegardes.  
  
DESCRIPTION **=** { **'***text***'** | **@***text_variable* }  
Spécifie le texte de format libre servant à décrire le jeu de sauvegarde. La chaîne peut compter jusqu'à 255 caractères.  
  
NAME **=** { *backup_set_name* | **@***backup_set_var* }  
Spécifie le nom du jeu de sauvegarde. Les noms peuvent contenir jusqu'à 128 caractères. Si l'option NAME n'est pas spécifiée, le nom reste vide.  
  
{ EXPIREDATE **=’***date***’** | RETAINDAYS **=** *days* }  
Spécifie la date à laquelle le jeu de sauvegarde de cette sauvegarde peut être écrasé. Si ces options sont toutes les deux utilisées, RETAINDAYS l'emporte sur EXPIREDATE.  
  
Si aucune de ces options n’est spécifiée, la date d’expiration est déterminée par le paramètre de configuration **mediaretention**. Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).   
  
> [!IMPORTANT]  
> Ces options empêchent seulement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'écraser un fichier. Le contenu des bandes peut être écrasé par d'autres méthodes, et les fichiers sur disque peuvent être supprimés à partir du système d'exploitation. Pour plus d'informations sur le contrôle du délai d'expiration, consultez SKIP et FORMAT dans cette rubrique.  
  
EXPIREDATE **=** { **’***date***’** | **@***date_var* } Spécifie la date à laquelle le jeu de sauvegarde expire et peut donc être écrasé. Si elle est fournie en tant que variable (@* date_var*), cette date doit suivre le format **datetime** configuré par le système et prendre l’une des formes suivantes :  
  
-   Une constante de chaîne (@*date_var* **=** date)  
-   Une variable de type chaîne de caractères (à l’exception des types de données **ntext** ou **text**)  
-   Un **smalldatetime**  
-   Une variable **datetime**  
  
Exemple :  
  
-   `'Dec 31, 2020 11:59 PM'`  
-   `'1/1/2021'`  
  
Pour plus d’informations sur la spécification des valeurs **datetime**, consultez [Types Date et Time](../../t-sql/data-types/date-and-time-types.md).  
  
> [!NOTE]  
> Pour ignorer la date d’expiration, utilisez l’option `SKIP`.  
  
RETAINDAYS **=** { *days* | **@***days_var* } Indique le nombre de jours qui doivent s’écouler avant de pouvoir remplacer le support de sauvegarde. S’il est fourni en tant que variable (**@***days_var*), sa valeur doit être un entier.  
  
**Options du support de sauvegarde**  
  
Ces options s'appliquent à l'ensemble du support de sauvegarde.  
  
{ **NOINIT** | INIT }  
 Détermine si l'opération de sauvegarde ajoute les nouvelles sauvegardes ou si elle remplace les jeux de sauvegardes déjà présents sur le support de sauvegarde. La valeur par défaut (NOINIT) consiste à ajouter les nouvelles sauvegardes après le jeu de sauvegarde le plus récent sur le support.  
  
> [!NOTE]  
> Pour plus d’informations sur les interactions entre les options { **NOINIT** | INIT } et { **NOSKIP** | SKIP }, reportez-vous à la section [Remarques](#general-remarks), plus loin dans cette rubrique.  
  
NOINIT  
 Indique que le jeu de sauvegarde est ajouté au support de sauvegarde spécifié, préservant ainsi les jeux de sauvegardes existants. Si un mot de passe de support est défini pour le support de sauvegarde, il doit être fourni. NOINIT est la valeur par défaut.  
  
Pour plus d’informations, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
INIT  
 Indique que tous les jeux de sauvegardes doivent être écrasés mais préserve l'en-tête de support. Si INIT est spécifié, tous les jeux de sauvegardes qui se trouvent sur l'unité concernée sont écrasés si les conditions l'autorisent. Par défaut, BACKUP vérifie les conditions ci-après et n'écrase pas le support de sauvegarde si l'une des conditions est vraie :  
  
-   Un jeu de sauvegarde n'a pas encore expiré. Pour plus d’informations, consultez les options `EXPIREDATE` et `RETAINDAYS`.  
-   Le nom du jeu de sauvegarde donné dans l'instruction BACKUP, s'il est fourni, ne correspond pas à celui du support de sauvegarde. Pour plus d'informations, consultez l'option NAME, plus haut dans cette section.  
  
Pour ignorer ces vérifications, utilisez l’option `SKIP`.  
  
Pour plus d’informations, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
{ **NOSKIP** | SKIP }  
Détermine si une opération de sauvegarde vérifie la date et l'heure d'expiration des jeux de sauvegardes figurant sur le support avant de les écraser.  
  
> [!NOTE]  
> Pour plus d’informations sur les interactions entre les options { **NOINIT** | INIT } et { **NOSKIP** | SKIP }, reportez-vous à la section « Remarques », plus loin dans cette rubrique.  
  
NOSKIP  
Ordonne à l'instruction BACKUP de vérifier la date d'expiration de tous les jeux de sauvegardes qui se trouvent sur le support, avant d'autoriser leur écrasement. Il s'agit du comportement par défaut.  
  
SKIP  
Désactive le contrôle de la date d'expiration et du nom qui est habituellement effectué par l'instruction BACKUP pour prévenir un écrasement des jeux de sauvegardes. Pour plus d'informations sur les interactions entre les options { INIT | NOINIT } et { NOSKIP | SKIP }, reportez-vous à « Remarques », plus loin dans cette rubrique.  
Pour afficher les dates d’expiration des jeux de sauvegardes, interrogez la colonne **expiration_date** de la table d’historique [backupset](../../relational-databases/system-tables/backupset-transact-sql.md).  
  
{ **NOFORMAT** | FORMAT }  
Spécifie si l'en-tête de support doit être écrit sur les volumes utilisés pour cette opération de sauvegarde, en écrasant l'en-tête de support et les jeux de sauvegardes existants.  
  
NOFORMAT  
Spécifie que l'opération de sauvegarde conserve l'en-tête de support et les jeux de sauvegardes existants sur les volumes de support utilisés pour cette opération de sauvegarde. Il s'agit du comportement par défaut.  
  
FORMAT  
Indique qu'un nouveau support de sauvegarde est créé. Si FORMAT est utilisé, l'opération de sauvegarde écrit un nouvel en-tête de support sur tous les volumes utilisés pour cette opération de sauvegarde. Le contenu précédent du volume devient non valide étant donné que l'en-tête de support et les jeux de sauvegardes existants sont écrasés.  
  
> [!IMPORTANT]  
> Soyez prudent lorsque vous utilisez `FORMAT`. Si vous formatez l'un des volumes d'un support de sauvegarde, la totalité du support de sauvegarde devient inutilisable. Par exemple, si une bande appartenant à un support de sauvegarde distribuée existant est initialisée, tout le support de sauvegarde devient inutilisable.  
  
La spécification de FORMAT implique `SKIP`. `SKIP` n’a pas besoin d’être explicitement spécifié.  
  
MEDIADESCRIPTION **=** { *text* | **@***text_variable* }  
Indique le texte de description de format libre du support de sauvegarde (maximum 255 caractères).  
  
MEDIANAME **=** { *media_name* | **@***media_name_variable* }  
Fournit le nom du support de sauvegarde complet. Le nom du support ne doit pas dépasser 128 caractères. Si `MEDIANAME` est spécifié, il doit correspondre au nom spécifié précédemment existant sur les volumes de sauvegarde. Si elle n'est pas spécifiée, ou si l'option SKIP l'est, aucune vérification du nom de support n'est effectuée.  
  
BLOCKSIZE **=** { *blocksize* | **@***blocksize_variable* }  
Indique, en octets, la taille physique du bloc. Les tailles prises en charge sont 512, 1024, 2048, 4096, 8192, 16384, 32768 et 65536 (64 Ko) octets. La valeur par défaut est 65536 pour les périphériques à bandes, 512 sinon. En règle générale, cette option est superflue car BACKUP sélectionne automatiquement une taille de bloc appropriée pour le périphérique. Si vous spécifiez explicitement une taille de bloc, la sélection automatique est annulée et remplacée.  
  
Si vous effectuez une sauvegarde que vous envisagez de copier sur un CD-ROM pour la restaurer à partir de celui-ci, spécifiez BLOCKSIZE=2048.  
  
> [!NOTE]  
> En règle générale, cette option n'affecte les performances que si les données sont écrites sur des périphériques à bandes.  
  
**Options de transfert de données**  
  
BUFFERCOUNT **=** { *buffercount* | **@***buffercount_variable* }  
Spécifie le nombre total de tampons d'E/S à utiliser pour l'opération de sauvegarde. Vous pouvez spécifier n'importe quel entier positif ; toutefois, un nombre élevé de tampons peut provoquer des erreurs liées à une insuffisance de mémoire. En effet, l'espace d'adressage virtuel peut s'avérer inapproprié dans la tâche Sqlservr.exe.  
  
L’espace total utilisé par les mémoires tampons est déterminé par : *buffercount/maxtransfersize*.  
  
> [!NOTE]  
> Pour des informations importantes sur l’utilisation de l’option `BUFFERCOUNT`, consultez le blog [Incorrect BufferCount data transfer option can lead to OOM condition](http://blogs.msdn.com/b/sqlserverfaq/archive/2010/05/06/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition.aspx).  
  
MAXTRANSFERSIZE **=** { *maxtransfersize* | ***@** maxtransfersize_variable* } Spécifie la plus grande unité de transfert, en octets, à utiliser entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le support de sauvegarde. Les valeurs possibles sont les multiples de 65536 octets (64 Ko), dans la limite de 4194304 octets (4 Mo).  

> [!NOTE]  
> Lors de la création de sauvegardes à l’aide du service SQL Writer, si la base de données est configurée avec [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md), ou si elle comprend des [groupes de fichiers à mémoire optimisée](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md), la valeur de `MAXTRANSFERSIZE` au moment de la restauration doit être supérieure ou égale à celle du `MAXTRANSFERSIZE` qui a été utilisée lors de la création de la sauvegarde. 

> [!NOTE]  
> Pour les bases de données comprenant un seul fichier de données et où [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) est activé, la valeur `MAXTRANSFERSIZE` par défaut est 65 536 (64 Ko). Pour les bases de données non chiffrées à l’aide de TDE, la valeur `MAXTRANSFERSIZE` par défaut est 1 048 576 (1 Mo) lors d’une sauvegarde vers DISK, et 65 536 (64 Ko) lors d’une sauvegarde vers VDI ou TAPE.
> Pour plus d’informations sur l’utilisation de la compression de sauvegarde avec des bases de données chiffrées avec TDE, consultez la section [Remarques](#general-remarks).
  
**Options de gestion des erreurs**  
  
Ces options vous permettent de déterminer si les sommes de contrôle de sauvegarde sont activées pour l’opération de sauvegarde, et si l’opération doit s’arrêter en présence d’une erreur.  
  
{ **NO_CHECKSUM** | CHECKSUM }  
 Détermine si les sommes de contrôle de sauvegarde sont activées.  
  
NO_CHECKSUM  
Désactive explicitement la génération de sommes de contrôle de sauvegarde (et la validation des sommes de contrôle de page). Il s'agit du comportement par défaut.  
  
CHECKSUM  
Indique que l’opération de sauvegarde vérifie dans chaque page les informations de somme de contrôle et de page endommagée, si elles sont activées et disponibles, et génère une somme de contrôle pour l’ensemble de la sauvegarde.  
  
L'utilisation des sommes de contrôle de sauvegarde peut affecter la charge de travail et le débit de sauvegarde.  
  
Pour plus d’informations, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR }  
Détermine si une opération de sauvegarde s'arrête ou continue après avoir rencontré une erreur de somme de contrôle de page.  
  
STOP_ON_ERROR  
Ordonne à BACKUP de s'arrêter si une somme de contrôle de page n'est pas validée. Il s'agit du comportement par défaut.  
  
CONTINUE_AFTER_ERROR  
Ordonne à BACKUP de continuer en dépit des erreurs rencontrées, telles que des sommes de contrôle de page non valides ou des pages endommagées.  
  
Si vous ne parvenez pas à effectuer une sauvegarde de la fin du journal à l’aide de l’option NO_TRUNCATE lorsque la base de données est endommagée, vous pouvez essayer d’effectuer une [sauvegarde de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) en spécifiant CONTINUE_AFTER_ERROR au lieu de NO_TRUNCATE.  
  
Pour plus d’informations, consultez [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
**Options de compatibilité**  
  
RESTART  
Depuis [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], n'a aucun effet. Elle est acceptée par cette version à des fins de compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
**Options de surveillance**  
  
STATS [ **=** *percentage* ]  
 Affiche un message à chaque fois qu’un autre *pourcentage* se termine et sert à évaluer l’état d’avancement de l’opération. Si *percentage* est omis, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche un message à chaque incrément de 10 pour cent.  
  
L'option STATS signale le pourcentage terminé comme seuil de rapport de l'intervalle suivant. C'est-à-dire approximativement le pourcentage spécifié ; par exemple, si STATS=10, et si le pourcentage terminé est 40 pour cent, l'option peut afficher 43 pour cent. Pour les jeux de sauvegardes volumineux, cela n'est pas un problème car le pourcentage terminé varie très lentement entre les appels d'E/S terminés.  
  
**Options des périphériques à bandes**  
**S’applique à** : SQL Server  
Ces options sont utilisées uniquement pour les périphériques À BANDES. S'il ne s'agit pas d'un périphérique à bandes, ces options sont ignorées.  
  
{ **REWIND** | NOREWIND }  
REWIND **S’applique à** : SQL Server Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libère et rembobine la bande. REWIND est le paramètre par défaut.  
  
NOREWIND **S’applique à** : SQL Server Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] maintient la bande ouverte après l’opération de sauvegarde. Cette option vous permet d'améliorer les performances lorsque vous effectuez plusieurs opérations de sauvegarde sur une bande.  
  
NOREWIND implique NOUNLOAD, et ces options sont incompatibles dans une instruction BACKUP unique.  
  
> [!NOTE]  
> Si vous utilisez `NOREWIND`, l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve la propriété du lecteur de bande jusqu’à ce qu’une instruction BACKUP ou RESTORE s’exécutant dans le même processus utilise l’option `REWIND` ou `UNLOAD`, ou jusqu’à l’arrêt de l’instance du serveur. Le fait de maintenir la bande ouverte empêche les autres processus d'y accéder. Pour savoir comment afficher la liste des bandes ouvertes et comment les fermer, consultez [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
{ **UNLOAD** | NOUNLOAD }    
**S’applique à** : SQL Server 
> [!NOTE]  
> `UNLOAD` et `NOUNLOAD` sont des paramètres de session qui sont conservés jusqu’à la fin de la session ou tant qu’ils ne sont pas réinitialisés par la spécification d’une autre option.  
  
UNLOAD **S’applique à** : SQL Server   
 Indique que la bande est automatiquement rembobinée et démontée lorsque la sauvegarde est terminée. UNLOAD est l'option par défaut au démarrage d'une session. 
  
NOUNLOAD **S’applique à** : SQL Server Indique qu’au terme de l’opération BACKUP, la bande reste chargée dans le lecteur de bande.  
  
> [!NOTE]  
> Dans le cas d’une sauvegarde sur une unité de sauvegarde sur bande, l’option `BLOCKSIZE` affecte les performances de l’opération de sauvegarde. En règle générale, cette option n'affecte les performances que si les données sont écrites sur des périphériques à bandes.  
  
**Options spécifiques au journal**  
**S’applique à** : SQL Server  
Ces options ne sont utilisées qu’avec `BACKUP LOG`.  
  
> [!NOTE]  
> Si vous ne voulez pas effectuer de sauvegarde du journal, utilisez le mode de récupération simple. Pour plus d’informations, consultez [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
{ NORECOVERY | STANDBY **=** *undo_file_name* }  
  NORECOVERY **S’applique à** : SQL Server   
  Effectue une sauvegarde de la fin du journal et laisse la base de données en état de restauration (RESTORING). NORECOVERY s'avère utile lors du basculement vers une base de données secondaire ou de l'exécution d'une sauvegarde de la fin du journal avant une opération RESTORE.  
  
  Pour effectuer au mieux une sauvegarde du journal qui évite la troncation du journal et place la base de données en état RESTORING de manière atomique, utilisez conjointement les options `NO_TRUNCATE` et `NORECOVERY`.  
  
  STANDBY **=** *standby_file_name* 
**S’applique à** : SQL Server   
  Effectue une sauvegarde de la fin du journal et laisse la base de données en lecture seule et en état STANDBY. La clause STANDBY écrit les données en attente (annulation avec option de restauration ultérieure). L'option STANDBY est semblable à BACKUP LOG WITH NORECOVERY suivie par RESTORE WITH STANDBY.  
  
  Le mode d’attente nécessite un fichier d’annulation, spécifié par *standby_file_name*, dont l’emplacement figure dans le journal de la base de données. Si le fichier spécifié existe déjà, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] l'écrase ; sinon, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] le crée. Le fichier d'annulation devient partie intégrante de la base de données.  
  
  Ce fichier contient les modifications annulées, qui doivent être restaurées si des opérations RESTORE LOG sont effectuées ultérieurement. Vous devez disposer d'un espace disque suffisant pour que le fichier d'annulation puisse contenir toutes les pages distinctes de la base de données qui ont été modifiées par suite du rejet des transactions non validées.  
  
NO_TRUNCATE  
**S’applique à** : SQL Server  
Indique que le journal n’est pas tronqué et que [!INCLUDE[ssDE](../../includes/ssde-md.md)] tente la sauvegarde, quel que soit l’état de la base de données. Par conséquent, les métadonnées d’une sauvegarde effectuée avec `NO_TRUNCATE` peuvent être incomplètes. Cette option permet de sauvegarder le journal lorsque la base de données est endommagée.  
  
L'option NO_TRUNCATE de BACKUP LOG revient à spécifier COPY_ONLY et CONTINUE_AFTER_ERROR.  
  
Sans l’option `NO_TRUNCATE`, l’état de la base de données doit avoir la valeur ONLINE. Si l’état de la base de données a la valeur SUSPENDED, vous pouvez créer une sauvegarde en spécifiant `NO_TRUNCATE`. Toutefois, si l’état de la base de données a la valeur OFFLINE ou EMERGENCY, l’option BACKUP n’est pas autorisée même avec l’option `NO_TRUNCATE`. Pour plus d’informations sur les états de base de données, consultez [États d’une base de données](../../relational-databases/databases/database-states.md).  
  
## <a name="about-working-with-sql-server-backups"></a>À propos de l’utilisation des sauvegardes SQL Server  
 Cette section présente les concepts de sauvegarde essentiels suivants :  
  
 [Types de sauvegarde](#Backup_Types)  
 [Troncation du journal des transactions](#Tlog_Truncation)  
 [Formatage du support de sauvegarde](#Formatting_Media)  
 [Utilisation des unités de sauvegarde et des supports de sauvegarde](#Backup_Devices_and_Media_Sets)  
 [Restauration de sauvegardes SQL Server](#Restoring_Backups)  
  
> [!NOTE]  
> Pour une présentation de la sauvegarde dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
###  <a name="Backup_Types"></a> Types de sauvegarde  
 Les types de sauvegarde pris en charge dépendent du mode de récupération de la base de données conformément à ce qui suit :  
  
-   Tous les modes de récupération prennent en charge les sauvegardes de données complètes et différentielles.  
  
    |Étendue de la sauvegarde|Types de sauvegarde|  
    |---------------------|------------------|  
    |Base de données entière|Les [sauvegardes de base de données](../../relational-databases/backup-restore/full-database-backups-sql-server.md) couvrent l’ensemble de la base de données.<br /><br /> Chaque sauvegarde de base de données peut éventuellement servir de base pour une série d’une ou de plusieurs [sauvegardes de bases de données différentielles](../../relational-databases/backup-restore/differential-backups-sql-server.md).|  
    |Base de données partielle|Les [sauvegardes partielles](../../relational-databases/backup-restore/partial-backups-sql-server.md)couvrent les groupes de fichiers en lecture/écriture et, éventuellement, un ou plusieurs fichiers ou groupes de fichiers en lecture seule.<br /><br /> Chaque sauvegarde partielle peut éventuellement servir de base pour une série d’une ou de plusieurs [sauvegardes partielles différentielles](../../relational-databases/backup-restore/differential-backups-sql-server.md).|  
    |Fichier ou groupe de fichiers|Les [sauvegardes de fichiers](../../relational-databases/backup-restore/full-file-backups-sql-server.md) couvrent un ou plusieurs fichiers ou groupes de fichiers et ne conviennent qu’aux bases de données contenant plusieurs groupes de fichiers. En mode de récupération simple, les sauvegardes de fichiers se limitent essentiellement aux groupes de fichiers secondaires en lecture seule.<br /> Chaque sauvegarde de fichiers peut éventuellement servir de base pour une série d’une ou de plusieurs [sauvegardes de fichiers différentielles](../../relational-databases/backup-restore/differential-backups-sql-server.md).|  
  
-   En mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, les sauvegardes standard incluent également les *sauvegardes des journaux de transactions* (ou *sauvegardes de fichier journal*) séquentielles qui sont nécessaires. Chaque sauvegarde de fichier journal couvre la partie du journal des transactions qui est active au moment de la création de la sauvegarde et inclut tous les enregistrements de journal qui n'ont pas été sauvegardés lors d'une précédente sauvegarde de journal.  
  
     Pour réduire au maximum les risques de perte de travail, mais avec un coût en termes de charge d'administration, vous devez planifier des sauvegardes de fichier journal fréquentes. La planification de sauvegardes différentielles entre des sauvegardes complètes peut réduire le temps de restauration en diminuant le nombre de sauvegardes de fichier journal à restaurer après la restauration des données.  
  
     Nous vous recommandons de placer les sauvegardes de fichier journal sur un autre volume que les sauvegardes de base de données.  
  
    > [!NOTE]  
    > Avant de pouvoir créer la première sauvegarde du fichier journal, vous devez créer une sauvegarde complète.  
  
-   Une *sauvegarde en copie seule* est une sauvegarde complète ou une sauvegarde de fichier journal qui est réalisée dans un but précis, et qui est indépendante de la séquence normale des sauvegardes standard. Pour créer une sauvegarde en copie seule, spécifiez l'option COPY_ONLY dans votre instruction BACKUP. Pour plus d’informations, consultez [Sauvegardes de copie uniquement &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).  
  
###  <a name="Tlog_Truncation"></a> Troncation du journal des transactions  
 Pour éviter de remplir le journal des transactions d'une base de données, les sauvegardes régulières sont essentielles. En mode de récupération simple, la troncation du journal se produit automatiquement après la sauvegarde de la base de données ; en mode de récupération complète, elle se produit automatiquement après la sauvegarde du journal des transactions. Toutefois, le processus de troncation peut parfois être différé. Pour plus d’informations sur les facteurs susceptibles de retarder la troncation du journal, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
> [!NOTE]  
> Les options `BACKUP LOG WITH NO_LOG` et `WITH TRUNCATE_ONLY` ne sont plus disponibles. Si vous utilisez le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions et si vous devez supprimer la chaîne de sauvegarde de fichier journal d'une base de données, passez au mode de récupération simple. Pour plus d’informations, consultez [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
###  <a name="Formatting_Media"></a> Formatage du support de sauvegarde  
 Le support de sauvegarde est formaté par une instruction BACKUP si, et uniquement si, l'une des conditions suivantes est vraie :  
  
-   L’option `FORMAT` est spécifiée.  
-   Le support est vide.  
-   L'opération écrit sur une bande magnétique de sauvegarde consécutive.  
  
###  <a name="Backup_Devices_and_Media_Sets"></a> Utilisation des unités de sauvegarde et des supports de sauvegarde  
  
#### <a name="backup-devices-in-a-striped-media-set-a-stripe-set"></a>Unités de sauvegarde d’un jeu de sauvegarde distribuée (jeu de bandes)  
 Un *jeu de bandes* est un ensemble de fichiers disque sur lesquels les données sont divisées en blocs et distribuées dans un ordre fixe. Le nombre d’unités de sauvegarde utilisées dans un jeu de bandes doit rester le même (sauf si le support est réinitialisé avec`FORMAT`).  
  
 L'exemple ci-dessous écrit une sauvegarde de la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] sur un nouveau jeu de sauvegarde distribuée qui utilise trois fichiers disque.  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO DISK='X:\SQLServerBackups\AdventureWorks1.bak',   
DISK='Y:\SQLServerBackups\AdventureWorks2.bak',   
DISK='Z:\SQLServerBackups\AdventureWorks3.bak'  
WITH FORMAT,  
   MEDIANAME = 'AdventureWorksStripedSet0',  
   MEDIADESCRIPTION = 'Striped media set for AdventureWorks2012 database;  
GO  
```  
  
 Lorsqu'une unité de sauvegarde a été définie comme faisant partie d'un jeu de bandes, elle ne peut plus être utilisée pour une sauvegarde d'unité unique, sauf si l'option FORMAT est spécifiée. De la même façon, une unité qui contient des sauvegardes non agrégées ne peut pas être utilisée dans un jeu d'agrégats, sauf si l'option FORMAT est spécifiée. Pour diviser un jeu de sauvegarde par bandes, utilisez l'option FORMAT.  
  
 Si ni MEDIANAME ni MEDIADESCRIPTION n’est spécifié lorsqu’un en-tête de support est écrit, le champ d’en-tête de support correspondant à l’élément non spécifié est vide.  
  
#### <a name="working-with-a-mirrored-media-set"></a>Utilisation d’un support de sauvegarde miroir  
 En règle générale, les sauvegardes ne sont pas mises en miroir et les instructions BACKUP contiennent simplement une clause TO. Toutefois, il est possible de créer jusqu'à quatre miroirs par support de sauvegarde. Dans le cas d'un support de sauvegarde miroir, l'opération de sauvegarde écrit dans plusieurs groupes d'unités de sauvegarde. Chaque groupe d'unités de sauvegarde constitue un miroir au sein du support de sauvegarde miroir. Chaque miroir doit utiliser le même nombre et le même type d'unités de sauvegarde physiques, lesquelles doivent toutes avoir les mêmes propriétés.  
  
 Pour effectuer une sauvegarde dans un support de sauvegarde miroir, tous les miroirs doivent être présents. Pour effectuer une sauvegarde sur un support de sauvegarde miroir, spécifiez la clause `TO` pour le premier miroir et spécifiez une clause `MIRROR TO` pour chacun des autres miroirs.  
  
 Pour un support de sauvegarde miroir, chaque clause `MIRROR TO` doit contenir le même nombre et le même type d’unités que la clause TO. L'exemple suivant écrit dans un support de sauvegarde miroir contenant deux miroirs et utilisant trois unités par miroir :  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO DISK='X:\SQLServerBackups\AdventureWorks1a.bak',   
  DISK='Y:\SQLServerBackups\AdventureWorks2a.bak',   
  DISK='Z:\SQLServerBackups\AdventureWorks3a.bak'  
MIRROR TO DISK='X:\SQLServerBackups\AdventureWorks1b.bak',   
  DISK='Y:\SQLServerBackups\AdventureWorks2b.bak',   
  DISK='Z:\SQLServerBackups\AdventureWorks3b.bak';  
GO  
```  
  
> [!IMPORTANT]  
> Cet exemple vous permet d'effectuer un test sur votre système local. En pratique, effectuer une sauvegarde dans plusieurs unités se trouvant sur le même lecteur nuit aux performances et élimine la redondance pour laquelle les supports de sauvegarde miroirs sont conçus.  
  
##### <a name="media-families-in-mirrored-media-sets"></a>Familles de supports de sauvegarde miroirs  
 Chaque unité de sauvegarde spécifiée dans la clause `TO` d’une instruction BACKUP correspond à une famille de supports. Par exemple, si la clause `TO` répertorie trois unités, BACKUP écrit des données dans trois familles de supports. Dans un support de sauvegarde miroir, chaque miroir doit contenir une copie de chacune des familles de supports. C'est pour cette raison que le nombre d'unités doit être identique pour tous les miroirs.  
  
 Si plusieurs unités sont spécifiées pour chaque miroir, leur ordre détermine la famille de supports qui est écrite sur chacune d'elles. Par exemple, dans chaque liste d'unités, la deuxième unité correspond à la deuxième famille de supports. Le tableau suivant établit la correspondance entre unités et familles de supports pour les unités de l'exemple ci-dessus.  
  
|Miroir|Famille de supports 1|Famille de supports 2|Famille de supports 3|  
|---------|---------|---------|---------|  
|0|`Z:\AdventureWorks1a.bak`|`Z:\AdventureWorks2a.bak`|`Z:\AdventureWorks3a.bak`|  
| 1|`Z:\AdventureWorks1b.bak`|`Z:\AdventureWorks2b.bak`|`Z:\AdventureWorks3b.bak`|  
  
 Une famille de supports doit toujours être sauvegardée sur la même unité à l'intérieur d'un miroir donné. Par conséquent, à chaque fois que vous utilisez un support de sauvegarde existant, répertoriez les unités de chaque miroir dans l'ordre dans lequel elles ont été spécifiées au moment de la création du support de sauvegarde.  
  
 Pour plus d’informations sur les jeux de supports en miroir, consultez [Jeux de supports de sauvegarde en miroir &#40;SQL Server&#41;](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md). Pour plus d’informations sur les supports de sauvegarde en général, consultez [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
###  <a name="Restoring_Backups"></a> Restauration de sauvegardes SQL Server  
 Pour restaurer une base de données et, éventuellement, la récupérer pour la mettre en ligne, ou pour restaurer un fichier ou un groupe de fichiers, utilisez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) ou les tâches [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] de **restauration**. Pour plus d’informations, consultez [Vue d’ensemble de la restauration et de la récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
##  <a name="Additional_Considerations"></a> Considérations supplémentaires à propos des options BACKUP  
  
###  <a name="Interactions_SKIP_etc"></a> Interactions de SKIP, NOSKIP, INIT et NOINIT  
 Ce tableau décrit les interactions entre les options { **NOINIT** | INIT } et { **NOSKIP** | SKIP }.  
  
> [!NOTE]  
> Si le support de bande est vide ou que le fichier de sauvegarde sur disque n'existe pas, toutes ces interactions écrivent un en-tête de support, puis se poursuivent. Si le support n'est pas vide et qu'il ne contient pas d'en-tête de support valide, ces opérations signalent qu'il ne s'agit pas d'un support MTF valide et mettent fin à l'opération de sauvegarde.  
  
||NOINIT|INIT|  
|------|------------|----------|  
|NOSKIP|Si le volume contient un en-tête de support valide, vérifie que le nom du support correspond à la valeur de `MEDIANAME`, si elle est spécifiée. Si les deux noms correspondent, ajoute le jeu de sauvegarde en gardant ceux qui existent déjà.<br /> Si le volume ne contient pas d'en-tête de support valide, une erreur se produit.|Si le volume contient un en-tête de support valide, effectue les vérifications suivantes :<br /><ul><li>Si `MEDIANAME` a été spécifié, vérifie que le nom indiqué correspond à celui qui est mentionné dans l’en-tête du support.<sup>1</sup></li><li>Vérifie qu'il n'y a pas déjà sur le support des jeux de sauvegardes qui ne seraient pas encore arrivés à expiration. S'il y en a, met fin à la sauvegarde.</li></ul><br />Si toutes ces vérifications sont validées, écrase tous les jeux de sauvegardes présents sur le support en ne conservant que l'en-tête de support.<br /> Si le volume ne contient pas d’en-tête de support valide, en génère un en utilisant les valeurs de `MEDIANAME` et `MEDIADESCRIPTION`, si elles sont spécifiées.|  
|SKIP|Si le volume contient un en-tête de support valide, ajoute le jeu de sauvegarde, en gardant ceux qui existent déjà.|Si le volume contient un en-tête de support valide<sup>2</sup>, écrase tous les jeux de sauvegardes présents sur le support en ne gardant que l’en-tête.<br /> Si le support est vide, génère un en-tête de support en utilisant les valeurs de `MEDIANAME` et `MEDIADESCRIPTION`, si elles sont spécifiées.|  

<sup>1</sup> L’utilisateur doit avoir le rôle serveur ou le rôle de base de données fixe approprié pour effectuer une opération de sauvegarde.    

<sup>2</sup> Pour être valide, il doit faire état du numéro de version MTF et d’autres informations d’en-tête. Si la version indiquée n'est pas prise en charge ou pas reconnue, une erreur se produit.  
  
## <a name="compatibility"></a>Compatibilité  
  
> [!CAUTION]  
> Les sauvegardes créées avec une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
BACKUP prend en charge l’option `RESTART` pour assurer la compatibilité descendante avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mais RESTART est inopérant.  
  
## <a name="general-remarks"></a>Remarques d’ordre général  
Les sauvegardes de base de données ou de fichier journal peuvent être ajoutées à n'importe quel périphérique de disque ou à bandes, ce qui permet de conserver au même emplacement physique la base de données et ses journaux de transactions.  
  
L'instruction BACKUP n'est pas autorisée dans une transaction explicite ou implicite.  
  
Les opérations de sauvegarde inter-plateformes, impliquant éventuellement des types de processeurs différents, peuvent être réalisées tant que le classement de la base de données est pris en charge par le système d'exploitation.  
 
Lorsque vous utilisez la compression de sauvegarde avec des bases de données ne comprenant qu’un seul fichier de données et où [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) est activé, il est recommandé d’utiliser pour le paramètre `MAXTRANSFERSIZE` une valeur **supérieure à 65 536 (64 Ko)**.   
Avec [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures, cela permet l’utilisation d’un algorithme de compression optimisé pour les bases de données chiffrées avec TDE, qui chiffre d’abord une page, la compresse, puis la chiffre de nouveau. Si vous utilisez `MAXTRANSFERSIZE = 65536` (64 Ko), la compression de sauvegarde pour les bases de données chiffrées avec TDE va directement compresser les pages chiffrées et peut ne pas fournir de bons taux de compression. Pour plus d’informations, consultez [Backup Compression for TDE-enabled Databases](http://blogs.msdn.microsoft.com/sqlcat/2016/06/20/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases/).

> [!NOTE]  
> Dans certains cas, la valeur par défaut `MAXTRANSFERSIZE` est supérieure à 64 Ko :
> * Quand la base de données a plusieurs fichiers de données créés, elle utilise `MAXTRANSFERSIZE` > 64 Ko
> * Quand vous effectuez une sauvegarde vers une URL, la valeur par défaut `MAXTRANSFERSIZE = 1048576` (1 Mo)
>   
> Même si une de ces conditions s’applique, vous devez définir explicitement `MAXTRANSFERSIZE` supérieure à 64 Ko dans votre commande de sauvegarde afin d’obtenir le nouvel algorithme de compression de sauvegarde.
  
Par défaut, chaque opération de sauvegarde réussie ajoute une entrée au journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et au journal des événements système. Si vous sauvegardez très fréquemment le journal, ces messages de réussite peuvent rapidement s'accumuler, créer des journaux d'erreurs très volumineux et compliquer la recherche d'autres messages. Dans de tels cas, vous pouvez supprimer ces entrées de journal en utilisant l'indicateur de trace 3226 si aucun de vos scripts ne dépend de ces entrées. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  
  
## <a name="interoperability"></a>Interopérabilité  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un processus de sauvegarde en ligne pour permettre qu’une base de données soit sauvegardée alors qu’elle est encore utilisée. Lors d'une sauvegarde, la plupart des opérations sont possibles ; par exemple, les instructions INSERT, UPDATE et DELETE sont autorisées.  
  
 Parmi les opérations qui ne peuvent pas être effectuées lors d'une sauvegarde de base de données ou du journal des transactions, citons :  
  
-   Les opérations de gestion de fichiers telles que l’instruction `ALTER DATABASE` avec les options `ADD FILE` ou `REMOVE FILE`.  
  
-   Les opérations de compactage de base de données ou de fichier. Cela comprend également les opérations de compactage automatique.  
  
Si une opération de sauvegarde chevauche une opération de réduction ou de gestion des fichiers, un conflit se produit. Quelle que soit l'opération effectuée la première, la seconde opération attend que le verrou défini par la première opération expire (le délai d'expiration est contrôlé par un paramètre d'expiration de la session). Si le verrou est libéré au cours du délai d'expiration, la seconde opération se poursuit. Si le verrou expire, la seconde opération échoue.  

## <a name="limitations-for-sql-database-managed-instance"></a>Limitations pour SQL Database Managed Instance
SQL Database Managed Instance peut sauvegarder une base de données sur une sauvegarde pouvant contenir jusqu’à 32 bandes, ce qui est suffisant pour les bases de données d’une taille maximale de 4 To si la compression de la sauvegarde est activée.

La taille maximale d’une bande de sauvegarde est de 195 Go (taille maximale de l’objet blob). Augmentez le nombre de bandes défini dans la commande de sauvegarde pour réduire la taille de chaque bande et ainsi ne pas dépasser cette limite.

> [!NOTE]
> Pour contourner cette limitation en local, effectuez la sauvegarde sur `DISK` et non sur `URL`, chargez le fichier de sauvegarde dans l’objet blob, puis effectuez une restauration. La restauration est possible pour des fichiers plus gros, car un autre type d’objet blob est utilisé.

 
## <a name="metadata"></a>Métadonnées  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intègre les tables d'historique de sauvegarde suivantes pour assurer le suivi des activités de sauvegarde :  
  
-   [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)  
-   [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
-   [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
-   [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
-   [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
Si une restauration est effectuée et si le jeu de sauvegarde n’est pas encore enregistré dans la base de données **msdb**, les tables d’historique de sauvegarde peuvent être modifiées.  
  
## <a name="security"></a>Sécurité  
 À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les options `PASSWORD` et `MEDIAPASSWORD` ne sont plus disponibles pour la création de sauvegardes. Il est encore possible de restaurer des sauvegardes créées avec des mots de passe.  
  
### <a name="permissions"></a>Autorisations  
 Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .  
  
 Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d'écrire sur l'unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doit avoir des autorisations d'écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. De tels problèmes pour le fichier physique de l'unité de sauvegarde peuvent n'apparaître que lorsque la ressource physique est sollicitée au moment de la sauvegarde ou de la restauration.  
  
##  <a name="examples"></a> Exemples  
 Cette section contient les exemples suivants :  
  
-   A. [Sauvegarde d'une base de données complète](#backing_up_db)  
-   B. [Sauvegarde de la base de données et du journal](#backing_up_db_and_log)  
-   C. [Création d’une sauvegarde de fichiers complète à partir de groupes de fichiers secondaires](#full_
-   file_backup)  
-   D. [Création d’une sauvegarde de fichiers différentielle à partir de groupes de fichiers secondaires](#differential_file_backup)  
-   E. [Création et sauvegarde sur un support de sauvegarde miroir d’une seule famille](#create_single_family_mirrored_media_set)  
-   F. [Création et sauvegarde sur un support de sauvegarde miroir de plusieurs familles](#create_multifamily_mirrored_media_set)  
-   G [Sauvegarde sur un support de sauvegarde miroir existant](#existing_mirrored_media_set)  
-   H. [Création d’une sauvegarde compressée sur un nouveau support de sauvegarde](#creating_compressed_backup_new_media_set)  
-   I. [Sauvegarde dans le service Stockage Blob Microsoft Azure](#url)  
  
> [!NOTE]  
> Les rubriques de procédures de sauvegarde contiennent des exemples supplémentaires. Pour plus d’informations, consultez [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
###  <a name="backing_up_db"></a> A. Sauvegarde d'une base de données complète  
 L’exemple ci-dessous sauvegarde la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] dans un fichier disque.  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
 TO DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'  
   WITH FORMAT;  
GO  
```  
  
###  <a name="backing_up_db_and_log"></a> B. Sauvegarde de la base de données et du journal  
 L'exemple suivant sauvegarde l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] qui utilise par défaut le mode de récupération simple. Pour prendre en charge les sauvegardes de fichier journal, la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] est modifiée pour utiliser le mode de récupération complète.  
  
 Ensuite, l’exemple utilise [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) pour créer une [unité de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md) logique où sauvegarder les données (`AdvWorksData`), puis crée une autre unité de sauvegarde logique où sauvegarder le journal (`AdvWorksLog`).  
  
 Enfin, l'exemple crée une sauvegarde complète de la base de données dans `AdvWorksData` et, après la mise à jour, sauvegarde le journal dans `AdvWorksLog`.  
  
```sql  
-- To permit log backups, before the full database backup, modify the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
   SET RECOVERY FULL;  
GO  
-- Create AdvWorksData and AdvWorksLog logical backup devices.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksData',   
'Z:\SQLServerBackups\AdvWorksData.bak';  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksLog',   
'X:\SQLServerBackups\AdvWorksLog.bak';  
GO  
  
-- Back up the full AdventureWorks2012 database.  
BACKUP DATABASE AdventureWorks2012 TO AdvWorksData;  
GO  
-- Back up the AdventureWorks2012 log.  
BACKUP LOG AdventureWorks2012  
   TO AdvWorksLog;  
GO  
```  
  
> [!NOTE]  
>  Pour une base de données de production, sauvegardez régulièrement le journal. Les sauvegardes du journal doivent être suffisamment fréquentes pour assurer une protection contre la perte des données.  
  
###  <a name="full_file_backup"></a> C. Création d'une sauvegarde de fichiers complète des groupes de fichiers secondaires  
 L'exemple suivant crée une sauvegarde complète de tous les fichiers se trouvant dans les deux groupes de fichiers secondaires.  
  
```sql  
--Back up the files in SalesGroup1:  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck';  
GO  
```  
  
###  <a name="differential_file_backup"></a> D. Création d'une sauvegarde de fichiers différentielle des groupes de fichiers secondaires  
 L'exemple suivant crée une sauvegarde différentielle de tous les fichiers se trouvant dans les deux groupes de fichiers secondaires.  
  
```sql  
--Back up the files in SalesGroup1:  
BACKUP DATABASE Sales  
   FILEGROUP = 'SalesGroup1',  
   FILEGROUP = 'SalesGroup2'  
   TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck'  
   WITH   
      DIFFERENTIAL;  
GO  
```  
  
###  <a name="create_single_family_mirrored_media_set"></a> E. Création et sauvegarde dans un support de sauvegarde miroir d'une seule famille  
 L'exemple ci-dessous illustre la création d'un support de sauvegarde miroir contenant une seule famille de supports et quatre miroirs, ainsi que la sauvegarde de la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] dans ces derniers.  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO TAPE = '\\.\tape0'  
MIRROR TO TAPE = '\\.\tape1'  
MIRROR TO TAPE = '\\.\tape2'  
MIRROR TO TAPE = '\\.\tape3'  
WITH  
   FORMAT,  
   MEDIANAME = 'AdventureWorksSet0';  
```  
  
###  <a name="create_multifamily_mirrored_media_set"></a> F. Création et sauvegarde dans un support de sauvegarde miroir de plusieurs familles  
 L'exemple suivant illustre la création d'un support de sauvegarde miroir dans lequel chaque miroir contient deux familles de supports. La base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] est sauvegardée sur les deux miroirs.  
  
```sql  
BACKUP DATABASE AdventureWorks2012  
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'  
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'  
WITH  
   FORMAT,  
   MEDIANAME = 'AdventureWorksSet1';  
```  
  
###  <a name="existing_mirrored_media_set"></a> G. Sauvegarde dans un support de sauvegarde miroir existant  
 L'exemple suivant illustre l'ajout d'un jeu de sauvegarde au support de sauvegarde créé dans l'exemple précédent.  
  
```sql  
BACKUP LOG AdventureWorks2012  
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'  
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'  
WITH   
   NOINIT,  
   MEDIANAME = 'AdventureWorksSet1';  
```  
  
> [!NOTE]  
>  NOINIT, par défaut, est indiqué ici pour plus de clarté.  
  
###  <a name="creating_compressed_backup_new_media_set"></a> H. Création d'une sauvegarde compressée dans un nouveau support de sauvegarde  
 L'exemple suivant illustre le formatage du support avec la création d'un nouveau jeu de médias et une sauvegarde complète compressée de la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)].  
  
```sql  
BACKUP DATABASE AdventureWorks2012 TO DISK='Z:\SQLServerBackups\AdvWorksData.bak'   
WITH   
   FORMAT,   
   COMPRESSION;  
```  

###  <a name="url"></a> I. Sauvegarde du service de stockage d’objets blob Microsoft Azure 
L’exemple effectue une sauvegarde complète de la base de données `Sales` vers le service Microsoft Azure Storage Blob.  Le nom du compte de stockage est `mystorageaccount`.  Le conteneur se nomme `myfirstcontainer`.  Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture, suppression et liste.  Les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (`https://mystorageaccount.blob.core.windows.net/myfirstcontainer`) ont été créées à l’aide d’une signature d’accès partagé associée à la stratégie d’accès stockée.  Pour plus d’informations sur la sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le service Microsoft Azure Storage Blob, consultez [Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) et [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

```sql  
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5;
```

  
## <a name="see-also"></a> Voir aussi  
 [Unités de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Jeux de supports, familles de supports et jeux de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [Sauvegardes de la fin du journal &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [DBCC SQLPERF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_helpfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Restauration fragmentaire de bases de données avec des tables à mémoire optimisée](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md)  
  
  
