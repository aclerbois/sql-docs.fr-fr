---
title: sp_help_maintenance_plan (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_help_maintenance_plan_TSQL
- sp_help_maintenance_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_maintenance_plan
ms.assetid: e972a510-960e-41d6-93c5-c71cd581a585
caps.latest.revision: 23
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 8c144581e7d1ed73da251d13419d5ec04a7fd687
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpmaintenanceplan-transact-sql"></a>sp_help_maintenance_plan (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie des informations sur le plan de maintenance spécifié. Si aucun plan n'est spécifié, cette procédure stockée renvoie des informations sur tous les plans de maintenance.  
  
> **Remarque :** cette procédure stockée est utilisée avec les plans de maintenance de base de données. Cette fonctionnalité a été remplacée par des plans de maintenance qui n'utilisent pas cette procédure stockée. Utilisez cette procédure pour gérer des plans de maintenance de base de données sur des installations qui ont été mises à niveau à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_maintenance_plan [ [ @plan_id = ] 'plan_id' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@plan_id =**] **'***plan_id***'**  
 Spécifie l’ID de plan du plan de maintenance. *plan_id* est **UNIQUEIDENTIFIER**. La valeur par défaut est NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *plan_id* est spécifié, **sp_help_maintenance_plan** renvoie trois tables : Plan, Database et Job.  
  
### <a name="plan-table"></a>Table Plan  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|Identificateur du plan de maintenance.|  
|**plan_name**|**sysname**|Nom du plan de maintenance.|  
|**date_created**|**datetime**|Date de création du plan de maintenance.|  
|**propriétaire**|**sysname**|Propriétaire du plan de maintenance.|  
|**max_history_rows**|**int**|Nombre maximal de lignes allouées pour l'enregistrement de l'historique du plan de maintenance dans la table système.|  
|**remote_history_server**|**int**|Le nom du serveur distant sur lequel le rapport d’historique peut être écrit.|  
|**max_remote_history_rows**|**int**|Nombre maximal de lignes allouées dans la table système d'un serveur distant sur lequel le rapport de l'historique peut être écrit.|  
|**user_defined_1**|**int**|La valeur par défaut est NULL.|  
|**user_defined_2**|**nvarchar(100)**|La valeur par défaut est NULL.|  
|**user_defined_3**|**datetime**|La valeur par défaut est NULL.|  
|**user_defined_4**|**uniqueidentifier**|La valeur par défaut est NULL.|  
  
### <a name="database-table"></a>Table Database  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|**database_name**|Nom de toutes les bases de données associées au plan de maintenance. *database_name* est de type **sysname**.|  
  
### <a name="job-table"></a>Table Job  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|**job_id**|Identificateur de tous les travaux associés au plan de maintenance. *job_id* est **uniqueidentifier**.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_maintenance_plan** est dans le **msdb** base de données.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_help_maintenance_plan**.  
  
## <a name="examples"></a>Exemples  
 Cet exemple fournit des informations détaillées sur le plan de maintenance FAD6F2AB-3571-11D3-9D4A-00C04FB925FC.  
  
```  
EXECUTE   sp_help_maintenance_plan   
   N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Procédures stockées de Plan de Maintenance de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  
