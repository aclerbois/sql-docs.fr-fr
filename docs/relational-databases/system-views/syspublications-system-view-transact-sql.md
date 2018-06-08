---
title: SYSPUBLICATIONS (vue système) (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- syspublications
- syspublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspublications view
ms.assetid: e5f57c32-efc0-4455-a74f-684dc2ae51f8
caps.latest.revision: 20
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 65634b15e901b260b4d0498aed9c1b2759170686
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="syspublications-system-view-transact-sql"></a>syspublications (vue système) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **syspublications** affichage expose les informations de publication. Cette vue est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**description**|**nvarchar(255)**|L’entrée descriptive pour la publication.|  
|**nom**|**sysname**|Nom unique associé à la publication.|  
|**pubid**|**int**|Colonne d'identité fournissant un ID unique pour la publication.|  
|**repl_freq**|**tinyint**|Fréquence de réplication :<br /><br /> **0** = en fonction des transactions (transactionnelle).<br /><br /> **1** = actualisation planifiée des tables (instantané).|  
|**status**|**tinyint**|État de la publication :<br /><br /> **0** = inactif.<br /><br /> **1** = actif.|  
|**sync_method**|**tinyint**|Méthode de synchronisation :<br /><br /> **0** = utilitaire de programme de copie en bloc natif (BCP).<br /><br /> **1** = caractère BCP.<br /><br /> **3** = simultané, ce qui signifie que natif BCP est utilisé mais les tables ne sont pas verrouillées lors de l’instantané.<br /><br /> **4** = Concurrent_c, ce qui signifie que les caractères BCP est utilisé mais les tables ne sont pas verrouillées lors de l’instantané.|  
|**snapshot_jobid**|**binary (16)**|Identifie le travail d'agent planifié pour créer l'instantané initial.|  
|**independent_agent**|**bit**|Spécifie s’il existe un Agent de Distribution autonome pour cette publication.<br /><br /> **0** = la publication utilise un Agent de Distribution partagé, et chaque paire de base de données de serveur de publication/abonné de base de données a un seul Agent partagé.<br /><br /> **1** = il existe un Agent de Distribution autonome pour cette publication.|  
|**immediate_sync**|**bit**|Indique si les fichiers de synchronisation sont créés ou recréés chaque fois que l’Agent d’instantané s’exécute, où **1** signifie qu’ils sont créés chaque fois que l’agent s’exécute.|  
|**enabled_for_internet**|**bit**|Indique si les fichiers de synchronisation pour la publication sont exposés à Internet via le protocole de transfert de fichiers (FTP) et d’autres services, où **1** signifie qu’ils sont accessibles à partir d’Internet.|  
|**allow_push**|**bit**|Indique si des abonnements envoyés sont autorisés pour la publication, où **1** signifie qu’ils sont autorisés.|  
|**allow_pull**|**bit**|Indique si les abonnements extraits sont autorisés pour la publication, où **1** signifie qu’ils sont autorisés.|  
|**allow_anonymous**|**bit**|Indique si les abonnements anonymes sont autorisés pour la publication, où **1** signifie qu’ils sont autorisés.|  
|**immediate_sync_ready**|**bit**|Indique si l'instantané a été généré par l'Agent d'instantané et peut être utilisé par les nouveaux abonnements. Cette colonne n'est pertinente que pour les publications à mise à jour immédiate. **1** indique que l’instantané est prêt.|  
|**allow_sync_tran**|**bit**|Indique si les abonnements de mise à jour immédiate sont autorisés pour la publication. **1** signifie que les abonnements de mise à jour immédiate sont autorisés.|  
|**autogen_sync_procs**|**bit**|Indique si la procédure stockée de synchronisation pour les abonnements de mise à jour immédiate est générée par le serveur de distribution. **1** signifie qu’il est généré sur le serveur de publication.|  
|**retention**|**int**|Durée, en heures, pendant laquelle les modifications de la publication sont conservées dans la base de données de distribution.|  
|**allow_queued_tran**|**bit**|Indique si la mise en file d'attente des modifications sur l'Abonné jusqu'à leur application sur le serveur de publication est activée. Si **1**, les modifications sur l’abonné sont en attente.|  
|**snapshot_in_defaultfolder**|**bit**|Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut. Si **0**, fichiers d’instantanés ont été stockés dans l’emplacement secondaire spécifié par *alternate_snapshot_folder*. Si la valeur est égale à 1, les fichiers d'instantané se trouvent dans le dossier par défaut.|  
|**alt_snapshot_folder**|**nvarchar(510)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|**pre_snapshot_script**|**nvarchar(510)**|Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de distribution exécute le script de pré-instantané avant toute exécution de scripts d'objets répliqués, lors de l'application d'un instantané sur un Abonné.|  
|**post_snapshot_script**|**nvarchar(510)**|Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de distribution exécute le script de post-instantané après que tous les autres scripts d'objets et les données répliqués ont été appliqués lors d'une synchronisation initiale.|  
|**compress_snapshot**|**bit**|Spécifie que l’instantané qui est écrite dans le *alt_snapshot_folder* emplacement doit être compressé dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] format CAB. **1** signifie que l’instantané sera compressé.|  
|**ftp_address**|**sysname**|L’adresse réseau du service FTP du serveur de distribution. Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané de la publication.|  
|**ftp_port**|**int**|Le numéro de port du service FTP du serveur de distribution. Spécifie où se trouvent les fichiers d’instantanés de publication pour l’Agent de Distribution à récupérer.|  
|**ftp_subdirectory**|**nvarchar(510)**|Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané si la publication prend en charge la propagation d'instantanés via FTP.|  
|**ftp_login**|**nvarchar (256)**|Nom d'utilisateur, utilisé pour la connexion au service FTP.|  
|**ftp_password**|**nvarchar(1048)**|Le mot de passe utilisé pour se connecter au service FTP.|  
|**allow_dts**|**bit**|Spécifie si la publication autorise les transformations DTS (Data Transformation Services) [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. **1** Spécifie que les transformations DTS sont autorisées.|  
|**allow_subscription_copy**|**bit**|Spécifie si la possibilité de copier les bases de données d'abonnement qui s'abonnent à cette publication a été activée. **1** signifie que la copie est autorisée.|  
|**centralized_conflicts**|**bit**|Spécifie si les enregistrements en conflit sont stockés sur le serveur de publication :<br /><br /> **0** = les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné qui a provoqué le conflit.<br /><br /> **1** = les enregistrements en conflit sont stockés sur le serveur de publication.|  
|**conflict_retention**|**int**|Spécifie (en jours) la durée de rétention des enregistrements des conflits.|  
|**conflict_policy**|**int**|Spécifie la stratégie de résolution de conflits à suivre lorsque l'option d'abonné avec mise à jour en attente est utilisée. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = serveur de publication gagne le conflit.<br /><br /> **2** = l’abonné gagne le conflit.<br /><br /> **3** = abonnement est réinitialisé.|  
|**queue_type**|**int**|Spécifie le type de file d'attente utilisé. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = .MSMQ. utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing pour stocker les transactions.<br /><br /> **2** = .sql, qui utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker les transactions.<br /><br /> Remarque : L’utilisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing a été déconseillée et n’est plus pris en charge.|  
|**ad_guidname**|**sysname**|Spécifie si la publication est publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un identificateur global unique (GUID) valide indique que la publication est publiée dans l'annuaire Active Directory ; le GUID correspond alors à l'objet de publication Active Directory objectGUID. Si la valeur est NULL, la publication n'est pas publiée dans l'annuaire Active Directory.<br /><br /> Remarque : La publication à Active Directory n’est plus pris en charge.|  
|**backward_comp_level**|**int**|Le niveau de compatibilité des bases de données peut avoir une des valeurs suivantes :<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].|  
|**allow_initialize_from_backup**|**bit**|Indique si les Abonnés peuvent initialiser un abonnement à cette publication à partir d'une sauvegarde plutôt que d'un instantané initial. **1** signifie que les abonnements peuvent être initialisés à partir d’une sauvegarde, et **0** signifie qu’ils ne peuvent pas. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).|  
|**min_autonosync_lsn**|**binary(1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Indique si la réplication de schéma est prise en charge pour la publication.<br /><br /> **1** = DDL instructions exécutées sur le serveur de publication sont répliquées.<br /><br /> **0** = indique que les instructions DDL ne sont pas répliquées. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**options**|**int**|Bitmap qui spécifie d'autres options de publication. Les valeurs des options au niveau des bits sont les suivantes :<br /><br /> **0 x 1** - activées pour la réplication d’égal à égal.<br /><br /> **0 x 2** -publier des modifications locales uniquement pour la réplication d’égal à égal.<br /><br /> **0 x 4** - activées pour non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés.<br /><br /> **0 x 8** - activées pour la détection de conflit d’égal à égal.|  
|**originator_id**|**smallint**|Identifie chaque nœud dans la topologie de réplication d'égal à égal pour les besoins de la détection de conflit. Pour plus d'informations, consultez [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)  
  
  
