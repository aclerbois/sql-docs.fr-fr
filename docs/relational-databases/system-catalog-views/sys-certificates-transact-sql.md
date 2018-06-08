---
title: Sys.Certificates (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- certificates
- certificates_TSQL
- sys.certificates_TSQL
- sys.certificates
dev_langs:
- TSQL
helpviewer_keywords:
- sys.certificates catalog view
ms.assetid: e5046102-a65c-401e-b80d-05636884dec9
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f5aaf349d050b2b1b1d27ae3067f003186c8c6eb
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syscertificates-transact-sql"></a>sys.certificates (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie une ligne pour chaque certificat dans la base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom du certificat. Unique dans la base de données.|  
|**certificate_id**|**int**|ID du certificat. Unique dans la base de données.|  
|**principal_id**|**int**|ID du principal de la base de données propriétaire de ce certificat.|  
|**pvt_key_encryption_type**|**char(2)**|Mode de chiffrement de la clé privée.<br /><br /> NA = Il n'existe aucune clé privée pour le certificat<br /><br /> MK = La clé privée est chiffrée par la clé principale<br /><br /> PW = La clé privée est chiffrée par un mot de passe défini par l'utilisateur<br /><br /> SK = La clé privée est chiffrée par la clé principale du service|  
|**pvt_key_encryption_type_desc**|**nvarchar(60)**|Description du mode de chiffrement de la clé privée.<br /><br /> NO_PRIVATE_KEY<br /><br /> ENCRYPTED_BY_MASTER_KEY<br /><br /> ENCRYPTED_BY_PASSWORD<br /><br /> ENCRYPTED_BY_SERVICE_MASTER_KEY|  
|**is_active_for_begin_dialog**|**bit**|Si cette valeur est égale à 1, ce certificat est utilisé pour lancer des échanges de service chiffrés.|  
|**issuer_name**|**nvarchar(442)**|Nom de l'émetteur du certificat.|  
|**cert_serial_number**|**nvarchar(64)**|Numéro de série du certificat.|  
|**sid**|**varbinary(85)**|Numéro d'identification de sécurité (SID) de connexion de ce certificat.|  
|**string_sid**|**nvarchar(128)**|Représentation en chaîne de caractères du numéro d'identification de sécurité (SID) de connexion de ce certificat|  
|**subject**|**nvarchar(4000)**|Objet de ce certificat.|  
|**expiry_date**|**datetime**|Date d'expiration du certificat.|  
|**start_date**|**datetime**|Moment où le certificat devient valide.|  
|**empreinte numérique**|**varbinary(32)**|Hachage SHA-1 du certificat. Hachage SHA-1 globalement unique.|  
|**attested_by**|**nvarchar(260)**|Utilisation réservée au système.|  
|pvt_key_last_backup_date|**datetime**|Date et heure de la dernière exportation de la clé privée du certificat.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
  
  
