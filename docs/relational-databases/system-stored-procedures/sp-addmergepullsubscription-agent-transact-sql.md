---
title: sp_addmergepullsubscription_agent (Transact-SQL) | Documents Microsoft
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
- sp_addmergepullsubscription_agent
- sp_addmergepullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addmergepullsubscription_agent
ms.assetid: a2f4b086-078d-49b5-8971-8a1e3f6a6feb
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a728fc2fff24001355a59a9df1f9701d83bd75fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddmergepullsubscriptionagent-transact-sql"></a>sp_addmergepullsubscription_agent (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un nouveau travail d'Agent permettant de planifier la synchronisation d'un abonnement par extraction de données (pull) à une publication de fusion. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergepullsubscription_agent [ [ @name = ] 'name' ]   
        , [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication =] 'publication'   
    [ , [ @publisher_security_mod e= ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher_encrypted_password = ] publisher_encrypted_password ]   
    [ , [ @subscriber = ] 'subscriber' ]   
    [ , [ @subscriber_db = ] 'subscriber_db' ]   
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]   
    [ , [ @subscriber_login = ] 'subscriber_login' ]   
    [ , [ @subscriber_password= ] 'subscriber_password' ]   
    [ , [ @distributor = ] 'distributor' ]   
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @encrypted_password = ] encrypted_password ]   
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
    [ , [ @merge_jobid = ] merge_jobid ]   
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]    
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @working_directory = ] 'working_directory' ]   
    [ , [ @use_ftp = ] 'use_ftp' ]   
    [ , [ @reserved = ] 'reserved' ]   
    [ , [ @use_interactive_resolver = ] 'use_interactive_resolver' ]   
    [ , [ @offloadagent = ] 'remote_agent_activation' ]   
    [ , [ @offloadserver = ] 'remote_agent_server_name']   
    [ , [ @job_name = ] 'job_name' ]   
    [ , [ @dynamic_snapshot_location = ] 'dynamic_snapshot_location' ]  
    [ , [ @use_web_sync = ] use_web_sync ]  
        [ , [ @internet_url = ] 'internet_url' ]  
    [ , [ @internet_login = ] 'internet_login' ]  
        [ , [ @internet_password = ] 'internet_password' ]  
    [ , [ @internet_security_mode = ] internet_security_mode ]  
        [ , [ @internet_timeout = ] internet_timeout ]  
    [ , [ @hostname = ] 'hostname' ]  
        [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@name =** ] **'***nom***'**  
 Nom de l'agent. *nom* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@publisher =** ] **'***publisher***'**  
 Est le nom du serveur de publication. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher_db =** ] **'***publisher_db***'**  
 Nom de la base de données du serveur de publication. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [  **@publication =** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher_security_mode =** ] *publisher_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un serveur de publication au cours d'une synchronisation. *publisher_security_mode* est **int**, avec 1 comme valeur par défaut. Si **0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
 [  **@publisher_login =** ] **'***publisher_login***'**  
 Nom de connexion à utiliser lors de la connexion à un serveur de publication pendant la synchronisation. *publisher_login* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@publisher_password =** ] **'***publisher_password***'**  
 Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Si possible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@publisher_encrypted_password =** ]*mot_de_passe_éditeur_crypté*  
 Paramètre *mot_de_passe_éditeur_crypté* n’est plus pris en charge. Tentative de définition de cette **bits** paramètre **1** entraîne une erreur.  
  
 [  **@subscriber =** ] **'***abonné***'**  
 Nom de l'Abonné. *abonné* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@subscriber_db =** ] **'***bd_abonné***'**  
 Est le nom de la base de données d’abonnement. *bd_abonné* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@subscriber_security_mode =** ] *subscriber_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un abonné au cours d'une synchronisation. *subscriber_security_mode* est **int**, avec 1 comme valeur par défaut. Si **0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. L'Agent de fusion se connecte toujours à l'Abonné local à l'aide de l'authentification Windows. Si une valeur est spécifiée pour ce paramètre, un message d'avertissement est retourné, mais la valeur est ignorée.  
  
 [  **@subscriber_login =** ] **'***subscriber_login***'**  
 Nom de connexion de l'abonné à utiliser lors de la connexion à un abonné au cours d'une synchronisation. *subscriber_login* est requise si *subscriber_security_mode* a la valeur **0**. *subscriber_login* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est spécifiée pour ce paramètre, un message d'avertissement est retourné, mais la valeur est ignorée.  
  
 [  **@subscriber_password =** ] **'***subscriber_password***'**  
 Mot de passe de l'abonné pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *subscriber_password* est requise si *subscriber_security_mode* a la valeur **0**. *subscriber_password* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est spécifiée pour ce paramètre, un message d'avertissement est retourné, mais la valeur est ignorée.  
  
 [  **@distributor =** ] **'***distributeur***'**  
 Est le nom du serveur de distribution. *serveur de distribution* est **sysname**, avec une valeur par défaut *publisher*; autrement dit, le serveur de publication est également le serveur de distribution.  
  
 [  **@distributor_security_mode =** ] *distributor_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un serveur de distribution au cours d'une synchronisation. *l’argument distributor_security_mode* est **int**, avec 0 comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** Spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
 [  **@distributor_login =** ] **'***distributor_login***'**  
 Nom de connexion du serveur de distribution à utiliser lors de la connexion au cours d'une synchronisation. *Cet argument* est requise si *distributor_security_mode* a la valeur **0**. *Cet argument* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@distributor_password =** ] **'***distributor_password***'**  
 Mot de passe du serveur de distribution. *distributor_password* est requise si *distributor_security_mode* a la valeur **0**. *distributor_password* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Si possible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@encrypted_password =** ] *encrypted_password*  
 Paramètre *encrypted_password* n’est plus pris en charge. Tentative de définition de cette **bits** paramètre **1** entraîne une erreur.  
  
 [  **@frequency_type =** ] *frequency_type*  
 Fréquence de planification de l'Agent de fusion. *frequency_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|À la demande|  
|**4**|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
|**128**|Périodique|  
|NULL (par défaut)||  
  
> [!NOTE]  
>  La valeur de **64** , l’Agent de fusion s’exécute en mode continu. Cela correspond au paramètre la **-continue** paramètre pour l’agent. Pour plus d’informations, consultez [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
 [  **@frequency_interval =** ] *frequency_interval*  
 Jour(s) où l'Agent de fusion s'exécute. *frequency_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Dimanche|  
|**2**|Lundi|  
|**3**|Mardi|  
|**4**|Mercredi|  
|**5**|Jeudi|  
|**6**|Vendredi|  
|**7**|Samedi|  
|**8**|Jour|  
|**9**|Jours de la semaine|  
|**10**|Jours de week-end|  
|NULL (par défaut)||  
  
 [  **@frequency_relative_interval =** ] *frequency_relative_interval*  
 Date de l'Agent de fusion. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuel relatif). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
|NULL (par défaut)||  
  
 [  **@frequency_recurrence_factor =** ] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@frequency_subday =** ] *frequency_subday*  
 Fréquence de replanification nécessaire pendant la période définie. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
|NULL (par défaut)||  
  
 [  **@frequency_subday_interval =** ] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_start_time_of_day=**] *active_start_time_of_day*  
 Heure de la journée à laquelle l’Agent de fusion est la première planifié, au format HHMMSS. *active_start_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_time_of_day =** ] *active_end_time_of_day*  
 Heure à laquelle l’Agent de fusion cesse d'être planifié, au format HHMMSS. *active_end_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [ **@active_start_date =** ] *active_start_date*  
 Est la date à laquelle l’Agent de fusion est premier planifiée, au format AAAAMMJJ. *active_start_date* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_date =** ] *active_end_date*  
 Date à laquelle l’Agent de fusion cesse d’être planifié, représentée au format AAAAMMJJ. *active_end_date* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@optional_command_line =** ] **'***optional_command_line***'**  
 Invite de commandes facultative fournie par l'Agent de distribution. *optional_command_line* est **nvarchar (255)**, avec une valeur par défaut ' '. Permet de fournir des paramètres supplémentaires à l'Agent de fusion, comme dans l'exemple suivant où le délai d'expiration par défaut de la requête est augmenté jusqu'à `600` secondes :  
  
```  
@optional_command_line = N'-QueryTimeOut 600'  
```  
  
 [  **@merge_jobid =** ] *merge_jobid*  
 Paramètre de sortie pour l'ID de travail. *id_travail_de_fusion* est **Binary (16)**, avec NULL comme valeur par défaut.  
  
 [  **@enabled_for_syncmgr =** ] **'***l’argument enabled_for_syncmgr***'**  
 Spécifie si l'abonnement peut être synchronisé à l'aide du Gestionnaire de synchronisation Windows. *l’argument enabled_for_syncmgr* est **nvarchar (5)**, avec FALSE comme valeur par défaut. Si **false**, l’abonnement n’est pas inscrit avec le Gestionnaire de synchronisation. Si **true**, l’abonnement est enregistré avec le Gestionnaire de synchronisation et peut être synchronisé sans démarrer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 [  **@ftp_address =** ] **'***ftp_address***'**  
 Pour compatibilité descendante uniquement.  
  
 [  **@ftp_port =** ] *ftp_port*  
 Pour compatibilité descendante uniquement.  
  
 [  **@ftp_login =** ] **'***ftp_login***'**  
 Pour compatibilité descendante uniquement.  
  
 [  **@ftp_password =** ] **'***ftp_password***'**  
 Pour compatibilité descendante uniquement.  
  
 [  **@alt_snapshot_folder =** ] **'***alternate_snapshot_folder***'**  
 Spécifie l'emplacement à partir duquel récupérer les fichiers d'instantané. *alternate_snapshot_folder* est **nvarchar (255)**, avec NULL comme valeur par défaut. Si la valeur est NULL, les fichiers d'instantané sont extraits à partir de l'emplacement par défaut spécifié par le serveur de publication.  
  
 [  **@working_directory =** ] **'***working_directory***'**  
 Nom du répertoire de travail utilisé pour stocker temporairement les fichiers de données et de schémas de la publication lorsque le protocole FTP est utilisé pour transférer les fichiers d'instantané. *working_directory* est **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
 [  **@use_ftp =** ] **'***use_ftp***'**  
 Spécifie que le protocole FTP est utilisé à la place du protocole normal pour l'extraction des instantanés. *use_ftp* est **nvarchar (5)**, avec FALSE comme valeur par défaut.  
  
 [  **@reserved =** ] **'***réservé***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [  **@use_interactive_resolver =** ] **'***use_interactive_resolver***'** ]  
 Utilise la résolution interactive des conflits pour tous les articles autorisant la résolution interactive. *use_interactive_resolver* est **nvarchar (5)**, avec FALSE comme valeur par défaut.  
  
 [  **@offloadagent =** ] **'***remote_agent_activation***'**  
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Paramètre *remote_agent_activation* à une valeur autre que **false** génère une erreur.  
  
 [  **@offloadserver =** ] **'***remote_agent_server_name***'**  
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Paramètre *remote_agent_server_name* à n’importe quelle valeur non NULL génère une erreur.  
  
 [  **@job_name =** ] **'***job_name***'** ]  
 Nom d'un travail de l'agent existant. *job_name* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est indiqué que lorsque l'abonnement est synchronisé grâce à un travail existant plutôt qu'un nouveau travail (étant le comportement par défaut). Si vous n’êtes pas un membre de la **sysadmin** rôle serveur fixe, vous devez spécifier *job_login* et *job_password* lorsque vous spécifiez *job_name*.  
  
 [  **@dynamic_snapshot_location =** ] **'***dynamic_snapshot_location***'** ]  
 Le chemin d’accès au dossier dans lequel les fichiers d’instantanés seront lues dans if un instantané de données filtrées doit être utilisé. *dynamic_snapshot_location* est **nvarchar (260)**, avec NULL comme valeur par défaut. Pour plus d'informations, consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 [  **@use_web_sync =** ] *use_web_sync*  
 Indique que la synchronisation Web est activée. *use_web_sync* est **bits**, avec 0 comme valeur par défaut. **1** Spécifie que l’abonnement par extraction de données peut être synchronisé via internet à l’aide de HTTP.  
  
 [  **@internet_url =** ] **'***internet_url***'**  
 Emplacement qui représente l'écouteur de réplication (REPLISAPI.DLL) de la synchronisation Web. *internet_url* est **nvarchar (260)**, avec NULL comme valeur par défaut. *internet_url* est une URL qualifiée complète, au format `http://server.domain.com/directory/replisapi.dll`. Si le serveur est configuré de manière à être à l'écoute sur un port autre que le port 80, le numéro de port doit également être fourni sous la forme `http://server.domain.com:portnumber/directory/replisapi.dll`, où `portnumber` représente le port.  
  
 [  **@internet_login =** ] **'***internet_login***'**  
 Nom de connexion que l'Agent de fusion utilise pour se connecter, à l'aide de l'authentification de base HTTP, au serveur Web qui héberge la synchronisation Web. *internet_login* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@internet_password =** ] **'***internet_password***'**  
 Mot de passe qu'utilise l'Agent de fusion lors de la connexion au serveur Web qui héberge la synchronisation Web avec l'authentification de base HTTP. *internet_password* est **nvarchar (524)**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
 [  **@internet_security_mode =** ] *internet_security_mode*  
 Méthode d'authentification utilisée par l'Agent de fusion pour se connecter au serveur Web pendant la synchronisation Web à l'aide du protocole HTTPS. *internet_security_mode* est **int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|L'authentification de base est utilisée.|  
|**1** (par défaut)|L'authentification intégrée de Windows est utilisée.|  
  
> [!NOTE]  
>  Il est recommandé d'utiliser l'authentification de base pour la synchronisation Web. Pour utiliser la synchronisation Web, vous devez établir une connexion SSL au serveur Web. Pour plus d’informations, consultez [Configurer la synchronisation Web](../../relational-databases/replication/configure-web-synchronization.md).  
  
 [  **@internet_timeout =** ] *internet_timeout*  
 Est la durée, en secondes, avant l’expiration de la demande de synchronisation Web. *internet_timeout* est **int**, avec une valeur par défaut **300** secondes.  
  
 [  **@hostname =** ] **'***nom d’hôte***'**  
 Remplace la valeur de HOST_NAME() lorsque cette fonction est utilisée dans la clause WHERE d'un filtre paramétré. *Nom d’hôte* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@job_login =** ] **'***job_login***'**  
 Nom de connexion du compte Windows sous lequel l'Agent s'exécute. *job_login* est **nvarchar (257)**, sans valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions d'Agent à l'Abonné et pour les connexions au serveur de distribution et au serveur de publication lors de l'utilisation de l'authentification intégrée de Windows.  
  
 [  **@job_password =** ] **'***job_password***'**  
 Mot de passe du compte Windows sous lequel l'Agent s'exécute. *job_password* est **sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour une sécurité optimale, les noms de connexion et les mots de passe doivent être fournis au moment de l'exécution.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergepullsubscription_agent** est utilisée dans la réplication de fusion et utilise des fonctionnalités similaires à celles [sp_addpullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md).  
  
 Pour obtenir un exemple montrant comment spécifier correctement les paramètres de sécurité lors de l’exécution **sp_addmergepullsubscription_agent**, consultez [créer un abonnement par extraction](../../relational-databases/replication/create-a-pull-subscription.md).  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_1_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_addmergepullsubscription_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un abonnement par extraction](../../relational-databases/replication/create-a-pull-subscription.md)   
 [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
