---
title: Sys.dm_cryptographic_provider_algorithms (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_cryptographic_provider_algorithms_TSQL
- sys.dm_cryptographic_provider_algorithms
- sys.dm_cryptographic_provider_algorithms_TSQL
- dm_cryptographic_provider_algorithms
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_algorithms dynamic management function
ms.assetid: 8bcccb37-5cfb-4e1e-a0bb-7ff4c279fe8e
caps.latest.revision: 12
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 9e8a16f5247cbd1ef142c4698c1e9781fd7be45d
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmcryptographicprovideralgorithms-transact-sql"></a>sys.dm_cryptographic_provider_algorithms (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne les algorithmes pris en charge par un fournisseur de gestion de clés Extensible (EKM).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_cryptographic_provider_algorithms ( provider_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *provider_id*  
 Numéro d'identification du fournisseur EKM, sans valeur par défaut.  
  
## <a name="tables-returned"></a>Tables retournées  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|algorithm_id|**int**|Numéro d'identification de l'algorithme.|  
|algorithm_tag|**nvarchar(60)**|Balise d'identification de l'algorithme.|  
|key_type|**nvarchar(128)**|Indique le type de clé. Retourne ASYMMETRIC KEY ou SYMMETRIC KEY.|  
|key_length|**int**|Indique la longueur de la clé en bits.|  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit être membre du rôle de base de données public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant affiche les options d'un fournisseur portant le numéro d'identification `1234567`.  
  
```  
SELECT * FROM sys.dm_cryptographic_provider_algorithms(1234567);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
