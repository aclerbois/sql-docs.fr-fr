---
title: Plan de Maintenance de base de données procédures stockées (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- database maintenance plans [SQL Server]
- system stored procedures [SQL Server], database maintenance plans
- maintenance plans [SQL Server], stored procedures
ms.assetid: f5f658e3-417e-4286-9213-5738266f3b28
caps.latest.revision: 14
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 43ba326369983800065b6ae23cfa83e35ad034f3
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="database-maintenance-plan-stored-procedures-transact-sql"></a>Procédures stockées du plan de maintenance de base de données (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge les suivant procédures stockées système qui permettent de configurer des tâches de maintenance. Ces procédures stockées s'utilisent avec des plans de maintenance de base de données. Cette fonctionnalité a été remplacée par des plans de maintenance qui n'utilisent pas ces procédures stockées. Utilisez ces procédures pour gérer des plans de maintenance de base de données sur des installations qui ont été mises à niveau à partir d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
|||  
|-|-|  
|[sp_add_maintenance_plan](../../relational-databases/system-stored-procedures/sp-add-maintenance-plan-transact-sql.md)|[sp_delete_maintenance_plan_db](../../relational-databases/system-stored-procedures/sp-delete-maintenance-plan-db-transact-sql.md)|  
|[sp_add_maintenance_plan_db](../../relational-databases/system-stored-procedures/sp-add-maintenance-plan-db-transact-sql.md)|[sp_delete_maintenance_plan_job](../../relational-databases/system-stored-procedures/sp-delete-maintenance-plan-job-transact-sql.md)|  
|[sp_add_maintenance_plan_job](../../relational-databases/system-stored-procedures/sp-add-maintenance-plan-job-transact-sql.md)|[sp_help_maintenance_plan](../../relational-databases/system-stored-procedures/sp-help-maintenance-plan-transact-sql.md)|  
|[sp_delete_maintenance_plan](../../relational-databases/system-stored-procedures/sp-delete-maintenance-plan-transact-sql.md)||  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)  
  
  
