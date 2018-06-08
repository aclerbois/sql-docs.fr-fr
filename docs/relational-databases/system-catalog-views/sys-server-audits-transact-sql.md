---
title: Sys.server_audits (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- server_audits_TSQL
- sys.server_audits_TSQL
- sys.server_audits
- server_audits
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_audits catalog view
ms.assetid: c2c4a000-1127-46a8-b1e9-947fd1136e1e
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f71da894aa31fa192c5abc6f64683fa1c2036f99
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysserveraudits-transact-sql"></a>sys.server_audits (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque audit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans une instance de serveur. Pour plus d’informations, consultez [SQL Server Audit &#40moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**audit_id**|**int**|ID de l'audit.|  
|**nom**|**sysname**|Nom de l’audit.|  
|**AUDIT_GUID**|**uniqueidentifier**|GUID de l’audit qui est utilisé pour énumérer les audits avec membre Server&#124;spécifications d’audit de base de données pendant le démarrage du serveur et base de données les opérations d’attachement.|  
|**create_date**|**datetime**|Date UTC de création de l'audit.|  
|**modify_date**|**datetime**|Date UTC de dernière modification de l'audit.|  
|**principal_id**|**int**|ID du propriétaire de l’audit, tel qu’enregistré dans le serveur.|  
|**type**|**char(2)**|Type d'audit :<br /><br /> SL – Journal des événements de sécurité NT<br /><br /> AL – Journal des événements d'applications NT<br /><br /> FL – Fichier sur le système de fichiers|  
|**type_desc**|**nvarchar(60)**|JOURNAL DE SÉCURITÉ<br /><br /> JOURNAL DES APPLICATIONS<br /><br /> FILE|  
|**ON_FAILURE**|**tinyint**|En cas d'échec d'écriture d'une entrée d'audit :<br /><br /> 0 – Continuer<br /><br /> 1 – Arrêter l'instance de serveur<br /><br /> 2 – Faire échouer l'opération|  
|**on_failure_desc**|**nvarchar(60)**|En cas d'échec d'écriture d'une entrée d'audit :<br /><br /> CONTINUE<br /><br /> SHUTDOWN SERVER INSTANCE<br /><br /> FAIL_OPERATION|  
|**is_state_enabled**|**tinyint**|0 – Désactivé<br /><br /> 1 – Activé|  
|**QUEUE_DELAY**|**int**|Durée maximale, en millisecondes, avant d'écrire sur disque. Si la valeur 0 est spécifiée, l'audit garantira une écriture avant qu'un événement puisse continuer.|  
|**prédicat**|**nvarchar(3000)**|L’expression de prédicat est appliquée à l’événement.|  
  
## <a name="permissions"></a>Autorisations  
 Les principaux avec le **ALTER ANY SERVER AUDIT** ou **VIEW ANY DEFINITION** autorisation ont accès à cette vue de catalogue. En outre, l’entité de sécurité ne doit pas être refusée **VIEW ANY DEFINITION** autorisation.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Créer un audit du serveur et une spécification d’audit du serveur](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
