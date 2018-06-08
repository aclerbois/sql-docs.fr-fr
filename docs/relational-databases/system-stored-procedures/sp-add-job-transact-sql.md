---
title: sp_add_job (Transact-SQL) | Documents Microsoft
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
- sp_add_job_TSQL
- sp_add_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_job
ms.assetid: 6ca8fe2c-7b1c-4b59-b4c7-e3b7485df274
caps.latest.revision: 31
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 4851db832cd587d486d3094eb23e8d64ba4b4cc2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddjob-transact-sql"></a>sp_add_job (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un nouveau travail exécuté par le service SQLServerAgent.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_job [ @job_name = ] 'job_name'  
     [ , [ @enabled = ] enabled ]   
     [ , [ @description = ] 'description' ]   
     [ , [ @start_step_id = ] step_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @category_id = ] category_id ]   
     [ , [ @owner_login_name = ] 'login' ]   
     [ , [ @notify_level_eventlog = ] eventlog_level ]   
     [ , [ @notify_level_email = ] email_level ]   
     [ , [ @notify_level_netsend = ] netsend_level ]   
     [ , [ @notify_level_page = ] page_level ]   
     [ , [ @notify_email_operator_name = ] 'email_name' ]   
          [ , [ @notify_netsend_operator_name = ] 'netsend_name' ]   
     [ , [ @notify_page_operator_name = ] 'page_name' ]   
     [ , [ @delete_level = ] delete_level ]   
     [ , [ @job_id = ] job_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_name =** ] **'***job_name***'**  
 Nom du travail. Le nom doit être unique et ne peut pas contenir le pourcentage (**%**) caractères. *job_name*est **nvarchar (128)**, sans valeur par défaut.  
  
 [  **@enabled =** ] *activé*  
 Indique l'état du travail ajouté. *activé*est **tinyint**, par défaut est 1 (activé). Si **0**, le travail n’est pas activé et qu’il ne s’exécute pas conformément à sa planification ; Toutefois, il peut être exécuté manuellement.  
  
 [  **@description =** ] **'***description***'**  
 Description du travail. *Description* est **nvarchar (512)**, avec NULL comme valeur par défaut. Si *description* est n’omis, « Aucune description disponible » est utilisée.  
  
 [  **@start_step_id =** ] *id_de_l*  
 Numéro d'identification de la première étape à exécuter pour le travail. *l’argument id_étape*est **int**, avec 1 comme valeur par défaut.  
  
 [  **@category_name =** ] **'***catégorie***'**  
 Catégorie du travail. *catégorie*est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@category_id =** ] *code catégorie*  
 Mécanisme qui ne tient pas compte de la langue définie et qui permet de spécifier une catégorie de travail. *code catégorie*est **int**, avec NULL comme valeur par défaut.  
  
 [  **@owner_login_name =** ] **'***connexion***'**  
 Nom du compte de connexion propriétaire du travail. *connexion*est **sysname**, avec NULL comme valeur par défaut, qui est interprété comme le nom de connexion actuel. Seuls les membres de la **sysadmin** rôle serveur fixe peut définir ou modifier la valeur de **@owner_login_name**. Si les utilisateurs qui ne sont pas membres de la **sysadmin** rôle définir ou modifier la valeur de **@owner_login_name**, l’exécution de cette procédure stockée échoue et une erreur est renvoyée.  
  
 [  **@notify_level_eventlog =** ] *niveau_journal_événements*  
 Valeur indiquant le moment auquel une entrée doit être ajoutée pour ce travail dans le journal des applications Microsoft Windows. *niveau_journal_événements*est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Never|  
|**1**|Si succès|  
|**2** (par défaut)|Si échec|  
|**3**|Always|  
  
 [  **@notify_level_email =** ] *niveau_courrier_électronique*  
 Valeur indiquant à quel moment envoyer un message électronique une fois ce travail achevé. *niveau_courrier_électronique*est **int**, avec une valeur par défaut **0**, ce qui signifie jamais. *niveau_courrier_électronique*utilise les mêmes valeurs que *niveau_journal_événements*.  
  
 [  **@notify_level_netsend =** ] *niveau_message_réseau*  
 Valeur indiquant à quel moment envoyer un message réseau une fois ce travail achevé. *niveau_message_réseau*est **int**, avec une valeur par défaut **0**, ce qui signifie jamais. *niveau_message_réseau* utilise les mêmes valeurs que *niveau_journal_événements*.  
  
 [  **@notify_level_page =** ] *niveau_page*  
 Valeur indiquant à quel moment envoyer une page une fois ce travail achevé. *niveau_page*est **int**, avec une valeur par défaut **0**, ce qui signifie jamais. *niveau_page*utilise les mêmes valeurs que *niveau_journal_événements*.  
  
 [  **@notify_email_operator_name =** ] **'***nom_adresse***'**  
 Adresse électronique de la personne à qui envoyer un courrier électronique lorsque *niveau_courrier_électronique* est atteinte. *nom_adresse* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@notify_netsend_operator_name =** ] **'***l’argument adresse_envoi_réseau***'**  
 Nom de l'opérateur à qui le message réseau est envoyé une fois ce travail exécuté. *l’argument adresse_envoi_réseau*est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@notify_page_operator_name =** ] **'***nom_page***'**  
 Nom de la personne à qui envoyer un message par radiomessagerie une fois ce travail exécuté. *nom_page*est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@delete_level =** ] *niveau_suppression*  
 Valeur indiquant à quel moment supprimer le travail. *l’argument niveau_suppression*est **int**, avec une valeur par défaut 0, ce qui signifie jamais. *niveau_suppression*utilise les mêmes valeurs que *niveau_journal_événements*.  
  
> [!NOTE]  
>  Lorsque *niveau_suppression* est **3**définis indépendamment de toute planification pour la tâche, la tâche est exécutée une seule fois. De plus, si un travail est supprimé, son historique est également supprimé.  
  
 [  **@job_id =** ] *job_id *** sortie**  
 Numéro d'identification du travail affecté si le travail est correctement créé. *job_id*est une variable output de type **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **@originating_server** Il existe dans **sp_add_job,** mais n’est ne pas répertorié dans les Arguments. **@originating_server** est réservé à un usage interne.  
  
 Après avoir **sp_add_job** a été exécutée pour ajouter un travail, **sp_add_jobstep** peut être utilisé pour ajouter des étapes qui effectuent les activités pour le travail. **sp_add_jobschedule** peut être utilisé pour créer la planification que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service Agent utilise pour exécuter le travail. Utilisez **sp_add_jobserver** pour définir le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance où la tâche s’exécute, et **sp_delete_jobserver** pour supprimer la tâche à partir de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.  
  
 Si le travail s’exécutera sur un ou plusieurs serveurs cibles dans un environnement multiserveur, utilisez **sp_apply_job_to_targets** pour définir les serveurs cibles ou de serveurs cibles pour le travail. Pour supprimer des travaux à partir des serveurs cibles ou les groupes de serveurs cibles, utilisez **sp_remove_job_from_targets**.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil dont l'interface graphique permet de gérer facilement les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, les utilisateurs doivent être un membre de la **sysadmin** rôle serveur fixe, ou disposer de l’un des éléments suivants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Agent de la base de données fixé, qui résident dans le **msdb** base de données :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour plus d’informations sur les autorisations spécifiques qui sont associés à chacun de ces fixe de la base de données, consultez [rôles de base de données fixe de SQL Server Agent](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut définir ou modifier la valeur de **@owner_login_name**. Si les utilisateurs qui ne sont pas membres de la **sysadmin** rôle définir ou modifier la valeur de **@owner_login_name**, l’exécution de cette procédure stockée échoue et une erreur est renvoyée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-job"></a>A. Ajout d'un travail  
 Cet exemple ajoute un nouveau travail appelé `NightlyBackups` (sauvegardes de nuit).  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-adding-a-job-with-pager-e-mail-and-net-send-information"></a>B. Ajout d'un travail contenant des informations de radiomessagerie, de messagerie électronique et d'envoi réseau  
 Dans l'exemple ci-dessous, un travail appelé `Ad hoc Sales Data Backup` avertit `François Ajenstat` (par radiomessagerie, messagerie électronique ou par message réseau) lorsqu'un travail échoue, et est supprimé une fois exécuté.  
  
> [!NOTE]  
>  Dans cet exemple, nous considérons qu'un opérateur nommé `François Ajenstat` avec une connexion appelée `françoisa` existe déjà.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_job  
    @job_name = N'Ad hoc Sales Data Backup',   
    @enabled = 1,  
    @description = N'Ad hoc backup of sales data',  
    @owner_login_name = N'françoisa',  
    @notify_level_eventlog = 2,  
    @notify_level_email = 2,  
    @notify_level_netsend = 2,  
    @notify_level_page = 2,  
    @notify_email_operator_name = N'François Ajenstat',  
    @notify_netsend_operator_name = N'François Ajenstat',   
    @notify_page_operator_name = N'François Ajenstat',  
    @delete_level = 1 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_add_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)   
 [sp_apply_job_to_targets &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_delete_jobserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md)   
 [sp_remove_job_from_targets &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-remove-job-from-targets-transact-sql.md)   
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
