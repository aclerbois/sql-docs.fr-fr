---
title: Sys.login_token (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 06/10/2016
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-catalog-views
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- login_token_TSQL
- sys.login_token_TSQL
- sys.login_token
- login_token
dev_langs:
- TSQL
helpviewer_keywords:
- sys.login_token catalog view
- logins [SQL Server], security tokens
- tokens [SQL Server]
ms.assetid: 86e06938-9d0a-44e5-99e2-55c8ef5f2f84
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 03e4877be05e3275b51c4e8ed2609b61a0c5dfd7
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="syslogintoken-transact-sql"></a>sys.login_token (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque principal de serveur appartenant au jeton de connexion.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**principal_id**|**int**|ID du principal. Cette valeur est unique dans le serveur.|  
|**SID**|**varbinary(85)**|Identificateur de sécurité du principal. S’il s’agit d’un principal Windows, **sid** = SID Windows. Si la connexion est mappée à un certificat, **sid** = GUID à partir du certificat.|  
|**nom**|**nvarchar (128)**|Nom de l’entité de sécurité. Cette valeur est unique dans le serveur.|  
|**type**|**nvarchar (128)**|Description du type de principal. Tous les types sont mappés aux **sid**. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> SQL LOGIN<br /><br /> WINDOWS LOGIN<br /><br /> WINDOWS GROUP<br /><br /> SERVER ROLE<br /><br /> LOGIN MAPPED TO CERTIFICATE<br /><br /> LOGIN MAPPED TO ASYMMETRIC KEY<br /><br /> CERTIFICATE<br /><br /> ASYMMETRIC KEY|  
|**utilisation**|**nvarchar (128)**|Indique que le principal prend part à l'évaluation des autorisations GRANT ou DENY, ou sert d'authentificateur.<br /><br /> Cette valeur peut être une des opérations suivantes :<br /><br /> GRANT OR DENY<br /><br /> DENY ONLY<br /><br /> AUTHENTICATOR|  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.user_token &#40; Transact-SQL &#41;](../../relational-databases/system-catalog-views/sys-user-token-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
