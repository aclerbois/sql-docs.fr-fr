---
title: sp_helppublication (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
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
- sp_helppublication_TSQL
- sp_helppublication
helpviewer_keywords:
- sp_helppublication
ms.assetid: e801c3f0-dcbd-4b4a-b254-949a05f63518
caps.latest.revision: 49
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 532c1d371596cff80a547414a316ed0ec401728a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelppublication-transact-sql"></a>sp_helppublication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie des informations sur une publication. Pour un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication, cette procédure stockée est exécutée sur le serveur de publication sur la base de données de publication. Pour une publication Oracle, cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helppublication [ [ @publication = ] 'publication' ]  
    [ , [ @found=] found OUTPUT]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =** ] **'***publication***'**  
 Nom de la publication à afficher. *publication* est de type sysname, avec une valeur par défaut **%**, qui retourne des informations sur toutes les publications.  
  
 [  **@found =** ] **'***trouvé***'** sortie  
 Indicateur désignant les lignes retournées. *trouvé*est **int** d’un paramètre OUTPUT, avec une valeur par défaut **23456**. **1** indique la publication a été trouvée. **0** indique la publication est introuvable.  
  
 [ **@publisher** =] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est de type sysname, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être spécifié lors de la demande des informations de publication à partir d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|pubid|**int**|ID de la publication.|  
|name|**sysname**|Nom de la publication.|  
|restricted|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|status|**tinyint**|État actuel de la publication.<br /><br /> **0** = inactif.<br /><br /> **1** = actif.|  
|tâche||Utilisé pour la compatibilité descendante.|  
|replication frequency|**tinyint**|Type de fréquence de réplication :<br /><br /> **0** = transactionnelle<br /><br /> **1** = instantané|  
|synchronization method|**tinyint**|Mode de synchronisation :<br /><br /> **0** = programme de copie en bloc natif (**bcp** utilitaire)<br /><br /> **1** = copie en bloc de caractères<br /><br /> **3** = simultané, ce qui signifie que cette copie en bloc natif (**bcp**utilitaire) est utilisé mais les tables ne sont pas verrouillées lors de l’instantané<br /><br /> **4** = Concurrent_c : copie en bloc de caractères est utilisée mais les tables ne sont pas verrouillées lors de l’instantané|  
|description|**nvarchar(255)**|Description facultative pour la publication.|  
|immediate_sync|**bit**|Indique si les fichiers de synchronisation sont créés ou recréés à chaque exécution de l’Agent d'instantané.|  
|enabled_for_internet|**bit**|Indique si les fichiers de synchronisation pour la publication sont accessibles sur Internet par le biais du protocole FTP et d'autres services.|  
|allow_push|**bit**|Indique si des abonnements par envoi de données (push) sont autorisés pour la publication.|  
|allow_pull|**bit**|Indique si des abonnements par extraction de données (pull) sont autorisés pour la publication.|  
|allow_anonymous|**bit**|Indique si des abonnements anonymes sont autorisés pour la publication.|  
|independent_agent|**bit**|Indique s'il existe une version autonome de l'Agent de distribution pour cette publication.|  
|immediate_sync_ready|**bit**|Indique si l'Agent d'instantané a généré un instantané utilisable par les nouveaux abonnements. Ce paramètre est défini seulement si la publication est configurée de telle sorte qu'un instantané soit toujours disponible pour les abonnements nouveaux ou réinitialisés.|  
|allow_sync_tran|**bit**|Indique si des abonnements mis à jour immédiatement sont autorisés pour la publication.|  
|autogen_sync_procs|**bit**|Indique s'il faut générer automatiquement les procédures stockées pour la prise en charge des abonnements mis à jour immédiatement.|  
|snapshot_jobid|**binary (16)**|ID de tâche planifiée|  
|retention|**int**|Volume des modifications, en heures, à enregistrer pour la publication donnée.|  
|has subscription|**bit**|Indique si la publication a des abonnements actifs. **1** signifie que la publication a des abonnements actifs, et **0** signifie ne qu’aucun abonnement à la publication.|  
|allow_queued_tran|**bit**|Spécifie si le désactive la file d’attente des modifications sur l’abonné jusqu'à ce qu’ils peuvent être appliquées sur le serveur de publication a été activée. Si **0**, les modifications sur l’abonné ne sont pas mises en attente.|  
|snapshot_in_defaultfolder|**bit**|Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut. Si **0**, fichiers d’instantanés ont été stockés dans l’emplacement secondaire spécifié par *alternate_snapshot_folder*. Si **1**, fichiers d’instantanés sont accessibles dans le dossier par défaut.|  
|alt_snapshot_folder|**nvarchar(255)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|pre_snapshot_script|**nvarchar(255)**|Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de distribution exécute le script de pré-instantané avant l'exécution des scripts d'objet répliqué, lors de l'application d'un instantané sur un Abonné.|  
|post_snapshot_script|**nvarchar(255)**|Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de distribution exécute le script de post-instantané après que tous les autres scripts et données d'objet répliqué ont été appliqués lors d'une synchronisation initiale.|  
|compress_snapshot|**bit**|Spécifie que l’instantané qui est écrite dans le *alt_snapshot_folder* emplacement doit être compressé dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] format CAB. **0** Spécifie que l’instantané ne sera pas compressé.|  
|ftp_address|**sysname**|L’adresse réseau du service FTP du serveur de distribution. Indique l'emplacement à partir duquel l'Agent de distribution ou l'Agent de fusion d'un abonné peut extraire les fichiers d'instantané de la publication.|  
|ftp_port|**int**|Le numéro de port du service FTP du serveur de distribution.|  
|ftp_subdirectory|**nvarchar(255)**|Indique l'emplacement à partir duquel l'Agent de distribution ou de fusion d'un abonné peut extraire les fichiers d'instantané si la publication prend en charge la propagation d'instantanés via FTP.|  
|ftp_login|**sysname**|Nom d'utilisateur, utilisé pour la connexion au service FTP.|  
|allow_dts|**bit**|Indique que la publication autorise les transformations de données. **0** Spécifie que les transformations DTS ne sont pas autorisées.|  
|allow_subscription_copy|**bit**|Spécifie si la possibilité de copier les bases de données d'abonnement qui s'abonnent à cette publication a été activée. **0** signifie que la copie n’est pas autorisée.|  
|centralized_conflicts|**bit**|Spécifie si les enregistrements en conflit sont stockés sur le serveur de publication :<br /><br /> **0** = les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné qui a provoqué le conflit.<br /><br /> **1** = les enregistrements en conflit sont stockés sur le serveur de publication.|  
|conflict_retention|**int**|Spécifie la durée de rétention des conflits en jours.|  
|conflict_policy|**int**|Spécifie la stratégie de résolution de conflits à suivre lorsque l'option d'abonné avec mise à jour en attente est utilisée. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** = serveur de publication gagne le conflit.<br /><br /> **2** = l’abonné gagne le conflit.<br /><br /> **3** = abonnement est réinitialisé.|  
|queue_type||Spécifie le type de file d'attente utilisé. Peut prendre l'une des valeurs suivantes :<br /><br /> **MSMQ** = utiliser [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing pour stocker les transactions.<br /><br /> **SQL** = utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker les transactions.<br /><br /> Remarque : La prise en charge de Message Queuing a été abandonné.|  
|backward_comp_level||Niveau de compatibilité de la base de données. Il peut avoir une des valeurs suivantes :<br /><br /> **90** = [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **100** = [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|publish_to_AD|**bit**|Spécifie si la publication est publiée dans l'annuaire [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory™. La valeur **1** indique qu’il est publié et la valeur **0** indique qu’il n’est pas publié.|  
|allow_initialize_from_backup|**bit**|Indique si les Abonnés peuvent initialiser un abonnement à cette publication à partir d'une sauvegarde plutôt qu'à partir de son instantané initial. **1** signifie que les abonnements peuvent être initialisés à partir d’une sauvegarde, et **0** signifie qu’ils ne peuvent pas. Pour plus d’informations, consultez [initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md) un abonné transactionnel sans instantané.|  
|replicate_ddl|**int**|Précise si la réplication de schéma est prise en charge pour la publication. **1** indique que les instructions data definition language (DDL) exécutées sur le serveur de publication sont répliquées, et **0** indique que les instructions DDL ne sont pas répliquées. Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|enabled_for_p2p|**int**|Indique si la publication est utilisable dans une topologie de réplication d'égal à égal. **1** indique que la publication prend en charge la réplication d’égal à égal. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).|  
|publish_local_changes_only|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|enabled_for_het_sub|**int**|Spécifie si la publication prend en charge les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur **1** signifie que non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés sont pris en charge. La valeur **0** signifient que seuls [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés sont pris en charge. Pour plus d’informations, consultez [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).|  
|enabled_for_p2p_conflictdetection|**int**|Spécifie si l'Agent de distribution détecte des conflits pour une publication activée pour la réplication d'égal à égal. La valeur **1** signifie que les conflits sont détectés. Pour plus d'informations, consultez [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
|originator_id|**int**|Spécifie un ID pour un nœud dans une topologie d'égal à égal. Cet ID est utilisé pour la détection de conflit si **enabled_for_p2p_conflictdetection** a la valeur **1**. Pour obtenir la liste des ID qui ont déjà été utilisés, interrogez la table système [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) .|  
|p2p_continue_onconflict|**int**|Indique si l'Agent de distribution continue à traiter les modifications lorsqu'un conflit est détecté. La valeur **1** signifie que l’agent continue à traiter les modifications.<br /><br /> **\*\* Attention \* \***  nous vous recommandons d’utiliser la valeur par défaut **0**. Lorsque cette option a la valeur **1**, l’Agent de Distribution tente de converger les données de la topologie en appliquant la ligne en conflit à partir du nœud qui possède l’ID de donneur d’ordre le plus élevé. Cette méthode ne garantit pas la convergence. Vous devez vous assurer que la topologie est cohérente après la détection d'un conflit. Pour plus d'informations, consultez « Gestion des conflits » dans [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
|alllow_partition_switch|**int**|Indique si les instructions ALTER TABLE…SWITCH peuvent être exécutées sur la base de données publiée. Pour plus d’informations, consultez [Répliquer des tables et des index partitionnés](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).|  
|replicate_partition_switch|**int**|Indique si les instructions ALTER TABLE…SWITCH exécutées sur la base de données publiée doivent être répliquées sur les Abonnés. Cette option est valide uniquement si *allow_partition_switch* a la valeur **1**.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_helppublication est utilisé dans la réplication transactionnelle et d'instantané.  
  
 sp_helppublication renvoie des informations sur toutes les publications dont l'utilisateur qui exécute cette procédure est propriétaire.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_helppublication](../../relational-databases/replication/codesnippet/tsql/sp-helppublication-trans_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe sysadmin sur le serveur de publication, les membres du rôle de base de données fixe db_owner de la base de données de publication ou les utilisateurs de la liste d'accès aux publications (PAL) peuvent exécuter sp_helppublication.  
  
 Pour un serveur de publication non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seuls les membres du rôle serveur fixe sysadmin sur le serveur de distribution, les membres du rôle de base de données fixe db_owner de la base de données de distribution ou les utilisateurs de la liste d'accès pour cette publication (PAL) peuvent exécuter sp_helppublication.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
