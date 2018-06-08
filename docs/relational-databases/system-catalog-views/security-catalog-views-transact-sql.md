---
title: Affichages catalogue de sécurité (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 02/27/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- cryptography [SQL Server], catalog views
- encryption [SQL Server], catalog views
- catalog views [SQL Server], security
- security catalog views [SQL Server]
ms.assetid: 4d5cf1bf-09a7-4ee0-9dbb-5c584750fc67
caps.latest.revision: 36
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a0a9eb44272ca932831b3084ab6fe2a0eb6840f1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="security-catalog-views-transact-sql"></a>Affichages catalogue liées à la sécurité (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Les informations relatives à la sécurité sont exposées dans des affichages catalogue qui sont optimisés pour fournir un maximum de performances et de fonctionnalités. Lorsque cela est possible, utilisez les affichages catalogue suivants pour accéder aux métadonnées du catalogue.  
  
## <a name="database-level-views"></a>Affichages de niveau base de données  
  
|||  
|-|-|  
|[sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)|[sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md) |  
|[sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)|[sys.master_key_passwords](../../relational-databases/system-catalog-views/sys-master-key-passwords-transact-sql.md) |  
|[sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|[sys.user_token](../../relational-databases/system-catalog-views/sys-user-token-transact-sql.md) |  
  
## <a name="server-level-views"></a>Affichages de niveau serveur  
  
|||  
|-|-|  
|[sys.credentials](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)|[sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)|  
|[sys.login_token](../../relational-databases/system-catalog-views/sys-login-token-transact-sql.md)|[sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)|  
|[sys.securable_classes](../../relational-databases/system-catalog-views/sys-securable-classes-transact-sql.md)|[sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)|  
|[sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)|[sys.system_components_surface_area_configuration](../../relational-databases/system-catalog-views/sys-system-components-surface-area-configuration-transact-sql.md)|  
  
## <a name="encryption-views"></a>Affichages relatifs au chiffrement  
  
|||  
|-|-|  
|[sys.asymmetric_keys](../../relational-databases/system-catalog-views/sys-asymmetric-keys-transact-sql.md)|[sys.cryptographic_providers](../../relational-databases/system-catalog-views/sys-cryptographic-providers-transact-sql.md)|  
|[sys.certificates](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|[sys.key_encryptions](../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)|  
|[sys.column_encryption_key_values](../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)|[sys.openkeys](../../relational-databases/system-catalog-views/sys-openkeys-transact-sql.md)|  
|[Sys.column_encryption_keys](../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)|[Sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)|  
|[Sys.column_master_key_definitions](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)|[sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)|  
|[sys.crypt_properties](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md)|[sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)|  
  
## <a name="sql-server-audit-views"></a>Affichages d'audit SQL Server  
  
|||  
|-|-|  
|[sys.server_audits](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)|[sys.server_file_audits](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)|  
|[sys.server_audit_specifications](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)|[sys.server_audit_specifications_details](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)|  
|[sys.database_ audit_specifications](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)|[Sys.audit_database_specification_details](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
