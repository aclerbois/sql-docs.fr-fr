---
title: sp_post_msx_operation (Transact-SQL) | Documents Microsoft
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
- sp_post_msx_operation
- sp_post_msx_operation_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_post_msx_operation
ms.assetid: 085deef8-2709-4da9-bb97-9ab32effdacf
caps.latest.revision: 29
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 6f6d0dfc8c9a9925f7bf2fa84c4b9330b99c60c3
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sppostmsxoperation-transact-sql"></a>sp_post_msx_operation (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Insère des opérations (lignes) dans le **sysdownloadlist** (table système) pour les serveurs cibles télécharger et exécuter.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_post_msx_operation  
     [ @operation = ] 'operation'  
     [ , [ @object_type = ] 'object' ]   
     { , [ @job_id = ] job_id }   
     [ , [ @specific_target_server = ] 'target_server' ]   
     [ , [ @value = ] value ]  
     [ , [ @schedule_uid = ] schedule_uid ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@operation =**] **'***opération***'**  
 Type de l'opération publiée. *opération*est **varchar(64)**, sans valeur par défaut. Opérations dépend de *object_type*.  
  
|Type d'objet|Opération|  
|-----------------|---------------|  
|**JOB**|INSERT<br /><br /> UPDATE<br /><br /> DELETE<br /><br /> START<br /><br /> STOP|  
|**SERVEUR**|RE-ENLIST<br /><br /> DEFECT<br /><br /> SYNC-TIME<br /><br /> SET-POLL|  
|**PLANIFICATION**|INSERT<br /><br /> UPDATE<br /><br /> DELETE|  
  
 [  **@object_type =**] **'***objet***'**  
 Type d'objet pour lequel l'opération doit être publiée. Les types valides sont **travail**, **SERVER**, et **planification**. *objet* est **varchar(64)**, avec une valeur par défaut **travail**.  
  
 [  **@job_id =**] *job_id*  
 Numéro d'identification du travail auquel l'opération s'applique. *job_id* est **uniqueidentifier**, sans valeur par défaut. **0 x 00** indique tous les travaux. Si *objet* est **SERVER**, puis *job_id*n’est pas obligatoire.  
  
 [ **@specific_target_server =**] **'***target_server***'**  
 Nom du serveur cible auquel l'opération spécifiée s'applique. Si *job_id* est spécifié, mais *serveur_cible* n’est pas spécifié, les opérations sont publiées pour tous les serveurs cibles du travail. *serveur_cible* est **nvarchar (30)**, avec NULL comme valeur par défaut.  
  
 [  **@value =**] *valeur*  
 Fréquence d'interrogation, en secondes. *value* est de type **int**, avec NULL comme valeur par défaut. Spécifiez ce paramètre uniquement si *opération* est **SET-POLL**.  
  
 [  **@schedule_uid=** ] *schedule_uid*  
 Identificateur unique de la planification à laquelle s'applique l'opération. *schedule_uid* est **uniqueidentifier**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_post_msx_operation** doit être exécuté à partir de la **msdb** base de données.  
  
 **sp_post_msx_operation** peut toujours être appelée en toute sécurité, car il détermine tout d’abord si le serveur actuel est un Agent Microsoft SQL Server multiserveur et, dans ce cas, si *objet*est un travail multiserveur.  
  
 Après une opération a été validée, elle apparaît dans le **sysdownloadlist** table. Après qu'un travail a été créé et publié, les modifications ultérieures apportées à ce travail doivent être aussi communiquées aux serveurs cibles (TSX). L'opération est également réalisée à l'aide de la liste de téléchargement. .  
  
 Il est fortement recommandé de gérer la liste de téléchargement à l'aide de SQL Server Management Studio. Pour plus d’informations, consultez [afficher ou modifier les travaux](http://msdn.microsoft.com/library/57f649b8-190c-4304-abd7-7ca5297deab7).  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, les utilisateurs doivent disposer du **sysadmin** rôle serveur fixe.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_delete_targetserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-targetserver-transact-sql.md)   
 [sp_resync_targetserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-resync-targetserver-transact-sql.md)   
 [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)   
 [sp_stop_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-stop-job-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [sp_update_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
