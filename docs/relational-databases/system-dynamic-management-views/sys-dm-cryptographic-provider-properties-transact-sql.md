---
title: Sys.dm_cryptographic_provider_properties (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_cryptographic_provider_properties_TSQL
- sys.dm_cryptographic_provider_properties
- sys.dm_cryptographic_provider_properties_TSQL
- dm_cryptographic_provider_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_properties dynamic management view
ms.assetid: 024b0095-6766-4189-a39a-d316c5ec2874
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: af1e68502ad7be83a8cfa8f3477420d7336e5f43
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmcryptographicproviderproperties-transact-sql"></a>sys.dm_cryptographic_provider_properties (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les fournisseurs de services de chiffrement inscrits.  
  
 
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|provider_id|**int**|Numéro d'identification du fournisseur de services de chiffrement.|  
|guid|**uniqueidentifier**|GUID unique du fournisseur.|  
|provider_version|**nvarchar (256)**|Version du fournisseur dans le format '*aa.bb.cccc.dd*'.|  
|sqlcrypt_version|**nvarchar (256)**|Version principale de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] API de chiffrement dans le format '*aa.bb.cccc.dd*'.|  
|friendly_name|**nvarchar(2048)**|Nom fourni par le fournisseur.|  
|authentication_type|**nvarchar (256)**|WINDOWS, BASIC ou autres.|  
|symmetric_key_support|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
|symmetric_key_export|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
|symmetric_key_import|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
|symmetric_key_persistance|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
|asymmetric_key_support|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
|asymmetric_key_export|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
|symmetric_key_import|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
|symmetric_key_persistance|**tinyint**|0 (non pris en charge)<br /><br /> 1 (pris en charge)|  
  
## <a name="remarks"></a>Notes  
 La vue sys.dm_cryptographic_provider_properties est visible au public.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
