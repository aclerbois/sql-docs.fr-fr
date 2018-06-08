---
title: SQL Server, objet Deprecated Features | Microsoft Docs
ms.custom: ''
ms.date: 05/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Deprecated Features
- performance counters [SQL Server], deprecated features
- deprecation [SQL Server], performance counters
- Deprecated Features object
ms.assetid: e95de9d6-c950-41cd-8aaa-be529c6de198
caps.latest.revision: 61
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 92353cf4dde604e191d26dc971edf83f16ae4b51
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-deprecated-features-object"></a>SQL Server, objet Deprecated Features
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  L'objet SQLServer:Deprecated Features de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit un compteur permettant de contrôler les fonctionnalités désignées comme dépréciées. Dans tous les cas, le compteur fournit un nombre d'utilisations indiquant combien de fois la fonctionnalité dépréciée a été rencontrée depuis le dernier démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 La valeur de ces compteurs est également disponible en exécutant l’instruction suivante :  
  
```  
SELECT * FROM sys.dm_os_performance_counters   
WHERE object_name = 'SQLServer:Deprecated Features';  
```  

Le tableau suivant décrit l’objet de performance **Deprecated Features** .

|**Compteur de l’objet SQL Server Deprecated Features**|Description|  
|-------------|-----------------|  
|**Utilisation**|Utilisation des fonctionnalités depuis le dernier démarrage de SQL Server.|
  
 Le tableau suivant décrit les instances du compteur de l'objet SQL Server Deprecated Features.  
  
|Instances du compteur de l'objet SQL Server Deprecated Features|Description|  
|------------------------------------------------------|-----------------|  
|'#' et '##' comme nom des tables temporaires et procédures stockées|Un identifiant ne contenant pas d'autres caractères que # a été rencontré. Utilisez au moins un caractère supplémentaire. Se produit une fois par compilation.|  
|Syntaxe d'appel de fonction '::'|La syntaxe d'appel de fonction :: a été rencontrée pour une fonction table. Remplacez par `SELECT column_list FROM` *<nom_fonction>*`()`. Par exemple, remplacez `SELECT * FROM ::fn_virtualfilestats(2,1)` par `SELECT * FROM sys.fn_virtualfilestats(2,1)`. Se produit une fois par compilation.|  
|\@ et noms commençant par \@\@ comme identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)]|Un identificateur commençant par \@ ou \@\@ a été trouvé. N’utilisez pas \@, \@v@ ou des noms commençant par \@\@ comme identificateurs. Se produit une fois par compilation.|  
|ADDING TAPE DEVICE|La fonctionnalité dépréciée sp_addumpdevice'**bande**' a été rencontrée. Utilisez à la place sp_addumpdevice'**disque**'. Se produit une fois par utilisation.|  
|Autorisation ALL|Nombre total de fois où la syntaxe GRANT ALL, DENY ALL ou REVOKE ALL a été rencontrée. Modifiez la syntaxe pour refuser des autorisations spécifiques. Se produit une fois par requête.|  
|ALTER DATABASE WITH TORN_PAGE_DETECTION|Nombre total d'utilisations de l'option TORN_PAGE_DETECTION de la fonctionnalité déconseillée ALTER DATABASE depuis le démarrage de l'instance du serveur. Utilisez à la place la syntaxe PAGE_VERIFY. Se produit une fois par utilisation dans une instruction DDL.|  
|ALTER LOGIN WITH SET CREDENTIAL|La syntaxe de fonctionnalité déconseillée ALTER LOGIN WITH SET CREDENTIAL ou ALTER LOGIN WITH NO CREDENTIAL a été rencontrée. Utilisez à la place la syntaxe ADD ou DROP CREDENTIAL. Se produit une fois par compilation.|  
|Azeri_Cyrilllic_90|L'événement se produit une fois par démarrage de base de données et une fois par utilisation de classement. Prévoyez de modifier les applications qui utilisent ce classement.|  
|Azeri_Latin_90|L'événement se produit une fois par démarrage de base de données et une fois par utilisation de classement. Prévoyez de modifier les applications qui utilisent ce classement.|  
|BACKUP DATABASE ou LOG TO TAPE|La fonctionnalité déconseillée BACKUP { DATABASE &#124; LOG } TO TAPE ou BACKUP { DATABASE &#124; LOG } TO *périphérique_à_bandes* a été rencontrée.<br /><br /> Utilisez à la place BACKUP { DATABASE &#124; LOG } TO DISK ou BACKUP { DATABASE &#124; LOG } TO *unité_de_disque*. Se produit une fois par utilisation.|  
|BACKUP DATABASE ou LOG WITH MEDIAPASSWORD|La fonctionnalité déconseillée BACKUP DATABASE WITH MEDIAPASSWORD ou BACKUP LOG WITH MEDIAPASSWORD a été rencontrée. N'utilisez pas WITH MEDIAPASSWORD.|  
|BACKUP DATABASE ou LOG WITH PASSWORD|La fonctionnalité déconseillée BACKUP DATABASE WITH PASSWORD ou BACKUP LOG WITH PASSWORD a été rencontrée. N'utilisez pas WITH PASSWORD.|  
|COMPUTE [BY]|La syntaxe COMPUTE ou COMPUTE BY a été rencontrée. Réécrivez la requête de manière à utiliser GROUP BY avec ROLLUP. Se produit une fois par compilation.|  
|CREATE FULLTEXT CATLOG IN PATH|Une instruction CREATE FULLTEXT CATLOG avec la clause IN PATH a été rencontrée. Cette clause n'a aucun effet dans cette version de SQL Server. Se produit une fois par utilisation.|  
|CREATE TRIGGER WITH APPEND|Une instruction CREATE TRIGGER avec la clause WITH APPEND a été rencontrée. Recréez à la place le déclencheur entier. Se produit une fois par utilisation dans une instruction DDL.|  
|CREATE_DROP_DEFAULT|La syntaxe CREATE DEFAULT ou DROP DEFAULT a été rencontrée. Réécrivez la commande en utilisant l'option DEFAULT de CREATE TABLE ou ALTER TABLE. Se produit une fois par compilation.|  
|CREATE_DROP_RULE|La syntaxe CREATE RULE a été rencontrée. Réécrivez la commande en utilisant des contraintes. Se produit une fois par compilation.|  
|Types de données text, ntext ou image|Un type de données **text**, **ntext**ou **image** a été rencontré. Réécrivez les applications de manière à utiliser le type de données **varchar(max)** et à supprimer la syntaxe des types de données **text**, **ntext**et **image** . Se produit une fois par requête.|  
||Nombre total de fois où le niveau de compatibilité 80 a été appliqué à une base de données. Projetez de mettre à niveau la base de données et l'application avant la prochaine version. Se produit également lorsqu’une base de données présentant le niveau de compatibilité 80 est démarrée.|  
|Niveau de compatibilité de base de données 100, 110, 120|Nombre total de fois où le niveau de compatibilité d’une base de données a été modifié. Projetez de mettre à niveau la base de données et l'application avant la prochaine version. Se produit également lorsqu’une base de données ayant un niveau de compatibilité déconseillé est démarrée.|  
|DATABASE_MIRRORING|Des références à la fonctionnalité de mise en miroir de bases de données ont été rencontrées. Prévoyez d’effectuer une mise à niveau vers des groupes de disponibilité Always On, ou si vous exécutez une édition de SQL Server qui ne prend pas en charge les groupes de disponibilité Always On, planifiez une migration vers la copie des journaux de transaction.|  
|database_principal_aliases|Des références à la fonctionnalité déconseillée sys.database_principal_aliases ont été rencontrées. Utilisez des rôles à la place d'alias. Se produit une fois par compilation.|  
|DATABASEPROPERTY|Une instruction a référencé DATABASEPROPERTY. Remplacez l'instruction DATABASEPROPERTY par DATABASEPROPERTYEX. Se produit une fois par compilation.|  
|DATABASEPROPERTYEX('IsFullTextEnabled')|Une instruction a référencé la propriété DATABASEPROPERTYEX IsFullTextEnabled. La valeur de cette propriété est sans effet. Les bases de données utilisateur sont toujours activées pour la recherche en texte intégral. N'utilisez pas cette propriété. Se produit une fois par compilation.|  
|DBCC [UN] PINTABLE|L'instruction DBCC PINTABLE ou DBCC UNPINTABLE a été rencontrée. Cette instruction est sans effet et doit être supprimée. Se produit une fois par requête.|  
|DBCC DBREINDEX|L'instruction DBCC DBREINDEX a été rencontrée. Réécrivez l'instruction de manière à utiliser l'option REBUILD de ALTER INDEX. Se produit une fois par requête.|  
|DBCC INDEXDEFRAG|L'instruction DBCC INDEXDEFRAG a été rencontrée. Réécrivez l'instruction de manière à utiliser l'option REORGANIZE de ALTER INDEX. Se produit une fois par requête.|  
|DBCC SHOWCONTIG|L'instruction DBCC SHOWCONTIG a été rencontrée. Interrogez sys.dm_db_index_physical_stats pour obtenir ces informations. Se produit une fois par requête.|  
|Mot clé DEFAULT comme valeur par défaut.|Une syntaxe qui utilise le mot clé DEFAULT comme valeur par défaut a été rencontrée. Ne pas utiliser. Se produit une fois par compilation.|  
|Algorithme de chiffrement déconseillé|L'algorithme de chiffrement RC4 déconseillé sera supprimé dans la prochaine version de SQL Server. Évitez d'utiliser cette fonctionnalité dans tout nouveau travail de développement et prévoyez de modifier les applications qui l'utilisent. L'algorithme RC4 est faible et uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et version ultérieure, le matériel chiffré à l'aide de RC4 ou de RC4_128 peut être déchiffré dans n'importe quel niveau de compatibilité.|  
|Algorithme de hachage déconseillé|Utilisation des algorithmes MD2, MD4, MD5, SHA ou SHA1.|  
|Algorithme DESX|Une syntaxe qui utilise l'algorithme de chiffrement DESX a été rencontrée. Utilisez un autre algorithme pour le chiffrement. Se produit une fois par compilation.|  
|dm_fts_active_catalogs|Le compteur dm_fts_active_catalogs reste toujours à 0 car certaines colonnes de la vue sys.dm_fts_active_catalogs ne sont pas déconseillées. Pour surveiller une colonne déconseillée, utilisez le compteur spécifique à la colonne ; par exemple, dm_fts_active_catalogs.is_paused.|  
|dm_fts_active_catalogs.is_paused|La colonne is_paused de la vue de gestion dynamique [sys.dm_fts_active_catalogs](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md) a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|dm_fts_active_catalogs.previous_status|La colonne previous_status de la vue de gestion dynamique sys.dm_fts_active_catalogs a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|dm_fts_active_catalogs.previous_status_description|La colonne previous_status_description de la vue de gestion dynamique sys.dm_fts_active_catalogs a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|dm_fts_active_catalogs.row_count_in_thousands|La colonne row_count_in_thousands de la vue de gestion dynamique sys.dm_fts_active_catalogs a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|dm_fts_active_catalogs.status|La colonne status de la vue de gestion dynamique sys.dm_fts_active_catalogs a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|dm_fts_active_catalogs.status_description|La colonne status_description de la vue de gestion dynamique sys.dm_fts_active_catalogs a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|dm_fts_active_catalogs.worker_count|La colonne worker_count de la vue de gestion dynamique sys.dm_fts_active_catalogs a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|dm_fts_memory_buffers|Le compteur dm_fts_memory_buffers reste toujours à 0 car la plupart des colonnes de la vue sys.dm_fts_memory_buffers ne sont pas déconseillées. Pour surveiller la colonne déconseillée, utilisez le compteur spécifique à la colonne : dm_fts_memory_buffers.row_count.|  
|dm_fts_memory_buffers.row_count|La colonne row_count de la vue de gestion dynamique [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md) a été rencontrée. Évitez d'utiliser cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|DROP INDEX avec nom en deux parties|DROP INDEX contient une syntaxe au format *nom_table.nom_index* . Remplacez-la par la syntaxe *nom_index* ON *nom_table* dans l’instruction DROP INDEX. Se produit une fois par compilation.|  
|EXT_CREATE_ALTER_SOAP_ENDPOINT|L'instruction CREATE ou ALTER ENDPOINT avec l'option FOR SOAP a été rencontrée. Les services Web XML natifs sont déconseillés. Utilisez à la place WFC (Windows Communications Foundation) ou ASP.NET.|  
|EXT_endpoint_webmethods|sys.endpoint_webmethods a été rencontré. Les services Web XML natifs sont déconseillés. Utilisez à la place WFC (Windows Communications Foundation) ou ASP.NET.|  
|EXT_soap_endpoints|sys.soap_endpoints a été rencontré. Les services Web XML natifs sont déconseillés. Utilisez à la place WFC (Windows Communications Foundation) ou ASP.NET.|  
|EXTPROP_LEVEL0TYPE|TYPE a été rencontré dans un level0type. Utilisez SCHEMA comme level0type et TYPE comme level1type. Se produit une fois par requête.|  
|EXTPROP_LEVEL0USER|level0type USER lorsqu'un level1type a également été spécifié. Utilisez USER comme level0type uniquement pour les propriétés étendues directement sur un utilisateur. Se produit une fois par requête.|  
|FASTFIRSTROW|La syntaxe FASTFIRSTROW a été rencontrée. Réécrivez les instructions de manière à utiliser la syntaxe OPTION (FAST *n*). Se produit une fois par compilation.|  
|FILE_ID|La syntaxe FILE_ID a été rencontrée. Réécrivez les instructions de manière à utiliser FILE_IDEX. Se produit une fois par compilation.|  
|fn_get_sql|La fonction fn_get_sql a été compilée. Utilisez sys.dm_exec_sql_text à la place. Se produit une fois par compilation.|  
|fn_servershareddrives|La fonction fn_servershareddrives a été compilée. Utilisez à la place sys.dm_io_cluster_shared_drives. Se produit une fois par compilation.|  
|fn_virtualservernodes|La fonction fn_virtualservernodes a été compilée. Utilisez à la place sys.dm_os_cluster_nodes. Se produit une fois par compilation.|  
|fulltext_catalogs|Le compteur fulltext_catalogs reste toujours à 0 car certaines colonnes de la vue sys.fulltext_catalogs ne sont pas déconseillées. Pour surveiller une colonne déconseillée, utilisez le compteur spécifique à la colonne ; par exemple, fulltext_catalogs.data_space_id. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|fulltext_catalogs.data_space_id|La colonne data_space_id de la vue de catalogue [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) a été rencontrée. N'utilisez pas cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|fulltext_catalogs.file_id|La colonne file_id de l'affichage catalogue sys.fulltext_catalogs a été rencontrée. N'utilisez pas cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|fulltext_catalogs.path|La colonne path de l'affichage catalogue sys.fulltext_catalogs a été rencontrée. N'utilisez pas cette colonne. Se produit chaque fois que l'instance du serveur détecte une référence à la colonne.|  
|FULLTEXTCATALOGPROPERTY('LogSize')|La propriété LogSize de la fonction FULLTEXTCATALOGPROPERTY a été rencontrée. Évitez d'utiliser cette propriété.|  
|FULLTEXTCATALOGPROPERTY('PopulateStatus')|La propriété PopulateStatus de la fonction FULLTEXTCATALOGPROPERTY a été rencontrée. Évitez d'utiliser cette propriété.|  
|FULLTEXTSERVICEPROPERTY('ConnectTimeout')|La propriété ConnectTimeout de la fonction FULLTEXTSERVICEPROPERTY a été rencontrée. Évitez d'utiliser cette propriété.|  
|FULLTEXTSERVICEPROPERTY('DataTimeout')|La propriété DataTimeout de la fonction FULLTEXTSERVICEPROPERTY a été rencontrée. Évitez d'utiliser cette propriété.|  
|FULLTEXTSERVICEPROPERTY('ResourceUsage')|La propriété ResourceUsage de la fonction FULLTEXTSERVICEPROPERTY a été rencontrée. Évitez d'utiliser cette propriété.|  
|GROUP BY ALL|Nombre total de fois où la syntaxe GROUP BY ALL a été rencontrée. Modifiez la syntaxe de manière à effectuer un regroupement en fonction de tables spécifiques.|  
|Hindi|L'événement se produit une fois par démarrage de base de données et une fois par utilisation de classement. Prévoyez de modifier les applications qui utilisent ce classement. Utilisez à la place Indic_General_90.|  
|Indicateur de table HOLDLOCK sans parenthèses||  
|IDENTITYCOL|La syntaxe INDENTITYCOL a été rencontrée. Réécrivez les instructions de manière à utiliser la syntaxe $identity. Se produit une fois par compilation.|  
|Liste de sélection de vue d'index sans COUNT_BIG (*)|La liste de sélection d'une vue indexée d'agrégation doit contenir COUNT_BIG (*).|  
|INDEX_OPTION|Une syntaxe CREATE TABLE, ALTER TABLE ou CREATE INDEX sans parenthèses autour des options a été rencontrée. Réécrivez l'instruction de manière à utiliser la syntaxe actuelle. Se produit une fois par requête.|  
|INDEXKEY_PROPERTY|La syntaxe INDEXKEY_PROPERTY a été rencontrée. Réécrivez les instructions de manière à interroger sys.index_columns. Se produit une fois par compilation.|  
|Indicateurs TVF indirects|L'application indirecte, par le biais d'une vue, des indicateurs de table à une invocation d'une fonction table à plusieurs instructions (TVF) sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|INSERT NULL dans des colonnes TIMESTAMP|Une valeur NULL a été insérée dans une colonne TIMESTAMP. Utilisez à la place une valeur par défaut. Se produit une fois par compilation.|  
|INSERT_HINTS||  
|Korean_Wansung_Unicode|L'événement se produit une fois par démarrage de base de données et une fois par utilisation de classement. Prévoyez de modifier les applications qui utilisent ce classement.|  
|Lithuanian_Classic|L'événement se produit une fois par démarrage de base de données et une fois par utilisation de classement. Prévoyez de modifier les applications qui utilisent ce classement.|  
|Macedonian|L'événement se produit une fois par démarrage de base de données et une fois par utilisation de classement. Prévoyez de modifier les applications qui utilisent ce classement. Utilisez à la place Macedonian_FYROM_90.|  
|MODIFY FILEGROUP READONLY|La syntaxe MODIFY FILEGROUP READONLY a été rencontrée. Réécrivez les instructions de manière à utiliser la syntaxe READ_ONLY. Se produit une fois par compilation.|  
|MODIFY FILEGROUP READWRITE|La syntaxe MODIFY FILEGROUP READWRITE a été rencontrée. Réécrivez les instructions de manière à utiliser la syntaxe READ_WRITE. Se produit une fois par compilation.|  
|Nom de la colonne à plus de deux parties|Une requête a utilisé un nom en 3 ou 4 parties dans la liste de colonnes. Modifiez la requête de manière à utiliser des noms en 2 parties conformes au standard. Se produit une fois par compilation.|  
|Indicateurs de table multiples sans virgule|Un espace a été utilisé comme séparateur des indicateurs de table. Utilisez à la place une virgule. Se produit une fois par compilation.|  
|NOLOCK ou READUNCOMMITTED dans UPDATE ou DELETE|NOLOCK ou READUNCOMMITTED a été rencontré dans la clause FROM d'une instruction UPDATE ou DELETE. Supprimez les indicateurs de table NOLOCK ou READUNCOMMITTED de la clause FROM.|  
|Opérateurs de jointure externe non-ANSI *= ou =\*|Une instruction qui utilise la syntaxe de jointure *= ou =\* a été rencontrée. Réécrivez l'instruction de manière à utiliser la syntaxe de jointure ANSI. Se produit une fois par compilation.|  
|numbered_stored_procedures||  
|numbered_procedure_parameters|Des références à la fonctionnalité déconseillée sys.numbered_procedure_parameters ont été rencontrées. Ne pas utiliser. Se produit une fois par compilation.|  
|numbered_procedures|Des références à la fonctionnalité déconseillée sys.numbered_procedures ont été rencontrées. Ne pas utiliser. Se produit une fois par compilation.|  
|Ancien style RAISEERROR|La syntaxe RAISERROR déconseillée (Format : RAISERROR entier chaîne) a été rencontrée. Réécrivez l'instruction en utilisant la syntaxe RAISERROR actuelle. Se produit une fois par compilation.|  
|OLEDB pour les connexions ad hoc|Le fournisseur SQLOLEDB n'est pas pris en charge. Utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour les connexions ad hoc.|  
|PERMISSIONS|Des références à la fonction intrinsèque PERMISSIONS ont été rencontrées. Interrogez à la place sys.fn_my_permissions. Se produit une fois par requête.|  
|ProcNums|La syntaxe déconseillée ProcNums a été rencontrée. Réécrivez les instructions de manière à supprimer ces références. Se produit une fois par compilation.|  
|READTEXT|La syntaxe READTEXT a été rencontrée. Réécrivez les applications de manière à utiliser le type de données **varchar(max)** et à supprimer la syntaxe du type de données **text** . Se produit une fois par requête.|  
|RESTORE DATABASE ou LOG WITH DBO_ONLY|La syntaxe RESTORE … WITH DBO_ONLY a été rencontrée. Utilisez plutôt RESTORE … RESTRICTED_USER.|  
|RESTORE DATABASE ou LOG WITH MEDIAPASSWORD|La syntaxe RESTORE … WITH MEDIAPASSWORD a été rencontrée. WITH MEDIAPASSWORD fournit un faible niveau de sécurité et doit être supprimé.|  
|RESTORE DATABASE ou LOG WITH PASSWORD|La syntaxe RESTORE … WITH PASSWORD a été rencontrée. WITH PASSWORD fournit un faible niveau de sécurité et doit être supprimé.|  
|Le déclencheur retourne des résultats|Cet événement se produit une fois par appel de déclencheur. Réécrivez le déclencheur de manière à ce qu'il ne retourne pas de jeux de résultats.|  
|ROWGUIDCOL|La syntaxe ROWGUIDCOL a été rencontrée. Réécrivez les instructions de manière à utiliser la syntaxe $rowguid. Se produit une fois par compilation.|  
|SET ANSI_NULLS OFF|La syntaxe SET ANSI_NULLS OFF a été rencontrée. Supprimez cette syntaxe déconseillée. Se produit une fois par compilation.|  
|SET ANSI_PADDING OFF|La syntaxe SET ANSI_PADDING OFF a été rencontrée. Supprimez cette syntaxe déconseillée. Se produit une fois par compilation.|  
|SET CONCAT_NULL_YIELDS_NULL OFF|La syntaxe ET CONCAT_NULL_YIELDS_NULL OFF a été rencontrée. Supprimez cette syntaxe déconseillée. Se produit une fois par compilation.|  
|SET DISABLE_DEF_CNST_CHK|La syntaxe SET DISABLE_DEF_CNST_CHK a été rencontrée. Elle est sans effet. Supprimez cette syntaxe déconseillée. Se produit une fois par compilation.|  
|SET FMTONLY ON|La syntaxe SET FMTONLY a été rencontrée. Supprimez cette syntaxe déconseillée. Se produit une fois par compilation.|  
|SET OFFSETS|La syntaxe SET OFFSETS a été rencontrée. Supprimez cette syntaxe déconseillée. Se produit une fois par compilation.|  
|SET REMOTE_PROC_TRANSACTIONS|La syntaxe de SET REMOTE_PROC_TRANSACTIONS a été rencontrée. Supprimez cette syntaxe déconseillée. Utilisez à la place des serveurs liés et sp_serveroption.|  
|SET ROWCOUNT|La syntaxe SET ROWCOUNT a été rencontrée dans une instruction DELETE, INSERT ou UPDATE. Réécrivez l'instruction en utilisant TOP. Se produit une fois par compilation.|  
|SETUSER|L'instruction SET USER a été rencontrée. Utilisez à la place EXECUTE AS. Se produit une fois par requête.|  
|sp_addapprole|La procédure sp_addapprole a été rencontrée. Utilisez à la place CREATE APPLICATION ROLE. Se produit une fois par requête.|  
|sp_addextendedproc|La procédure sp_addextendedproc a été rencontrée. Utilisez à la place CLR. Se produit une fois par compilation.|  
|sp_addlogin|La procédure sp_addlogin a été rencontrée. Utilisez à la place CREATE LOGIN. Se produit une fois par requête.|  
|sp_addremotelogin|La procédure sp_addremotelogin a été rencontrée. Utilisez à la place des serveurs liés.|  
|sp_addrole|La procédure sp_addrole a été rencontrée. Utilisez à la place CREATE ROLE. Se produit une fois par requête.|  
|sp_addserver|La procédure sp_addserver a été rencontrée. Utilisez à la place des serveurs liés.|  
|sp_addtype|La procédure sp_addtype a été rencontrée. Utilisez à la place CREATE TYPE. Se produit une fois par compilation.|  
|sp_adduser|La procédure sp_adduser a été rencontrée. Utilisez à la place CREATE USER. Se produit une fois par requête.|  
|sp_approlepassword|La procédure sp_approlepassword a été rencontrée. Utilisez à la place ALTER APPLICATION ROLE. Se produit une fois par requête.|  
|sp_attach_db|La procédure sp_attach_db a été rencontrée. Utilisez à la place CREATE DATABASE FOR ATTACH. Se produit une fois par requête.|  
|sp_attach_single_file_db|La procédure sp_single_file_db a été rencontrée. Utilisez à la place CREATE DATABASE FOR ATTACH_REBUILD_LOG. Se produit une fois par requête.|  
|sp_bindefault|La procédure sp_bindefault a été rencontrée. Utilisez à la place le mot clé DEFAULT de ALTER TABLE ou CREATE TABLE. Se produit une fois par compilation.|  
|sp_bindrule|La procédure sp_bindrule a été rencontrée. Utilisez à la place des contraintes de validation. Se produit une fois par compilation.|  
|sp_bindsession|La procédure sp_bindsession a été rencontrée. Utilisez à la place MARS (Multiple Active Result Sets) ou des transactions distribuées. Se produit une fois par compilation.|  
|sp_certify_removable|La procédure sp_certify_removable a été rencontrée. Utilisez à la place sp_detach_db. Se produit une fois par requête.|  
|sp_changeobjectowner|La procédure sp_changeobjectowner a été rencontrée. Utilisez à la place ALTER SCHEMA ou ALTER AUTHORIZATION. Se produit une fois par requête.|  
|sp_change_users_login|La procédure sp_change_users_login a été rencontrée. Utilisez à la place ALTER USER. Se produit une fois par requête.|  
|sp_configure allow updates'|L'option allow updates de sp_configure a été rencontrée. Les tables système ne peuvent plus être mises à jour. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure 'disallow results from triggers'|L'option disallow result sets from triggers de sp_configure a été rencontrée. Pour désactiver les jeux de résultats provenant des déclencheurs, utilisez sp_configure pour affecter la valeur 1 à cette option. Se produit une fois par requête.|  
|sp_configure 'ft crawl bandwidth (max)'|L'option ft crawl bandwidth (max) de sp_configure a été rencontrée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure 'ft crawl bandwidth (min)'|L'option ft crawl bandwidth (min) de sp_configure a été rencontrée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure 'ft notify bandwidth (max)'|L'option ft notify bandwidth (max) de sp_configure a été rencontrée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure 'ft notify bandwidth (min)'|L'option ft notify bandwidth (min) de sp_configure a été rencontrée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure 'locks'|L'option locks de sp_configure a été rencontrée. Les verrous ne peuvent plus être configurés. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure 'open objects'|L'option open objects de sp_configure a été rencontrée. Le nombre d'objets ouverts ne peut plus être configuré. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure "priority boost"|L'option renforcement de priorité de sp_configure a été rencontrée. Ne pas utiliser. Se produit une fois par requête. À la place, utilisez l’option start /high … program.exe de Windows.|  
|sp_configure 'remote proc trans'|L'option remote proc trans de sp_configure a été rencontrée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_configure 'set working set size'|L'option set working set size de sp_configure a été rencontrée. La taille de la plage de travail ne peut plus être configurée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_control_dbmasterkey_password|La procédure stockée sp_control_dbmasterkey_password ne vérifie pas s'il existe une clé principale. Cette opération est autorisée à des fins de compatibilité descendante, mais affiche un avertissement. Ce comportement est déconseillé. Dans une version ultérieure, la clé principale doit exister et le mot de passe utilisé dans la procédure stockée sp_control_dbmasterkey_password doit être identique à un des mots de passe utilisés pour chiffrer la clé principale de la base de données.|  
|sp_create_removable|La procédure sp_create_removable a été rencontrée. Utilisez à la place CREATE DATABASE. Se produit une fois par requête.|  
|sp_db_vardecimal_storage_format|Le format de stockage **vardecimal** a été rencontré. Utilisez à la place la compression de données.|  
|sp_dbcmptlevel|La procédure sp_dbcmptlevel a été rencontrée. Utilisez plutôt ALTER DATABASE … SET COMPATIBILITY_LEVEL. Se produit une fois par requête.|  
|sp_dbfixedrolepermission|La procédure sp_dbfixedrolepermission a été rencontrée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_dboption|La procédure sp_dboption a été rencontrée. Utilisez à la place ALTER DATABASE et DATABASEPROPERTYEX. Se produit une fois par compilation.|  
|sp_dbremove|La procédure sp_dbremove a été rencontrée. Utilisez à la place DROP DATABASE. Se produit une fois par requête.|  
|sp_defaultdb|La procédure sp_defaultdb a été rencontrée. Utilisez à la place ALTER LOGIN. Se produit une fois par compilation.|  
|sp_defaultlanguage|La procédure sp_defaultlanguage a été rencontrée. Utilisez à la place ALTER LOGIN. Se produit une fois par compilation.|  
|sp_denylogin|La procédure sp_denylogin a été rencontrée. Utilisez à la place ALTER LOGIN DISABLE. Se produit une fois par requête.|  
|sp_depends|La procédure sp_depends a été rencontrée. Utilisez à la place sys.dm_sql_referencing_entities et sys.dm_sql_referenced_entities. Se produit une fois par requête.|  
|sp_detach_db \@keepfulltextindexfile|L’argument \@keepfulltextindexfile a été rencontré dans une instruction sp_detach_db. N'utilisez pas cet argument.|  
|sp_dropalias|La procédure sp_dropalias a été rencontrée. Remplacez les alias par une combinaison de comptes d'utilisateurs et de rôles de base de données. Utilisez sp_dropalias pour supprimer les alias dans les bases de données mises à niveau. Se produit une fois par compilation.|  
|sp_dropapprole|La procédure sp_dropapprole a été rencontrée. Utilisez à la place DROP APPLICATION ROLE. Se produit une fois par requête.|  
|sp_dropextendedproc|La procédure sp_dropextendedproc a été rencontrée. Utilisez à la place CLR. Se produit une fois par compilation.|  
|sp_droplogin|La procédure sp_droplogin a été rencontrée. Utilisez à la place DROP LOGIN. Se produit une fois par requête.|  
|sp_dropremotelogin|La procédure sp_dropremotelogin a été rencontrée. Utilisez à la place des serveurs liés.|  
|sp_droprole|La procédure sp_droprole a été rencontrée. Utilisez à la place DROP ROLE. Se produit une fois par requête.|  
|sp_droptype|La procédure sp_droptype a été rencontrée. Utilisez à la place DROP TYPE.|  
|sp_dropuser|La procédure sp_dropuser a été rencontrée. Utilisez à la place DROP USER. Se produit une fois par requête.|  
|sp_estimated_rowsize_reduction_for_vardecimal|Le format de stockage **vardecimal** a été rencontré. Utilisez à la place la compression de données et sp_estimate_data_compression_savings.|  
|sp_fulltext_catalog|La procédure sp_fulltext_catalog a été rencontrée. Utilisez à la place CREATE/ALTER/DROP FULLTEXT CATALOG Se produit une fois par compilation.|  
|sp_fulltext_column|La procédure sp_fulltext_column a été rencontrée. Utilisez à la place ALTER FULLTEXT INDEX. Se produit une fois par compilation.|  
|sp_fulltext_database|La procédure sp_fulltext_database a été rencontrée. Utilisez à la place ALTER DATABASE. Se produit une fois par compilation.|  
|sp_fulltext_service \@action=clean_up|L'option clean_up de la procédure sp_fulltext_service a été rencontrée. Se produit une fois par requête.|  
|sp_fulltext_service \@action=connect_timeout|L'option connect_timeout de la procédure sp_fulltext_service a été rencontrée. Se produit une fois par requête.|  
|sp_fulltext_service \@action=data_timeout|L'option data_timeout de la procédure sp_fulltext_service a été rencontrée. Se produit une fois par requête.|  
|sp_fulltext_service \@action=resource_usage|L'option resource_usage de la procédure sp_fulltext_service a été rencontrée. Cette option est sans effet. Se produit une fois par requête.|  
|sp_fulltext_table|La procédure sp_fulltext_table a été rencontrée. Utilisez à la place CREATE/ALTER/DROP FULLTEXT INDEX. Se produit une fois par compilation.|  
|sp_getbindtoken|La procédure sp_getbindtoken a été rencontrée. Utilisez à la place MARS (Multiple Active Result Sets) ou des transactions distribuées. Se produit une fois par compilation.|  
|sp_grantdbaccess|La procédure sp_grantdbaccess a été rencontrée. Utilisez à la place CREATE USER. Se produit une fois par requête.|  
|sp_grantlogin|La procédure sp_grantlogin a été rencontrée. Utilisez à la place CREATE LOGIN. Se produit une fois par requête.|  
|sp_help_fulltext_catalog_components|La procédure sp_help_fulltext_catalog_components a été rencontrée. Cette procédure retourne des lignes vides. N'utilisez pas cette procédure. Se produit une fois par compilation.|  
|sp_help_fulltext_catalogs|La procédure sp_help_fulltext_catalogs a été rencontrée. Interrogez à la place sys.fulltext_catalogs. Se produit une fois par compilation.|  
|sp_help_fulltext_catalogs_cursor|La procédure sp_help_fulltext_catalogs_cursor a été rencontrée. Interrogez à la place sys.fulltext_catalogs. Se produit une fois par compilation.|  
|sp_help_fulltext_columns|La procédure sp_help_fulltext_columns a été rencontrée. Interrogez à la place sys.fulltext_index_columns. Se produit une fois par compilation.|  
|sp_help_fulltext_columns_cursor|La procédure sp_help_fulltext_columns_cursor a été rencontrée. Interrogez à la place sys.fulltext_index_columns. Se produit une fois par compilation.|  
|sp_help_fulltext_tables|La procédure sp_help_fulltext_tables a été rencontrée. Interrogez à la place sys.fulltext_indexes. Se produit une fois par compilation.|  
|sp_help_fulltext_tables_cursor|La procédure sp_help_fulltext_tables_cursor a été rencontrée. Interrogez à la place sys.fulltext_indexes. Se produit une fois par compilation.|  
|sp_helpdevice|La procédure sp_helpdevice a été rencontrée. Interrogez à la place sys.backup_devices. Se produit une fois par requête.|  
|sp_helpextendedproc|La procédure sp_helpextendedproc a été rencontrée. Utilisez à la place CLR. Se produit une fois par compilation.|  
|sp_helpremotelogin|La procédure sp_helpremotelogin a été rencontrée. Utilisez à la place des serveurs liés.|  
|sp_indexoption|La procédure sp_indexoption a été rencontrée. Utilisez à la place ALTER INDEX. Se produit une fois par compilation.|  
|sp_lock|La procédure sp_lock a été rencontrée. Interrogez à la place sys.dm_tran_locks. Se produit une fois par requête.|  
|sp_password|La procédure sp_password a été rencontrée. Utilisez à la place ALTER LOGIN. Se produit une fois par requête.|  
|sp_remoteoption|La procédure sp_remoteoption a été rencontrée. Utilisez à la place des serveurs liés.|  
|sp_renamedb|La procédure sp_renamedb a été rencontrée. Utilisez à la place ALTER DATABASE. Se produit une fois par requête.|  
|sp_resetstatus|La procédure sp_resetstatus a été rencontrée. Utilisez à la place ALTER DATABASE. Se produit une fois par requête.|  
|sp_revokedbaccess|La procédure sp_revokedbaccess a été rencontrée. Utilisez à la place DROP USER. Se produit une fois par requête.|  
|sp_revokelogin|La procédure sp_revokelogin a été rencontrée. Utilisez à la place DROP LOGIN. Se produit une fois par requête.|  
|sp_srvrolepermission|La procédure déconseillée sp_srvrolepermission a été rencontrée. Ne pas utiliser. Se produit une fois par requête.|  
|sp_unbindefault|La procédure sp_unbindefault a été rencontrée. Utilisez à la place le mot clé DEFAULT dans les instructions ALTER TABLE ou CREATE TABLE. Se produit une fois par compilation.|  
|sp_unbindrule|La procédure sp_unbindrule a été rencontrée. Utilisez des contraintes de validation à la place de règles. Se produit une fois par compilation.|  
|SQL_AltDiction_CP1253_CS_AS|L'événement se produit une fois par démarrage de base de données et une fois par utilisation de classement. Prévoyez de modifier les applications qui utilisent ce classement.|  
|Littéraux de chaîne comme alias de colonne|Une syntaxe contenant une chaîne utilisée comme un alias de colonne dans une instruction SELECT, telle que `'string' = expression`, a été rencontrée. Ne pas utiliser. Se produit une fois par compilation.|  
|sys.sql_dependencies|Des références à sys.sql_dependencies ont été rencontrées. Utilisez à la place sys.sql_expression_dependencies. Se produit une fois par compilation.|  
|sysaltfiles|Des références à sysaltfiles ont été rencontrées. Utilisez à la place sys.master_files. Se produit une fois par compilation.|  
|syscacheobjects|Des références à syscacheobjects ont été rencontrées. Utilisez à la place sys.dm_exec_cached_plans, sys.dm_exec_plan_attributes et sys.dm_exec_sql_text. Se produit une fois par compilation.|  
|syscolumns|Des références à syscolumns ont été rencontrées. Utilisez à la place sys.columns. Se produit une fois par compilation.|  
|syscomments|Des références à syscomments ont été rencontrées. Utilisez à la place sys.sql_modules. Se produit une fois par compilation.|  
|sysconfigures|Des références à la table sysconfigures ont été rencontrées. Référencez à la place la vue sys.sysconfigures. Se produit une fois par compilation.|  
|sysconstraints|Des références à sysconstraints ont été rencontrées. Utilisez à la place sys.check_constraints, sys.default_constraints, sys.key_constraints ou sys.foreign_keys. Se produit une fois par compilation.|  
|syscurconfigs|Des références à syscurconfigs ont été rencontrées. Utilisez à la place sys.configurations. Se produit une fois par compilation.|  
|sysdatabases|Des références à sysdatabases ont été rencontrées. Utilisez à la place sys.databases. Se produit une fois par compilation.|  
|sysdepends|Des références à sysdepends ont été rencontrées. Utilisez à la place sys.sql_dependencies. Se produit une fois par compilation.|  
|sysdevices|Des références à sysdevices ont été rencontrées. Utilisez à la place sys.backup_devices. Se produit une fois par compilation.|  
|sysfilegroups|Des références à sysfilegroups ont été rencontrées. Utilisez à la place sys.filegroups. Se produit une fois par compilation.|  
|sysfiles|Des références à sysfiles ont été rencontrées. Utilisez à la place sys.database_files. Se produit une fois par compilation.|  
|sysforeignkeys|Des références à sysforeignkeys ont été rencontrées. Utilisez à la place sys.foreign_keys. Se produit une fois par compilation.|  
|sysfulltextcatalogs|Des références à sysfulltextcatalogs ont été rencontrées. Utilisez à la place sys.fulltext_catalogs. Se produit une fois par compilation.|  
|sysindexes|Des références à sysindexes ont été rencontrées. Utilisez à la place sys.indexes, sys.partitions, sys.allocation_units et sys.dm_db_partition_stats. Se produit une fois par compilation.|  
|sysindexkeys|Des références à sysindexkeys ont été rencontrées. Utilisez sys.index_columns à la place. Se produit une fois par compilation.|  
|syslockinfo|Des références à syslockinfo ont été rencontrées. Utilisez à la place sys.dm_tran_locks. Se produit une fois par compilation.|  
|syslogins|Des références à syslogins ont été rencontrées. Utilisez à la place sys.server_principals et sys.sql_logins. Se produit une fois par compilation.|  
|sysmembers|Des références à sysmembers ont été rencontrées. Utilisez à la place sys.database_role_members. Se produit une fois par compilation.|  
|sysmessages|Des références à sysmessages ont été rencontrées. Utilisez à la place sys.messages. Se produit une fois par compilation.|  
|sysobjects|Des références à sysobjects ont été rencontrées. Utilisez à la place sys.objects. Se produit une fois par compilation.|  
|sysoledbusers|Des références à sysoledbusers ont été rencontrées. Utilisez à la place sys.linked_logins. Se produit une fois par compilation.|  
|sysopentapes|Des références à sysopentapes ont été rencontrées. Utilisez à la place sys.dm_io_backup_tapes. Se produit une fois par compilation.|  
|sysperfinfo|Des références à sysperfinfo ont été rencontrées. Utilisez à la place sys.dm_os_performance_counters. . Se produit une fois par compilation.|  
|syspermissions|Des références à syspermissions ont été rencontrées. Utilisez à la place sys.database_permissions et sys.server_permissions. Se produit une fois par compilation.|  
|sysprocesses|Des références à sysprocesses ont été rencontrées. Utilisez à la place sys.dm_exec_connections, sys.dm_exec_sessions et sys.dm_exec_requests. Se produit une fois par compilation.|  
|sysprotects|Des références à sysprotects ont été rencontrées. Utilisez à la place sys.database_permissions et sys.server_permissions. Se produit une fois par compilation.|  
|sysreferences|Des références à sysreferences ont été rencontrées. Utilisez à la place sys.foreign_keys. Se produit une fois par compilation.|  
|sysremotelogins|Des références à sysremotelogins ont été rencontrées. Utilisez à la place sys.remote_logins. Se produit une fois par compilation.|  
|sysservers|Des références à sysservers ont été rencontrées. Utilisez à la place sys.servers. Se produit une fois par compilation.|  
|systypes|Des références à systypes ont été rencontrées. Utilisez à la place sys.types. Se produit une fois par compilation.|  
|sysusers|Des références à sysusers ont été rencontrées. Utilisez à la place sys.database_principals. Se produit une fois par compilation.|  
|Indicateur de table sans WITH|Une instruction utilisant des indicateurs de table sans le mot clé WITH a été rencontrée. Modifiez les instructions de manière à inclure le mot clé WITH. Se produit une fois par compilation.|  
|Option de table text in row|Des références à l'option de table 'text in row' ont été rencontrées. Utilisez à la place sp_tableoption 'large value types out of row'. Se produit une fois par requête.|  
|TEXTPTR|Des références à la fonction TEXTPTR ont été rencontrées. Réécrivez les applications de manière à utiliser le type de données **varchar(max)** et à supprimer la syntaxe des types de données **text**, **ntext**et **image** . Se produit une fois par requête.|  
|TEXTVALID|Des références à la fonction TEXTVALID ont été rencontrées. Réécrivez les applications de manière à utiliser le type de données **varchar(max)** et à supprimer la syntaxe des types de données **text**, **ntext**et **image** . Se produit une fois par requête.|  
|timestamp|Nombre total des fois où le type de données **timestamp** déconseillé a été rencontré dans une instruction DDL. Utilisez à la place le type de données **rowversion** .|  
|UPDATETEXT ou WRITETEXT|L'instruction UPDATETEXT ou WRITETEXT a été rencontrée. Réécrivez les applications de manière à utiliser le type de données **varchar(max)** et à supprimer la syntaxe des types de données **text**, **ntext**et **image** . Se produit une fois par requête.|  
|USER_ID|Des références à la fonction USER_ID ont été rencontrées. Utilisez à la place la fonction DATABASE_PRINCIPAL_ID. Se produit une fois par compilation.|  
|Utilisation d'OLEDB pour les serveurs liés||  
|Format de stockage vardecimal|Le format de stockage **vardecimal** a été rencontré. Utilisez à la place la compression de données.|  
|XMLDATA|La syntaxe FOR XML a été rencontrée. Utilisez la génération XSD en modes RAW et AUTO. Il n'y a aucun remplacement pour le mode explicite. Se produit une fois par compilation.|  
|XP_API|Une instruction de procédure stockée étendue a été rencontrée. Ne pas utiliser.|  
|xp_grantlogin|La procédure xp_grantlogin a été rencontrée. Utilisez à la place CREATE LOGIN. Se produit une fois par compilation.|  
|xp_loginconfig|La procédure xp_loginconfig a été rencontrée. Utilisez à la place l'argument IsIntegratedSecurityOnly de SERVERPROPERTY. Se produit une fois par requête.|  
|xp_revokelogin|La procédure xp_revokelogin a été rencontrée. Utilisez à la place ALTER LOGIN DISABLE ou DROP LOGIN. Se produit une fois par compilation.|  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités du moteur de base de données déconseillées dans SQL Server 2016](../../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)   
 [Fonctionnalités de recherche en texte intégral déconseillées dans SQL Server 2016](../../relational-databases/search/deprecated-full-text-search-features-in-sql-server-2016.md)   
 [Classe d'événements Deprecation Announcement](../../relational-databases/event-classes/deprecation-announcement-event-class.md)   
 [Classe d'événements Deprecation Final Support](../../relational-databases/event-classes/deprecation-final-support-event-class.md)   
 [Fonctionnalités du moteur de base de données supprimées dans SQL Server 2016](../../database-engine/discontinued-database-engine-functionality-in-sql-server-2016.md)   
 [Fonctionnalités de recherche en texte intégral abandonnées dans SQL Server 2016](http://msdn.microsoft.com/library/70587b3c-cc77-4681-924d-a1df7cdf1517)   
 [Utiliser des objets SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md)  
  
  
