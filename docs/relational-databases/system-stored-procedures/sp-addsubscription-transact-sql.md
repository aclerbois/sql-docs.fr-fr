---
title: sp_addsubscription (Transact-SQL) | Documents Microsoft
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_addsubscription
- sp_addsubscription_TSQL
helpviewer_keywords:
- sp_addsubscription
ms.assetid: 61ddf287-1fa0-4c1a-8657-ced50cebf0e0
caps.latest.revision: 53
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 08f0e46bde340eb1b64f8c7ad9ba2d1f8ec63d9f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddsubscription-transact-sql"></a>sp_addsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un abonnement à une publication et définit l'état de l'abonné. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addsubscription [ @publication = ] 'publication'  
    [ , [ @article = ] 'article']  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
        [ , [ @sync_type = ] 'sync_type' ]  
    [ , [ @status = ] 'status'  
        [ , [ @subscription_type = ] 'subscription_type' ]  
    [ , [ @update_mode = ] 'update_mode' ]  
    [ , [ @loopback_detection = ] 'loopback_detection' ]  
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
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @enabled_for_syncmgr= ] 'enabled_for_syncmgr' ]  
    [ , [ @offloadagent= ] remote_agent_activation]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] 'dts_package_location' ]  
    [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @backupdevicetype = ] 'backupdevicetype' ]  
    [ , [ @backupdevicename = ] 'backupdevicename' ]  
    [ , [ @mediapassword = ] 'mediapassword' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @fileidhint = ] fileidhint ]  
    [ , [ @unload = ] unload ]  
    [ , [ @subscriptionlsn = ] subscriptionlsn ]  
    [ , [ @subscriptionstreams = ] subscriptionstreams ]  
    [ , [ @subscriber_type = ] subscriber_type ]  
    [ , [ @memory_optimized = ] memory_optimized ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @publication=] '*publication*'  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [ @article=] '*article*'  
 Nom de l'article auquel la publication est abonnée. *article* est **sysname**, avec une valeur par défaut de tous les. Si la valeur définie est all (tous), l'abonnement s'ajoute à tous les articles de la publication. Seules les valeurs all ou NULL sont prises en charge par les serveurs de publication Oracle.  
  
 [ @subscriber=] '*abonné*'  
 Nom de l'Abonné. *abonné* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ @destination_db=] '*destination_db*'  
 Nom de la base de données de destination dans laquelle les données répliquées seront placées. *destination_db* est **sysname**, avec NULL comme valeur par défaut. Lorsqu’elle est NULL, *destination_db* est définie sur le nom de la base de données de publication. Pour les serveurs de publication Oracle, *destination_db* doit être spécifié. Pour un abonné non SQL Server, spécifiez une valeur (destination par défaut) pour *destination_db*.  
  
 [ @sync_type=] '*sync_type*'  
 Type de synchronisation d'abonnement. *sync_type* est **nvarchar (255)**, et peut prendre l’une des valeurs suivantes :  
  
|Valeur| Description|  
|-----------|-----------------|  
|aucun|L'abonnement dispose déjà du schéma et des données initiales destinées aux tables publiées.<br /><br /> Remarque : Cette option est déconseillée. Utilisez plutôt la prise en charge de la réplication uniquement.|  
|automatic (valeur par défaut)|Le schéma et les données initiales des tables publiées sont transférés en premier lieu vers l'Abonné.|  
|replication support only|Fournit une génération automatique au niveau de l'Abonné des procédures stockées personnalisées de l'article et des déclencheurs qui prennent en charge les abonnements de mise à jour, le cas échéant. Considère que l'Abonné dispose déjà du schéma et des données initiales pour les tables publiées. Lors de la configuration d'une topologie de réplication transactionnelle d'égal à égal, veillez à ce que les données de tous les nœuds de la topologie soient identiques. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).<br /><br /> *Non pris en charge pour les abonnements aux publications non-SQL Server.*|  
|initialize with backup|Le schéma et les données initiales destinées aux tables publiées proviennent d'une sauvegarde de la base de données de publication. L'abonné est censé avoir accès à une sauvegarde de la base de données de publication. L’emplacement du type de sauvegarde et de support pour la sauvegarde sont spécifiés par *backupdevicename* et *backupdevicetype*. Lors de l'utilisation de cette option, il n'est pas nécessaire de suspendre la topologie de réplication transactionnelle d'égal à égal pendant la configuration.<br /><br /> *Non pris en charge pour les abonnements aux publications non-SQL Server.*|  
|initialize from lsn|Utilisé lorsque vous ajoutez un nœud à une topologie de réplication transactionnelle d'égal à égal. Utilisé avec la propriété @subscriptionlsn pour vérifier que toutes les transactions appropriées sont répliquées sur le nouveau nœud. Considère que l'Abonné dispose déjà du schéma et des données initiales pour les tables publiées. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).|  
  
> [!NOTE]  
>  Les données et les tables système sont toujours transférées.  
  
 [ @status=] '*état*'  
 État de l'abonnement. *état* est **sysname**, avec NULL comme valeur par défaut. Lorsque ce paramètre n'est pas défini explicitement, la réplication lui donne automatiquement l'une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|active|L'abonnement est initialisé et prêt à accepter des modifications. Cette option est définie lorsque la valeur de *sync_type* est none, initialize with backup ou prise en charge de la réplication uniquement.|  
|subscribed|L'abonnement doit être initialisé. Cette option est définie lorsque la valeur de *sync_type* est automatique.|  
  
 [ @subscription_type=] '*subscription_type*'  
 Est le type d’abonnement. *subscription_type* est **nvarchar (4)**, avec push comme valeur par défaut. Peut avoir la valeur push ou pull (émission ou extraction de données). Les Agents de Distribution des abonnements envoyés et résider sur le serveur de distribution et les Agents de Distribution des abonnements extraits sur l’abonné. *subscription_type* peut être extrait pour créer un abonnement par extraction nommé connu du serveur de publication. Pour plus d’informations, consultez [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md).  
  
> [!NOTE]  
>  Les abonnements anonymes ne doivent pas utiliser cette procédure stockée.  
  
 [ @update_mode=] '*update_mode*'  
 Est le type de mise à jour. *update_mode* est **nvarchar (30)**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|read only (valeur par défaut)|L'abonnement est en lecture seule. Les modifications effectuées chez l'abonné ne sont pas renvoyées au serveur de publication.|  
|sync tran|Active la prise en charge des abonnements de mise à jour immédiate. Non pris en charge pour les serveurs de publication Oracle.|  
|queued tran|Active l'abonnement pour la mise à jour en attente. Les modifications de données peuvent être effectuées chez l'abonné, stockées dans une file d'attente, puis propagées vers le serveur de publication. Non pris en charge pour les serveurs de publication Oracle.|  
|failover|Active l'abonnement pour la mise à jour immédiate avec mise à jour en attente sous forme de basculement. Les modifications de données peuvent être effectuées chez l'abonné, puis propagées immédiatement vers le serveur de publication. Si le serveur de publication et l'abonné ne sont pas connectés, il est possible de changer de mode de mise à jour afin que les modifications de données effectuées chez l'abonné soient stockées dans une file d'attente jusqu'à ce que l'abonné et le serveur de publication soient reconnectés. Non pris en charge pour les serveurs de publication Oracle.|  
|queued failover|Active l'abonnement en tant qu'abonnement de mise à jour en attente, avec possibilité de passer au mode de mise à jour immédiate. Les modifications de données peuvent être effectuées chez l'abonné et stockées dans une file d'attente, jusqu'à ce qu'une connexion soit établie entre l'abonné et le serveur de publication. Lorsqu'une connexion permanente est établie, il est possible de passer au mode de mise à jour immédiate. Non pris en charge pour les serveurs de publication Oracle.|  
  
 Notez que les valeurs synctran et queued tran ne sont pas autorisées si l’objet d’un abonnement de publication autorisant le DTS.  
  
 [ @loopback_detection=] '*détection_de_boucle*'  
 Indique si l'Agent de distribution envoie des transactions à un abonné qui en est l'auteur. *l’argument détection_de_boucle* est **nvarchar (5)**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|true|L'Agent de distribution n'envoie pas à l'abonné ses propres transactions. Utilisé avec la réplication transactionnelle bidirectionnelle. Pour plus d'informations, voir [Réplication transactionnelle bidirectionnelle](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|false|L'Agent de distribution renvoie à l'abonné ses propres transactions.|  
|NULL (par défaut)|Prend automatiquement la valeur true pour un Abonné [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et false pour un Abonné non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 [ @frequency_type=] *frequency_type*  
 Fréquence de planification de la tâche de distribution. *frequency_type* est de type int et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|1|Une fois|  
|2|À la demande|  
|4|Tous les jours|  
|8|Semaine|  
|16|Mois|  
|32|Mensuelle relative|  
|64 (valeur par défaut)|Démarrage automatique|  
|128|Périodique|  
  
 [ @frequency_interval=] *frequency_interval*  
 Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est **int**, avec NULL comme valeur par défaut.  
  
 [ @frequency_relative_interval=] *frequency_relative_interval*  
 Date de l'Agent de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur 32 (fréquence mensuelle relative). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|1|Première|  
|2|Seconde|  
|4|Troisième|  
|8|Quatrième|  
|16|Dernière|  
|NULL (par défaut)||  
  
 [ @frequency_recurrence_factor=] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est **int**, avec NULL comme valeur par défaut.  
  
 [ @frequency_subday=] *frequency_subday*  
 Indique, en minutes, la fréquence de replanification pendant la période définie. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|1|Une fois|  
|2|Seconde|  
|4|Minute|  
|8|Heure|  
|NULL||  
  
 [ @frequency_subday_interval=] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est **int**, avec NULL comme valeur par défaut.  
  
 [ @active_start_time_of_day=] *active_start_time_of_day*  
 Heure à laquelle l’Agent de distribution est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [ @active_end_time_of_day=] *active_end_time_of_day*  
 L’heure de la journée à laquelle l’Agent de Distribution cesse d’être planifié, représentée au format HHMMSS. *active_end_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [ @active_start_date=] *active_start_date*  
 Date à laquelle l’Agent de distribution est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est **int**, avec NULL comme valeur par défaut.  
  
 [ @active_end_date=] *active_end_date*  
 Date à laquelle l’Agent de distribution cesse d'être planifié, au format AAAAMMJJ. *active_end_date* est **int**, avec NULL comme valeur par défaut.  
  
 [ @optional_command_line=] '*optional_command_line*'  
 Invite de commandes facultative à exécuter. *optional_command_line* est **nvarchar (4000)**, avec NULL comme valeur par défaut.  
  
 [ @reserved=] '*réservé*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @enabled_for_syncmgr=] '*l’argument enabled_for_syncmgr*'  
 Indique si l’abonnement peut être synchronisé via [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestionnaire de synchronisation Windows. *l’argument enabled_for_syncmgr* est **nvarchar (5)**, avec FALSE comme valeur par défaut. Si la valeur est false, l'abonnement n'est pas enregistré par le Gestionnaire de synchronisation Windows. Si la valeur est true, l'abonnement est enregistré par le Gestionnaire de synchronisation Windows et il peut ensuite être synchronisé, sans qu'il soit nécessaire de démarrer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Non pris en charge pour les serveurs de publication Oracle.  
  
 [ @offloadagent= ] '*remote_agent_activation*'  
 Indique si l'Agent peut être activé à distance. *remote_agent_activation* est **bits** avec une valeur par défaut 0.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est conservé que pour la compatibilité descendante des scripts.  
  
 [ @offloadserver=] '*remote_agent_server_name*'  
 Indique le nom réseau du serveur à utiliser pour l'activation à distance. *remote_agent_server_name*est **sysname**, avec NULL comme valeur par défaut.  
  
 [ @dts_package_name=] '*l’argument dts_package_name*'  
 Spécifie le nom du package DTS (Data Transformation Services). *l’argument dts_package_name* est un **sysname** avec NULL comme valeur par défaut. Par exemple, pour spécifier un package de DTSPub_Package, le paramètre est : `@dts_package_name = N'DTSPub_Package'`. Ce paramètre est disponible avec les abonnements envoyés. Pour ajouter des informations de package DTS à un abonnement extrait, utilisez sp_addpullsubscription_agent.  
  
 [ @dts_package_password=] '*dts_package_password*'  
 Spécifie le mot de passe du package, s'il existe. *dts_package_password* est **sysname** avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Vous devez spécifier un mot de passe si *l’argument dts_package_name* est spécifié.  
  
 [ @dts_package_location=] '*dts_package_location*'  
 Spécifie l'emplacement du package. *dts_package_location* est un **nvarchar(12)**, avec une valeur par défaut du serveur de distribution. L'emplacement du package peut prendre la valeur distributor ou subscriber.  
  
 [ @distribution_job_name=] '*distribution_job_name*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @publisher=] '*publisher*'  
 Spécifie un non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être spécifié pour un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
 [ @backupdevicetype=] '*backupdevicetype*'  
 Indique le type d'unité de sauvegarde utilisé lors de l'initialisation d'un Abonné à partir d'une sauvegarde. *backupdevicetype* est **nvarchar (20)**, et peut prendre l’une des valeurs suivantes :  
  
|Valeur| Description|  
|-----------|-----------------|  
|logical (valeur par défaut)|L'unité de sauvegarde est une unité logique.|  
|disk|L'unité de sauvegarde est un lecteur de disque.|  
|tape|L'unité de sauvegarde est un lecteur de bande.|  
  
 *backupdevicetype* est uniquement utilisé lorsque *sync_method*a pour valeur initialize_with_backup.  
  
 [ @backupdevicename=] '*backupdevicename*'  
 Indique le nom de l'unité utilisée lors de l'initialisation d'un Abonné à partir d'une sauvegarde. *backupdevicename* est **nvarchar (1000)**, avec NULL comme valeur par défaut.  
  
 [ @mediapassword=] '*mediapassword*'  
 Indique un mot de passe pour le support spécifié, si un mot de passe a été défini lors du formatage du support. *MEDIAPASSWORD* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 [ @password=] '*mot de passe*'  
 Indique un mot de passe pour la sauvegarde, si un mot de passe a été défini lors de la création de celle-ci. *mot de passe*est **sysname**, avec NULL comme valeur par défaut.  
  
 [ @fileidhint=] *fileidhint*  
 Identifie une valeur ordinale du jeu de sauvegarde à restaurer. *fileidhint* est **int**, avec NULL comme valeur par défaut.  
  
 [ @unload=] *décharger*  
 Indique si une unité de sauvegarde sur bande doit être déchargée une fois l'initialisation de la sauvegarde terminée. *décharger* est **bits**, avec une valeur par défaut de 1. 1 indique que la bande doit être déchargée. *décharger* est uniquement utilisé lorsque *backupdevicetype* est une bande.  
  
 [ @subscriptionlsn=] *subscriptionlsn*  
 Spécifie le numéro séquentiel dans le journal auquel un abonnement doit commencer à remettre des modifications à un nœud dans une topologie de réplication transactionnelle d'égal à égal. Utilisé avec un @sync_type valeur Initialize from lsn pour vous assurer que toutes les transactions appropriées sont répliquées vers un nouveau nœud. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 [ @subscriptionstreams=] *subscriptionstreams*  
 Nombre de connexions autorisées par l'Agent de distribution afin d'appliquer des lots de modifications en parallèle à un Abonné, tout en conservant bon nombre des caractéristiques transactionnelles présentes lors de l'utilisation d'un thread unique. *subscriptionstreams* est **tinyint**, avec NULL comme valeur par défaut. Une plage de valeurs allant de 1 à 64 est prise en charge. Ce paramètre n'est pas pris en charge pour les abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les serveurs de publication Oracle ou les abonnements d'égal à égal. Chaque fois que des flux d'abonnements sont utilisés, des lignes supplémentaires sont ajoutées au tableau msreplication_subscriptions (1 par flux) avec agent_id défini à NULL.  
  
> [!NOTE]  
>  Les flux d'abonnements ne fonctionnent pas pour les articles configurés pour fournir [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour utiliser les flux d'abonnements, configurez les articles afin qu'ils fournissent des appels de procédures stockées à la place.  
  
 [ @subscriber_type=] *subscriber_type*  
 Type d'abonné. *subscriber_type* est **tinyint**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|0 (valeur par défaut)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Abonné|  
|1|Serveur de la source de données ODBC.|  
|2|Base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet|  
|3|Fournisseur OLE DB|  
  
 [ @memory_optimized=] *memory_optimized*  
 Indique que l’abonnement prend en charge les tables optimisées en mémoire. *memory_optimized* est **bits**, où 1 est égale à true (l’abonnement prend en charge les tables optimisées en mémoire).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 sp_addsubscription est utilisé lors des réplications d'instantané et transactionnelle.  
  
 Lorsque sp_addsubscription est exécuté par un membre du rôle serveur fixe sysadmin pour créer un abonnement envoyé, la tâche de l'Agent de distribution est implicitement créée et s'exécute sous le compte du service Agent SQL Server. Nous vous recommandons d’exécuter [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md) et spécifiez les informations d’identification d’un compte Windows différent, spécifique à l’agent pour @job_login et @job_password. Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 sp_addsubscription empêche les abonnés ODBC et OLE DB d'accéder aux publications qui :  
  
-   Ont été créées avec natif *sync_method* dans l’appel à [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
-   Contiennent des articles qui ont été ajoutés à la publication avec le [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) procédure stockée qui avait une *pre_creation_cmd* valeur du paramètre 3 (tronquer).  
  
-   Tentative de définition *update_mode* valeur sync tran.  
  
-   ont un article configuré pour utiliser des instructions paramétrables.  
  
 En outre, si une publication a la *allow_queued_tran* option définie sur true (ce qui permet la file d’attente des modifications sur l’abonné jusqu'à ce qu’ils peuvent être appliquées sur le serveur de publication), la colonne timestamp d’un article est générée en tant que **timestamp**, et les modifications sur cette colonne sont envoyées à l’abonné. L'abonné génère et met à jour la valeur de la colonne timestamp. Pour un abonné ODBC ou OLE DB, sp_addsubscription échoue si une tentative est faite pour vous abonner à une publication qui a *allow_queued_tran* définie sur true et articles possèdent des colonnes timestamp.  
  
 Si un abonnement n’utilise pas un package DTS, il ne peut pas s’abonner à une publication qui a la valeur *allow_transformable_subscriptions*. Si la table issue de la publication doit être répliquée vers un abonnement DTS et un abonnement non-DTS, deux publications indépendantes doivent être créées : une pour chaque type d'abonnement.  
  
 Lors de la sélection des options **sync_type** , *replication support only*, *initialize with backup*ou *initialize from lsn*, l'Agent de lecture du journal doit s'exécuter après l'exécution de **sp_addsubscription**, afin que les scripts d'installation soient écrits dans la base de données de distribution. L'Agent de lecture du journal doit s'exécuter sous un compte membre du rôle serveur fixe **sysadmin** . Lorsque l'option **sync_type** a la valeur *Automatic*, aucune action particulière de l'Agent de lecture du journal n'est requise.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe sysadmin ou du rôle de base de données fixe db_owner peuvent exécuter sp_addsubscription. Pour les abonnements par extraction de données (pull), les utilisateurs ayant une connexion à la liste d'accès aux publications peuvent exécuter sp_addsubscription.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addsubscription-trans_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Créer un abonnement pour un abonné non-SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
