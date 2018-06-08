---
title: sp_update_jobstep (Transact-SQL) | Documents Microsoft
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
- sp_update_jobstep
- sp_update_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_jobstep
ms.assetid: e158802c-c347-4a5d-bf75-c03e5ae56e6b
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 67986dbdbace492fc2fb82bcb94e6cc32a05616a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spupdatejobstep-transact-sql"></a>sp_update_jobstep (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie la valeur d'une étape d'un travail qui est utilisé pour effectuer des activités automatisées.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_jobstep   
     {   [@job_id =] job_id   
       | [@job_name =] 'job_name' } ,  
     [@step_id =] step_id  
     [ , [@step_name =] 'step_name' ]  
     [ , [@subsystem =] 'subsystem' ]   
     [ , [@command =] 'command' ]  
     [ , [@additional_parameters =] 'parameters' ]  
     [ , [@cmdexec_success_code =] success_code ]  
     [ , [@on_success_action =] success_action ]   
     [ , [@on_success_step_id =] success_step_id ]  
     [ , [@on_fail_action =] fail_action ]   
     [ , [@on_fail_step_id =] fail_step_id ]  
     [ , [@server =] 'server' ]   
     [ , [@database_name =] 'database' ]  
     [ , [@database_user_name =] 'user' ]   
     [ , [@retry_attempts =] retry_attempts ]  
     [ , [@retry_interval =] retry_interval ]   
     [ , [@os_run_priority =] run_priority ]  
     [ , [@output_file_name =] 'file_name' ]   
     [ , [@flags =] flags ]  
     [ ,  {   [ @proxy_id = ] proxy_id   
            | [ @proxy_name = ] 'proxy_name' }   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id =**] *job_id*  
 Numéro d'identification du travail auquel l'étape appartient. *job_id*est **uniqueidentifier**, avec NULL comme valeur par défaut. Soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [  **@job_name =**] **'***job_name***'**  
 Nom du travail auquel l'étape appartient. *job_name*est **sysname**, avec NULL comme valeur par défaut. Soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [ **@step_id =**] *step_id*  
 Numéro d'identification de l'étape d'un travail à modifier. Il est impossible de modifier ce numéro. *l’argument id_étape*est **int**, sans valeur par défaut.  
  
 [  **@step_name =**] **'***nom_de_l***'**  
 Nouveau nom de l'étape. *nom_de_l*est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@subsystem =**] **'***sous-système***'**  
 Sous-système utilisé par l’Agent Microsoft SQL Server pour exécuter *commande*. *sous-système* est **nvarchar (40)**, avec NULL comme valeur par défaut.  
  
 [  **@command =**] **'***commande***'**  
 Les commandes à exécuter via *sous-système*. *commande* est **nvarchar (max)**, avec NULL comme valeur par défaut.  
  
 [  **@additional_parameters =**] **'***paramètres***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [  **@cmdexec_success_code =**] *code_succès*  
 La valeur retournée par une **CmdExec** commande du sous-système pour indiquer que *commande* exécutée avec succès. *code_succès* est **int**, avec NULL comme valeur par défaut.  
  
 [ **@on_success_action =**] *success_action*  
 L’action à exécuter si l’étape réussit. *action_succès* est **tinyint**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1**|Quitter avec succès.|  
|**2**|Sortie avec échec|  
|**3**|Passage à l'étape suivante|  
|**4**|Passez à l’étape *id_étape_succès.*|  
  
 [ **@on_success_step_id =**] *success_step_id*  
 Le numéro d’identification de l’étape du travail à exécuter si l’étape réussit et *action_succès* est **4**. *id_étape_succès* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@on_fail_action =**] *action_échec*  
 L’action à exécuter si l’étape échoue. *action_échec* est **tinyint**, avec NULL comme valeur par défaut et peut avoir l’une des valeurs suivantes.  
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1**|Quitter avec succès.|  
|**2**|Sortie avec échec|  
|**3**|Passage à l'étape suivante|  
|**4**|Passez à l’étape *id_étape_échec **.*|  
  
 [  **@on_fail_step_id =**] *id_étape_échec*  
 Le numéro d’identification de l’étape du travail à exécuter si l’étape échoue et *action_échec* est **4**. *id_étape_échec* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@server =**] **'***server***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *serveur* est **nvarchar (128)**, avec NULL comme valeur par défaut.  
  
 [  **@database_name =**] **'***base de données***'**  
 Nom de la base de données dans laquelle l'étape [!INCLUDE[tsql](../../includes/tsql-md.md)] doit être exécutée. *base de données*est **sysname**. Les noms placés entre crochets ([ ]) ne sont pas autorisés. La valeur par défaut est NULL.  
  
 [  **@database_user_name =**] **'***utilisateur***'**  
 Nom du compte d'utilisateur à utiliser lors de l'exécution d'une étape [!INCLUDE[tsql](../../includes/tsql-md.md)]. *utilisateur*est **sysname**, avec NULL comme valeur par défaut.  
  
 [ **@retry_attempts =**] *retry_attempts*  
 Nombre de tentatives à effectuer si l'étape échoue. *retry_attempts*est **int**, avec NULL comme valeur par défaut.  
  
 [ **@retry_interval =**] *retry_interval*  
 Nombre de minutes s'écoulant entre chaque tentative de reprise. *intervalle_entre_reprises* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@os_run_priority =**] *priorité_exécution*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [  **@output_file_name =**] **'***nom_fichier***'**  
 Nom du fichier dans lequel est enregistré le résultat de l'étape. *file_name* est **nvarchar(200)**, avec NULL comme valeur par défaut. Ce paramètre est valide uniquement avec les commandes fonctionnant dans les sous-systèmes [!INCLUDE[tsql](../../includes/tsql-md.md)] ou CmdExec.  
  
 Pour définir le redéfinir à NULL, vous devez définir *nom_fichier_sortie* sur une chaîne vide (' ') ou à une chaîne de caractères vides, mais vous ne pouvez pas utiliser le **CHAR(32)** (fonction). Vous pouvez par exemple définir cet argument avec une chaîne vide comme suit :  
  
 **@output_file_name = ' '**  
  
 [  **@flags =**] *indicateurs*  
 Une option qui contrôle le comportement. *indicateurs* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|Écrasement du fichier de sortie.|  
|**2**|Ajout au fichier de sortie|  
|**4**|Écriture de la sortie de l'étape d'un travail Transact-SQL dans l'historique des étapes.|  
|**8**|Écriture du journal dans la table (remplace l'historique existant)|  
|**16**|Écriture du journal dans la table (s'ajoute à l'historique existant)|  
  
 [ **@proxy_id**=] *proxy_id*  
 Numéro d'identification du proxy sous lequel s'exécute l'étape de travail. *proxy_id* est de type **int**, avec NULL comme valeur par défaut. Si aucun *proxy_id* est spécifié, aucun *proxy_name* est spécifié et aucune *nom_utilisateur* est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Agent.  
  
 [ **@proxy_name**=] **'***proxy_name***'**  
 Nom du proxy sous lequel s'exécute l'étape de travail. *proxy_name* est de type **sysname**, avec NULL comme valeur par défaut. Si aucun *proxy_id* est spécifié, aucun *proxy_name* est spécifié et aucune *nom_utilisateur* est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Agent.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_update_jobstep** doit être exécuté à partir de la **msdb** base de données.  
  
 La mise à jour de l'étape d'un travail incrémente le numéro de version de ce travail.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
 Seuls les membres du **sysadmin** mettre à jour une étape de travail appartienne à un autre utilisateur.  
  
 Si l'étape d'un travail nécessite un accès à un proxy, le créateur de l'étape doit avoir accès au proxy pour l'étape du travail. Tous les sous-systèmes, excepté Transact-SQL, requièrent un compte proxy. Membres de **sysadmin** ont accès à tous les serveurs proxy et pouvez utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service de l’Agent pour le proxy.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant modifie le nombre de tentatives de reprises pour la première étape du travail `Weekly Sales Data Backup`. À la fin de l'exemple, ce nombre s'élève à `10`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1,  
    @retry_attempts = 10 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher ou modifier les travaux](http://msdn.microsoft.com/library/57f649b8-190c-4304-abd7-7ca5297deab7)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
