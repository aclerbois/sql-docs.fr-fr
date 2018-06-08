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
- sp_addpushsubscription_agent_TSQL
- sp_addpushsubscription_agent
helpviewer_keywords:
- sp_addpushsubscription_agent
ms.assetid: 1fdd2052-50d8-4318-8aa7-fc635d5cad18
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d111bef33c73d2417ddccf88c5bb083be6c2a35f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddpushsubscriptionagent-transact-sql"></a>sp_addpushsubscription_agent (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un nouveau travail de l'agent planifié utilisé pour synchroniser un abonnement envoyé avec une publication transactionnelle. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addpushsubscription_agent [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @job_name = ] 'job_name' ]   
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
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @distribution_job_name = ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @subscriber_provider = ] 'subscriber_provider' ]   
    [ , [ @subscriber_datasrc = ] 'subscriber_datasrc' ]   
    [ , [ @subscriber_location = ] 'subscriber_location' ]  
    [ , [ @subscriber_provider_string = ] 'subscriber_provider_string' ]   
    [ , [ @subscriber_catalog = ] 'subscriber_catalog' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@subscriber =**] **'***abonné***'**  
 Nom de l'Abonné. *abonné* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@subscriber_db =**] **'***bd_abonné***'**  
 Est le nom de la base de données d’abonnement. *bd_abonné* est **sysname**, avec NULL comme valeur par défaut. Pour un abonné non SQL Server, spécifiez la valeur **(destination par défaut)** pour *bd_abonné*.  
  
 [  **@subscriber_security_mode =**] *subscriber_security_mode*  
 Mode de sécurité à utiliser lors de la connexion à un abonné au cours d'une synchronisation. *subscriber_security_mode* est **int**, avec 1 comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** Spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  Pour les abonnements mis à jour en attente, utilisez l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les connexions aux abonnés et spécifiez un compte différent pour la connexion à chaque abonné. Pour tous les autres abonnements, utilisez l'authentification Windows.  
  
 [  **@subscriber_login =**] **'***subscriber_login***'**  
 Nom de connexion de l'abonné à utiliser lors de la connexion à un abonné au cours d'une synchronisation. *subscriber_login* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@subscriber_password =**] **'***subscriber_password***'**  
 Mot de passe de l'Abonné. *subscriber_password* est requise si *subscriber_security_mode* a la valeur **0**. *subscriber_password* est **sysname**, avec NULL comme valeur par défaut. Si un mot de passe d'abonné est utilisé, il est automatiquement chiffré.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@job_login =** ] **'***job_login***'**  
 Nom de connexion du compte Windows sous lequel l'Agent s'exécute. *job_login* est **nvarchar (257)**, avec NULL comme valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions de l'Agent au serveur de distribution et pour les connexions à l'abonné utilisant l'authentification intégrée de Windows.  
  
 [  **@job_password =** ] **'***job_password***'**  
 Mot de passe du compte Windows sous lequel l'Agent s'exécute. *job_password* est **sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
 [  **@job_name =** ] **'***job_name***'**  
 Nom d'un travail de l'agent existant. *job_name* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est indiqué que lorsque l'abonnement est synchronisé grâce à un travail existant plutôt qu'un nouveau travail (étant le comportement par défaut). Si vous n’êtes pas un membre de la **sysadmin** rôle serveur fixe, vous devez spécifier *job_login* et *job_password* lorsque vous spécifiez *job_name*.  
  
 [  **@frequency_type =** ] *frequency_type*  
 Fréquence de planification de l'Agent de distribution. *frequency_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|À la demande|  
|**4**|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Mensuelle relative|  
|**64** (par défaut)|Démarrage automatique|  
|**128**|Périodique|  
  
> [!NOTE]  
>  La valeur de **64** provoque l’Agent de Distribution s’exécute en mode continu. Cela correspond au paramètre la **-continue** paramètre pour l’agent. Pour plus d'informations, consultez [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
 [  **@frequency_interval =** ] *frequency_interval*  
 Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est **int**, avec 1 comme valeur par défaut.  
  
 [  **@frequency_relative_interval =** ] *frequency_relative_interval*  
 Date de l'Agent de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuel relatif). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**1** (par défaut)|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
  
 [  **@frequency_recurrence_factor =** ] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est **int**, avec 0 comme valeur par défaut.  
  
 [  **@frequency_subday =** ] *frequency_subday*  
 Fréquence de replanification nécessaire pendant la période définie. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4** (par défaut)|Minute|  
|**8**|Heure|  
  
 [  **@frequency_subday_interval =** ] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est **int**, avec 5 comme valeur par défaut.  
  
 [  **@active_start_time_of_day =** ] *active_start_time_of_day*  
 Heure à laquelle l’Agent de distribution est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est **int**, avec 0 comme valeur par défaut.  
  
 [  **@active_end_time_of_day =** ] *active_end_time_of_day*  
 L’heure de la journée à laquelle l’Agent de Distribution cesse d’être planifié, représentée au format HHMMSS. *active_end_time_of_day* est **int**, avec 235959 comme valeur par défaut.  
  
 [ **@active_start_date =** ] *active_start_date*  
 Date à laquelle l’Agent de distribution est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est **int**, avec 0 comme valeur par défaut.  
  
 [  **@active_end_date =** ] *active_end_date*  
 Date à laquelle l’Agent de distribution cesse d'être planifié, au format AAAAMMJJ. *active_end_date* est **int**, avec 99991231 comme valeur par défaut.  
  
 [  **@dts_package_name =** ] **'***l’argument dts_package_name***'**  
 Spécifie le nom du package DTS (Data Transformation Services). *l’argument dts_package_name* est un **sysname** avec NULL comme valeur par défaut. Par exemple, pour spécifier le nom de package `DTSPub_Package`, le paramètre est le suivant : `@dts_package_name = N'DTSPub_Package'`.  
  
 [  **@dts_package_password =** ] **'***dts_package_password***'**  
 Spécifie le mot de passe requis pour exécuter le package. *dts_package_password* est **sysname** avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Vous devez spécifier un mot de passe si *l’argument dts_package_name* est spécifié.  
  
 [  **@dts_package_location =** ] **'***dts_package_location***'**  
 Spécifie l'emplacement du package. *dts_package_location* est un **nvarchar(12)**, avec une valeur par défaut du serveur de distribution. L’emplacement du package peut être **distributeur** ou **abonné**.  
  
 [  **@enabled_for_syncmgr =** ] **'***l’argument enabled_for_syncmgr***'**  
 Indique si l’abonnement peut être synchronisé via [!INCLUDE[msCoName](../../includes/msconame-md.md)] le Gestionnaire de synchronisation. *l’argument enabled_for_syncmgr* est **nvarchar (5)**, avec FALSE comme valeur par défaut. Si **false**, l’abonnement n’est pas inscrit avec le Gestionnaire de synchronisation. Si **true**, l’abonnement est enregistré avec le Gestionnaire de synchronisation et peut être synchronisé sans démarrer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 [  **@distribution_job_name =** ] **'***distribution_job_name***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [  **@publisher =** ] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@subscriber_provider=** ] **'***subscriber_provider***'**  
 Est l’identificateur de programme unique (PROGID) avec lequel le fournisseur OLE DB pour le[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données est enregistrée. *subscriber_provider* est **sysname**, avec NULL comme valeur par défaut. *subscriber_provider* doit être unique pour le fournisseur OLE DB installé sur le serveur de distribution. *subscriber_provider* est uniquement prise en charge non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.  
  
 [  **@subscriber_datasrc=** ] **'***subscriber_datasrc***'**  
 Est le nom de la source de données comme interprété par le fournisseur OLE DB. *subscriber_datasrc* est **nvarchar (4000)**, avec NULL comme valeur par défaut. *subscriber_datasrc* est transmis comme propriété DBPROP_INIT_DATASOURCE pour initialiser le fournisseur OLE DB. *subscriber_datasrc* est uniquement prise en charge non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.  
  
 [  **@subscriber_location=** ] **'***subscriber_location***'**  
 Est l’emplacement de la base de données comme interprété par le fournisseur OLE DB. *subscriber_location* est **nvarchar (4000)**, avec NULL comme valeur par défaut. *subscriber_location* est transmis comme propriété DBPROP_INIT_LOCATION pour initialiser le fournisseur OLE DB. *subscriber_location* est uniquement prise en charge non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.  
  
 [  **@subscriber_provider_string=** ] **'***subscriber_provider_string***'**  
 Chaîne de connexion propre au fournisseur OLE DB qui identifie la source de données. *subscriber_provider_string* est **nvarchar (4000)**, avec NULL comme valeur par défaut. *subscriber_provider_string* est passé à IDataInitialize ou défini comme propriété DBPROP_INIT_PROVIDERSTRING pour initialiser le fournisseur OLE DB. *subscriber_provider_string* est uniquement prise en charge non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.  
  
 [  **@subscriber_catalog=** ] **'***subscriber_catalog***'**  
 Catalogue à utiliser lors d’une connexion au fournisseur OLE DB. *subscriber_catalog* est **sysname**, avec NULL comme valeur par défaut. *subscriber_catalog* est transmis comme propriété DBPROP_INIT_CATALOG pour initialiser le fournisseur OLE DB. *subscriber_catalog* est uniquement prise en charge non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addpullsubscription_agent** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addpushsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_addpushsubscription_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Créer un abonnement pour un abonné non-SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
