---
title: sp_helpmergepublication (Transact-SQL) | Documents Microsoft
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
- sp_helpmergepublication
- sp_helpmergepublication_TSQL
helpviewer_keywords:
- sp_helpmergepublication
ms.assetid: dfe1e1e1-9a65-406a-aced-6385a078e135
caps.latest.revision: 55
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c419566cd0e43f27644433c6a6bbe5b597742b5f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpmergepublication-transact-sql"></a>sp_helpmergepublication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie des informations sur une publication de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergepublication [ [ @publication = ] 'publication' ]  
    [ , [ @found = ] 'found' OUTPUT ]  
    [ , [ @publication_id = ] 'publication_id' OUTPUT ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @publication **=** ] **'***publication***'**  
 Nom de la publication. *publication*est **sysname**, avec une valeur par défaut **%**, qui retourne des informations sur toutes les publications de fusion dans la base de données actuelle.  
  
 [ @found **=** ] **'***trouvé***'** sortie  
 Indicateur désignant les lignes. *trouvé*est **int** d’un paramètre OUTPUT, avec NULL comme valeur par défaut. **1** indique la publication a été trouvée. **0** indique la publication est introuvable.  
  
 [ @publication_id **=**] **'***publication_id***'** sortie  
 Numéro d'identification de la publication. *id_de_la_publication* est **uniqueidentifier** d’un paramètre OUTPUT, avec NULL comme valeur par défaut.  
  
 [ @reserved **=**] **'***réservé***'**  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *réservé* est **nvarchar (20)**, avec NULL comme valeur par défaut.  
  
 [ @publisher **=** ] **'***publisher***'**  
 Le nom du serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [@publisher_db **=** ] **'***publisher_db***'**  
 Le nom de la base de données de publication. *publisher_db* est **sysname**, avec NULL comme valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|id|**int**|Numéro séquentiel de la publication dans la liste de l'ensemble de résultats.|  
|name|**sysname**|Nom de la publication.|  
|description|**nvarchar(255)**|Description de la publication.|  
|status|**tinyint**|Indique quand les données de publication sont disponibles.|  
|retention|**int**|Temps nécessaire pour enregistrer les métadonnées relatives aux modifications des articles dans la publication. Les unités utilisées pour cette période peuvent être des jours, des semaines, des mois ou des années. Pour plus d'informations sur ces unités, consultez la colonne retention_period_unit.|  
|sync_mode|**tinyint**|Mode de synchronisation de cette publication :<br /><br /> **0** = programme de copie en bloc natif (**bcp** utilitaire)<br /><br /> **1** = copie en bloc de caractères|  
|allow_push|**int**|Détermine si des abonnements par envoi de données (push) peuvent être créés pour la publication concernée. **0** signifie qu’un abonnement par émission de données n’est pas autorisé.|  
|allow_pull|**int**|Détermine si des abonnements par extraction de données (pull) peuvent être créés pour la publication concernée. **0** signifie qu’un abonnement par extraction n’est pas autorisé.|  
|allow_anonymous|**int**|Détermine si des abonnements anonymes peuvent être créés pour la publication concernée. **0** signifie qu’un abonnement anonyme n’est pas autorisé.|  
|centralized_conflicts|**int**|Détermine si les enregistrements en conflit sont stockés sur le serveur de publication donné :<br /><br /> **0** = les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné qui a provoqué le conflit.<br /><br /> **1** = tous les enregistrements en conflit sont stockés sur le serveur de publication.|  
|priority|**float(8)**|Priorité de l'abonnement en boucle.|  
|snapshot_ready|**tinyint**|Indique si l'instantané de cette publication est prêt :<br /><br /> **0** = instantané est prêt à être utilisé.<br /><br /> **1** = instantané n’est pas prêt à être utilisé.|  
|publication_type|**int**|Type de publication :<br /><br /> **0** = instantané.<br /><br /> **1** = transactionnelle.<br /><br /> **2** = fusion.|  
|pubid|**uniqueidentifier**|Identificateur unique de la publication.|  
|snapshot_jobid|**binary (16)**|ID de travail de l'Agent d'instantané. Pour obtenir l’entrée pour le travail d’instantané dans le [sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md) (table système), vous devez convertir cette valeur hexadécimale à **uniqueidentifier**.|  
|enabled_for_internet|**int**|Détermine si la publication est activée pour Internet. Si **1**, les fichiers de synchronisation pour la publication sont placés dans le `C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\Ftp` active. L'utilisateur doit créer le répertoire FTP (File Transfer Protocol). Si **0**, la publication n’est pas activée pour l’accès Internet.|  
|dynamic_filter|**int**|Indique si un filtre de lignes paramétrable est utilisé. **0** signifie un filtre de lignes paramétrable n’est pas utilisé.|  
|has_subscription|**bit**|Indique si la publication comporte des abonnements. **0** signifie qu’il n’y a actuellement aucun abonnement à cette publication.|  
|snapshot_in_default_folder|**bit**|Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut.<br /><br /> Si **1**, fichiers d’instantanés sont accessibles dans le dossier par défaut.<br /><br /> Si **0**, fichiers d’instantanés sont stockés dans l’emplacement secondaire spécifié par **alt_snapshot_folder**. Les emplacements secondaires peuvent se trouver sur un autre serveur, un lecteur réseau ou un support amovible (tel qu'un CD-ROM ou des disques amovibles). Vous pouvez également enregistrer les fichiers d'instantané sur un site FTP, pour permettre à l'Abonné de les extraire plus tard.<br /><br /> Remarque : Ce paramètre peut avoir la valeur true et néanmoins avoir un emplacement le **alt_snapshot_folder** paramètre. Cette combinaison spécifie que les fichiers d'instantané sont stockés à la fois dans l'emplacement par défaut et dans l'emplacement secondaire.|  
|alt_snapshot_folder|**nvarchar(255)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|pre_snapshot_script|**nvarchar(255)**|Spécifie un pointeur vers un **.sql** scripts de fichier que l’Agent de fusion exécute avant tout de l’objet répliqué lors de l’application de l’instantané sur l’abonné.|  
|post_snapshot_script|**nvarchar(255)**|Spécifie un pointeur vers un **.sql** fichier que l’Agent de fusion exécute après que tous les autres scripts et objet répliqués données ont été appliquées lors d’une synchronisation initiale.|  
|compress_snapshot|**bit**|Spécifie que l’instantané est écrite dans le **alt_snapshot_folder** emplacement est compressé dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] format CAB.|  
|ftp_address|**sysname**|Adresse réseau du service FTP du serveur de distribution. Spécifie l'emplacement à partir duquel l'Agent fusion peut extraire les fichiers d'instantané de la publication.|  
|ftp_port|**int**|Numéro de port du service FTP du serveur de distribution. **ftp_port** a une valeur par défaut de **21**. Spécifie l'emplacement où l'Agent de fusion peut accéder aux fichiers d'instantané de la publication.|  
|ftp_subdirectory|**nvarchar(255)**|Spécifie l'emplacement où l'Agent de fusion peut accéder aux fichiers d'instantanés lorsque l'instantané est envoyé via FTP.|  
|ftp_login|**sysname**|Le nom d’utilisateur est utilisé pour se connecter au service FTP.|  
|conflict_retention|**int**|Indique la période de rétention, en jours, pendant laquelle les conflits sont conservés. Au terme du nombre de jours spécifié, la ligne en conflit est purgée de la table des conflits.|  
|keep_partition_changes|**int**|Indique si l'optimisation de la synchronisation intervient pour cette publication. **keep_partition_changes** a une valeur par défaut de **0**. La valeur **0** signifie que la synchronisation n’est pas optimisée, et les partitions envoyées à tous les abonnés sont vérifiées lors de la modification des données dans une partition.<br /><br /> **1** signifie que la synchronisation est optimisée et seuls les abonnés ayant des lignes dans la partition modifiée sont concernés.<br /><br /> Remarque : Par défaut, les publications de fusion utilisent les partitions précalculées, ce qui offre un meilleur niveau d’optimisation que cette option. Pour plus d’informations, consultez [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md) et [optimiser les performances de filtre paramétré avec des Partitions précalculées](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).|  
|allow_subscription_copy|**int**|Spécifie si la possibilité de copier les bases de données d'abonnement qui s'abonnent à cette publication a été activée. La valeur **0** signifie que la copie n’est pas autorisée.|  
|allow_synctoalternate|**int**|Spécifie si un partenaire de synchronisation différent est autorisé pour se synchroniser avec le serveur de publication. La valeur **0** signifie un partenaire de synchronisation n’est pas autorisé.|  
|validate_subscriber_info|**nvarchar(500)**|Donne la liste des fonctions utilisées pour extraire les informations d'Abonné et valider les critères de filtre de lignes paramétrable sur l'Abonné. Permet de vérifier la cohérence du partitionnement des informations avec chaque fusion.|  
|backward_comp_level|**int**|Niveau de compatibilité de la base de données. Il peut avoir une des valeurs suivantes :<br /><br /> **90** = [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP1<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2<br /><br /> **100** = [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|publish_to_activedirectory|**bit**|Spécifie si les informations de publication sont publiées dans Active Directory. La valeur **0** signifie que les informations de publication ne sont pas disponibles à partir d’Active Directory.<br /><br /> Ce paramètre est déconseillé et il n'est pris en charge que pour la compatibilité descendante des scripts. Vous ne pouvez plus ajouter d'informations de publication dans Active Directory.|  
|max_concurrent_merge|**int**|Nombre de processus de fusion simultanés. Si **0**, il n’existe aucune limite au nombre de processus de fusion simultanés en cours d’exécution à un moment donné.|  
|max_concurrent_dynamic_snapshots|**int**|Nombre maximal de sessions d'instantané filtrée pouvant être exécutées simultanément par rapport à la publication de fusion. Si **0**, il n’existe aucune limite au nombre maximal de sessions d’instantanés de données filtrées simultanées pouvant être exécutés simultanément par rapport à la publication à un moment donné.|  
|use_partition_groups|**int**|Détermine si des partitions précalculées sont utilisées. La valeur **1** signifie que des partitions précalculées est utilisées.|  
|num_of_articles|**int**|Nombre d'articles dans la publication.|  
|replicate_ddl|**int**|Indique si les modifications de schéma des tables publiées sont répliquées. La valeur **1** signifie que les modifications de schéma sont répliquées.|  
|publication_number|**smallint**|Numéro affecté à cette publication.|  
|allow_subscriber_initiated_snapshot|**bit**|Détermine si les Abonnés peuvent lancer le processus de génération d'instantané de données filtrées. La valeur **1** signifie que les abonnés peuvent initier le processus d’instantané.|  
|allow_web_synchronization|**bit**|Détermine si la publication est activée pour la synchronisation Web. La valeur **1** signifie que la synchronisation Web est activée.|  
|web_synchronization_url|**nvarchar(500)**|URL Internet utilisé pour la synchronisation Web.|  
|allow_partition_realignment|**bit**|Détermine si les suppressions sont envoyées à l'abonné lorsque la modification de la ligne sur le serveur de publication entraîne la modification de sa partition. La valeur **1** signifie que les suppressions sont envoyées à l’abonné.  Pour plus d’informations, consultez [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).|  
|retention_period_unit|**tinyint**|Définit l'unité utilisée lors la définition de la rétention. Cela peut prendre l’une des valeurs suivantes :<br /><br /> **0** = jour<br /><br /> **1** = semaine<br /><br /> **2** = mois<br /><br /> **3** = année|  
|has_downloadonly_articles|**bit**|Indique si des articles qui appartiennent à la publication sont des articles téléchargeables uniquement. La valeur **1** indique qu’il existe des articles en téléchargement uniquement.|  
|decentralized_conflicts|**int**|Indique si les enregistrements en conflit sont stockés sur l'Abonné qui a généré le conflit. La valeur **0** indique que les enregistrements en conflit ne sont pas stockés sur l’abonné. La valeur 1 indique que les enregistrements en conflit sont stockés sur l'Abonné.|  
|generation_leveling_threshold|**int**|Spécifie le nombre de modifications qui sont contenus dans une génération. Une génération est un ensemble de modifications qui sont remis à un serveur de publication ou un abonné|  
|automatic_reinitialization_policy|**bit**|Indique si les modifications sont téléchargées depuis l'Abonné avant une réinitialisation automatique. La valeur **1** indique que les modifications sont téléchargées à partir de l’abonné avant une réinitialisation automatique. La valeur 0 indique que des modifications ne sont pas téléchargées avant une réinitialisation automatique.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_helpmergepublication est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Les membres de la liste d'accès à la publication d'une publication peuvent exécuter sp_helpmergepublication pour cette publication. Les membres du rôle de base de données fixe db_owner de la base de données de publication peuvent exécuter sp_helpmergepublication pour obtenir des informations sur toutes les publications.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_helpmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-helpmergepublication-_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
