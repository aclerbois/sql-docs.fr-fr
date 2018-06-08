---
title: Sys.key_encryptions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.key_encryptions
- key_encryptions_TSQL
- sys.key_encryptions_TSQL
- key_encryptions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.key_encryptions catalog view
ms.assetid: c39cecf8-af63-40b9-98e5-f84a5bf3ae54
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 160c4852299ff710c4c281dc93231a6385194376
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syskeyencryptions-transact-sql"></a>sys.key_encryptions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Renvoie une ligne pour chaque chiffrement à clé symétrique spécifié au moyen de la clause ENCRYPTION BY de l'instruction CREATE SYMMETRIC KEY.  

  
|Noms des colonnes|Types de données| Description|  
|------------------|----------------|-----------------|  
|**key_id**|**int**|ID de la clé chiffrée.|  
|**empreinte numérique**|**varbinary(32)**|Hachage SHA-1 du certificat avec lequel la clé est chiffrée ou GUID de la clé symétrique avec laquelle la clé est chiffrée.|  
|**crypt_type**|**char (4)**|Type de chiffrement :<br /><br /> ESKS = Chiffrement par clé symétrique<br /><br /> ESP3, ESP2 ou ESKP = chiffrement par mot de passe<br /><br /> EPUC = Chiffrement par certificat<br /><br /> EPUA = Chiffrement par clé asymétrique<br /><br /> ESKM = Chiffrement par clé principale|  
|**crypt_type_desc**|**nvarchar(60)**|Description du type de chiffrement :<br /><br /> ENCRYPTION BY SYMMETRIC KEY<br /><br /> ENCRYPTION BY PASSWORD <br />(À partir de [!INCLUDE[sssqlv14_md](../../includes/sssqlv14-md.md)], inclut un numéro de version pour une utilisation par le CSS.)<br /><br /> ENCRYPTION BY CERTIFICATE<br /><br /> ENCRYPTION BY ASYMMETRIC KEY<br /><br /> ENCRYPTION BY MASTER KEY<br /><br /> Remarque : Windows DPAPI est utilisé pour protéger la clé principale du service.|  
|**crypt_property**|**varbinary(max)**|Bits signés ou chiffrés.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
  
