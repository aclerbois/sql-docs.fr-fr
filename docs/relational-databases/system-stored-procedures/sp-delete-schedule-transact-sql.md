---
title: sp_delete_schedule (Transact-SQL) | Documents Microsoft
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
- sp_delete_schedule
- sp_delete_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_schedule
ms.assetid: 18b2c985-47b8-49c8-82d1-8a4af3d7d33a
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 14fd520f5447092e5f82dc786696148f3dbe3bd3
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdeleteschedule-transact-sql"></a>sp_delete_schedule (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime une planification.  
 
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_schedule { [ @schedule_id = ] schedule_id | [ @schedule_name = ] 'schedule_name' } ,  
     [ @force_delete = ] force_delete  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@schedule_id=** ] *id_de_la_planification*  
 Numéro d'identification de la planification à supprimer. *id_de_la_planification* est **int**, avec NULL comme valeur par défaut.  
  
> **Remarque :** soit *id_de_la_planification* ou *nom_de_la_planification* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [  **@schedule_name=** ] **'***nom_de_la_planification***'**  
 Nom de la planification à supprimer. *nom_de_la_planification* est **sysname**, avec NULL comme valeur par défaut.  
  
> **Remarque :** soit *id_de_la_planification* ou *nom_de_la_planification* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [ **@force_delete** =] *force_delete*  
 Spécifie si la procédure doit échouer lorsque la planification est attachée à un travail. *Force_delete* est de type bit, avec une valeur par défaut **0**. Lorsque *force_delete* est **0**, la procédure stockée échoue si la planification est attachée à un travail. Lorsque *force_delete* est **1**, la planification est supprimée indépendamment de si la planification est attachée à un travail.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Par défaut, il est impossible de supprimer une planification si elle est attachée à un travail. Pour supprimer une planification qui est attachée à un travail, spécifiez la valeur **1** pour *force_delete*. La suppression d'une planification n'arrête pas les travaux en cours d'exécution.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Notez que le propriétaire du travail peut joindre un travail à une planification et détacher un travail d'une planification sans être le propriétaire de la planification. Toutefois, une planification ne peut pas être supprimée si le détachement la conserve sans travaux, sauf si l’appelant est propriétaire de la planification.  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
 Seuls les membres de la **sysadmin** rôle peut supprimer une planification de travail appartenant à un autre utilisateur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-deleting-a-schedule"></a>A. Suppression d'une planification  
 L'exemple suivant supprime la planification `NightlyJobs`. Si la planification est attachée à un travail, l'exemple ne la supprime pas.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="b-deleting-a-schedule-attached-to-a-job"></a>B. Suppression d'une planification attachée à un travail  
 L'exemple suivant supprime la planification `RunOnce`, qu'elle soit ou non attachée à un travail.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = 'RunOnce',  
    @force_delete = 1;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Implémenter des travaux](http://msdn.microsoft.com/library/69e06724-25c7-4fb3-8a5b-3d4596f21756)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
  
