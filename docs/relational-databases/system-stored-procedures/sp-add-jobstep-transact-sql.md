---
title: sp_add_jobstep (Transact-SQL) | Documents Microsoft
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
- sp_add_jobstep_TSQL
- sp_add_jobstep
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_jobstep
ms.assetid: 97900032-523d-49d6-9865-2734fba1c755
caps.latest.revision: 80
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: fc7970a2d38786a49beed08e63068be1abab4d51
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddjobstep-transact-sql"></a>sp_add_jobstep (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute une étape (opération) à un travail.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_jobstep [ @job_id = ] job_id | [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     { , [ @step_name = ] 'step_name' }   
     [ , [ @subsystem = ] 'subsystem' ]   
     [ , [ @command = ] 'command' ]   
     [ , [ @additional_parameters = ] 'parameters' ]   
          [ , [ @cmdexec_success_code = ] code ]   
     [ , [ @on_success_action = ] success_action ]   
          [ , [ @on_success_step_id = ] success_step_id ]   
          [ , [ @on_fail_action = ] fail_action ]   
          [ , [ @on_fail_step_id = ] fail_step_id ]   
     [ , [ @server = ] 'server' ]   
     [ , [ @database_name = ] 'database' ]   
     [ , [ @database_user_name = ] 'user' ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @retry_interval = ] retry_interval ]   
     [ , [ @os_run_priority = ] run_priority ]   
     [ , [ @output_file_name = ] 'file_name' ]   
     [ , [ @flags = ] flags ]   
     [ , { [ @proxy_id = ] proxy_id   
         | [ @proxy_name = ] 'proxy_name' } ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id =** ] *job_id*  
 Numéro d'identification du travail auquel ajouter l'étape. *job_id* est **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
 [  **@job_name =** ] **'***job_name***'**  
 Nom du travail auquel ajouter l'étape. *job_name* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [  **@step_id =** ] *id_de_l*  
 Numéro d'identification de la séquence de l'étape de travail. Début de numéros d’identification à l’étape **1** et incrémentés sans intervalle. Si une étape est insérée dans une séquence existante, les numéros de cette séquence sont ajustés automatiquement. Une valeur est fournie si *id_de_l* n’est pas spécifié. *l’argument id_étape*est **int**, avec NULL comme valeur par défaut.  
  
 [  **@step_name =** ] **'***nom_de_l***'**  
 Nom de l'étape. *nom_de_l*est **sysname**, sans valeur par défaut.  
  
 [  **@subsystem =** ] **'***sous-système***'**  
 Sous-système utilisé par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service de l’Agent pour exécuter *commande*. *sous-système* est **nvarchar (40)**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|'**ACTIVESCRIPTING**'|Script actif<br /><br /> **\*\* Important \*\*** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|  
|'**CMDEXEC**'|Commande du système d'exécution ou programme exécutable|  
|'**DISTRIBUTION**'|Travail de l'Agent de distribution de réplication|  
|'**INSTANTANÉ**'|Travail de l'Agent d'instantané de réplication|  
|'**LOGREADER**'|Travail de l'Agent de lecture du journal de réplications|  
|'**FUSION**'|Travail de l'Agent de fusion de réplication|  
|'**QueueReader**'|Travail de l'Agent de lecture de la file d'attente de réplication|  
|'**ANALYSISQUERY**'|Requête Analysis Services (MDX, DMX).|  
|'**ANALYSISCOMMAND**'|Commande Analysis Services (XMLA).|  
|'**Dts**'|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Exécution du package|  
|'**PowerShell**'|script PowerShell|  
|'**TSQL**' (valeur par défaut)|Instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]|  
  
 [  **@command=** ] **'***commande***'**  
 Les commandes à exécuter par **SQLServerAgent** via *sous-système*. *commande* est **nvarchar (max)**, avec NULL comme valeur par défaut. L'Agent SQL Server effectue une substitution de jetons qui offre la même souplesse que les variables lorsque vous écrivez des logiciels.  
  
> [!IMPORTANT]  
>  Une macro d’échappement doit désormais accompagner tous les jetons utilisés dans les étapes de travail, sans quoi les étapes de travail échoue. De plus, vous devez désormais mettre les noms de jeton entre parenthèses et placer un signe dollar (`$`) au début de la syntaxe du jeton. Par exemple :  
>   
>  `$(ESCAPE_`*nom de la macro*`(DATE))`  
  
 Pour plus d’informations sur ces jetons et de mise à jour des étapes de travail pour utiliser la nouvelle syntaxe de jeton, consultez [utiliser des jetons dans les étapes de travail](http://msdn.microsoft.com/library/105bbb66-0ade-4b46-b8e4-f849e5fc4d43).  
  
> [!IMPORTANT]  
>  Tout utilisateur Windows qui dispose des autorisations d'écriture dans le journal des événements Windows peut accéder aux étapes de travail activées par les alertes WMI ou par les alertes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Pour éviter ce risque de sécurité, les jetons de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent qui peuvent être utilisés dans des travaux activés par des alertes sont désactivés par défaut. Ces jetons sont : **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** et **WMI(***propriété***)**. Notez que dans cette version, l'utilisation des jetons est étendue toutes les alertes.  
>   
>  Si vous devez utiliser ces jetons, assurez-vous d'abord que seuls les membres des groupes de sécurité Windows approuvés, comme le groupe Administrateurs, disposent des autorisations d'écriture pour le journal d'événements de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réside. Ensuite, pour activer ces jetons, cliquez avec le bouton droit sur **SQL Server Agent** dans l’Explorateur d’objets, sélectionnez **Propriétés**, puis dans la page **Système d’alerte** qui s’affiche, sélectionnez l’option **Remplacer les jetons pour toutes les réponses de travaux aux alertes** .  
  
 [  **@additional_parameters=** ] **'***paramètres***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *paramètres* est **ntext**, avec NULL comme valeur par défaut.  
  
 [  **@cmdexec_success_code =** ] *code*  
 La valeur retournée par une **CmdExec** commande du sous-système pour indiquer que *commande* exécutée avec succès. *code*est **int**, avec une valeur par défaut **0**.  
  
 [  **@on_success_action=** ] *action_succès*  
 Action à effectuer si l'étape est exécutée correctement. *action_succès*est **tinyint**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1** (par défaut)|Quitter avec succès|  
|**2**|Sortie avec échec|  
|**3**|Passez à l'étape suivante|  
|**4**|Passez à l’étape *id_étape_succès*|  
  
 [ **@on_success_step_id =** ] *success_step_id*  
 ID de l’étape du travail à exécuter si l’étape réussit et *action_succès*est **4**. *id_étape_succès*est **int**, avec une valeur par défaut **0**.  
  
 [  **@on_fail_action=** ] *action_échec*  
 L’action à exécuter si l’étape échoue. *action_échec*est **tinyint**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (action)|  
|-----------|----------------------------|  
|**1**|Quitter avec succès|  
|**2** (par défaut)|Sortie avec échec|  
|**3**|Passez à l'étape suivante|  
|**4**|Passez à l’étape *id_étape_échec*|  
  
 [  **@on_fail_step_id=** ] *id_étape_échec*  
 ID de l’étape du travail à exécuter si l’étape échoue et *action_échec*est **4**. *id_étape_échec*est **int**, avec une valeur par défaut **0**.  
  
 [  **@server =**] **'***server***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *serveur*est **nvarchar (30)**, avec NULL comme valeur par défaut.  
  
 [  **@database_name=** ] **'***base de données***'**  
 Nom de la base de données dans laquelle l'étape [!INCLUDE[tsql](../../includes/tsql-md.md)] doit être exécutée. *base de données* est **sysname**, avec une valeur par défaut NULL, auquel cas la **master** base de données est utilisée. Les noms placés entre crochets ([ ]) ne sont pas autorisés. Pour une étape de travail ActiveX, la *base de données* est le nom de l’étape utilise le langage de script.  
  
 [  **@database_user_name=** ] **'***utilisateur***'**  
 Nom du compte d'utilisateur à utiliser lors de l'exécution d'une étape [!INCLUDE[tsql](../../includes/tsql-md.md)]. *utilisateur* est **sysname**, avec NULL comme valeur par défaut. Lorsque *utilisateur* est NULL, l’étape est exécutée dans le contexte de l’utilisateur du propriétaire du travail sur *base de données*.  L'Agent SQL Server inclut ce paramètre uniquement si le propriétaire du travail est un sysadmin SQL Server. Dans ce cas, l'étape Transact-SQL donnée est exécutée dans le contexte du nom d'utilisateur SQL Server spécifié. Si le propriétaire du travail n’est pas un administrateur système SQL Server, puis l’étape Transact-SQL sera toujours être exécuté dans le contexte de la connexion qui possède ce travail, et le @database_user_name paramètre sera ignoré.  
  
 [  **@retry_attempts=** ] *retry_attempts*  
 Nombre de tentatives à effectuer si l'étape échoue. *retry_attempts*est **int**, avec une valeur par défaut **0**, indiquant aucune tentative de tentatives.  
  
 [ **@retry_interval=** ] *retry_interval*  
 Nombre de minutes s'écoulant entre chaque tentative de reprise. *intervalle_entre_reprises*est **int**, avec une valeur par défaut **0**, ce qui indique un **0**-intervalle minute.  
  
 [  **@os_run_priority =** ] *priorité_exécution*  
 Réservé.  
  
 [  **@output_file_name=** ] **'***nom_fichier***'**  
 Nom du fichier dans lequel est enregistré le résultat de l'étape. *file_name*est **nvarchar(200)**, avec NULL comme valeur par défaut. *file_name*peut inclure un ou plusieurs jetons répertoriés sous *commande*. Ce paramètre est valide uniquement avec les commandes en cours d’exécution le [!INCLUDE[tsql](../../includes/tsql-md.md)], **CmdExec**, **PowerShell**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], ou [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sous-systèmes.  
  
 [  **@flags=** ] *indicateurs*  
 Option contrôlant le comportement. *indicateurs* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|Écrasement du fichier de sortie|  
|**2**|Ajout au fichier de sortie|  
|**4**|Écriture de la sortie de l'étape d'un travail [!INCLUDE[tsql](../../includes/tsql-md.md)] dans l'historique des étapes.|  
|**8**|Écriture du journal dans la table (remplace l'historique existant)|  
|**16**|Écriture du journal dans la table (s'ajoute à l'historique existant)|  
|**32**|Écriture de toute la sortie dans l'historique des travaux|  
|**64**|Création d'un événement Windows à utiliser comme signal pour l'étape de travail Cmd à abandonner|  
  
 [ **@proxy_id** =] *proxy_id*  
 Numéro d’identification du proxy pour lequel l’étape de travail s’exécute en tant que. *proxy_id* est de type **int**, avec NULL comme valeur par défaut. Si aucun *proxy_id* est spécifié, aucun *proxy_name* est spécifié et aucune *nom_utilisateur* est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Agent.  
  
 [ **@proxy_name** =] **'***proxy_name***'**  
 Nom du proxy sous lequel s'exécute l'étape de travail. *proxy_name* est de type **sysname**, avec NULL comme valeur par défaut. Si aucun *proxy_id* est spécifié, aucun *proxy_name* est spécifié et aucune *nom_utilisateur* est spécifié, l’étape de travail s’exécute en tant que compte de service pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’Agent.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_add_jobstep** doit être exécuté à partir de la **msdb** base de données.  
  
 SQL Server Management Studio offre un moyen simple et graphique de gérer les tâches, et est recommandé pour la création et la gestion de l'infrastructure de travail.  
  
 Une étape de travail doit spécifier un proxy, sauf si le créateur de l’étape de travail est un membre de la **sysadmin** rôle de sécurité fixe.  
  
 Un proxy peut être identifié par *proxy_name* ou *proxy_id*.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
 Le créateur de l'étape de travail doit disposer des droits d'accès au proxy pour cette étape. Membres de la **sysadmin** rôle de serveur fixe a accès à tous les serveurs proxy. Pour les autres utilisateurs, les droits d'accès à un proxy doivent être octroyés explicitement.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre la création de l'étape d'un travail permettant de modifier l'accès à une base de données appelée Sales et de lui affecter le mode d'accès en lecture seule. En outre, cet exemple indique 5 tentatives de reprises, chacune d'elles étant exécutée toutes les 5 minutes.  
  
> [!NOTE]  
>  Cet exemple suppose que la `Weekly Sales Data Backup` travail existe déjà.  
  
```  
USE msdb;  
GO  
EXEC sp_add_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_name = N'Set database to read only',  
    @subsystem = N'TSQL',  
    @command = N'ALTER DATABASE SALES SET READ_ONLY',   
    @retry_attempts = 5,  
    @retry_interval = 5 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher ou modifier les travaux](http://msdn.microsoft.com/library/57f649b8-190c-4304-abd7-7ca5297deab7)   
 [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_update_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
