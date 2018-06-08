---
title: Sys.sysoledbusers (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-compatibility-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.sysoledbusers
- sys.sysoledbusers_TSQL
- sysoledbusers
- sysoledbusers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysoledbusers system table
- sys.sysoledbusers compatibility view
ms.assetid: fe924c17-9cad-4b2b-8124-1e0fd82931e3
caps.latest.revision: 34
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 9bed55c42d7d656f53e24acb1d859658666ce901
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssysoledbusers-transact-sql"></a>sys.sysoledbusers (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

    
> [!IMPORTANT]  
>  Cette table système [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] est incluse dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en tant que vue seulement pour des raisons de compatibilité descendante. Nous vous recommandons d’utiliser [affichages catalogue](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md) à la place.  
  
 Contient une ligne pour chaque mappage d'utilisateur et de mot de passe pour le serveur lié spécifié. **sysoledbusers** est stocké dans le **master** base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**rmtsrvid**|**smallint**|Numéro d'identification de sécurité (SID) du serveur.|  
|**rmtloginame**|**nvarchar(** 128 **)**|Nom de la connexion à distance qui **loginsid** mappée par lié **rmtservid**.|  
|**rmtpassword**|**nvarchar(** 128 **)**|Renvoie NULL.|  
|**loginsid**|**varbinary(** 85 **)**|SID de la connexion locale à mapper.|  
|**status**|**smallint**|Si la valeur est 1, le mappage doit utiliser les informations d'identification de l'utilisateur.|  
|**changedate**|**datetime**|Date de la dernière modification des informations de mappage.|  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
