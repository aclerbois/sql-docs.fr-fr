---
title: sp_adddynamicsnapshot_job (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_adddynamicsnapshot_job
- sp_adddynamicsnapshot_job_TSQL
helpviewer_keywords:
- sp_adddynamicsnapshot_job
ms.assetid: ef50ccf6-e360-4e4b-91b9-6706b8fabefa
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2a4967dd959be15f8f9bb1ef4654486b6fee6ace
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spadddynamicsnapshotjob-transact-sql"></a>sp_adddynamicsnapshot_job (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un travail de l'Agent qui crée l'instantané de données filtrées pour une publication avec des filtres de lignes paramétrables. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication. Un administrateur utilise cette procédure stockée pour créer manuellement des travaux d'instantané de données filtrées pour des Abonnés.  
  
> [!NOTE]  
>  Pour créer un travail d'instantané de données filtrées, un travail d'instantané standard doit déjà exister pour la publication.  
  
 Pour plus d’informations, voir [Instantanés des publications de fusion avec des filtres paramétrés](../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adddynamicsnapshot_job [ @publication = ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]   
    [ , [ @host_name = ] 'host_name' ]   
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' OUTPUT ]   
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' OUTPUT ]   
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication à laquelle le travail d'instantané de données filtrées est ajouté. *publication* est **sysname**, sans valeur par défaut.  
  
 [ **@suser_sname**=] **'***suser_sname***'**  
 Valeur utilisée lors de la création d’un instantané de données filtrées pour un abonnement filtré par la valeur de la [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) fonction sur l’abonné. *SUSER_SNAME* est **sysname**, sans valeur par défaut. *SUSER_SNAME* doit avoir la valeur NULL si cette fonction n’est pas utilisée pour filtrer dynamiquement la publication.  
  
 [ **@host_name**=] **'***host_name***'**  
 Valeur utilisée lors de la création d’un instantané de données filtrées pour un abonnement filtré par la valeur de la [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) fonction sur l’abonné. *HOST_NAME* est **sysname**, sans valeur par défaut. *HOST_NAME* doit avoir la valeur NULL si cette fonction n’est pas utilisée pour filtrer dynamiquement la publication.  
  
 [ **@dynamic_snapshot_jobname**=] **'***dynamic_snapshot_jobname***'**  
 Nom du travail d'instantané de données filtrées créé. *dynamic_snapshot_jobname* est **sysname**, avec NULL comme valeur par défaut et est un paramètre OUTPUT facultatif. Si spécifié, *dynamic_snapshot_jobname* doit correspondre à un travail unique sur le serveur de distribution. S'il n'est pas spécifié, un nom de travail est automatiquement créé et renvoyé dans l'ensemble de résultats, où le nom est créé comme suit :  
  
```  
'dyn_' + <name of the standard snapshot job> + <GUID>  
```  
  
> [!NOTE]  
>  Lors de la création du nom du travail d'instantané dynamique, vous pouvez tronquer le nom du travail d'instantané standard.  
  
 [ **@dynamic_snapshot_jobid**=] **'***dynamic_snapshot_jobid***'**  
 Identificateur du travail d'instantané de données filtrées créé. *dynamic_snapshot_jobid* est **uniqueidentifier**, avec NULL comme valeur par défaut et est un paramètre OUTPUT facultatif.  
  
 [  **@frequency_type=**] *frequency_type*  
 Fréquence à laquelle doit être planifié le travail d'instantané de données filtrées. *frequency_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|À la demande|  
|**4** (par défaut)|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
|**128**|Périodique|  
  
 [  **@frequency_interval =** ] *frequency_interval*  
 Périodicité (en jours) du travail d'instantané de données filtrées. *frequency_interval* est **int**, avec la valeur par défaut 1 et varie en fonction de la valeur de *frequency_type*.  
  
|Valeur de *frequency_type*|Effet sur *frequency_interval*|  
|--------------------------------|-------------------------------------|  
|**1**|*frequency_interval* n’est pas utilisée.|  
|**4** (par défaut)|Chaque *frequency_interval* jours, avec une valeur par défaut de tous les jours.|  
|**8**|*frequency_interval* prend une ou plusieurs des opérations suivantes (combinées avec un [ &#124; &#40;au niveau du bit OR&#41; &#40;Transact-SQL&#41; ](../../t-sql/language-elements/bitwise-or-transact-sql.md) opérateur logique) :<br /><br /> **1** = dimanche &#124; **2** = lundi &#124; **4** = mardi &#124; **8** = mercredi &#124; **16** = Jeudi &#124; **32** = vendredi &#124; **64** = samedi|  
|**16**|Sur le *frequency_interval* jour du mois.|  
|**32**|*frequency_interval* est une des opérations suivantes :<br /><br /> **1** = dimanche &#124; **2** = lundi &#124; **3** = mardi &#124; **4** = mercredi &#124; **5** = Jeudi &#124; **6** = vendredi &#124; **7** = samedi &#124; **8** = jour &#124; **9** = jour de semaine &#124; **10** = jour de week-end|  
|**64**|*frequency_interval* n’est pas utilisée.|  
|**128**|*frequency_interval* n’est pas utilisée.|  
  
 [  **@frequency_subday=**] *frequency_subday*  
 Spécifie les unités de *frequency_subday_interval*. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4** (par défaut)|Minute|  
|**8**|Heure|  
  
 [  **@frequency_subday_interval=**] *frequency_subday_interval*  
 Est le nombre de *frequency_subday* périodes entre chaque exécution du travail. *frequency_subday_interval* est **int**, avec 5 comme valeur par défaut.  
  
 [  **@frequency_relative_interval=**] *frequency_relative_interval*  
 Occurrence du travail d'instantané de données filtrées pour chaque mois. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuel relatif). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**1** (par défaut)|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
  
 [  **@frequency_recurrence_factor=**] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est **int**, avec 0 comme valeur par défaut.  
  
 [  **@active_start_date=**] *active_start_date*  
 Date à laquelle le travail d'instantané de données filtrées est planifié pour la première fois, exprimée au format AAAAMMJJ. *active_start_date* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_date=**] *active_end_date*  
 Date à laquelle le travail d'instantané de données filtrées cesse d'être planifié, exprimée au format AAAAMMJJ. *active_end_date* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_start_time_of_day=**] *active_start_time_of_day*  
 Heure du jour à laquelle le travail d'instantané de données filtrées est planifié pour la première fois, exprimée au format HHMMSS. *active_start_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_time_of_day=**] *active_end_time_of_day*  
 Heure du jour à laquelle le travail d'instantané de données filtrées cesse d'être planifié, exprimée au format HHMMSS. *active_end_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identifie le travail de capture instantanée de données filtrées dans le [MSdynamicsnapshotjobs](../../relational-databases/system-tables/msdynamicsnapshotjobs-transact-sql.md) (table système).|  
|**dynamic_snapshot_jobname**|**sysname**|Nom du travail d'instantané de données filtrées.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Identifie de façon unique le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] travail de l’Agent sur le serveur de distribution.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adddynamicsnapshot_job** est utilisé dans la réplication de fusion pour les publications qui utilisent un filtre paramétré.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-adddynamicsnapshot-jo_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_adddynamicsnapshot_job**.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un instantané d’une Publication de fusion avec des filtres paramétrés](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Filtres de lignes paramétrés](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [sp_dropdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdynamicsnapshot-job-transact-sql.md)   
 [sp_helpdynamicsnapshot_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdynamicsnapshot-job-transact-sql.md)  
  
  
