---
title: sp_delete_jobsteplog (Transact-SQL) | Documents Microsoft
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
- sp_delete_jobsteplog
- sp_delete_jobsteplog_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobsteplog
ms.assetid: e9ef4c99-abde-4038-b6a3-a25dcbaf0958
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: c2d4284f32030339a5c60e211b911c5e7cd783b9
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdeletejobsteplog-transact-sql"></a>sp_delete_jobsteplog (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime tous les journaux d'étapes de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sont spécifiés via les arguments. Utilisez cette procédure stockée pour mettre à jour le **sysjobstepslogs** de table dans le **msdb** base de données.  
  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
       [ , [ @step_id = ] step_id | [ @step_name = ] 'step_name' ]  
       [ , [ @older_than = ] 'date' ]  
       [ , [ @larger_than = ] 'size_in_bytes' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id =**] **'***job_id***'**  
 Numéro d'identification du travail qui contient le journal d'étapes de travail à supprimer. *job_id* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@job_name =**] **'***job_name***'**  
 Nom du travail. *job_name* est **sysname**, avec NULL comme valeur par défaut.  
  
> **Remarque :** soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [ **@step_id =**] *step_id*  
 Numéro d'identification de l'étape de travail pour laquelle le journal d'étapes doit être supprimé. Si ne pas inclus, tous les journaux d’étapes de travail sont supprimés, sauf si **@older_than** ou **@larger_than** sont spécifiés. *l’argument id_étape* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@step_name =**] **'***nom_de_l***'**  
 Nom de l'étape de travail pour laquelle le journal d'étapes doit être supprimé. *nom_de_l* est **sysname**, avec NULL comme valeur par défaut.  
  
> **Remarque :** soit *id_de_l* ou *nom_de_l* peut être spécifié, mais ne peut pas être spécifiés.  
  
 [  **@older_than =**] **'***date***'**  
 Date et heure du plus ancien journal d'étapes de travail à conserver. Tous les journaux d'étapes de travail antérieurs à cette date/heure sont supprimés. *date* est **datetime**, avec NULL comme valeur par défaut. Les deux **@older_than** et **@larger_than** peut être spécifié.  
  
 [  **@larger_than =**] **'***size_in_bytes***'**  
 Taille en octets du journal d'étapes de travail le plus volumineux à conserver. Tous les journaux d'étapes de travail dont la taille est supérieure à celle spécifiée sont supprimés. Les deux **@larger_than** et **@older_than** peut être spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_delete_jobsteplog** est dans le **msdb** base de données.  
  
 Si aucun argument sauf **@job_id** ou **@job_name** sont spécifiés, tous les journaux d’étapes pour le travail spécifié sont supprimés.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
 Seuls les membres du **sysadmin** peut supprimer un journal d’étapes de travail appartenant à un autre utilisateur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-removing-all-job-step-logs-from-a-job"></a>A. Suppression de tous les journaux d'étapes d'un travail particulier  
 L'exemple suivant montre la suppression de tous les journaux d'étapes du travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup';  
GO  
```  
  
### <a name="b-removing-the-job-step-log-for-a-particular-job-step"></a>B. Suppression du journal d'étapes de travail pour une étape particulière  
 L'exemple suivant montre la suppression du journal d'étapes de travail pour l'étape 2 du travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 2;  
GO  
```  
  
### <a name="c-removing-all-job-step-logs-based-on-age-and-size"></a>C. Suppression de tous les journaux d'étapes de travail en fonction de l'ancienneté et de la taille.  
 L'exemple suivant montre la suppression de tous les journaux d'étapes de travail créés avant le 25 octobre 2005 à midi et dont la taille est supérieure à 100 mégaoctets (Mo) pour le travail `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @older_than = '10/25/2005 12:00:00',  
    @larger_than = 104857600;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobsteplog-transact-sql.md)   
 [Procédures stockées de l’Agent SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  
