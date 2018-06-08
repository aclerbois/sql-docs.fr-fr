---
title: Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2016 | Microsoft Docs
ms.custom: ''
ms.date: 05/09/2018
ms.prod: sql
ms.prod_service: high-availability
ms.component: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server]
- Database Engine [SQL Server], backward compatibility
- deprecation [SQL Server], feature list
ms.assetid: c10eeaa5-3d3c-49b4-a4bd-5dc4fb190142
caps.latest.revision: 215
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 799f2b0df6a33d70006baf4b1389584cd7acf801
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34722319"
---
# <a name="deprecated-database-engine-features-in-sql-server-2016"></a>Fonctionnalités du moteur de base de données déconseillées dans SQL Server 2016
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

Cette rubrique décrit les fonctionnalités [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] déconseillées qui sont toujours disponibles dans [!INCLUDE[sssql15-md](../includes/sssql15-md.md)]. Les fonctions déconseillées ne doivent pas être utilisées dans de nouvelles applications.  
  
Quand une fonctionnalité est marquée comme étant dépréciée, cela signifie que :
-  La fonctionnalité est en mode de maintenance uniquement. Aucune nouvelle modification ne lui sera apportée, notamment celles liées à l’interopérabilité avec de nouvelles fonctionnalités.
-  Nous nous efforçons de ne pas retirer une fonctionnalité dépréciée des futures versions pour faciliter les mises à niveau. Cependant, dans de rares cas, nous pouvons décider de retirer définitivement une fonctionnalité de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si elle limite de futures innovations.
-  Pour un nouveau travail de développement, nous vous recommandons de ne pas utiliser des fonctionnalités dépréciées.      

Pour [!INCLUDE[sssqlv14-md](../includes/sssqlv14-md.md)], consultez [Fonctionnalités du moteur de base de données dépréciées dans SQL Server 2017](../database-engine/deprecated-database-engine-features-in-sql-server-2017.md).

Vous pouvez surveiller l'utilisation de fonctionnalités déconseillées à l'aide du compteur de performance Objet [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Fonctionnalités déconseillées et des événements de suivi. Pour plus d’informations, consultez [Utiliser des objets SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
La valeur de ces compteurs est également disponible en exécutant l’instruction suivante :  
  
```sql  
SELECT * FROM sys.dm_os_performance_counters   
WHERE object_name = 'SQLServer:Deprecated Features';  
```  
  
## <a name="features-deprecated-in-the-next-version-of-sql-server"></a>Fonctionnalités dépréciées dans la prochaine version de SQL Server
 Les fonctionnalités suivantes du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ne seront pas prises en charge dans la prochaine version de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans vos nouveaux développements et modifiez dès que possible les applications qui y ont recours. La valeur **Nom de la fonctionnalité** apparaît dans les événements de trace comme ObjectName et dans les compteurs de performance et `sys.dm_os_performance_counters` comme nom d’instance. La valeur de l’ **ID de la fonctionnalité** apparaît dans les événements de suivi comme ObjectId.  
  
|Catégorie|Fonctionnalité déconseillée|Remplacement|Nom de la fonctionnalité|ID de la fonctionnalité|  
|--------------|------------------------|-----------------|------------------|----------------|  
|Sauvegarde et restauration|RESTORE { DATABASE &#124; LOG } WITH [MEDIA]PASSWORD est toujours déconseillé. BACKUP { DATABASE &#124; LOG } WITH PASSWORD et BACKUP { DATABASE &#124; LOG } WITH MEDIAPASSWORD ont été retirés.|Aucun.|BACKUP DATABASE ou LOG WITH PASSWORD<br /><br /> BACKUP DATABASE ou LOG WITH MEDIAPASSWORD|104<br /><br /> 103|  
|Niveaux de compatibilité|Mise à niveau à partir de version 110 ([!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]).|Quand le [support](http://aka.ms/sqllifecycle) n’est plus assuré pour une version de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le niveau de compatibilité de base de données associé est marqué comme étant déprécié. Cependant, nous continuons le plus longtemps possible d’assurer le support des applications certifiées sur n’importe quel niveau de compatibilité de base de données pris en charge de façon à faciliter la mise à niveau. Pour plus d’informations sur les niveaux de compatibilité, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|Niveau de compatibilité 100 de la base de données|108|  
|Objets de base de données|Possibilité de retourner les jeux de résultats à partir de déclencheurs|None|Le déclencheur retourne des résultats|12|  
|Chiffrement|Le chiffrement à l'aide de RC4 ou RC4_128 est déconseillé et est planifié pour être supprimé dans la prochaine version. Le déchiffrement de RC4 et RC4_128 n'est pas déconseillé.|Utilisez un autre algorithme de chiffrement, par exemple AES.|Algorithme de chiffrement déconseillé|253|  
|Serveurs distants|sp_addremotelogin<br /><br /> sp_addserver<br /><br /> sp_dropremotelogin<br /><br /> sp_helpremotelogin<br /><br /> sp_remoteoption|Remplacez les serveurs distants à l'aide de serveurs liés. sp_addserver ne peut être utilisé qu’avec l’option « local ».|sp_addremotelogin<br /><br /> sp_addserver<br /><br /> sp_dropremotelogin<br /><br /> sp_helpremotelogin<br /><br /> sp_remoteoption|70<br /><br /> 69<br /><br /> 71<br /><br /> 72<br /><br /> 73|  
|Serveurs distants|\@\@remserver|Remplacez les serveurs distants à l'aide de serveurs liés.|None|None|  
|Serveurs distants|SET REMOTE_PROC_TRANSACTIONS|Remplacez les serveurs distants à l'aide de serveurs liés.|SET REMOTE_PROC_TRANSACTIONS|110|  
|Indicateurs de table|Indicateur de table HOLDLOCK sans parenthèses.|Utilisez HOLDLOCK avec la parenthèse.|Indicateur de table HOLDLOCK sans parenthèses|167|  
  
## <a name="features-deprecated-in-a-future-version-of-sql-server"></a>Fonctionnalités dépréciées dans une future version de SQL Server  
 Les fonctionnalités du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ci-dessous sont prises en charge dans la prochaine version de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], mais seront dépréciées dans une version ultérieure. La version spécifique de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n'a pas été déterminée.  
  
|Catégorie|Fonctionnalité déconseillée|Remplacement|Nom de la fonctionnalité|ID de la fonctionnalité|  
|--------------|------------------------|-----------------|------------------|----------------|  
|Niveaux de compatibilité|sp_dbcmptlevel|ALTER DATABASE … SET COMPATIBILITY_LEVEL. Pour plus d’informations, consultez [Niveau de compatibilité ALTER DATABASE &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md).|sp_dbcmptlevel|80|  
|Niveaux de compatibilité|Niveau de compatibilité 110 et 120 de la base de données|Projetez de mettre à niveau la base de données et l'application avant la prochaine version.|Niveau de compatibilité 110 de la base de données<br /><br /> Niveau de compatibilité 120 de la base de données||  
|XML|Génération de schéma XDR en ligne|La directive XMLDATA de l'option FOR XML est déconseillée. Utilisez la génération XSD en mode RAW et AUTO. Il n'existe aucune solution de remplacement pour la directive XMLDATA en mode EXPLICIT.|XMLDATA|181|  
|Sauvegarde et restauration|BACKUP { DATABASE &#124; LOG } TO TAPE<br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_tape*|BACKUP { DATABASE &#124; LOG } TO DISK<br /><br /> BACKUP { DATABASE &#124; LOG } TO *device_that_is_a_disk*|BACKUP DATABASE ou LOG TO TAPE|235|  
|Sauvegarde et restauration|sp_addumpdevice'**tape**'|sp_addumpdevice'**disk**'|ADDING TAPE DEVICE|236|  
|Sauvegarde et restauration|sp_helpdevice|sys.backup_devices|sp_helpdevice|100|  
|Classements|Korean_Wansung_Unicode<br /><br /> Lithuanian_Classic<br /><br /> SQL_AltDiction_CP1253_CS_AS|Aucun. Ces classements existent dans [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], mais ne sont pas visibles via fn_helpcollations.|Korean_Wansung_Unicode<br /><br /> Lithuanian_Classic<br /><br /> SQL_AltDiction_CP1253_CS_AS|191<br /><br /> 192<br /><br /> 194|  
|Classements|Hindi<br /><br /> Macedonian|Ces classements existent dans [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] et versions ultérieures, mais ne sont pas visibles via fn_helpcollations. Utilisez à la place Macedonian_FYROM_90 et Indic_General_90.|Hindi<br /><br /> Macedonian|190<br /><br /> 193|  
|Classements|Azeri_Latin_90<br /><br /> Azeri_Cyrilllic_90|Azeri_Latin_100<br /><br /> Azeri_Cyrilllic_100|Azeri_Latin_90<br /><br /> Azeri_Cyrilllic_90|232<br /><br /> 233|  
|Configuration|SET ANSI_NULLS OFF et option de base de données ANSI_NULLS OFF<br /><br /> SET ANSI_PADDING OFF et option de base de données ANSI_PADDING OFF<br /><br /> SET CONCAT_NULL_YIELDS_NULL OFF et option de base de données CONCAT_NULL_YIELDS_NULL OFF<br /><br /> SET OFFSETS|Aucun.<br /><br /> ANSI_NULLS, ANSI_PADDING et CONCAT_NULLS_YIELDS_NULL sont toujours définies avec la valeur ON. SET OFFSETS ne sera pas disponible.|SET ANSI_NULLS OFF<br /><br /> SET ANSI_PADDING OFF<br /><br /> SET CONCAT_NULL_YIELDS_NULL OFF<br /><br /> SET OFFSETS<br /><br /> ALTER DATABASE SET ANSI_NULLS OFF<br /><br /> ALTER DATABASE SET ANSI_PADDING OFF<br /><br /> ALTER DATABASE SET CONCAT_NULL_YIELDS_NULL OFF|111<br /><br /> 113<br /><br /> 112<br /><br /> 36<br /><br /> 111<br /><br /> 113<br /><br /> 112|  
|Types de données|sp_addtype<br /><br /> sp_droptype|CREATE TYPE<br /><br /> DROP TYPE|sp_addtype<br /><br /> sp_droptype|62<br /><br /> 63|  
|Types de données|Syntaxe de**timestamp** pour le type de données **rowversion** |Syntaxe du type de données**rowversion** |timestamp|158|  
|Types de données|Possibilité d'insérer des valeurs NULL dans les colonnes **timestamp** .|Utilisez DEFAULT à la place.|INSERT NULL dans des colonnes TIMESTAMP|179|  
|Types de données|Option de table 'text in row'|Utilisez les types de données **varchar(max)**, **nvarchar(max)** et **varbinary(max)**. Pour plus d’informations, consultez [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|Option de table text in row|9|  
|Types de données|Types de données :<br /><br /> **texte**<br /><br /> **ntext**<br /><br /> **image**|Utilisez les types de données **varchar(max)**, **nvarchar(max)** et **varbinary(max)** .|Types de données : **text**, **ntext** ou **image**|4|  
|Gestion de base de données|sp_attach_db<br /><br /> sp_attach_single_file_db|Instruction CREATE DATABASE avec l'option FOR ATTACH. Pour recréer plusieurs fichiers journaux, lorsqu'un ou plusieurs d'entre eux possèdent un nouvel emplacement, utilisez FOR ATTACH_REBUILD_LOG.|sp_attach_db<br /><br /> sp_attach_single_file_db|81<br /><br /> 82|  
|Objets de base de données|CREATE DEFAULT<br /><br /> DROP DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|Mot clé DEFAULT dans CREATE TABLE et ALTER TABLE|CREATE_DROP_DEFAULT<br /><br /> sp_bindefault<br /><br /> sp_unbindefault|162<br /><br /> 64<br /><br /> 65|  
|Objets de base de données|CREATE RULE<br /><br /> DROP RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule|Mot clé CHECK dans CREATE TABLE et ALTER TABLE|CREATE_DROP_RULE<br /><br /> sp_bindrule<br /><br /> sp_unbindrule|161<br /><br /> 66<br /><br /> 67|  
|Objets de base de données|sp_change_users_login|Utilisez ALTER USER.|sp_change_users_login|231|  
|Objets de base de données|sp_depends|sys.dm_sql_referencing_entities et sys.dm_sql_referenced_entities|sp_depends|19|  
|Objets de base de données|sp_renamedb|MODIFY NAME dans ALTER DATABASE|sp_renamedb|79|  
|Objets de base de données|sp_getbindtoken|Utilisez MARS ou les transactions distribuées.|sp_getbindtoken|98|  
|Options de base de données|sp_bindsession|Utilisez MARS ou les transactions distribuées.|sp_bindsession|97|  
|Options de base de données|sp_resetstatus|ALTER DATABASE SET { ONLINE &#124; EMERGENCY }|sp_resetstatus|83|  
|Options de base de données|Option TORN_PAGE_DETECTION d'ALTER DATABASE|Option PAGE_VERIFY TORN_PAGE DETECTION d'ALTER DATABASE|ALTER DATABASE WITH TORN_PAGE_DETECTION|102|  
|DBCC|DBCC DBREINDEX|Option REBUILD d'ALTER INDEX.|DBCC DBREINDEX|11|  
|DBCC|DBCC INDEXDEFRAG|Option REORGANIZE d'ALTER INDEX|DBCC INDEXDEFRAG|18|  
|DBCC|DBCC SHOWCONTIG|sys.dm_db_index_physical_stats|DBCC SHOWCONTIG|10|  
|DBCC|DBCC PINTABLE<br /><br /> DBCC UNPINTABLE|Cette option n'a aucun effet.|DBCC [UN] PINTABLE|189|  
|Propriétés étendues|Level0type = 'type' et Level0type = 'USER' pour ajouter des propriétés étendues aux objets de type de niveau 1 ou 2.|Utilisez Level0type = 'USER' uniquement pour ajouter une propriété étendue directement à un utilisateur ou un rôle.<br /><br /> Utilisez Level0type = 'SCHEMA' pour ajouter une propriété étendue aux types level-1 comme TABLE ou VIEW ou aux types level-2 comme COLUMN ou TRIGGER. Pour plus d’informations, consultez [sp_addextendedproperty &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md).|EXTPROP_LEVEL0TYPE<br /><br /> EXTPROP_LEVEL0USER|13<br /><br /> 14|  
|Programmation des procédures stockées étendues|srv_alloc<br /><br /> srv_convert<br /><br /> srv_describe<br /><br /> srv_getbindtoken<br /><br /> srv_got_attention<br /><br /> srv_message_handler<br /><br /> srv_paramdata<br /><br /> srv_paraminfo<br /><br /> srv_paramlen<br /><br /> srv_parammaxlen<br /><br /> srv_paramname<br /><br /> srv_paramnumber<br /><br /> srv_paramset<br /><br /> srv_paramsetoutput<br /><br /> srv_paramstatus<br /><br /> srv_paramtype<br /><br /> srv_pfield<br /><br /> srv_pfieldex<br /><br /> srv_rpcdb<br /><br /> srv_rpcname<br /><br /> srv_rpcnumber<br /><br /> srv_rpcoptions<br /><br /> srv_rpcowner<br /><br /> srv_rpcparams<br /><br /> srv_senddone<br /><br /> srv_sendmsg<br /><br /> srv_sendrow<br /><br /> srv_setcoldata<br /><br /> srv_setcollen<br /><br /> srv_setutype<br /><br /> srv_willconvert<br /><br /> srv_wsendmsg|Utilisez l'intégration CLR à la place.|XP_API|20|  
|Programmation des procédures stockées étendues|sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc|Utilisez l'intégration CLR à la place.|sp_addextendedproc<br /><br /> sp_dropextendedproc<br /><br /> sp_helpextendedproc|94<br /><br /> 95<br /><br /> 96|  
|Procédures stockées étendues|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|Utiliser CREATE_LOGIN<br /><br /> Utiliser l'argument DROP LOGIN IsIntegratedSecurityOnly de SERVERPROPERTY|xp_grantlogin<br /><br /> xp_revokelogin<br /><br /> xp_loginConfig|44<br /><br /> 45<br /><br /> 59|  
|Fonctions|fn_get_sql|sys.dm_exec_sql_text|fn_get_sql|151|  
|Algorithmes de hachage|Algorithmes MD2, MD4, MD5, SHA et SHA1. Ils ne sont pas disponibles sous le niveau de compatibilité 130.|Utilisez SHA2_256 ou SHA2_512.|Algorithme de hachage déconseillé||  
|Haute disponibilité|mise en miroir de bases de données|[!INCLUDE[ssHADR](../includes/sshadr-md.md)]<br /><br /> Si votre édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne prend pas en charge [!INCLUDE[ssHADR](../includes/sshadr-md.md)], utilisez la copie des journaux de transaction.|DATABASE_MIRRORING|267|  
|Options d'index|sp_indexoption|ALTER INDEX|sp_indexoption|78|  
|Options d'index|Syntaxe CREATE TABLE, ALTER TABLE ou CREATE INDEX sans parenthèses autour des options.|Réécrivez l'instruction de manière à utiliser la syntaxe actuelle.|INDEX_OPTION|33|  
|Options d'instance|option sp_configure 'allow updates'|Les tables système ne peuvent plus être mises à jour. La valeur n'a pas d'effet.|sp_configure allow updates'|173|  
|Options d'instance|Options sp_configure :<br /><br /> 'locks'<br /><br /> 'open objects'<br /><br /> 'set working set size'|Désormais configuré automatiquement. La valeur n'a pas d'effet.|sp_configure 'locks'<br /><br /> sp_configure 'open objects'<br /><br /> sp_configure 'set working set size'|174<br /><br /> 175<br /><br /> 176|  
|Options d'instance|Option sp_configure 'priority boost'|Les tables système ne peuvent plus être mises à jour. La valeur n'a pas d'effet. À la place, utilisez l’option start /high ... program.exe de Windows.|sp_configure "priority boost"|199|  
|Options d'instance|Option sp_configure 'remote proc trans'|Les tables système ne peuvent plus être mises à jour. La valeur n'a pas d'effet.|sp_configure 'remote proc trans'|37|  
|Serveurs liés|Spécification du fournisseur SQLOLEDB pour les serveurs liés.|SQL Server Native Client (SQLNCLI)|SQLOLEDDB pour les serveurs liés|19|  
|Verrouillage|sp_lock|sys.dm_tran_locks|sp_lock|99|  
|Métadonnées|FILE_ID<br /><br /> INDEXKEY_PROPERTY|FILE_IDEX<br /><br /> sys.index_columns|FILE_ID<br /><br /> INDEXKEY_PROPERTY|15<br /><br /> 17|  
|Services Web XML natifs|Instruction CREATE ENDPOINT ou ALTER ENDPOINT avec l'option FOR SOAP.<br /><br /> sys.endpoint_webmethods<br /><br /> sys.soap_endpoints|Utilisez à la place WFC (Windows Communications Foundation) ou ASP.NET.|CREATE/ALTER ENDPOINT<br /><br /> sys.endpoint_webmethods<br /><br /> EXT_soap_endpoints<br /><br /> sys.soap_endpoints|21<br /><br /> 22<br /><br /> 23|  
|Bases de données supprimables|sp_certify_removable<br /><br /> sp_create_removable|sp_detach_db|sp_certify_removable<br /><br /> sp_create_removable|74<br /><br /> 75|  
|Bases de données supprimables|sp_dbremove|DROP DATABASE|sp_dbremove|76|  
|Sécurité|Syntaxe ALTER LOGIN WITH SET CREDENTIAL|Remplacée par la nouvelle syntaxe ALTER LOGIN ADD et DROP CREDENTIAL|ALTER LOGIN WITH SET CREDENTIAL|230|  
|Sécurité|sp_addapprole<br /><br /> sp_dropapprole|CREATE APPLICATION ROLE<br /><br /> DROP APPLICATION ROLE|sp_addapprole<br /><br /> sp_dropapprole|53<br /><br /> 54|  
|Sécurité|sp_addlogin<br /><br /> sp_droplogin|CREATE LOGIN<br /><br /> DROP LOGIN|sp_addlogin<br /><br /> sp_droplogin|39<br /><br /> 40|  
|Sécurité|sp_adduser<br /><br /> sp_dropuser|CREATE USER<br /><br /> DROP USER|sp_adduser<br /><br /> sp_dropuser|49<br /><br /> 50|  
|Sécurité|sp_grantdbaccess<br /><br /> sp_revokedbaccess|CREATE USER<br /><br /> DROP USER|sp_grantdbaccess<br /><br /> sp_revokedbaccess|51<br /><br /> 52|  
|Sécurité|sp_addrole<br /><br /> sp_droprole|CREATE ROLE<br /><br /> DROP ROLE|sp_addrole<br /><br /> sp_droprole|56<br /><br /> 57|  
|Sécurité|sp_approlepassword<br /><br /> sp_password|ALTER APPLICATION ROLE<br /><br /> ALTER LOGIN|sp_approlepassword<br /><br /> sp_password|55<br /><br /> 46|  
|Sécurité|sp_changeobjectowner|ALTER SCHEMA ou ALTER AUTHORIZATION|sp_changeobjectowner|58|  
|Sécurité|sp_control_dbmasterkey_password|Une clé principale doit exister et le mot de passe doit être correct.|sp_control_dbmasterkey_password|274|  
|Sécurité|sp_defaultdb<br /><br /> sp_defaultlanguage|ALTER LOGIN|sp_defaultdb<br /><br /> sp_defaultlanguage|47<br /><br /> 48|  
|Sécurité|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|ALTER LOGIN DISABLE<br /><br /> CREATE LOGIN<br /><br /> DROP LOGIN|sp_denylogin<br /><br /> sp_grantlogin<br /><br /> sp_revokelogin|42<br /><br /> 41<br /><br /> 43|  
|Sécurité|USER_ID|DATABASE_PRINCIPAL_ID|USER_ID|16|  
|Sécurité|sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|Ces procédures stockées renvoient des informations qui étaient correctes dans [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]. Le résultat ne reflète pas les modifications apportées aux hiérarchies d'autorisations implémentées dans [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]. Pour plus d'informations, consultez [Autorisations des rôles serveur fixes](http://msdn.microsoft.com/library/ms175892\(SQL.100\).aspx).|sp_srvrolepermission<br /><br /> sp_dbfixedrolepermission|61<br /><br /> 60|  
|Sécurité|GRANT ALL<br /><br /> DENY ALL<br /><br /> REVOKE ALL|Autorisations spécifiques GRANT, DENY et REVOKE.|Autorisation ALL|35|  
|Sécurité|Fonction intrinsèque PERMISSIONS|Interrogez à la place sys.fn_my_permissions.|PERMISSIONS|170|  
|Sécurité|SETUSER|EXECUTE AS|SETUSER|165|  
|Sécurité|Algorithmes de chiffrement RC4 et DESX|Utilisez un autre algorithme, par exemple AES.|Algorithme DESX|238|  
|Options définies|SET FMTONLY|[sys.dm_exec_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md), [sys.dm_exec_describe_first_result_set_for_object &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md), [sp_describe_first_result_set &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), and [sp_describe_undeclared_parameters &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md).|SET FMTONLY|250|  
|Options de configuration de serveur|Option c2 audit<br /><br /> Trace par défaut activée (option)|[common criteria compliance enabled (option de configuration de serveur)](../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)<br /><br /> [Événements étendus](../relational-databases/extended-events/extended-events.md)|sp_configure 'c2 audit mode'<br /><br /> sp_configure 'default trace enabled'|252<br /><br /> 253|  
|Classes SMO|**Microsoft.SQLServer. Management.Smo.Information** (classe)<br /><br /> **Microsoft.SQLServer. Management.Smo.Settings** (classe)<br /><br /> **Microsoft.SQLServer.Management. Smo.DatabaseOptions** (classe)<br /><br /> **Microsoft.SqlServer.Management.Smo. DatabaseDdlTrigger.NotForReplication** (propriété)|**Microsoft.SqlServer.  Management.Smo.Server** (classe)<br /><br /> **Microsoft.SqlServer.  Management.Smo.Server** (classe)<br /><br /> **Microsoft.SqlServer. Management.Smo.Database** (classe)<br /><br /> None|None|None|  
|SQL Server Agent|Notification**net send** <br /><br /> Notification par radiomessagerie|Notification par courrier électronique<br /><br /> Notification par courrier électronique |None|None|  
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|Intégration de l’Explorateur de solutions dans [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]||None|None|  
|Procédures stockées système|sp_db_increased_partitions|Aucun. La prise en charge de plus de partitions est disponible par défaut dans [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].|sp_db_increased_partitions|253|  
|Tables système|sysaltfiles<br /><br /> syscacheobjects<br /><br /> syscolumns<br /><br /> syscomments<br /><br /> sysconfigures<br /><br /> sysconstraints<br /><br /> syscurconfigs<br /><br /> sysdatabases<br /><br /> sysdepends<br /><br /> sysdevices<br /><br /> sysfilegroups<br /><br /> sysfiles<br /><br /> sysforeignkeys<br /><br /> sysfulltextcatalogs<br /><br /> sysindexes<br /><br /> sysindexkeys<br /><br /> syslockinfo<br /><br /> syslogins<br /><br /> sysmembers<br /><br /> sysmessages<br /><br /> sysobjects<br /><br /> sysoledbusers<br /><br /> sysopentapes<br /><br /> sysperfinfo<br /><br /> syspermissions<br /><br /> sysprocesses<br /><br /> sysprotects<br /><br /> sysreferences<br /><br /> sysremotelogins<br /><br /> sysservers<br /><br /> systypes<br /><br /> sysusers|Vues de compatibilité. Pour plus d’informations, consultez [Vues de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md).<br /><br /> **Important :** les vues de compatibilité n’exposent pas de métadonnées pour les fonctionnalités qui ont été introduites dans [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]. Il est recommandé de mettre à niveau les applications pour pouvoir utiliser les affichages catalogue. Pour plus d’informations, consultez [Affichages catalogue &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/catalog-views-transact-sql.md).|sysaltfiles<br /><br /> syscacheobjects<br /><br /> syscolumns<br /><br /> syscomments<br /><br /> sysconfigures<br /><br /> sysconstraints<br /><br /> syscurconfigs<br /><br /> sysdatabases<br /><br /> sysdepends<br /><br /> sysdevices<br /><br /> sysfilegroups<br /><br /> sysfiles<br /><br /> sysforeignkeys<br /><br /> sysfulltextcatalogs<br /><br /> sysindexes<br /><br /> sysindexkeys<br /><br /> syslockinfo<br /><br /> syslogins<br /><br /> sysmembers<br /><br /> sysmessages<br /><br /> sysobjects<br /><br /> sysoledbusers<br /><br /> sysopentapes<br /><br /> sysperfinfo<br /><br /> syspermissions<br /><br /> sysprocesses<br /><br /> sysprotects<br /><br /> sysreferences<br /><br /> sysremotelogins<br /><br /> sysservers<br /><br /> systypes<br /><br /> sysusers|141<br /><br /> None<br /><br /> 133<br /><br /> 126<br /><br /> 146<br /><br /> 131<br /><br /> 147<br /><br /> 142<br /><br /> 123<br /><br /> 144<br /><br /> 128<br /><br /> 127<br /><br /> 130<br /><br /> 122<br /><br /> 132<br /><br /> 134<br /><br /> 143<br /><br /> 140<br /><br /> 119<br /><br /> 137<br /><br /> 125<br /><br /> 139<br /><br /> 145<br /><br /> 157<br /><br /> 121<br /><br /> 153<br /><br /> 120<br /><br /> 129<br /><br /> 138<br /><br /> 136<br /><br /> 135<br /><br /> 124|  
|Tables système|sys.numbered_procedures<br /><br /> sys.numbered_procedure_parameters|None|numbered_procedures<br /><br /> numbered_procedure_parameters|148<br /><br /> 149|  
|Fonctions système|fn_virtualservernodes<br /><br /> fn_servershareddrives|sys.dm_os_cluster_nodes<br /><br /> sys.dm_io_cluster_shared_drives|fn_virtualservernodes<br /><br /> fn_servershareddrives|155<br /><br /> 156|  
|Vues système|sys.sql_dependencies|sys.sql_expression_dependencies|sys.sql_dependencies|198|  
|Compression de table|Utilisation du format de stockage vardecimal.|Le format de stockage vardecimal est déconseillé. La compression des données[!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] compresse les valeurs décimales ainsi que d’autres types de données. Nous vous recommandons d'utiliser la compression de données au lieu du format de stockage vardecimal.|Format de stockage vardecimal|200|  
|Compression de table|Utilisation de la procédure sp_db_vardecimal_storage_format.|Le format de stockage vardecimal est déconseillé. La compression des données[!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] compresse les valeurs décimales ainsi que d’autres types de données. Nous vous recommandons d'utiliser la compression de données au lieu du format de stockage vardecimal.|sp_db_vardecimal_storage_format|201|  
|Compression de table|Utilisation de la procédure sp_estimated_rowsize_reduction_for_vardecimal.|Utilisez à la place la compression de données et la procédure sp_estimate_data_compression_savings.|sp_estimated_rowsize_reduction_for_vardecimal|202|  
|Indicateurs de table|Spécification de NOLOCK ou READUNCOMMITTED dans la clause FROM d'une instruction UPDATE ou DELETE.|Supprimez les indicateurs de table NOLOCK ou READUNCOMMITTED de la clause FROM.|NOLOCK ou READUNCOMMITTED dans UPDATE ou DELETE| 1|  
|Indicateurs de table|Spécification des indicateurs de table sans utilisation du mot clé WITH.|Utilisez WITH.|Indicateur de table sans WITH|8|  
|Indicateurs de table|INSERT_HINTS||INSERT_HINTS|34|  
|Pointeurs de texte|WRITETEXT<br /><br /> UPDATETEXT<br /><br /> READTEXT|None|UPDATETEXT ou WRITETEXT<br /><br /> READTEXT|115<br /><br /> 114|  
|Pointeurs de texte|TEXTPTR()<br /><br /> TEXTVALID()|None|TEXTPTR<br /><br /> TEXTVALID|5<br /><br /> 6|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Séquence d'appel de fonction ::|Remplacée par SELECT *column_list* FROM sys.\<*function_name*>().<br /><br /> Par exemple, remplacez `SELECT * FROM ::fn_virtualfilestats(2,1)`par `SELECT * FROM sys.fn_virtualfilestats(2,1)`.|Syntaxe d'appel de fonction '::'|166|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Références de colonnes en 3 et 4 parties.|Noms en 2 parties dans le fonctionnement standard.|Nom de la colonne à plus de deux parties|3|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Une chaîne entre guillemets utilisée en tant qu'alias de colonne pour une expression dans une liste SELECT :<br /><br /> '*string_alias*' = *expression*|*expression* [AS] *column_alias*<br /><br /> *expression* [AS] [*column_alias*]<br /><br /> *expression* [AS] "*column_alias*"<br /><br /> *expression* [AS] '*column_alias*'<br /><br /> *column_alias* = *expression*|Littéraux de chaîne comme alias de colonne|184|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Procédures numérotées|Aucun. Ne pas utiliser.|ProcNums|160|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Syntaxe*table_name.index_name* dans DROP INDEX|Syntaxe*index_name* ON *table_name* dans DROP INDEX.|DROP INDEX avec nom en deux parties|163|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Pas d'instructions [!INCLUDE[tsql](../includes/tsql-md.md)] se terminant avec un point-virgule.|Terminez les instructions [!INCLUDE[tsql](../includes/tsql-md.md)] avec un point-virgule (;).|None|None|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|GROUP BY ALL|Utilisez la solution personnalisée cas par cas avec UNION ou une table dérivée.|GROUP BY ALL|169|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|ROWGUIDCOL comme nom de colonne dans les instructions DML.|Utilisez $rowguid.|ROWGUIDCOL|182|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|IDENTITYCOL comme nom de colonne dans les instructions DML.|Utilisez $identity.|IDENTITYCOL|183|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Utilisation de #, ## comme table temporaire et noms de procédure stockée temporaires.|Utilisez au moins un caractère supplémentaire.|'#' et '##' comme nom des tables temporaires et procédures stockées|185|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Utilisation de \@, \@\@ ou \@\@ comme identificateurs [!INCLUDE[tsql](../includes/tsql-md.md)].|N’utilisez pas \@, \@\@ ou des noms commençant par \@\@ comme identificateurs.|\@ et noms commençant par \@\@ comme identificateurs [!INCLUDE[tsql](../includes/tsql-md.md)]|186.|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Utilisation du mot clé DEFAULT comme valeur par défaut.|N'utilisez pas le mot DEFAULT comme valeur par défaut.|Mot clé DEFAULT comme valeur par défaut.|187|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Utilisation d'un espace comme séparateur entre les indicateurs de table.|Utilisez une virgule pour séparer les indicateurs de table.|Indicateurs de table multiples sans virgule|168|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|La liste de sélection d’une vue indexée d’agrégation doit contenir COUNT_BIG (\*) dans le mode de compatibilité 90.|Utilisez COUNT_BIG (\*).|Liste de sélection de vue d’index sans COUNT_BIG (\*)|2|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Application indirecte des indicateurs de table à un appel d'une fonction table à plusieurs instructions via une vue.|Aucun.|Indicateurs TVF indirects|7|  
|[!INCLUDE[tsql](../includes/tsql-md.md)]|Syntaxe ALTER DATABASE :<br /><br /> MODIFY FILEGROUP READONLY<br /><br /> MODIFY FILEGROUP READWRITE|MODIFY FILEGROUP READ_ONLY<br /><br /> MODIFY FILEGROUP READ_WRITE|MODIFY FILEGROUP READONLY<br /><br /> MODIFY FILEGROUP READWRITE|195<br /><br /> 196|  
|Autres|DB-Library<br /><br /> Embedded SQL for C|Bien que le [!INCLUDE[ssDE](../includes/ssde-md.md)] prenne toujours en charge les connexions des applications existantes qui utilisent les API DB-Library et Embedded SQL, il n'inclut pas les fichiers ou la documentation nécessaires aux tâches de programmation dans les applications qui utilisent ces API. Une version future du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] n'intègrera plus la prise en charge des connexions à partir des applications DB-Library ou Embedded SQL. N'utilisez pas DB-Library ni Embedded SQL pour développer de nouvelles applications. Supprimez toutes les dépendances à DB-Library ou à Embedded SQL lorsque vous modifiez les applications existantes. À la place de ces API, utilisez l’espace de noms SQLClient ou une API comme ODBC. [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] n'inclut pas la DLL DB-Library requise pour exécuter ces applications. Pour exécuter les applications DB-Library ou Embedded SQL, vous devez utiliser la DLL DB-Library à partir de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 6.5, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 7.0 ou [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)].|None|None|  
|Outils|Générateur de profils SQL Server pour la capture de trace|Utilisez le Générateur de profils d'événements étendus incorporé dans SQL Server Management Studio.|SQL Server Profiler|None|  
|Outils|SQL Server Profiler pour Trace Replay|[SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md)|SQL Server Profiler|None|  
|Objets TMO (Trace Management Objects)|L'espace de noms Microsoft.SqlServer.Management.Trace (contient les API pour les objets Trace et Replay SQL Server)|Configuration de trace : <xref:Microsoft.SqlServer.Management.XEvent><br /><br /> Lecture de trace : <xref:Microsoft.SqlServer.XEvent.Linq><br /><br /> Relecture de trace : Aucune|||  
|Procédures stockées, fonctions et affichages catalogue Trace SQL|sp_trace_create<br /><br /> sp_trace_setevent<br /><br /> sp_trace_setfilter<br /><br /> sp_trace_setstatus<br /><br /> fn_trace_geteventinfo<br /><br /> fn_trace_getfilterinfo<br /><br /> fn_trace_getinfo<br /><br /> fn_trace_gettable<br /><br /> sys.traces<br /><br /> sys.trace_events<br /><br /> sys.trace_event_bindings<br /><br /> sys.trace_categories<br /><br /> sys.trace_columns<br /><br /> sys.trace_subclass_values|[Événements étendus](../relational-databases/extended-events/extended-events.md)|sp_trace_create<br /><br /> sp_trace_setevent<br /><br /> sp_trace_setfilter<br /><br /> sp_trace_setstatus<br /><br /> fn_trace_geteventinfo<br /><br /> fn_trace_getfilterinfo<br /><br /> fn_trace_getinfo<br /><br /> fn_trace_gettable<br /><br /> sys.traces<br /><br /> sys.trace_events<br /><br /> sys.trace_event_bindings<br /><br /> sys.trace_categories<br /><br /> sys.trace_columns<br /><br /> sys.trace_subclass_values|258<br /><br /> 260<br /><br /> 261<br /><br /> 259<br /><br /> 256<br /><br /> 257|
|Options définies|**SET ROWCOUNT** pour les instructions **INSERT**, **UPDATE**et **DELETE**|Mot clé TOP|SET ROWCOUNT|109|  

  
> [!NOTE]  
> Le paramètre **OUTPUT** de cookie pour **sp_setapprole** est actuellement documenté comme **varbinary(8000)** , ce qui correspond à la longueur maximale correcte. Cependant, l’implémentation actuelle retourne **varbinary(50)**. Si les développeurs ont alloué **varbinary(50)** , l’application peut nécessiter des modifications si la taille de retour des cookies augmente dans une future version. Bien qu'il ne s'agisse pas d'un problème de suppression de fonctionnalités, ce phénomène est mentionné dans cette rubrique car les réglages de l'application sont similaires. Pour plus d’informations, consultez [sp_setapprole &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités du moteur de base de données supprimées dans SQL Server 2016](../database-engine/discontinued-database-engine-functionality-in-sql-server-2016.md)     
 [Fonctionnalités dépréciées du moteur de base de données dans SQL Server 2017](../database-engine/deprecated-database-engine-features-in-sql-server-2017.md)    
