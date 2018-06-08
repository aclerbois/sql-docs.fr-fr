---
title: Sys.dm_cryptographic_provider_keys (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_cryptographic_provider_keys_TSQL
- dm_cryptographic_provider_keys_TSQL
- dm_cryptographic_provider_keys
- sys.dm_cryptographic_provider_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_keys dynamic management function
ms.assetid: 5a8c1421-c56b-44b5-96e5-4f01782a0c7c
caps.latest.revision: 11
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 26d789882ec717a2d69796a90284c22a3ad2aa44
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmcryptographicproviderkeys-transact-sql"></a>sys.dm_cryptographic_provider_keys (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les clés fournies par un fournisseur de gestion de clés extensible (EKM).  

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dm_cryptographic_provider_keys ( provider_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *provider_id*  
 Numéro d'identification du fournisseur EKM, sans valeur par défaut.  
  
## <a name="tables-returned"></a>Tables retournées  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**key_id**|**int**|Numéro d'identification de la clé sur le fournisseur.|  
|**key_name**|**nvarchar(512)**|Nom de la clé sur le fournisseur.|  
|**key_thumbprint**|**varbinary(32)**|Empreinte numérique du fournisseur de la clé.|  
|**algorithm_id**|**int**|Numéro d'identification de l'algorithme sur le fournisseur.|  
|**algorithm_tag**|**int**|Balise de l'algorithme sur le fournisseur.|  
|**key_type**|**nchar(256)**|Type de clé sur le fournisseur.|  
|**key_length**|**int**|Longueur de la clé sur le fournisseur.|  
  
## <a name="permissions"></a>Autorisations  
 Lorsque cette vue est interrogée, elle authentifie le contexte utilisateur auprès du fournisseur et énumère toutes les clés visibles à l'utilisateur.  
  
 Si l'utilisateur ne peut pas s'authentifier auprès du fournisseur EKM, aucune information de clé n'est retournée.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant affiche les propriétés de clé pour un fournisseur portant le numéro d'identification `1234567`.  
  
```  
SELECT * FROM sys.dm_cryptographic_provider_keys(1234567);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
