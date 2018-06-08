---
title: sysmergepublications (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- sysmergepublications
- sysmergepublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepublications system table
ms.assetid: 7f82c6c3-22d1-47c0-a92b-4d64b98cc455
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 40763216075a551c244e72b0516e7ff2383ff913
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysmergepublications-transact-sql"></a>sysmergepublications (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque publication de fusion définie dans la base de données. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publisher** (serveur de publication)|**sysname**|Nom du serveur par défaut.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication par défaut.|  
|**nom**|**sysname**|Nom de la publication.|  
|**description**|**nvarchar(255)**|Brève description de la publication.|  
|**retention**|**int**|La période de rétention pour l’ensemble de l’ensemble de la publication, où l’unité est indiquée par la valeur de la **retention_period_unit** colonne.|  
|**publication_type**|**tinyint**|Indique que la publication est filtrée :<br /><br /> **0** = non filtrée.<br /><br /> **1** = filtrée.|  
|**pubid**|**uniqueidentifier**|Numéro d'identification unique de cette publication. Ce numéro est généré lors de l'ajout de la publication.|  
|**DesignMasterID**|**uniqueidentifier**|Réservé pour un usage ultérieur.|  
|**parentid**|**uniqueidentifier**|Indique la publication parente à partir de laquelle la publication paire courante ou la publication de sous-ensemble a été créée (utilisé pour les topologies de publication hiérarchiques).|  
|**sync_mode**|**tinyint**|Mode de synchronisation de la publication :<br /><br /> **0** = native.<br /><br /> **1** = caractère.|  
|**allow_push**|**int**|Indique si la publication autorise les abonnements par envoi de données (push).<br /><br /> **0** = les abonnements par envoi de données non autorisés.<br /><br /> **1** = par envoi de données des abonnements sont autorisés.|  
|**allow_pull**|**int**|Indique si la publication autorise les abonnements par extraction de données (pull).<br /><br /> **0** = les abonnements par extraction de données non autorisés.<br /><br /> **1** = par extraction de données des abonnements sont autorisés.|  
|**allow_anonymous**|**int**|Indique si la publication autorise les abonnements anonymes.<br /><br /> **0** = les abonnements anonymes sont ne pas autorisés.<br /><br /> **1** = anonyme les abonnements sont autorisés.|  
|**centralized_conflicts**|**int**|Indique si les enregistrements conflictuels sont stockés côté serveur de publication :<br /><br /> **0** = les enregistrements en conflit ne sont pas stockés sur le serveur de publication.<br /><br /> **1** = les enregistrements en conflit sont stockés sur le serveur de publication.|  
|**status**|**tinyint**|Réservé pour un usage ultérieur.|  
|**snapshot_ready**|**tinyint**|Indique l'état de l'instantané de la publication :<br /><br /> **0** = instantané n’est pas prêt à être utilisé.<br /><br /> **1** = instantané est prêt à être utilisé.<br /><br /> **2** = un nouvel instantané pour cette publication doit être créée.|  
|**enabled_for_internet**|**bit**|Indique si les fichiers de synchronisation pour la publication sont accessibles sur Internet, par l'intermédiaire de FTP et d'autres services.<br /><br /> **0** = la synchronisation de fichiers sont accessibles à partir d’Internet.<br /><br /> **1** = la synchronisation de fichiers ne sont pas accessibles à partir d’Internet.|  
|**dynamic_filters**|**bit**|Indique si la publication est filtrée à l'aide d'un filtre de lignes paramétrable.<br /><br /> **0** = la publication est filtrée par ligne pas.<br /><br /> **1** = la publication est filtrée par ligne.|  
|**snapshot_in_defaultfolder**|**bit**|Indique si les fichiers d'instantané sont stockés dans le dossier par défaut :<br /><br /> **0** = l’instantané de fichiers se trouvent dans le dossier par défaut.<br /><br /> **1** = l’instantané de fichiers sont stockés dans l’emplacement spécifié par **alt_snapshot_folder**.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Emplacement de l'autre dossier pour l'instantané.|  
|**pre_snapshot_script**|**nvarchar(255)**|Pointeur vers un. **sql** scripts de fichier que l’Agent de fusion exécute avant tout objet de réplication lors de l’application de l’instantané sur l’abonné.|  
|**post_snapshot_script**|**nvarchar(255)**|Le pointeur vers un. **sql** fichier que l’Agent de fusion s’exécute une fois toutes les autres la réplication de scripts d’objets et données ont été appliquées lors d’une synchronisation initiale.|  
|**compress_snapshot**|**bit**|Spécifie si l’instantané écrit à le **alt_snapshot_folder** emplacement est compressé dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] format CAB. **0** Spécifie que le fichier n’est pas compressé.|  
|**ftp_address**|**sysname**|Adresse réseau du service de transfert de protocole FTP (File) pour le serveur de distribution. Indique l'emplacement à partir duquel l'Agent de fusion peut extraire les fichiers d'instantané de la publication, si le protocole FTP est activé.|  
|**ftp_port**|**int**|Le numéro de port du service FTP du serveur de distribution.|  
|**ftp_subdirectory**|**nvarchar(255)**|Sous-répertoire à partir duquel l'Agent de fusion peut extraire les fichiers d'instantané.|  
|**ftp_login**|**sysname**|Nom de l'utilisateur, utilisé pour la connexion au service FTP.|  
|**ftp_password**|**nvarchar (524)**|Le mot de passe utilisé pour se connecter au service FTP.|  
|**conflict_retention**|**int**|Indique la période de rétention, en jours, pendant laquelle les conflits sont conservés. À la fin de cette période, la ligne de conflits est purgée de la table de conflits.|  
|**keep_before_values**|**int**|Indique si l'optimisation de la synchronisation intervient pour cette publication :<br /><br /> **0** = synchronisation n’est pas optimisée et les partitions envoyées à tous les abonnés seront vérifiées lorsque de la modification des données dans une partition.<br /><br /> **1** = la synchronisation est optimisée et seuls les abonnés ayant des lignes dans la partition modifiée sont concernés.|  
|**allow_subscription_copy**|**bit**|Indique si la possibilité de copier la base de données d'abonnement a été activée. **0** signifie que la copie n’est pas autorisée.|  
|**allow_synctoalternate**|**bit**|Spécifie si un partenaire de synchronisation différent est autorisé pour se synchroniser avec le serveur de publication. **0** signifie qu’un partenaire de synchronisation n’est pas autorisé.|  
|**validate_subscriber_info**|**nvarchar(500)**|Donne la liste des fonctions utilisées pour extraire les informations d'Abonné et valider les critères de filtre de lignes paramétrable sur l'Abonné.|  
|**ad_guidname**|**sysname**|Spécifie si la publication est publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un GUID valide indique que la publication est publiée dans Active Directory, et le GUID est l’objet de publication Active Directory **objectGUID**. Si la valeur est NULL, la publication n'est pas publiée dans l'annuaire Active Directory.|  
|**backward_comp_level**|**int**|Niveau de compatibilité de base de données. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].|  
|**max_concurrent_merge**|**int**|Le nombre maximal de processus de fusion simultanés autorisés. La valeur **0** pour cette propriété signifie qu’il n’existe aucune limite au nombre de processus de fusion simultanés en cours d’exécution à un moment donné. Cette propriété définit une limite concernant le nombre de processus de fusion simultanés qui peuvent être exécutées sur une publication de fusion à la fois. Si, au même moment, le nombre de processus d'instantané planifiés dépasse le nombre maximal autorisé, les travaux en excès sont placés dans une file d'attente jusqu'à achèvement d'un processus de fusion en cours.|  
|**max_concurrent_dynamic_snapshots**|**int**|Nombre maximal de sessions d'instantanés de données filtrées simultanées autorisées exécutables sur la publication de fusion. Si **0**, il n’existe aucune limite au nombre maximal de sessions d’instantanés de données filtrées simultanées pouvant être exécutés simultanément par rapport à la publication à un moment donné. Cette propriété permet de définir un nombre maximal de processus d'instantané simultanés exécutables sur une publication de fusion à un moment donné. Si, au même moment, le nombre de processus d'instantané planifiés dépasse le nombre maximal autorisé, les travaux en excès sont placés dans une file d'attente jusqu'à achèvement d'un processus de fusion en cours.|  
|**use_partition_groups**|**smallint**|Spécifie si la publication utilise des partitions précalculées.|  
|**dynamic_filters_function_list**|**nvarchar(500)**|Liste délimitée par des points-virgules des fonctions utilisées dans les filtres de lignes paramétrables de la publication.|  
|**partition_id_eval_proc**|**sysname**|Spécifie le nom de la procédure qu'exécute l'Agent de fusion d'un Abonné pour déterminer l'ID de partition affecté à celui-ci.|  
|**publication_number**|**smallint**|Spécifie la colonne d’identité qui fournit un mappage de 2 octets à **pubid**. **pubid** est un identificateur global unique pour une publication, alors que le numéro de publication est unique que dans une base de données spécifiée.|  
|**replicate_ddl**|**int**|Indique si la réplication de schéma est prise en charge pour la publication.<br /><br /> **0** = DDL instructions ne sont pas répliquées.<br /><br /> **1** = DDL instructions exécutées sur le serveur de publication sont répliquées.<br /><br /> Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**allow_subscriber_initiated_snapshot**|**bit**|Indique que les Abonnés peuvent initier le processus qui génère l'instantané d'une publication à l'aide de filtres paramétrés. **1** indique que les abonnés peuvent initier le processus d’instantané.|  
|**dynamic_snapshot_queue_timeout**|**int**|Spécifie le nombre de minutes pendant lesquelles un Abonné doit patienter dans la file d'attente avant que ne démarre le processus de génération d'instantané lors de l'utilisation de filtres paramétrés.|  
|**dynamic_snapshot_ready_timeout**|**int**|Spécifie le nombre de minutes pendant lesquelles un Abonné attend que se déroule le processus de génération d'instantané lors de l'utilisation de filtres paramétrés.|  
|**Serveur de distribution**|**sysname**|Nom du serveur de distribution de la publication.|  
|**snapshot_jobid**|**binary (16)**|Identifie le travail d'Agent qui génère l'instantané lorsque l'Abonné peut initier le processus de génération d'instantané.|  
|**allow_web_synchronization**|**bit**|Spécifie si la publication est activée pour la synchronisation Web, où **1** signifie que la synchronisation Web est activée pour la publication.|  
|**web_synchronization_url**|**nvarchar(500)**|Spécifie la valeur par défaut de l'URL Internet utilisée pour la synchronisation Web.|  
|**allow_partition_realignment**|**bit**|Indique si les suppressions sont envoyées à l'Abonné lorsque la modification de la ligne sur le serveur de publication amène celui-ci à modifier sa partition.<br /><br /> **0** = les données d’une ancienne partition demeurent sur l’abonné, où les modifications apportées à ces données sur le serveur de publication ne sont pas répliquées sur l’abonné, mais les modifications apportées sur l’abonné sont répliquées sur le serveur de publication.<br /><br /> **1** = les suppressions à l’abonné pour refléter les résultats d’une modification de partition en supprimant des données qui ne font plus partie de la partition de l’abonné.<br /><br /> Pour plus d’informations, consultez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).<br /><br /> Remarque : Les données qui demeurent sur l’abonné lorsque cette valeur est **0** doit être traitée comme si elle était en lecture seule ; toutefois, cela n’est pas strictement appliquée par le système de réplication.|  
|**retention_period_unit**|**tinyint**|Définit l’unité utilisée lors de la définition *rétention*, qui peut être une des valeurs suivantes :<br /><br /> **0** = jour.<br /><br /> **1** = semaine.<br /><br /> **2** = mois.<br /><br /> **3** = année.|  
|**decentralized_conflicts**|**int**|Indique si les enregistrements en conflit sont stockés dans l'Abonné à l'origine du conflit :<br /><br /> **0** = les enregistrements en conflit ne sont pas stockés sur l’abonné.<br /><br /> **1** = les enregistrements en conflit sont stockés sur l’abonné.|  
|**generation_leveling_threshold**|**int**|Indique le nombre de modifications contenues dans une génération. Une génération est une collection de modifications remises à un serveur de publication ou à un Abonné.|  
|**automatic_reinitialization_policy**|**bit**|Indique si les modifications sont téléchargées depuis l'Abonné avant une réinitialisation automatique.<br /><br /> **1** = modifications sont téléchargées à partir de l’abonné avant une réinitialisation automatique.<br /><br /> **0** = modifications ne sont pas téléchargées avant une réinitialisation automatique.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)  
  
  
