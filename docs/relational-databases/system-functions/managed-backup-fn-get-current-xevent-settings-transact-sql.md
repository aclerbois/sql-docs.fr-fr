---
title: managed_backup.fn_get_current_xevent_settings (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- fn_get_current_xevent_settings
- smart_admin.fn_get_current_xevent_settings_TSQL
- fn_get_current_xevent_settings_TSQL
- smart_admin.fn_get_current_xevent_settings
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_current_xevent_settings
- fn_get_current_xevent_settings
ms.assetid: 95d3adaa-bb9d-4833-b8b4-3d9fd4f9c82a
caps.latest.revision: 11
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 26fc0678d8597cc8a56211e829bdc598c734f168
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="managedbackupfngetcurrentxeventsettings-transact-sql"></a>managed_backup.fn_get_current_xevent_settings (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque type d'événement étendu pris en charge par Smart Admin.  
  
 Utilisez cette fonction pour retourner ou consulter les paramètres des événements étendus actuels afin d'identifier le type des événements qui sont configurables et les configurations actuelles.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
smart_admin.fn_get_current_xevent_settings ()   
```  
  
##  <a name="Arguments"></a> Arguments  
 Cette fonction n'a aucun argument.  
  
## <a name="table-returned"></a>Table retournée  
 Les canaux d'administration, analytique et opérationnel des événements étendus sont nécessaires, activés par défaut et non configurables.  
  
|Nom de la colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|Event_name|NVARCHAR(128)|Type d'événement étendu|  
|is_configurable|NVARCHAR(128)|Cette option est définie **True** si l’événement est configurable, sinon elle valeur **False**.|  
|is_enabled|NVARCHAR(128)|A la valeur True si l'événement est activé, sinon, a la valeur False. Utilisez smart_admin.sp_set_parameter pour activer les événements de débogage.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert **sélectionnez** autorisations sur la fonction.  
  
## <a name="examples"></a>Exemples  
 Cet exemple renvoie tous les événements étendus avec leur état actuel.  
  
```  
SELECT *   
FROM smart_admin.fn_get_current_xevent_settings ()  
  
```  
  
  
