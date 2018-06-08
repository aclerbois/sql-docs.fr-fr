---
title: sp_add_jobschedule (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_add_jobschedule
- sp_add_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_jobschedule
ms.assetid: ffce19d9-d1d6-45b4-89fd-ad0f60822ba0
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e71329e07595e2deb448bfd2635845c52c4f3a67
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddjobschedule-transact-sql"></a>sp_add_jobschedule (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée la planification d'un travail.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_jobschedule [ @job_id = ] job_id, | [ @job_name = ] 'job_name', [ @name = ] 'name'  
     [ , [ @enabled = ] enabled_flag ]  
     [ , [ @freq_type = ] frequency_type ]  
     [ , [ @freq_interval = ] frequency_interval ]  
     [ , [ @freq_subday_type = ] frequency_subday_type ]  
     [ , [ @freq_subday_interval = ] frequency_subday_interval ]  
     [ , [ @freq_relative_interval = ] frequency_relative_interval ]  
     [ , [ @freq_recurrence_factor = ] frequency_recurrence_factor ]  
     [ , [ @active_start_date = ] active_start_date ]  
     [ , [ @active_end_date = ] active_end_date ]  
     [ , [ @active_start_time = ] active_start_time ]  
     [ , [ @active_end_time = ] active_end_time ]  
     [ , [ @schedule_id = ] schedule_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@job_id=** ] *job_id*  
 Numéro d'identification du travail auquel ajouter la planification. *job_id* est **uniqueidentifier**, sans valeur par défaut.  
  
 [  **@job_name=** ] **'***job_name***'**  
 Nom du travail auquel ajouter la planification. *job_name* est **nvarchar (128)**, sans valeur par défaut.  
  
> [!NOTE]  
>  Soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [  **@name=** ] **'***nom***'**  
 Nom de la planification. *nom* est **nvarchar (128)**, sans valeur par défaut.  
  
 [  **@enabled=** ] *enabled_flag*  
 Indique l'état actuel de la planification. *enabled_flag* est **tinyint**, avec une valeur par défaut **1** (activé). Si **0**, la planification n’est pas activée. Lorsque la planification n'est pas activée, le travail n'est pas exécuté.  
  
 [ **@freq_type=** ] *frequency_type*  
 Valeur qui indique à quel moment le travail doit s'exécuter. *frequency_type* est **int**, avec une valeur par défaut **0**, et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**4**|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Tous les mois, relatif à *frequency_interval.*|  
|**64**|Lancer lorsque le service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre.|  
|**128**|Lancer lorsque l'ordinateur est inactif.|  
  
 [  **@freq_interval=** ] *frequency_interval*  
 Jour d'exécution du travail. *frequency_interval* est **int**, avec une valeur par défaut 0 et varie en fonction de la valeur de *frequency_type* comme indiqué dans le tableau suivant :  
  
|Valeur|Effet|  
|-----------|------------|  
|**1** (une fois)|*frequency_interval* n’est pas utilisée.|  
|**4** (quotidienne)|Chaque *frequency_interval* jours.|  
|**8** (hebdomadaire)|*frequency_interval* prend une ou plusieurs des opérations suivantes (combinées avec un opérateur logique OR) :<br /><br /> 1 = Dimanche<br /><br /> 2 = Lundi<br /><br /> 4 = mardi<br /><br /> 8 = mercredi<br /><br /> 16 = jeudi<br /><br /> 32 = vendredi<br /><br /> 64 = samedi|  
|**16** (mensuellement)|Sur le *frequency_interval* jour du mois.|  
|**32** (mensuel relatif)|*frequency_interval* est une des opérations suivantes :<br /><br /> 1 = Dimanche<br /><br /> 2 = Lundi<br /><br /> 3 = Mardi<br /><br /> 4 = Mercredi<br /><br /> 5 = Jeudi<br /><br /> 6 = Vendredi<br /><br /> 7 = Samedi<br /><br /> 8 = Jour<br /><br /> 9 = Jour de semaine<br /><br /> 10 = Jour de week-end|  
|**64** (lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarrage du service Agent)|*frequency_interval* n’est pas utilisée.|  
|**128**|*frequency_interval* n’est pas utilisée.|  
  
 [  **@freq_subday_type=** ] *frequency_subday_type*  
 Spécifie les unités de *frequency_subday_interval*. *frequency_subday_type* est **int**, sans valeur par défaut et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**0x1**|À une heure spécifiée|  
|**0x4**|Minutes|  
|**0x8**|Heures|  
  
 [  **@freq_subday_interval=** ] *frequency_subday_interval*  
 Nombre de *frequency_subday_type* périodes entre chaque exécution du travail. *frequency_subday_interval* est **int**, avec 0 comme valeur par défaut.  
  
 [  **@freq_relative_interval=** ] *frequency_relative_interval*  
 Définit également la *frequency_interval* lorsque *frequency_type* a la valeur **32** (mensuel relatif).  
  
 *frequency_relative_interval* est **int**, sans valeur par défaut et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**1**|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
  
 *frequency_relative_interval* indique l’occurrence de l’intervalle. Par exemple, si *frequency_relative_interval* a la valeur **2**, *frequency_type* a la valeur **32**, et *frequency_interval* a la valeur **3**, le travail planifié a lieu le deuxième mardi de chaque mois.  
  
 [ **@freq_recurrence_factor=** ] *frequency_recurrence_factor*  
 Nombre de semaines ou de mois devant s'écouler entre chaque exécution planifiée du travail. *frequency_recurrence_factor* est utilisée uniquement si *frequency_type* a la valeur **8**, **16**, ou **32**. *frequency_recurrence_factor* est **int**, avec 0 comme valeur par défaut.  
  
 [ **@active_start_date=** ] *active_start_date*  
 Date à laquelle l'exécution du travail peut commencer. *active_start_date* est **int**, sans valeur par défaut. La date est au format AAAAMMJJ. Si *active_start_date* est défini, la date doit être supérieure ou égale à 19900101.  
  
 Après avoir créé la planification, examinez la date de début et assurez-vous qu'elle est correcte. Pour plus d’informations, consultez la section « Planification des Date de début » dans [créer et attacher les planifications de travaux](http://msdn.microsoft.com/library/079c2984-0052-4a37-a2b8-4ece56e6b6b5).  
  
 [  **@active_end_date=** ] *active_end_date*  
 Date à laquelle l'exécution du travail peut s'arrêter. *active_end_date* est **int**, sans valeur par défaut. La date est au format AAAAMMJJ.  
  
 [ **@active_start_time=** ] *active_start_time*  
 Heure de n’importe quel jour entre *active_start_date* et *active_end_date* pour commencer l’exécution du travail. *heure_de_début_active* est **int**, sans valeur par défaut. L'heure est au format HHMMSS et est exprimée sur 24 heures.  
  
 [**@active_end_time= *** heure_fin_active*  
 Heure de n’importe quel jour entre *active_start_date* et *active_end_date* pour arrêter l’exécution de travail. *heure_fin_active* est **int**, sans valeur par défaut. L'heure est au format HHMMSS et est exprimée sur 24 heures.  
  
 [  **@schedule_id=***id_de_la_planification***sortie**  
 Numéro d'identification de planification affecté à la planification si le travail est correctement créé. *id_de_la_planification* est une variable output de type **int**, sans valeur par défaut.  
  
 [ **@schedule_uid**=] *schedule_uid *** sortie**  
 Identificateur unique de la planification. *schedule_uid* est une variable de type **uniqueidentifier**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Il est désormais possible de gérer la planification des travaux indépendamment des travaux. Pour ajouter une planification à un travail, utilisez **sp_add_schedule** pour créer la planification et **sp_attach_schedule** pour l’associer à un travail.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
 
 ## <a name="example"></a>Exemple
 L’exemple suivant assigne une planification du travail `SaturdayReports` qui s’exécute tous les samedis à 2 h 00.
```sql  
EXEC msdb.dbo.sp_add_jobschedule 
        @job_name = N'SaturdayReports', -- Job name
        @name = N'Weekly_Sat_2AM',  -- Schedule name
        @freq_type = 8, -- Weekly
        @freq_interval = 64, -- Saturday
        @freq_recurrence_factor = 1, -- every week
        @active_start_time = 20000 -- 2:00 AM
```
  
## <a name="see-also"></a>Voir aussi  
 [Créer des planifications et les attacher à des travaux](http://msdn.microsoft.com/library/079c2984-0052-4a37-a2b8-4ece56e6b6b5)   
 [Planifier un travail](http://msdn.microsoft.com/library/f626390a-a3df-4970-b7a7-a0529e4a109c)   
 [Créer une planification](http://msdn.microsoft.com/library/8c7ef3b3-c06d-4a27-802d-ed329dc86ef3)   
 [Procédures stockées de l’Agent SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_update_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
