---
title: sp_changepublication (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/29/2017
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
- sp_changepublication
- sp_changepublication_TSQL
helpviewer_keywords:
- sp_changepublication
ms.assetid: c36e5865-25d5-42b7-b045-dc5036225081
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e86b9a77a7b84a54b0e452a2e93dae9167492167
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchangepublication-transact-sql"></a>sp_changepublication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les propriétés d'une publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_changepublication [ [ @publication = ] 'publication' ]  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@property =** ] **'***propriété***'**  
 Propriété de publication à modifier. *propriété* est **nvarchar (255)**.  
  
 [  **@value =** ] **'***valeur***'**  
 Nouvelle valeur de la propriété. *valeur* est **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
 Le tableau ci-dessous décrit les propriétés modifiables de la publication et les limites liées aux valeurs de ces propriétés.  
  
|Propriété|Valeur| Description|  
|--------------|-----------|-----------------|  
|**allow_anonymous**|**true**|Les abonnements anonymes peuvent être créés pour la publication concernée, et *immediate_sync* doit également être **true**. Ne peut pas être modifiée pour les publications d'égal à égal.|  
||**false**|Il n'est pas possible de créer des abonnements anonymes pour la publication concernée. Ne peut pas être modifiée pour les publications d'égal à égal.|  
|**allow_initialize_from_backup**|**true**|Les abonnés peuvent initialiser un abonnement à cette publication à partir d'une sauvegarde au lieu de le faire à partir d'un instantané initial. Cette propriété ne peut pas être modifiée pour non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publications.|  
||**false**|Les abonnés doivent utiliser l'instantané initial. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**allow_partition_switch**|**true**|Les instructions ALTER TABLE…SWITCH peuvent être exécutées sur la base de données publiée. Pour plus d’informations, consultez [Répliquer des tables et des index partitionnés](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).|  
||**false**|Les instructions ALTER TABLE…SWITCH ne peuvent pas être exécutées sur la base de données publiée.|  
|**allow_pull**|**true**|Les abonnements par extraction de données (pull) sont autorisés pour la publication concernée. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**false**|Les abonnements par extraction de données (pull) sont pas autorisés pour la publication concernée. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**allow_push**|**true**|Les abonnements par envoi de données (push) sont autorisés pour la publication concernée.|  
||**false**|Les abonnements par envoi de données (push) ne sont pas autorisés pour la publication concernée.|  
|**allow_subscription_copy**|**true**|Active la possibilité de copier les bases de données qui sont abonnées à la publication. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**false**|Désactive la possibilité de copier les bases de données qui sont abonnées à la publication. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**alt_snapshot_folder**||Emplacement du dossier de remplacement pour l'instantané.|  
|**centralized_conflicts**|**true**|Les enregistrements en conflit sont stockés sur le serveur de publication. Ne peut être modifiée qu'en l'absence d'abonnements actifs. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**false**|Les enregistrements en conflit sont stockés à la fois sur le serveur de publication et sur l'Abonné ayant généré le conflit. Ne peut être modifiée qu'en l'absence d'abonnements actifs. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**compress_snapshot**|**true**|Un instantané dans un dossier d'instantanés de remplacement est compressé au format de fichier .cab. L'instantané se trouvant dans le dossier d'instantané par défaut ne peut pas être compressé.|  
||**false**|L'instantané n'est pas compressé ; c'est le comportement par défaut de la réplication.|  
|**conflict_policy**|**wins de pub**|Politique de résolution des conflits de mise à jour des Abonnés dans laquelle le serveur de publication l'emporte dans le conflit. Cette propriété ne peut être modifiée qu'en l'absence d'abonnements actifs. Non pris en charge pour les serveurs de publication Oracle.|  
||**sub reinit**|Pour la mise à jour des Abonnés, en cas de conflit, l'abonnement doit être réinitialisé. Cette propriété ne peut être modifiée qu'en l'absence d'abonnements actifs. Non pris en charge pour les serveurs de publication Oracle.|  
||**Sub wins**|Politique de résolution des conflits de mise à jour des Abonnés dans laquelle l'Abonné l'emporte dans le conflit. Cette propriété ne peut être modifiée qu'en l'absence d'abonnements actifs. Non pris en charge pour les serveurs de publication Oracle.|  
|**conflict_retention**||**int** qui spécifie la période de rétention de conflit, en jours. La rétention par défaut est de 14 jours. **0** signifie qu’aucun nettoyage de conflit n’est nécessaire. Non pris en charge pour les serveurs de publication Oracle.|  
|**description**||Entrée facultative décrivant la publication.|  
|**enabled_for_het_sub**|**true**|Permet à la publication de prendre en charge des Abonnés non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **enabled_for_het_sub** ne peut pas être modifié lorsqu’il existe des abonnements à la publication. Vous devrez peut-être exécuter [procédures stockées de réplication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) pour se conformer aux exigences suivantes avant de définir **enabled_for_het_sub** sur true :<br /> - **allow_queued_tran** doit être **false**.<br /> - **allow_sync_tran** doit être **false**.<br /> La modification **enabled_for_het_sub** à **true** peut modifier les paramètres de publication existant. Pour plus d’informations, consultez [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md). Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**false**|La publication ne prend pas en charge les Abonnés non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**enabled_for_internet**|**true**|La publication est activée pour Internet, et le protocole FTP peut être utilisé pour le transfert des fichiers d'instantané vers un abonné. Les fichiers de synchronisation de la publication sont placés dans le répertoire suivant : C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\ftp. *ftp_address* ne peut pas être NULL. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**false**|La publication n'est pas activée pour Internet. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**enabled_for_p2p**|**true**|La publication prend en charge la réplication d'égal à égal. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /> Pour définir **enabled_for_p2p** à **true**, les restrictions suivantes s’appliquent :<br /> - **allow_anonymous** doit être **false**<br /> - **allow_dts** doit être **false**.<br /> - **allow_initialize_from_backup** doit être **true**<br /> - **allow_queued_tran** doit être **false**.<br /> - **allow_sync_tran** doit être **false**.<br /> - **enabled_for_het_sub** doit être **false**.<br /> - **independent_agent** doit être **true**.<br /> - **repl_freq** doit être **continue**.<br /> - **replicate_ddl** doit être **1**.|  
||**false**|La publication ne prend pas en charge la réplication d'égal à égal. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ftp_address**||Emplacement accessible par FTP des fichiers d'instantané de la publication. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ftp_login**||Nom d'utilisateur permettant la connexion au service FTP ; la valeur ANONYMOUS est autorisée. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ftp_password**||Mot de passe associé au nom d'utilisateur utilisé pour la connexion au service FTP. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ftp_port**||Numéro de port du service FTP du serveur de distribution. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ftp_subdirectory**||Spécifie où sont créés les fichiers de capture instantanée si la publication prend en charge la propagation d’instantanés via FTP. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**immediate_sync**|**true**|Les fichiers de synchronisation sont créés ou recréés à chaque exécution de l'Agent d'instantané. Les Abonnés peuvent obtenir les fichiers de synchronisation immédiatement après l'abonnement si l'Agent d'instantané a été exécuté une fois avant l'abonnement. Les nouveaux abonnements obtiennent les fichiers de synchronisation les plus récents, générés lors de la dernière exécution de l'Agent d'instantané. *independent_agent* doit également être **true**. Consultez les remarques ci-dessous pour plus d’informations sur les **immediate_sync**.|  
||**false**|Les fichiers de synchronisation sont créés uniquement lors de nouveaux abonnements. Les Abonnés ne peuvent recevoir les fichiers de synchronisation après s'être abonnés qu'après le lancement et l'exécution de l'Agent d'instantané.|  
|**independent_agent**|**true**|La publication a son propre Agent de distribution dédié.|  
||**false**|La publication utilise un Agent de distribution partagé, et chaque paire base de données de publication/base de données d'abonnement a un Agent partagé.|  
|**p2p_continue_onconflict**|**true**|L'Agent de distribution continue à traiter les modifications lorsqu'un conflit est détecté.<br /> **Attention :** nous vous recommandons d’utiliser la valeur par défaut `FALSE`. Lorsque cette option a la valeur `TRUE`, l’Agent de Distribution tente de converger les données de la topologie en appliquant la ligne en conflit à partir du nœud qui possède l’ID de donneur d’ordre le plus élevé. Cette méthode ne garantit pas la convergence. Vous devez vous assurer que la topologie est cohérente après la détection d'un conflit. Pour plus d'informations, consultez « Gestion des conflits » dans [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
||**false**|L'Agent de distribution cesse de traiter les modifications lorsqu'un conflit est détecté.|  
|**post_snapshot_script**||Spécifie l'emplacement d'un fichier de script [!INCLUDE[tsql](../../includes/tsql-md.md)] que l'Agent de distribution exécute après l'application de tous les autres scripts et données d'objet répliqués au cours d'une synchronisation initiale.|  
|**pre_snapshot_script**||Spécifie l'emplacement d'un fichier de script [!INCLUDE[tsql](../../includes/tsql-md.md)] que l'Agent de distribution exécute avant l'application de tous les autres scripts et données d'objet répliqués au cours d'une synchronisation initiale.|  
|**publish_to_ActiveDirectory**|**true**|Ce paramètre est déconseillé et il n'est pris en charge que pour la compatibilité descendante des scripts. Vous ne pouvez plus ajouter d'informations de publication à [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.|  
||**false**|Supprime les informations de publication d'Active Directory.|  
|**queue_type**|**sql**|Utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker les transactions. Cette propriété ne peut être modifiée qu'en l'absence d'abonnements actifs.<br /><br /> : La prise en charge à l’aide de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing a été arrêtée. La valeur de **msmq** pour *valeur* génère une erreur.|  
|**repl_freq**|**Continue**|Publie la sortie de toutes les transactions enregistrées dans le journal.|  
||**Capture instantanée**|Publie uniquement les événements de synchronisation planifiés.|  
|**replicate_ddl**|**1**|Les instructions DDL (Data Definition Language) exécutées sur le serveur de publication sont répliquées. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**0**|Les instructions DDL ne sont pas répliquées. Cette propriété ne peut pas être modifiée pour les publications non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La réplication des modifications de schéma ne peut pas être désactivée lorsque la réplication d'égal à égal est utilisée.|  
|**replicate_partition_switch**|**true**|Les instructions ALTER TABLE…SWITCH exécutées sur la base de données publiée doivent être répliquées sur les Abonnés. Cette option est valide uniquement si *allow_partition_switch* est définie sur TRUE. Pour plus d’informations, consultez [Répliquer des tables et des index partitionnés](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).|  
||**false**|Les instructions ALTER TABLE…SWITCH ne doivent pas être répliquées sur les Abonnés.|  
|**retention**||**int** représentant la période de rétention, en heures, à l’activité d’abonnement. Si un abonnement n'est pas actif durant la période de rétention, il est supprimé.|  
|**snapshot_in_defaultfolder**|**true**|Les fichiers d'instantané sont stockés dans le dossier d'instantané par défaut. Si *alt_snapshot_folder*est également spécifiée, les fichiers d’instantanés sont stockés dans la valeur par défaut et autres emplacements.|  
||**false**|Fichiers d’instantanés sont stockés dans l’emplacement secondaire spécifié par *alt_snapshot_folder*.|  
|**status**|**Active**|Les données de publication sont disponibles immédiatement pour les Abonnés lors de la création de la publication. Non pris en charge pour les serveurs de publication Oracle.|  
||**Inactif**|Les données de publication ne sont pas disponibles pour les Abonnés lors de la création de la publication. Non pris en charge pour les serveurs de publication Oracle.|  
|**sync_method**|**native**|Utilise la copie en bloc en mode natif de toutes les tables lors de la synchronisation des abonnements.|  
||**character**|Utilise la copie en bloc en mode caractère de toutes les tables lors de la synchronisation des abonnements.|  
||**simultanées**|Utilise la copie en bloc en mode natif de toutes les tables, mais ne verrouille pas les tables au cours de l'instantané. Non valide pour la réplication d'instantané.|  
||**concurrent_c**|Utilise la copie en bloc en mode caractère de toutes les tables, mais ne verrouille pas les tables au cours du processus de génération de l'instantané. Non valide pour la réplication d'instantané.|  
|**identificateur de tâche**||Cette propriété est déconseillée et n'est plus prise en charge.|  
|**allow_drop**|**true**|Permet de `DROP TABLE` DLL prend en charge pour les articles qui font partie de la réplication transactionnelle. Version de la prise en charge minimale : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Service Pack 2 ou version ultérieure et [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Service Pack 1 ou version ultérieure. Référence supplémentaire : [Ko 3170123](https://support.microsoft.com/en-us/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactional-replication-in-sql-server-2014-or-in-sql-server-2016-sp1)|
||**false**|Désactive `DROP TABLE` DLL prend en charge pour les articles qui font partie de la réplication transactionnelle. Il s’agit de la **par défaut** valeur de cette propriété.|
|**NULL** (par défaut)||Retourne la liste des valeurs prises en charge pour *propriété*.|  
  
[  **@force_invalidate_snapshot =** ] *force_invalidate_snapshot*  
 Signale que l'action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  - **0** Spécifie que les modifications de l’article n’invalident pas l’instantané n’est pas valide. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  - **1** Spécifie que les modifications apportées à l’article peuvent invalider l’instantané n’est pas valide. Si certains abonnements existants nécessitent un nouvel instantané, cette valeur autorise le marquage de l'instantané existant comme obsolète, et la génération d'un nouvel instantané.   
Consultez la section Remarques pour connaître les propriétés dont la modification nécessite la génération d'un nouvel instantané.  
  
[ **@force_reinit_subscription =** ] *force_reinit_subscription*  
 Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bits** avec une valeur par défaut **0**.  
  - **0** Spécifie que les modifications de l’article ne provoquent pas la réinitialisation des abonnements. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  - **1** indique que les modifications apportées à l’article entraînent la réinitialisation des abonnements existants et autorise la réinitialisation des abonnements se produise.  
  
[ **@publisher** =] **'***publisher***'**  
 Spécifie un serveur de publication non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
  > [!NOTE]  
  >  *serveur de publication* ne doit pas être utilisé lors de la modification des propriétés de l’article sur un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changepublication** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
 Après avoir modifié les propriétés suivantes, vous devez générer un nouvel instantané, et vous devez spécifier une valeur de **1** pour le *force_invalidate_snapshot* paramètre.  
-   **alt_snapshot_folder**  
-   **compress_snapshot**  
-   **enabled_for_het_sub**  
-   **ftp_address**  
-   **ftp_login**  
-   **ftp_password**  
-   **ftp_port**  
-   **ftp_subdirectory**  
-   **post_snapshot_script**  
-   **pre_snapshot_script**  
-   **snapshot_in_defaultfolder**  
-   **sync_mode**  
  
Objets de publication Active Directory à l’aide du **publish_to_active_directory** paramètre, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objet doit déjà être créé dans Active Directory.  
  
## <a name="impact-of-immediate-sync"></a>Impact de la synchronisation immédiate  
 Lorsque la synchronisation immédiate est activée, toutes les modifications dans le journal sont suivies immédiatement après que la capture instantanée initiale est générée même si aucun abonnement. Modifications journalisées sont utilisées lorsqu’un client est à l’aide de sauvegarde pour ajouter un nouveau nœud homologue. Une fois la sauvegarde est restaurée, l’homologue est synchronisé avec les autres modifications qui se produisent après que la sauvegarde a été effectuée. Étant donné que les commandes sont suivies dans la base de données de distribution, la logique de synchronisation peut examiner le dernier LSN de sauvegarde et l’utiliser comme point de départ, sachant que la commande est disponible si la sauvegarde a été effectuée au sein de la période de rétention maximale. (La valeur par défaut pour la période de rétention minimale est 0 période de rétention des heures et le nombre maximal est de 24 heures.)  
  
 Lorsque la synchronisation immédiate est désactivée, les modifications sont conservées au moins la période de rétention minimale et immédiatement nettoyées pour toutes les transactions qui sont déjà répliquées. Si la synchronisation immédiate est désactivée et configurée avec la période de rétention par défaut, il est probable que les modifications nécessaires après la sauvegarde aient été nettoyées et que le nouveau nœud homologue ne soit pas correctement initialisé. La seule option conssite à suspendre la topologie. L'activation de la synchronisation immédiate offre davantage de flexibilité et est le paramètre recommandé pour la réplication P2P.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_changepublication](../../relational-databases/replication/codesnippet/tsql/sp-changepublication-tra_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_changepublication**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
