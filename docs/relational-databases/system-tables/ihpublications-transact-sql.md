---
title: IHpublications (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- IHpublications_TSQL
- IHpublications
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublications system table
ms.assetid: b519a101-fa53-44be-bd55-6ea79245b5d1
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 0bf90e755f3ba467882cfcf5f9602b1c6a607444
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ihpublications-transact-sql"></a>IHpublications (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **IHpublications** (table système) contient une ligne pour chaque publication non SQL Server à l’aide du serveur de distribution en cours. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**pubid**|**int**|Colonne d'identité fournissant un ID unique pour la publication.|  
|**nom**|**sysname**|Nom unique associé à la publication.|  
|**repl_freq**|**tinyint**|Fréquence de réplication :<br /><br /> **0** = en fonction des transactions.<br /><br /> **1** = actualisation planifiée des tables.|  
|**status**|**tinyint**|État de la publication pouvant prendre la valeur :<br /><br /> **0** = inactif.<br /><br /> **1** = actif.|  
|**sync_method**|**tinyint**|Méthode de synchronisation :<br /><br /> **1** = copie en bloc de caractères.<br /><br /> **4** = Concurrent_c : copie en bloc de caractères est utilisée mais les tables ne sont pas verrouillées lors de l’instantané.|  
|**snapshot_jobid**|**binaire**|ID de tâche planifiée.|  
|**enabled_for_internet**|**bit**|Indique si les fichiers de synchronisation pour la publication sont exposés à Internet via FTP et d’autres services, où **1** signifie qu’ils sont accessibles à partir d’Internet.|  
|**immediate_sync_ready**|**bit**|Indique si les fichiers de synchronisation sont disponibles, où **1** signifie qu’ils sont disponibles. *Pas pris en charge pour les éditeurs non SQL.*|  
|**allow_queued_tran**|**bit**|Indique si la mise en file d'attente des modifications sur l'Abonné jusqu'à leur application sur le serveur de publication est activée. Si **1**, les modifications sur l’abonné sont en attente. *Pas pris en charge pour les éditeurs non SQL.*|  
|**allow_sync_tran**|**bit**|Indique si les abonnements de mise à jour immédiate sont autorisés pour la publication. **1** signifie que les abonnements de mise à jour immédiate sont autorisés. *Pas pris en charge pour les éditeurs non SQL.*|  
|**autogen_sync_procs**|**bit**|Indique si la procédure stockée de synchronisation pour les abonnements de mise à jour immédiate est générée par le serveur de distribution. **1** signifie qu’il est généré sur le serveur de publication. *Pas pris en charge pour les éditeurs non SQL.*|  
|**snapshot_in_defaultfolder**|**bit**|Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut. Si **0**, fichiers d’instantanés ont été stockés dans l’emplacement secondaire spécifié par *alternate_snapshot_folder*. Si **1**, fichiers d’instantanés sont accessibles dans le dossier par défaut.|  
|**alt_snapshot_folder**|**nvarchar(510)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|**pre_snapshot_script**|**nvarchar(510)**|Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de distribution exécute le script de pré-instantané avant toute exécution de scripts d'objets répliqués, lors de l'application d'un instantané sur un Abonné.|  
|**post_snapshot_script**|**nvarchar(510)**|Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de distribution exécute le script de post-instantané après que tous les autres scripts d'objets et les données répliqués ont été appliqués lors d'une synchronisation initiale.|  
|**compress_snapshot**|**bit**|Spécifie que l’instantané qui est écrite dans le *alt_snapshot_folder* emplacement doit être compressé dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] format CAB. **0** Spécifie que l’instantané ne sera pas compressé.|  
|**ftp_address**|**sysname**|L’adresse réseau du service FTP du serveur de distribution. Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané de la publication.|  
|**ftp_port**|**int**|Le numéro de port du service FTP du serveur de distribution. Indique l'emplacement à partir duquel l'Agent de distribution peut extraire les fichiers d'instantané de la publication.|  
|**ftp_subdirectory**|**nvarchar(510)**|Spécifie l'emplacement d'où l'Agent de distribution peut extraire les fichiers d'instantané si la publication prend en charge la propagation d'instantanés via FTP.|  
|**ftp_login**|**nvarchar (256)**|Nom d'utilisateur, utilisé pour la connexion au service FTP.|  
|**ftp_password**|**nvarchar(1048)**|Le mot de passe utilisé pour se connecter au service FTP.|  
|**allow_dts**|**bit**|Indique que la publication autorise les transformations de données. **1** Spécifie que les transformations DTS sont autorisées. *Pas pris en charge pour les éditeurs non SQL.*|  
|**allow_anonymous**|**bit**|Indique si les abonnements anonymes sont autorisés pour la publication, où **1** signifie qu’ils sont autorisés.|  
|**centralized_conflicts**|**bit**|Spécifie si les enregistrements en conflit sont stockés sur le serveur de publication :<br /><br /> **0** = les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné qui a provoqué le conflit.<br /><br /> **1** = les enregistrements en conflit sont stockés sur le serveur de publication.<br /><br /> *Pas pris en charge pour les éditeurs non SQL.*|  
|**conflict_retention**|**int**|Spécifie la durée de rétention des conflits en jours. *Pas pris en charge pour les éditeurs non SQL.*|  
|**conflict_policy**|**int**|Spécifie la stratégie de résolution de conflits à suivre lorsque l'option d'abonné avec mise à jour en attente est utilisée. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = serveur de publication gagne le conflit.<br /><br /> **2** = l’abonné gagne le conflit.<br /><br /> **3** = abonnement est réinitialisé.<br /><br /> *Pas pris en charge pour les éditeurs non SQL.*|  
|**queue_type**|**int**|Spécifie le type de file d'attente utilisé. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = msmq utilisant [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing pour stocker les transactions.<br /><br /> **2** = sql, qui utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker les transactions.<br /><br /> Cette colonne n’est pas utilisée par non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveurs de publication.<br /><br /> Remarque : L’utilisation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing a été déconseillée et n’est plus pris en charge.<br /><br /> *Cette colonne n’est pas pris en charge pour les éditeurs non SQL.*|  
|**ad_guidname**|**sysname**|Spécifie si la publication est publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un identificateur global unique (GUID) valide indique que la publication est publiée dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory et le GUID de l’objet de publication Active Directory est **objectGUID**. Si la valeur est NULL, la publication n'est pas publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. *Pas pris en charge pour les éditeurs non SQL.*|  
|**backward_comp_level**|**int**|Le niveau de compatibilité des bases de données peut avoir une des valeurs suivantes :<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].<br /><br /> *Pas pris en charge pour les éditeurs non SQL.*|  
|**description**|**nvarchar(255)**|Entrée décrivant la publication.|  
|**independent_agent**|**bit**|Spécifie s’il existe un Agent de Distribution autonome pour cette publication.<br /><br /> **0** = la publication utilise un Agent de Distribution partagé, et chaque paire de base de données de serveur de publication/abonné de base de données a un seul Agent partagé.<br /><br /> **1** = il existe un Agent de Distribution autonome pour cette publication.|  
|**immediate_sync**|**bit**|Indique si les fichiers de synchronisation sont créés ou recréés chaque fois que l’Agent d’instantané s’exécute, où **1** signifie qu’ils sont créés chaque fois que l’agent s’exécute.|  
|**allow_push**|**bit**|Indique si des abonnements envoyés sont autorisés pour la publication, où **1** signifie qu’ils sont autorisés.|  
|**allow_pull**|**bit**|Indique si les abonnements extraits sont autorisés pour la publication, où **1** signifie qu’ils sont autorisés.|  
|**retention**|**int**|La quantité de modifications, en heures, à enregistrer pour la publication concernée.|  
|**allow_subscription_copy**|**bit**|Spécifie si la possibilité de copier les bases de données d'abonnement qui s'abonnent à cette publication a été activée. **1** signifie que la copie est autorisée.|  
|**allow_initialize_from_backup**|**bit**|Indique si les Abonnés peuvent initialiser un abonnement à cette publication à partir d'une sauvegarde plutôt qu'à partir de son instantané initial. **1** signifie que les abonnements peuvent être initialisés à partir d’une sauvegarde, et **0** signifie qu’ils ne peuvent pas. Pour plus d’informations, consultez [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md). *Pas pris en charge pour les éditeurs non SQL.*|  
|**min_autonosync_lsn**|**binary(1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Précise si la réplication de schéma est prise en charge pour la publication. **1** indique que les instructions DDL exécutées sur le serveur de publication sont répliquées, et **0** indique que les instructions DDL ne sont pas répliquées. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md). *Pas pris en charge pour les éditeurs non SQL.*|  
|**options**|**int**|Image précisant les options de publication supplémentaires, où les valeurs des options au niveau du bit peuvent être :<br /><br /> **0 x 1** - activées pour la réplication d’égal à égal.<br /><br /> **0 x 2** -publier les modifications locales uniquement.<br /><br /> **0 x 4** - activées pour les abonnés non SQL Server.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [SYSPUBLICATIONS &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/syspublications-system-view-transact-sql.md)   
 [SYSPUBLICATIONS &#40;Transact-SQL&#41;](../../relational-databases/system-tables/syspublications-transact-sql.md)  
  
  
