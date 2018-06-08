---
title: sp_update_schedule (Transact-SQL) | Documents Microsoft
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
- sp_update_schedule
- sp_update_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_schedule
ms.assetid: 97b3119b-e43e-447a-bbfb-0b5499e2fefe
caps.latest.revision: 42
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ed8a500af524796cf98a16f9da75aae003375c1a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spupdateschedule-transact-sql"></a>sp_update_schedule (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les paramètres d'une planification de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_update_schedule   
    {   [ @schedule_id = ] schedule_id   
      | [ @name = ] 'schedule_name' }  
    [ , [ @new_name = ] new_name ]  
    [ , [ @enabled = ] enabled ]  
    [ , [ @freq_type = ] freq_type ]  
    [ , [ @freq_interval = ] freq_interval ]   
    [ , [ @freq_subday_type = ] freq_subday_type ]   
    [ , [ @freq_subday_interval = ] freq_subday_interval ]   
    [ , [ @freq_relative_interval = ] freq_relative_interval ]   
    [ , [ @freq_recurrence_factor = ] freq_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time = ] active_start_time ]   
    [ , [ @active_end_time = ] active_end_time ]   
    [ , [ @owner_login_name = ] 'owner_login_name' ]  
    [ , [ @automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@schedule_id =** ] *id_de_la_planification*  
 Identificateur de la planification à modifier. *id_de_la_planification* est **int**, sans valeur par défaut. Soit *id_de_la_planification* ou *nom_de_la_planification* doit être spécifié.  
  
 [  **@name =** ] **'***nom_de_la_planification***'**  
 Nom de la planification à modifier. *nom_de_la_planification*est **sysname**, sans valeur par défaut. Soit *id_de_la_planification* ou *nom_de_la_planification* doit être spécifié.  
  
 [ **@new_name**=] *nouveau_nom*  
 Nouveau nom de la planification. *nouveau_nom* est **sysname**, avec NULL comme valeur par défaut. Lorsque *nouveau_nom* est NULL, le nom de la planification reste inchangé.  
  
 [  **@enabled =** ] *activé*  
 Indique l'état actuel de la planification. *activé*est **tinyint**, avec une valeur par défaut **1** (activé). Si **0**, la planification n’est pas activée. Si la planification n'est pas activée, aucun travail n'est exécuté dans cette dernière.  
  
 [ **@freq_type =** ] *freq_type*  
 Valeur indiquant à quel moment un travail doit être exécuté. *freq_type*est **int**, avec une valeur par défaut **0**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**4**|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Tous les mois, relatif à *freq_interval.*|  
|**64**|Lancé au démarrage du service SQLServerAgent|  
|**128**|Exécution pendant une période d'inactivité de l'ordinateur.|  
  
 [  **@freq_interval =** ] *freq_interval*  
 Jours d’exécution du travail. *freq_interval* est **int**, avec une valeur par défaut **0**et varie en fonction de la valeur de *freq_type*.  
  
|Valeur de *freq_type*|Effet sur *freq_interval*|  
|---------------------------|--------------------------------|  
|**1** (une fois)|*freq_interval* n’est pas utilisée.|  
|**4** (quotidienne)|Chaque *freq_interval* jours.|  
|**8** (hebdomadaire)|*freq_interval* prend une ou plusieurs des opérations suivantes (combinées avec un **OR** opérateur logique) :<br /><br /> **1** = dimanche<br /><br /> **2** = lundi<br /><br /> **4** = mardi<br /><br /> **8** = mercredi<br /><br /> **16** = jeudi<br /><br /> **32** = vendredi<br /><br /> **64** = samedi|  
|**16** (mensuellement)|Sur le *freq_interval* jour du mois.|  
|**32** (mensuel relatif)|*freq_interval* est une des opérations suivantes :<br /><br /> **1** = dimanche<br /><br /> **2** = lundi<br /><br /> **3** = mardi<br /><br /> **4** = mercredi<br /><br /> **5** = jeudi<br /><br /> **6** = vendredi<br /><br /> **7** = samedi<br /><br /> **8** = jour<br /><br /> **9** = jour de semaine<br /><br /> **10** = jour de week-end|  
|**64** (démarrage de service SQLServerAgent)|*freq_interval* n’est pas utilisée.|  
|**128**|*freq_interval* n’est pas utilisée.|  
  
 [ **@freq_subday_type =** ] *freq_subday_type*  
 Spécifie les unités de *freq_subday_interval **.* *freq_subday_type*est **int**, avec une valeur par défaut **0**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**0x1**|À une heure spécifiée|  
|**0x2**|Secondes|  
|**0x4**|Minutes|  
|**0x8**|Heures|  
  
 [  **@freq_subday_interval =** ] *freq_subday_interval*  
 Le nombre de *freq_subday_type* périodes entre chaque exécution d’un travail. *freq_subday_interval*est **int**, avec une valeur par défaut **0**.  
  
 [ **@freq_relative_interval =** ] *freq_relative_interval*  
 Occurrence d’un travail de *freq_interval* dans chaque mois, si *freq_interval* est **32** (mensuel relatif). *freq_relative_interval*est **int**, avec une valeur par défaut **0**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description (unité)|  
|-----------|--------------------------|  
|**1**|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
  
 [ **@freq_recurrence_factor =** ] *freq_recurrence_factor*  
 Nombre de semaines ou de mois devant s'écouler entre chaque exécution planifiée d'un travail. *freq_recurrence_factor* est utilisée uniquement si *freq_type* est **8**, **16**, ou **32**. *freq_recurrence_factor*est **int**, avec une valeur par défaut **0**.  
  
 [ **@active_start_date =** ]  *active_start_date*  
 La date à laquelle l’exécution d’un travail peut commencer. *active_start_date*est **int**, avec NULL comme valeur par défaut, ce qui indique la date du jour. La date est au format AAAAMMJJ. Si *active_start_date* n’est pas NULL, la date doit être supérieure ou égale à 19900101.  
  
 Après avoir créé la planification, examinez la date de début et assurez-vous qu'elle est correcte. Pour plus d’informations, consultez la section « Planification des Date de début » dans [créer et attacher les planifications de travaux](http://msdn.microsoft.com/library/079c2984-0052-4a37-a2b8-4ece56e6b6b5).  
  
 [  **@active_end_date =** ] *active_end_date*  
 Date à laquelle l'exécution d'un travail peut s'arrêter. *active_end_date*est **int**, avec une valeur par défaut **99991231**, ce qui indique le 31 décembre 9999. La mise en forme est la suivante : AAAAMMJJ.  
  
 [  **@active_start_time =** ] *heure_de_début_active*  
 L’heure sur n’importe quel jour entre *active_start_date* et *active_end_date* pour commencer l’exécution d’un travail. *heure_de_début_active*est **int**, avec une valeur 000000 par défaut, ce qui signifie 12:00:00 a.m. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [  **@active_end_time =** ] *heure_fin_active*  
 L’heure sur n’importe quel jour entre *active_start_date* et *active_end_date* pour arrêter l’exécution d’une tâche. *heure_fin_active*est **int**, avec une valeur par défaut **235959**, ce qui indique à 11:59:59 PM sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [ **@owner_login_name**=] **'***owner_login_name***'**]  
 Nom du principal de serveur qui détient la planification. *owner_login_name* est **sysname**, avec NULL comme valeur par défaut, ce qui signifie que la planification est détenue par le créateur.  
  
 [  **@automatic_post =**] *envoi_automatique*  
 Réservé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Tous les travaux qui utilisent la planification utilisent immédiatement les nouveaux paramètres. Cependant, la modification d'une planification n'entraîne pas l'arrêt des travaux en cours d'exécution.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
 Seuls les membres du **sysadmin** peuvent modifier une planification détenue par un autre utilisateur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment changer l'état activé de la planification `NightlyJobs` en `0` et comment définir le propriétaire à `terrid`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_schedule  
    @name = 'NightlyJobs',  
    @enabled = 0,  
    @owner_login_name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Créer des planifications et les attacher à des travaux](http://msdn.microsoft.com/library/079c2984-0052-4a37-a2b8-4ece56e6b6b5)   
 [Planifier un travail](http://msdn.microsoft.com/library/f626390a-a3df-4970-b7a7-a0529e4a109c)   
 [Créer une planification](http://msdn.microsoft.com/library/8c7ef3b3-c06d-4a27-802d-ed329dc86ef3)   
 [Procédures stockées de l’Agent SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobschedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
