---
title: sp_apply_job_to_targets (Transact-SQL) | Documents Microsoft
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
f1_keywords:
- sp_apply_job_to_targets
- sp_apply_job_to_targets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_apply_job_to_targets
ms.assetid: 4a3e9173-7e3c-4100-a9ac-2f5d2c60a8b0
caps.latest.revision: 34
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7e7ec6285f807dd51b0a67ad55ab2e53634584e5
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spapplyjobtotargets-transact-sql"></a>sp_apply_job_to_targets (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Applique un travail à un ou plusieurs serveurs cibles ou aux serveurs cibles d'un ou de plusieurs groupes de serveurs cibles.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_apply_job_to_targets { [ @job_id = ] job_id | [ @job_name = ] 'job_name' }  
     [ , [ @target_server_groups = ] 'target_server_groups' ]   
     [ , [ @target_servers = ] 'target_servers' ]   
     [ , [ @operation = ] 'operation' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id =**] *job_id*  
 Numéro d'identification du travail à appliquer aux serveurs ou groupes de serveurs cibles spécifiés. *job_id* est **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
 [  **@job_name =**] **'***job_name***'**  
 Nom du travail à appliquer aux serveurs ou groupes de serveurs cibles associés spécifiés. *job_name* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [ **@target_server_groups =**]  **'***target_server_groups***'**  
 Liste référençant, entre virgules, les groupes de serveurs cibles auxquels le travail spécifié est appliqué. *groupes_serveurs_cibles* est **nvarchar (2048)**, avec NULL comme valeur par défaut.  
  
 [  **@target_servers=** ] **'***serveurs_cibles***'**  
 Liste référençant, entre virgules, les serveurs cibles auxquels le travail spécifié est appliqué. *serveurs_cibles*est **nvarchar (2048)**, avec NULL comme valeur par défaut.  
  
 [  **@operation=** ] **'***opération***'**  
 Indique si le travail spécifié doit être appliqué ou supprimé des serveurs ou groupes de serveurs cibles indiqués. *opération*est **varchar(7)**, avec une valeur par défaut de l’appliquer. Les opérations valides sont **appliquer** et **supprimer**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_apply_job_to_targets** fournit un moyen simple pour appliquer (ou supprimer) un travail à partir de plusieurs serveurs cibles, et constitue une alternative à l’appel **sp_add_jobserver** (ou **sp_delete_jobserver**) une fois pour chaque serveur cible est requis.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant applique le travail `Backup Customer Information` créé précédemment à tous les serveurs cibles du groupe `Servers Maintaining Customer Information`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_apply_job_to_targets  
    @job_name = N'Backup Customer Information',  
    @target_server_groups = N'Servers Maintaining Customer Information',   
    @operation = N'APPLY' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
