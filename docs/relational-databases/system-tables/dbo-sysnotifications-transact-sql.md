---
title: dbo.sysnotifications (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 03/04/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-tables
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- dbo.sysnotifications_TSQL
- sysnotifications
- sysnotifications_TSQL
- dbo.sysnotifications
dev_langs:
- TSQL
helpviewer_keywords:
- sysnotifications system table
ms.assetid: c5150d18-e8b7-48a7-ada7-77c583af6e41
caps.latest.revision: 
author: stevestein
ms.author: sstein
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: ea3f7a24c97a2ee0fd9664df0b81eb5b03f7f4b0
ms.sourcegitcommit: c556eaf60a49af7025db35b7aa14beb76a8158c5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="dbosysnotifications-transact-sql"></a>dbo.sysnotifications (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque notification.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**alert_id**|**int**|ID de l'alerte.|  
|**operator_id**|**int**|ID de l'opérateur à qui la notification doit être envoyée.|  
|**notification_method**|**tinyint**|Méthode de notification :<br /><br /> **1** = courrier électronique<br /><br /> **2** = radiomessagerie<br /><br /> **4** = **netsend**<br /><br /> **7** = all|  
  
  
