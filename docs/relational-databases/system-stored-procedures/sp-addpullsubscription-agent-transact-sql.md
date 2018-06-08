---
title: sp_addpullsubscription_agent (Transact-SQL) | Documents Microsoft
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
- sp_addpullsubscription_agent
- sp_addpullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addpullsubscription_agent
ms.assetid: b9c2eaed-6d2d-4b78-ae9b-73633133180b
caps.latest.revision: 44
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 82689b234a6294ef4c13c801ba88a7d700b0db44
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddpullsubscriptionagent-transact-sql"></a>sp_addpullsubscription_agent (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
 
  Ajoute un nouveau travail de l'Agent planifié, utilisé pour synchroniser un abonnement par extraction de données (pull) avec une publication transactionnelle. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addpullsubscription_agent [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]          , [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor = ] 'distributor' ]  
    [ , [ @distribution_db = ] 'distribution_db' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]  
    [ , [ @distributor_login = ] 'distributor_login' ]  
    [ , [ @distributor_password = ] 'distributor_password' ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
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
    [ , [ @distribution_jobid = ] distribution_jobid OUTPUT ]  
    [ , [ @encrypted_distributor_password = ] encrypted_distributor_password ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port = ] ftp_port ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]  
    [ , [ @working_directory = ] 'working_directory' ]  
    [ , [ @use_ftp = ] 'use_ftp' ]  
    [ , [ @publication_type = ] publication_type ]  
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @offloadagent = ] 'remote_agent_activation' ]  
    [ , [ @offloadserver = ] 'remote_agent_server_name']  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publisher=**] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher_db=**] **' *** publisher_db'*  
 Nom de la base de données du serveur de publication. *publisher_db* est **sysname**, avec NULL comme valeur par défaut. *publisher_db* est ignoré par les serveurs de publication Oracle.  
  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@subscriber=**] **'***abonné***'**  
 Nom de l'Abonné. *abonné* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis.  
  
 [  **@subscriber_db=**] **'***bd_abonné***'**  
 Est le nom de la base de données d’abonnement. *bd_abonné* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis.  
  
 [  **@subscriber_security_mode=**] *subscriber_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un abonné au cours d'une synchronisation. *subscriber_security_mode* est **int,** avec NULL comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** Spécifie l’authentification Windows.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. L'Agent de distribution se connecte toujours à l'Abonné local à l'aide de l'authentification Windows. Si une valeur différente de NULL ou **1** est spécifié pour ce paramètre, un message d’avertissement est retourné.  
  
 [  **@subscriber_login =**] **'***subscriber_login***'**  
 Est la connexion à l’abonné à utiliser lors de la connexion à un abonné lors de la synchronisation. *subscriber_login* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est précisée pour ce paramètre, un message d'avertissement est retourné mais la valeur reste ignorée.  
  
 [  **@subscriber_password=**] **'***subscriber_password***'**  
 Mot de passe de l'Abonné. *subscriber_password* est requise si *subscriber_security_mode* a la valeur **0**. *subscriber_password* est **sysname**, avec NULL comme valeur par défaut. Si un mot de passe d'abonné est utilisé, il est automatiquement chiffré.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est précisée pour ce paramètre, un message d'avertissement est retourné mais la valeur reste ignorée.  
  
 [  **@distributor=**] **'***distributeur***'**  
 Est le nom du serveur de distribution. *serveur de distribution* est **sysname**, avec une valeur par défaut la valeur spécifiée par *publisher*.  
  
 [  **@distribution_db=**] **'***distribution_db***'**  
 Est le nom de la base de données de distribution. *distribution_db* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@distributor_security_mode=**] *distributor_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un serveur de distribution au cours d'une synchronisation. *l’argument distributor_security_mode* est **int**, avec une valeur par défaut **1**. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** Spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
 [  **@distributor_login=**] **'***distributor_login***'**  
 Nom de connexion du serveur de distribution à utiliser lors de la connexion au cours d'une synchronisation. *Cet argument* est requise si *distributor_security_mode* a la valeur **0**. *Cet argument* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@distributor_password =**] **'***distributor_password***'**  
 Mot de passe du serveur de distribution. *distributor_password* est requise si *distributor_security_mode* a la valeur **0**. *distributor_password* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@optional_command_line=**] **'***optional_command_line***'**  
 Invite de commandes facultative de l'Agent de distribution. Par exemple, **- DefinitionFile** C:\Distdef.txt ou **- CommitBatchSize** 10. *optional_command_line* est **nvarchar (4000)**, avec la valeur par défaut est une chaîne vide.  
  
 [  **@frequency_type=**] *frequency_type*  
 Fréquence de planification de l'Agent de distribution. *frequency_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2** (par défaut)|À la demande|  
|**4**|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
|**128**|Périodique|  
  
> [!NOTE]  
>  La valeur de **64** provoque l’Agent de Distribution s’exécute en mode continu. Cela correspond au paramètre la **-continue** paramètre pour l’agent. Pour plus d'informations, consultez [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
 [  **@frequency_interval=**] *frequency_interval*  
 Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est **int**, avec 1 comme valeur par défaut.  
  
 [  **@frequency_relative_interval=**] *frequency_relative_interval*  
 Date de l'Agent de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuel relatif). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**1** (par défaut)|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
  
 [  **@frequency_recurrence_factor=**] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est **int**, avec une valeur par défaut **1**.  
  
 [  **@frequency_subday=**] *frequency_subday*  
 Fréquence de replanification nécessaire pendant la période définie. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**1** (par défaut)|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
  
 [  **@frequency_subday_interval=**] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est **int**, avec une valeur par défaut **1**.  
  
 [  **@active_start_time_of_day=**] *active_start_time_of_day*  
 Heure à laquelle l’Agent de distribution est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est **int**, avec une valeur par défaut **0**.  
  
 [  **@active_end_time_of_day=**] *active_end_time_of_day*  
 L’heure de la journée à laquelle l’Agent de Distribution cesse d’être planifié, représentée au format HHMMSS. *active_end_time_of_day* est **int**, avec une valeur par défaut **0**.  
  
 [  **@active_start_date=**] *active_start_date*  
 Date à laquelle l’Agent de distribution est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est **int**, avec une valeur par défaut **0**.  
  
 [  **@active_end_date=**] *active_end_date*  
 Date à laquelle l’Agent de distribution cesse d'être planifié, au format AAAAMMJJ. *active_end_date* est **int**, avec une valeur par défaut **0**.  
  
 [  **@distribution_jobid =**] *id_tâche_distribution *** sortie**  
 ID de l'Agent de distribution pour ce travail. *id_tâche_distribution* est **Binary (16)**, avec la valeur par défaut est NULL et il est un paramètre de sortie.  
  
 [  **@encrypted_distributor_password=**] *mot_de_passe_distributeur_crypté*  
 Paramètre *mot_de_passe_distributeur_crypté* n’est plus pris en charge. Tentative de définition de cette **bits** paramètre **1** entraîne une erreur.  
  
 [  **@enabled_for_syncmgr=**] **'***l’argument enabled_for_syncmgr***'**  
 Indique si l’abonnement peut être synchronisé via [!INCLUDE[msCoName](../../includes/msconame-md.md)] le Gestionnaire de synchronisation. *l’argument enabled_for_syncmgr* est **nvarchar (5)**, avec FALSE comme valeur par défaut. Si **false**, l’abonnement n’est pas inscrit avec le Gestionnaire de synchronisation. Si **true**, l’abonnement est enregistré avec le Gestionnaire de synchronisation et peut être synchronisé sans démarrer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 [  **@ftp_address=**] **'***ftp_address***'**  
 Pour compatibilité descendante uniquement.  
  
 [  **@ftp_port=**] *ftp_port*  
 Pour compatibilité descendante uniquement.  
  
 [  **@ftp_login=**] **'***ftp_login***'**  
 Pour compatibilité descendante uniquement.  
  
 [  **@ftp_password=**] **'***ftp_password***'**  
 Pour compatibilité descendante uniquement.  
  
 [  **@alt_snapshot_folder=** ] **' *** alternate_snapshot_folder'*  
 Indique l'emplacement du dossier de remplacement pour l'instantané. *alternate_snapshot_folder* est **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
 [ **@working_directory**=] **'***working_director***'**  
 Nom du répertoire de travail utilisé pour stocker les fichiers de données et de schéma destinés à la publication. *working_directory* est **nvarchar (255)**, avec NULL comme valeur par défaut. Le nom doit être indiqué au format UNC.  
  
 [ **@use_ftp**=] **'***use_ftp***'**  
 Spécifie l’utilisation de FTP au lieu du protocole usuel pour extraire les instantanés. *use_ftp* est **nvarchar (5)**, avec FALSE comme valeur par défaut.  
  
 [ **@publication_type**=] *publication_type*  
 Indique le type de réplication de la publication. *publication_type* est un **tinyint** avec une valeur par défaut **0**. Si **0**, publication est un type de transaction. Si **1**, publication est de type instantané. Si **2**, publication est un type de fusion.  
  
 [ **@dts_package_name**=] **'***l’argument dts_package_name***'**  
 Spécifie le nom du package DTS. *l’argument dts_package_name* est un **sysname** avec NULL comme valeur par défaut. Par exemple, pour spécifier un package nommé `DTSPub_Package`, le paramètre est le suivant : `@dts_package_name = N'DTSPub_Package'`.  
  
 [ **@dts_package_password**=] **'***dts_package_password***'**  
 Spécifie le mot de passe du package, s'il existe. *dts_package_password* est **sysname** avec NULL comme valeur par défaut, ce qui signifie un mot de passe n’est pas sur le package.  
  
> [!NOTE]  
>  Vous devez spécifier un mot de passe si *l’argument dts_package_name* est spécifié.  
  
 [ **@dts_package_location**=] **'***dts_package_location***'**  
 Spécifie l'emplacement du package. *dts_package_location* est un **nvarchar(12)**, avec une valeur par défaut **abonné**. L’emplacement du package peut être **distributeur** ou **abonné**.  
  
 [ **@reserved**=] **'***réservé***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ **@offloadagent**=] '*remote_agent_activation*'  
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Paramètre *remote_agent_activation* à une valeur autre que **false** génère une erreur.  
  
 [ **@offloadserver**=] '*remote_agent_server_name*'  
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Paramètre *remote_agent_server_name* à n’importe quelle valeur non NULL génère une erreur.  
  
 [ **@job_name**=] '*job_name*'  
 Nom d'un travail de l'agent existant. *job_name* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est indiqué que lorsque l'abonnement est synchronisé grâce à un travail existant plutôt qu'un nouveau travail (étant le comportement par défaut). Si vous n’êtes pas un membre de la **sysadmin** rôle serveur fixe, vous devez spécifier *job_login* et *job_password* lorsque vous spécifiez *job_name*.  
  
 [ **@job_login**=] **'***job_login***'**  
 Nom de connexion du compte Windows sous lequel l'Agent s'exécute. *job_login* est **nvarchar (257)**, sans valeur par défaut. C'est ce compte Windows qui est destiné à toujours être utilisé pour les connexions des Agents à l'Abonné.  
  
 [ **@job_password**=] **'***job_password***'**  
 Mot de passe du compte Windows sous lequel l'Agent s'exécute. *job_password* est **sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addpullsubscription_agent** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_addpullsubscription_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un abonnement par extraction](../../relational-databases/replication/create-a-pull-subscription.md)   
 [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
