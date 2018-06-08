---
title: sp_help_jobhistory (Transact-SQL) | Documents Microsoft
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
- sp_help_jobhistory_TSQL
- sp_help_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobhistory
ms.assetid: a944d44e-411b-4735-8ce4-73888d4262d7
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e001e9e0ea0dd7dfdbe64a788db465125b04e414
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpjobhistory-transact-sql"></a>sp_help_jobhistory (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Fournit des informations sur les travaux pour les serveurs dans le domaine d'administration multiserveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_jobhistory [ [ @job_id = ] job_id ]   
     [ , [ @job_name = ] 'job_name' ]   
     [ , [ @step_id = ] step_id ]   
     [ , [ @sql_message_id = ] sql_message_id ]   
     [ , [ @sql_severity = ] sql_severity ]   
     [ , [ @start_run_date = ] start_run_date ]   
     [ , [ @end_run_date = ] end_run_date ]   
     [ , [ @start_run_time = ] start_run_time ]   
     [ , [ @end_run_time = ] end_run_time ]   
     [ , [ @minimum_run_duration = ] minimum_run_duration ]   
     [ , [ @run_status = ] run_status ]   
     [ , [ @minimum_retries = ] minimum_retries ]   
     [ , [ @oldest_first = ] oldest_first ]   
     [ , [ @server = ] 'server' ]   
     [ , [ @mode = ] 'mode' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id=** ] *job_id*  
 Numéro d’identification du travail. *job_id* est **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
 [  **@job_name=** ] **'***job_name***'**  
 Nom du travail. *job_name* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@step_id=** ] *id_de_l*  
 Le numéro d’identification de l’étape. *l’argument id_étape* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@sql_message_id=** ] *id_du_message_sql*  
 Numéro d'identification du message d'erreur retourné par Microsoft SQL Server lors de l'exécution du travail. *id_du_message_sql* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@sql_severity=** ] *gravité_sql*  
 Niveau de gravité du message d'erreur renvoyé par SQL Server lors de l'exécution du travail. *gravité_sql* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@start_run_date=** ] *date_de_début_d*  
 Date de début de l'exécution du travail. *date_de_début_d*est **int**, avec NULL comme valeur par défaut. *date_de_début_d* doit respecter le format AAAAMMJJ, où AAAA représente une année à quatre chiffres, MM est un nom de deux chiffres du mois et DD est un nom de jour.  
  
 [  **@end_run_date=** ] *ce paramètre*  
 Date de fin de l'exécution du travail. *Ce paramètre* est **int**, avec NULL comme valeur par défaut. *Ce paramètre*doit respecter le format AAAAMMJJ, où AAAA représente une année à quatre chiffres, MM est un nom de deux chiffres du mois et DD est un nom de jour.  
  
 [  **@start_run_time=** ] *heure_de_début_d*  
 Heure du début d'exécution du travail. *heure_de_début_d* est **int**, avec NULL comme valeur par défaut. *heure_de_début_d*doit respecter le format HHMMSS, où HH est une heure de la journée, MM est une minute de deux caractères de la journée et SS est une seconde de deux caractères de la journée.  
  
 [  **@end_run_time=** ] *heure_de_fin_d*  
 Heure de fin d'exécution du travail. *heure_de_fin_d* est **int**, avec NULL comme valeur par défaut. *heure_de_fin_d*doit respecter le format HHMMSS, où HH est une heure de la journée, MM est une minute de deux caractères de la journée et SS est une seconde de deux caractères de la journée.  
  
 [  **@minimum_run_duration=** ] *durée_d*  
 Durée minimale pour l'exécution du travail. *durée_d* est **int**, avec NULL comme valeur par défaut. *durée_d*doit respecter le format HHMMSS, où HH est une heure de la journée, MM est une minute de deux caractères de la journée et SS est une seconde de deux caractères de la journée.  
  
 [  **@run_status=** ] *état_de_l*  
 État de l'exécution du travail. *état_de_l* est **int**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Échec|  
|**1**|Opération réussie|  
|**2**|Reprise (étape uniquement)|  
|**3**|Opération annulée|  
|**4**|Messages en cours|  
|**5**|Unknown|  
  
 [  **@minimum_retries=** ] *minimum_de_reprises*  
 Nombre minimal de tentatives d'exécution d'un travail. *minimum_de_reprises* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@oldest_first=** ] *le_plus_ancien_en_premier*  
 Indique s'il faut présenter en premier le résultat des travaux les plus anciens. *le_plus_ancien_en_premier* est **int**, avec une valeur par défaut **0**, laquelle présente tout d’abord les travaux les plus récents. **1** présente tout d’abord les travaux les plus anciens.  
  
 [  **@server=** ] **'***server***'**  
 Le nom du serveur sur lequel le travail a été effectué. *serveur* est **nvarchar (30)**, avec NULL comme valeur par défaut.  
  
 [  **@mode=** ] **'***mode***'**  
 Indique si SQL Server doit imprimer toutes les colonnes du jeu de résultats (**complète**) ou un résumé des colonnes. *mode* est **varchar(7)**, avec une valeur par défaut **Résumé**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 La liste des colonnes dépend de la valeur de *mode*. L’ensemble plus complet de colonnes est indiqué ci-dessous et est retourné lorsque *mode* est plein.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**instance_id**|**int**|Numéro d'identification de l'entrée d'historique.|  
|**job_id**|**uniqueidentifier**|Numéro d’identification du travail.|  
|**job_name**|**sysname**|Nom du travail.|  
|**step_id**|**int**|Numéro d’identification de l’étape (seront **0** pour un historique des travaux).|  
|**step_name**|**sysname**|Nom de l'étape (NULL pour un historique des travaux).|  
|**sql_message_id**|**int**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], représente le numéro d'erreur [!INCLUDE[tsql](../../includes/tsql-md.md)] le plus récent affiché pendant l'exécution de la commande.|  
|**gravité_sql**|**int**|Pour une étape [!INCLUDE[tsql](../../includes/tsql-md.md)], représente le degré de gravité [!INCLUDE[tsql](../../includes/tsql-md.md)] le plus élevé obtenu pendant l'exécution de la commande.|  
|**message**|**nvarchar(1024)**|Message d'historique d'étape ou de travail.|  
|**run_status**|**int**|Résultat du travail ou de l'étape.|  
|**run_date**|**int**|Date de début d'exécution du travail ou de l'étape.|  
|**run_time**|**int**|Heure de début d'exécution du travail ou de l'étape.|  
|**run_duration**|**int**|Durée, au format HHMMSS, de l'exécution du travail ou de l'étape.|  
|**operator_emailed**|**nvarchar(20)**|Opérateur qui a reçu un courrier électronique concernant ce travail (NULL pour un historique d'étape).|  
|**operator_netsent**|**nvarchar(20)**|Opérateur qui a reçu un message réseau concernant ce travail (NULL pour un historique d'étape).|  
|**operator_paged**|**nvarchar(20)**|Opérateur qui a reçu un message par radiomessagerie concernant ce travail (NULL pour un historique d'étape).|  
|**retries_attempted**|**int**|Nombre de reprises de l'étape (0 pour un historique d'étape).|  
|**server**|**nvarchar(30)**|Serveur sur lequel est exécutée l'étape ou le travail. Est toujours (**local**).|  
  
## <a name="remarks"></a>Notes  
 **sp_help_jobhistory** renvoie un rapport contenant l’historique des travaux planifiés spécifiés. Si aucun paramètre n'est précisé, le rapport contient l'historique de tous les travaux planifiés.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
 Membres de la **SQLAgentUserRole** rôle de base de données peut uniquement afficher l’historique des travaux dont ils sont propriétaires.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-job-information-for-a-job"></a>A. Affichage de toutes les informations d'un travail  
 L'exemple ci-dessous répertorie toutes les informations du travail `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobhistory   
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-that-match-certain-conditions"></a>B. Affichage des informations sur les travaux répondant à certaines conditions  
 L'exemple ci-dessous imprime toutes les colonnes et toutes les informations de tous les travaux et toutes les étapes de travail qui ont échoué avec un message d'erreur `50100` (message d'erreur personnalisé) et un degré de gravité égal à `20`.  
  
```  
USE msdb  
GO  
  
EXEC dbo.sp_help_jobhistory  
    @sql_message_id = 50100,  
    @sql_severity = 20,  
    @run_status = 0,  
    @mode = N'FULL' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_purge_jobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-purge-jobhistory-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
