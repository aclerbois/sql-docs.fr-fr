---
title: sp_changesubstatus (Transact-SQL) | Documents Microsoft
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
- sp_changesubstatus
- sp_changesubstatus_TSQL
helpviewer_keywords:
- sp_changesubstatus
ms.assetid: 9370e47a-d128-4f15-9224-1c3642770c39
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 94c26330636d4e13fe84a2776a72315a418d3d96
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchangesubstatus-transact-sql"></a>sp_changesubstatus (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie l'état d'un abonné existant. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changesubstatus [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
        , [ @status = ] 'status'  
    [ , [ @previous_status = ] 'previous_status' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @distribution_jobid = ] distribution_jobid ]  
    [ , [ @from_auto_sync = ] from_auto_sync ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @offloadagent= ] remote_agent_activation ]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] dts_package_location ]  
    [ , [ @skipobjectactivation = ] skipobjectactivation  
  [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec une valeur par défaut **%**. Si *publication* n’est pas spécifié, toutes les publications sont affectées.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article. Doit être unique et propre à la publication. *article* est **sysname**, avec une valeur par défaut **%**. Si *article* n’est pas spécifiée, tous les articles sont affectées.  
  
 [  **@subscriber=**] **'***abonné***'**  
 Nom de l'Abonné pour lequel changer l'état. *abonné* est **sysname**, avec une valeur par défaut **%**. Si *abonné* n’est pas spécifié, état est modifié pour tous les abonnés à l’article spécifié.  
  
 [  **@status =**] **'***état***'**  
 L’état de l’abonnement dans le **syssubscriptions** table. *état* est **sysname**, sans valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Active**|L'abonné est synchronisé et reçoit des données.|  
|**Inactif**|L'entrée de l'abonné existe, sans abonnement.|  
|**abonné**|L'abonné demande des données mais n'est pas encore synchronisé.|  
  
 [  **@previous_status=**] **'***previous_status***'**  
 État antérieur de l'abonnement. *previous_status* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre vous permet de modifier les abonnements ayant actuellement cet état, ce qui permet de fonctions de groupe sur un ensemble spécifique d’abonnements (par exemple, si tous les abonnements pour revenir **abonné**).  
  
 [  **@destination_db=**] **'***destination_db***'**  
 Nom de la base de données de destination. *destination_db* est **sysname**, avec une valeur par défaut **%**.  
  
 [  **@frequency_type=**] *frequency_type*  
 Fréquence de planification de la tâche de distribution. *frequency_type* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@frequency_interval=**] *frequency_interval*  
 Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@frequency_relative_interval=**] *frequency_relative_interval*  
 Date de la tâche de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur 32 (fréquence mensuelle relative). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
|NULL (par défaut)||  
  
 [  **@frequency_recurrence_factor=**] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@frequency_subday=**] *frequency_subday*  
 Indique, en minutes, la fréquence de replanification pendant la période définie. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
|NULL (par défaut)||  
  
 [  **@frequency_subday_interval=**] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_start_time_of_day=**] *active_start_time_of_day*  
 Heure du jour de la première planification de la tâche de distribution, au format HHMMSS. *active_start_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_time_of_day=**] *active_end_time_of_day*  
 Heure à laquelle la tâche de distribution cesse d'être planifiée, exprimée au format HHMMSS. *active_end_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_start_date=**] *active_start_date*  
 Date de première planification de la tâche de distribution, au format AAAAMMJJ. *active_start_date* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_date=**] *active_end_date*  
 Date à laquelle la tâche de distribution cesse d'être planifiée, exprimée au format AAAAMMJJ. *active_end_date* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@optional_command_line=**] **'***optional_command_line***'**  
 Invite de commandes facultative. *optional_command_line* est **nvarchar (4000)**, avec NULL comme valeur par défaut.  
  
 [  **@distribution_jobid=**] *id_tâche_distribution*  
 ID du travail de l'Agent de distribution auprès du serveur de distribution de l'abonnement lorsque l'abonnement passe de l'état inactif à actif. Dans les autres cas, il n'est pas défini. Si plus d'un Agent de distribution est impliqué dans un seul appel à cette procédure stockée, le résultat n'est pas défini. *id_tâche_distribution* est **Binary (16)**, avec NULL comme valeur par défaut.  
  
 [  **@from_auto_sync=**] *from_auto_sync*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [  **@ignore_distributor=**] *ignore_distributor*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [  **@offloadagent=** ] *remote_agent_activation*  
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Paramètre *remote_agent_activation* à une valeur autre que **0** génère une erreur.  
  
 [  **@offloadserver=** ] **'***remote_agent_server_name***'**  
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Paramètre *remote_agent_server_name* à n’importe quelle valeur non NULL, génère une erreur.  
  
 [ **@dts_package_name**=] **'***l’argument dts_package_name***'**  
 Spécifie le nom du package DTS (Data Transformation Services). *l’argument dts_package_name* est un **sysname**, avec NULL comme valeur par défaut. Par exemple, pour un package nommé **DTSPub_Package** vous spécifiez `@dts_package_name = N'DTSPub_Package'`.  
  
 [ **@dts_package_password**=] **'***dts_package_password***'**  
 Spécifie le mot de passe du package. *dts_package_password* est **sysname** avec une valeur par défaut NULL, qui indique que la propriété de mot de passe ne doit être modifiée.  
  
> [!NOTE]  
>  Un package DTS doit avoir un mot de passe.  
  
 [ **@dts_package_location**=] *dts_package_location*  
 Spécifie l'emplacement du package. *dts_package_location* est un **int**, avec une valeur par défaut **0**. Si **0**, l’emplacement du package est sur le serveur de distribution. Si **1**, l’emplacement du package est sur l’abonné. L’emplacement du package peut être **distributeur** ou **abonné**.  
  
 [ **@skipobjectactivation**=] *skipobjectactivation*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [  **@distribution_job_name=** ] **'***distribution_job_name***'**  
 Nom du travail de distribution. *distribution_job_name* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ **@publisher**=] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé lors de la modification des propriétés de l’article sur un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changesubstatus** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
 **sp_changesubstatus** modifie l’état de l’abonné dans le **syssubscriptions** table avec l’état modifié. Si nécessaire, il met à jour l’état de l’article dans la **sysarticles** table pour indiquer active ou inactive. Si nécessaire, il définit l’indicateur de réplication ou désactiver le **sysobjects** table pour la table répliquée.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle de serveur fixe **db_owner** rôle de base de données fixe, ou le créateur de l’abonnement peut exécuter **sp_changesubstatus**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
