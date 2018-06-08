---
title: Événements DDL | Microsoft Docs
ms.custom: ''
ms.date: 11/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: triggers
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-ddl
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- DDL events
- DDL triggers, events
- events [SQL Server], DDL
ms.assetid: 62ef24b4-3553-4aed-b62a-670980bae501
caps.latest.revision: 29
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: fabc1bee19dd04ed8943cea23067e675b3a76f08
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ddl-events"></a>Événements DDL
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
  Les tableaux suivants répertorient les événements DDL qui peuvent être utilisés pour activer une notification d'événements ou un déclencheur DDL. Notez que chaque événement correspond à une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] ou à une procédure stockée, la syntaxe de l'instruction étant modifiée pour inclure un trait de soulignement (_) entre les mots clés.  
  
> [!IMPORTANT]  
>  Les procédures stockées système qui exécutent des opérations de type DDL peuvent également activer des notifications d'événements et des déclencheurs DDL. Testez vos déclencheurs et notifications d'événements DDL afin de déterminer leur réponse aux procédures stockées système qui sont exécutées. Par exemple, l’instruction CREATE TYPE et la procédure stockée **sp_addtype** activent toutes les deux un déclencheur ou une notification d’événements DDL créés sur un événement CREATE_TYPE.  
  
## <a name="ddl-statements-that-have-server-or-database-scope"></a>Instructions DDL qui ont une étendue à l'échelle du serveur ou de la base de données  
 Les déclencheurs ou les notifications d'événements DDL peuvent être créés de façon à se déclencher en réponse aux événements ci-dessous, lorsqu'ils se produisent dans la base de données dans laquelle le déclencheur ou la notification d'événement sont créés ou à un emplacement quelconque dans l'instance du serveur.  
  
||||  
|-|-|-|  
|CREATE_APPLICATION_ROLE (S’applique à l’instruction CREATE APPLICATION ROLE et à **sp_addapprole**. Si un nouveau schéma est créé, cet événement déclenche également un événement CREATE_SCHEMA.)|ALTER_APPLICATION_ROLE (S’applique à l’instruction ALTER APPLICATION ROLE et à **sp_approlepassword**.)|DROP_APPLICATION_ROLE (S’applique à l’instruction DROP APPLICATION ROLE et à **sp_dropapprole**.)|  
|CREATE_ASSEMBLY|ALTER_ASSEMBLY|DROP_ASSEMBLY|  
|CREATE_ASYMMETRIC_KEY|ALTER_ASYMMETRIC_KEY|DROP_ASYMMETRIC_KEY|  
|ALTER_AUTHORIZATION|ALTER_AUTHORIZATION_DATABASE (S’applique à l’instruction ALTER AUTHORIZATION quand ON DATABASE est spécifié, ainsi qu’à **sp_changedbowner**.)||  
|CREATE_BROKER_PRIORITY|CREATE_BROKER_PRIORITY|CREATE_BROKER_PRIORITY|  
|CREATE_CERTIFICATE|ALTER_CERTIFICATE|DROP_CERTIFICATE|  
|CREATE_CONTRACT|DROP_CONTRACT||  
|CREATE_CREDENTIAL|ALTER_CREDENTIAL|DROP_CREDENTIAL|  
|GRANT_DATABASE|DENY_DATABASE|REVOKE_DATABASE|  
|CREATE_DATABASE_AUDIT_SPEFICIATION|ALTER_DATABASE_AUDIT_SPEFICIATION|DENY_DATABASE_AUDIT_SPEFICIATION|  
|CREATE_DATABASE_ENCRYPTION_KEY|ALTER_DATABASE_ENCRYPTION_KEY|DROP_DATABASE_ENCRYPTION_KEY|  
|CREATE_DEFAULT|DROP_DEFAULT||  
|BIND_DEFAULT (S’applique à **sp_bindefault**.)|UNBIND_DEFAULT (S’applique à **sp_unbindefault**.)||  
|CREATE_EVENT_NOTIFICATION|DROP_EVENT_NOTIFICATION||  
|CREATE_EXTENDED_PROPERTY (S’applique à **sp_addextendedproperty**.)|ALTER_EXTENDED_PROPERTY (S’applique à **sp_updateextendedproperty**.)|DROP_EXTENDED_PROPERTY (S’applique à **sp_dropextendedproperty**.)|  
|CREATE_FULLTEXT_CATALOG (S’applique à l’instruction CREATE FULLTEXT CATALOG et à **sp_fulltextcatalog** quand *create* est spécifié.)|ALTER_FULLTEXT_CATALOG (S’applique à l’instruction ALTER FULLTEXT CATALOG, à **sp_fulltextcatalog** quand *start_incremental*, *start_full*, *Stop*ou *Rebuild* est spécifié, et à **sp_fulltext_database** quand *enable* est spécifié.)|DROP_FULLTEXT_CATALOG (S’applique à l’instruction DROP FULLTEXT CATALOG et à **sp_fulltextcatalog** quand *drop* est spécifié.)|  
|CREATE_FULLTEXT_INDEX (S’applique à l’instruction CREATE FULLTEXT INDEX et à **sp_fulltexttable** quand *create* est spécifié.)|ALTER_FULLTEXT_INDEX (S’applique à l’instruction ALTER FULLTEXT INDEX, à **sp_fulltextcatalog** quand *start_full*, *start_incremental*ou *stop* est spécifié, à **sp_fulltext_column**et **sp_fulltext_table** quand toute action autre que *create* ou *drop* est spécifiée.)|DROP_FULLTEXT_INDEX (S’applique à l’instruction DROP FULLTEXT INDEX et à **sp_fulltexttable** quand *drop* est spécifié.)|  
|CREATE_FULLTEXT_STOPLIST|ALTER_FULLTEXT_STOPLIST|DROP_FULLTEXT_STOPLIST|  
|CREATE_FUNCTION|ALTER_FUNCTION|DROP_FUNCTION|  
|CREATE_INDEX|ALTER_INDEX (S’applique à l’instruction ALTER INDEX et à **sp_indexoption**.)|DROP_INDEX|  
|CREATE_MASTER_KEY|ALTER_MASTER_KEY|DROP_MASTER_KEY|  
|CREATE_MESSAGE_TYPE|ALTER_MESSAGE_TYPE|DROP_MESSAGE_TYPE|  
|CREATE_PARTITION_FUNCTION|ALTER_PARTITION_FUNCTION|DROP_PARTITION_FUNCTION|  
|CREATE_PARTITION_SCHEME|ALTER_PARTITION_SCHEME|DROP_PARTITION_SCHEME|  
|CREATE_PLAN_GUIDE (S’applique à **sp_create_plan_guide**.)|ALTER_PLAN_GUIDE (S’applique à **sp_control_plan_guide** quand ENABLE, ENABLE ALL, DISABLE ou DISABLE ALL est spécifié.)|DROP_PLAN_GUIDE (S’applique à **sp_control_plan_guide** quand DROP ou DROP ALL est spécifié.)|  
|CREATE_PROCEDURE|ALTER_PROCEDURE (S’applique à l’instruction ALTER PROCEDURE et à **sp_procoption**.)|DROP_PROCEDURE|  
|CREATE_QUEUE|ALTER_QUEUE|DROP_QUEUE|  
|CREATE_REMOTE_SERVICE_BINDING|ALTER_REMOTE_SERVICE_BINDING|DROP_REMOTE_SERVICE_BINDING|  
|CREATE_SPATIAL_INDEX|||  
|RENAME (S’applique à **sp_rename**)|||  
|CREATE_ROLE (S’applique à l’instruction CREATE ROLE, à **sp_addrole**et à **sp_addgroup**.)|ALTER_ROLE|DROP_ROLE (S’applique à l’instruction DROP ROLE, à **sp_droprole**et à **sp_dropgroup**.)|  
|ADD_ROLE_MEMBER|DROP_ROLE_MEMBER||  
|CREATE_ROUTE|ALTER_ROUTE|DROP_ROUTE|  
|CREATE_RULE|DROP_RULE||  
|BIND_RULE (S’applique à **sp_bindrule**.)|UNBIND_RULE (S’applique à **sp_unbindrule**.)||  
|CREATE_SCHEMA (S’applique à l’instruction CREATE SCHEMA, à **sp_addrole**, à **sp_adduser**, à **sp_addgroup**et à **sp_grantdbaccess**.)|ALTER_SCHEMA (S’applique à l’instruction ALTER SCHEMA et à **sp_changeobjectowner**.)|DROP_SCHEMA|  
|CREATE_SEARCH_PROPERTY_LIST|ALTER_SEARCH_PROPERTY_LIST|DROP_SEARCH_PROPERTY_LIST|  
|CREATE_SEQUENCE_EVENTS|CREATE_SEQUENCE_EVENTS|CREATE_SEQUENCE_EVENTS|  
|CREATE_SERVER_ROLE|ALTER_SERVER_ROLE|DROP_SERVER_ROLE|  
|CREATE_SERVICE|ALTER_SERVICE|DROP_SERVICE|  
|ALTER_SERVICE_MASTER_KEY|BACKUP_SERVICE_MASTER_KEY|RESTORE_SERVICE_MASTER_KEY|  
|ADD_SIGNATURE (pour les opérations de signature sur les objets non compris dans l'étendue du schéma ; base de données, assembly, déclencheur)|DROP_SIGNATURE||  
|ADD_SIGNATURE_SCHEMA_OBJECT (pour les objets compris dans l'étendue du schéma ; procédures stockées, fonctions)|DROP_SIGNATURE_SCHEMA_OBJECT||  
|CREATE_SPATIAL_INDEX|ALTER_INDEX peut être utilisé pour les index spatiaux.|DROP_INDEX peut être utilisé pour les index spatiaux.|  
|CREATE_STATISTICS|DROP_STATISTICS|UPDATE_STATISTICS|  
|CREATE_SYMMETRIC_KEY|ALTER_SYMMETRIC_KEY|DROP_SYMMETRIC_KEY|  
|CREATE_SYNONYM|DROP_SYNONYM||  
|CREATE_TABLE|ALTER_TABLE (S’applique à l’instruction ALTER TABLE et à **sp_tableoption**.)|DROP_TABLE|  
|CREATE_TRIGGER|ALTER_TRIGGER (S’applique à l’instruction ALTER TRIGGER et à **sp_settriggerorder**.)|DROP_TRIGGER|  
|CREATE_TYPE (S’applique à l’instruction CREATE TYPE et à **sp_addtype**.)|DROP_TYPE (S’applique à l’instruction DROP TYPE et à **sp_droptype**.)||  
|CREATE_USER (S’applique à l’instruction CREATE USER, à **sp_adduser**et à **sp_grantdbaccess**.)|ALTER_USER (S’applique à l’instruction ALTER USER et à **sp_change_users_login**.)|DROP_USER (S’applique à l’instruction DROP USER, à **sp_dropuser**et à **sp_revokedbaccess**.)|  
|CREATE_VIEW|ALTER_VIEW|DROP_VIEW|  
|CREATE_XML_INDEX|ALTER_INDEX peut être utilisé pour les index XML.|DROP_INDEX peut être utilisé pour les index XML.|  
|CREATE_XML_SCHEMA_COLLECTION|ALTER_XML_SCHEMA_COLLECTION|DROP_XML_SCHEMA_COLLECTION|  
  
## <a name="ddl-statements-that-have-server-scope"></a>Instructions DDL qui ont une étendue à l'échelle du serveur  
 Les déclencheurs ou les notifications d'événements DDL peuvent être créés de façon à se déclencher en réponse aux événements suivants lorsqu'ils se produisent à un emplacement quelconque dans l'instance du serveur.  
  
||||  
|-|-|-|  
|ALTER_AUTHORIZATION_SERVER|ALTER_SERVER_CONFIGURATION|ALTER_INSTANCE (S’applique à **sp_configure** et à **sp_addserver** quand une instance du serveur local est spécifiée.)|  
|CREATE_AVAILABILITY_GROUP|ALTER_AVAILABILITY_GROUP|DROP_AVAILABILITY_GROUP|  
|CREATE_CREDENTIAL|ALTER_CREDENTIAL|DROP_CREDENTIAL|  
|CREATE_CRYPTOGRAPHIC_PROVIDER|ALTER_CRYPTOGRAPHIC_PROVIDER|DROP_CRYPTOGRAPHIC_PROVIDER|  
|CREATE_DATABASE|ALTER_DATABASE (S’applique à l’instruction ALTER DATABASE et à **sp_fulltext_database**.)|DROP_DATABASE|  
|CREATE_ENDPOINT|ALTER_ENDPOINT|DROP_ENDPOINT|  
|CREATE_EVENT_SESSION|ALTER_EVENT_SESSION|DROP_EVENT_SESSION|  
|CREATE_EXTENDED_PROCEDURE (S’applique à **sp_addextendedproc**.)|DROP_EXTENDED_PROCEDURE (S’applique à **sp_dropextendedproc**.)||  
|CREATE_LINKED_SERVER (S’applique à **sp_addlinkedserver**.)|ALTER_LINKED_SERVER (S’applique à **sp_serveroption**.)|DROP_LINKED_SERVER (S’applique à **sp_dropserver** quand un serveur lié est spécifié.)|  
|CREATE_LINKED_SERVER_LOGIN (S’applique à **sp_addlinkedsrvlogin**.)|DROP_LINKED_SERVER_LOGIN (S’applique à **sp_droplinkedsrvlogin**.)||  
|CREATE_LOGIN (S’applique à l’instruction CREATE LOGIN, à **sp_addlogin**, à **sp_grantlogin**, à **xp_grantlogin**et à **sp_denylogin** en cas d’utilisation sur une connexion inexistante qui doit être créée de manière implicite.)|ALTER_LOGIN (S’applique à l’instruction ALTER LOGIN, à **sp_defaultdb**, à **sp_defaultlanguage**, à **sp_password**et à **sp_change_users_login** quand *Auto_Fix* est spécifié.)|DROP_LOGIN (S’applique à l’instruction DROP LOGIN, à **sp_droplogin**, à **sp_revokelogin**et à **xp_revokelogin**.)|  
|CREATE_MESSAGE (S’applique à **sp_addmessage**.)|ALTER_MESSAGE (S’applique à **sp_altermessage**.)|DROP_MESSAGE (S’applique à **sp_dropmessage**.)|  
|CREATE_REMOTE_SERVER (S’applique à **sp_addserver**.)|ALTER_REMOTE_SERVER (S’applique à **sp_setnetname**.)|DROP_REMOTE_SERVER (S’applique à **sp_dropserver** quand un serveur distant est spécifié.)|  
|CREATE_RESOURCE_POOL|ALTER_RESOURCE_POOL|DROP_RESOURCE_POOL|  
|GRANT_SERVER|DENY_SERVER|REVOKE_SERVER|  
|ADD_SERVER_ROLE_MEMBER|DROP_SERVER_ROLE_MEMBER||  
|CREATE_SERVER_AUDIT|ALTER_SERVER_AUDIT|DROP_SERVER_AUDIT|  
|CREATE_SERVER_AUDIT_SPECIFICATION|ALTER_SERVER_AUDIT_SPECIFICATION|DROP_SERVER_AUDIT_SPECIFICATION|  
|CREATE_WORKLOAD_GROUP|ALTER_WORKLOAD_GROUP|DROP_WORKLOAD_GROUP|  
  
## <a name="see-also"></a> Voir aussi  
 [Déclencheurs DDL](../../relational-databases/triggers/ddl-triggers.md)   
 [Notifications d’événements](../../relational-databases/service-broker/event-notifications.md)   
 [Groupes d’événements DDL](../../relational-databases/triggers/ddl-event-groups.md)  
  
  
