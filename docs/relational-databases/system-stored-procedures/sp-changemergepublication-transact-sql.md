---
title: sp_changemergepublication (Transact-SQL) | Documents Microsoft
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
- sp_changemergepublication_TSQL
- sp_changemergepublication
helpviewer_keywords:
- sp_changemergepublication
ms.assetid: 81fe1994-7678-4852-980b-e02fedf1e796
caps.latest.revision: 44
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 016105db76d618ed2753fc95dc5a7ee5d1288b14
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchangemergepublication-transact-sql"></a>sp_changemergepublication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les propriétés d'une publication de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changemergepublication [ @publication= ] 'publication'  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@property=**] **'***propriété***'**  
 Propriété à modifier pour la publication donnée. *propriété* est **sysname**, et peut être une des valeurs répertoriée dans le tableau suivant.  
  
 [  **@value=**] **'***valeur***'**  
 Nouvelle valeur de la propriété spécifiée. *valeur* est **nvarchar (255)**, et peut être une des valeurs répertoriée dans le tableau suivant.  
  
 Le tableau ci-dessous décrit les propriétés modifiables de la publication ainsi que les limites liées aux valeurs de ces propriétés.  
  
|Propriété|Valeur| Description|  
|--------------|-----------|-----------------|  
|**allow_anonymous**|**true**|Les abonnements anonymes sont autorisés.|  
||**false**|Les abonnements anonymes ne sont pas autorisés.|  
|**allow_partition_realignment**|**true**|Les opérations de suppression sont envoyées à l'Abonné pour refléter les résultats d'une modification de partition en supprimant des données qui ne font plus partie de la partition de l'Abonné. Il s'agit du comportement par défaut.|  
||**false**|Les données d'une ancienne partition demeurent sur l'Abonné : les modifications apportées à ces données sur le serveur de publication ne sont pas répliquées sur l'Abonné. À la place, les modifications apportées à l'Abonné sont répliquées vers le serveur de publication. Cela permet de conserver dans un abonnement des données issues d'une ancienne partition afin qu'elles soient accessibles à des fins d'historique.|  
|**allow_pull**|**true**|Les abonnements par extraction de données (pull) sont autorisés pour la publication concernée.|  
||**false**|Les abonnements par extraction de données (pull) sont pas autorisés pour la publication concernée.|  
|**allow_push**|**true**|Les abonnements par envoi de données (push) sont autorisés pour la publication concernée.|  
||**false**|Les abonnements par envoi de données (push) ne sont pas autorisés pour la publication concernée.|  
|**allow_subscriber_initiated_snapshot**|**true**|L'Abonné peut initier le processus d'instantané.|  
||**false**|L'Abonné ne peut pas initier le processus d'instantané.|  
|**allow_subscription_copy**|**true**|Vous pouvez copier les bases de données d'abonnement qui sont abonnées à la publication.|  
||**false**|Vous ne pouvez pas copier les bases de données d'abonnement qui sont abonnées à la publication.|  
|**allow_synctoalternate**|**true**|Permet à un serveur partenaire de synchronisation différent de se synchroniser avec le serveur de publication.|  
||**false**|Ne permet pas à un serveur partenaire de synchronisation différent de se synchroniser avec le serveur de publication.|  
|**allow_web_synchronization**|**true**|Les abonnements peuvent être synchronisés via HTTPS.|  
||**false**|Les abonnements ne peuvent pas être synchronisés via HTTPS.|  
|**alt_snapshot_folder**||Spécifie l'emplacement de l'autre dossier de l'instantané.|  
|**automatic_reinitialization_policy**|**1**|Les modifications sont téléchargées depuis l'Abonné avant que l'abonnement ne soit réinitialisé.|  
||**0**|L'abonnement est réinitialisé sans téléchargement préalable des modifications.|  
|**centralized_conflicts**|**true**|Tous les enregistrements en conflit sont stockés sur le serveur de publication. Vous devez réinitialiser les abonnés existants si vous modifiez cette propriété.|  
||**false**|Les enregistrements en conflit sont stockés sur le serveur qui est sorti perdant de la résolution du conflit. Vous devez réinitialiser les abonnés existants si vous modifiez cette propriété.|  
|**compress_snapshot**|**true**|L'instantané se trouvant dans un dossier d'instantané de remplacement est compressé au format CAB. L'instantané se trouvant dans le dossier d'instantané par défaut ne peut pas être compressé. La modification de cette propriété requiert un nouvel instantané.|  
||**false**|L'instantané n'est pas compressé par défaut. La modification de cette propriété requiert un nouvel instantané.|  
|**conflict_logging**|**publisher** (serveur de publication)|Les enregistrements en conflit sont stockés sur le serveur de publication.|  
||**subscriber** (Abonné)|Les enregistrements en conflit sont stockés dans l'Abonné à l'origine du conflit. Non pris en charge pour [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés *.*|  
||**both** (les deux)|Les enregistrements en conflit sont stockés dans le serveur de publication et l'Abonné.|  
|**conflict_retention**||Un **int** qui spécifie la période de rétention en jours, pendant laquelle les conflits sont conservés. Paramètre *conflict_retention* à **0** signifie aucun nettoyage de conflit n’est nécessaire.|  
|**description**||Description de la publication.|  
|**dynamic_filters**|**true**|La publication est filtrée sur une clause dynamique.|  
||**false**|La publication n'est pas filtrée dynamiquement.|  
|**enabled_for_internet**|**true**|La publication est activée pour Internet. Le protocole FTP (File Transfer Protocol) peut être utilisé pour le transfert des fichiers d'instantané vers un Abonné. Les fichiers de synchronisation de la publication sont placés dans le répertoire C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\ftp.|  
||**false**|La publication n'est pas activée pour Internet.|  
|**ftp_address**||L’adresse réseau du service FTP du serveur de distribution. Indique l'emplacement de stockage des fichiers d'instantané de la publication.|  
|**ftp_login**||Le nom d’utilisateur qui est utilisé pour se connecter au service FTP.|  
|**ftp_password**||Mot de passe utilisateur utilisé pour la connexion au service FTP.|  
|**ftp_port**||Le numéro de port du service FTP du serveur de distribution. Indique le numéro de port TCP du site FTP où sont stockés les fichiers d'instantané de la publication.|  
|**ftp_subdirectory**||Indique l'emplacement où sont créés les fichiers d'instantanés si la publication prend en charge la propagation d'instantanés par FTP.|  
|**generation_leveling_threshold**|**int**|Spécifie le nombre de modifications qui sont contenus dans une génération. Une génération est une collection de modifications remises à un serveur de publication ou à un Abonné.|  
|**keep_partition_changes**|**true**|La synchronisation est optimisée et seuls les abonnés qui détiennent des lignes dans les partitions modifiées sont concernés. La modification de cette propriété requiert un nouvel instantané.|  
||**false**|La synchronisation n'est pas optimisée et les partitions envoyées à tous les abonnés seront vérifiées lors de la modification de leurs données. La modification de cette propriété requiert un nouvel instantané.|  
|**max_concurrent_merge**||Il s’agit d’un **int** qui représente le nombre maximal de processus de fusion simultanés qui peuvent être exécutées sur une publication. Si ce nombre est 0, il n'y a pas de limite. Si ce nombre est supérieur au nombre planifié de processus de fusion à exécuter simultanément, les travaux en trop sont placés dans une file d'attente jusqu'à ce qu'un processus de fusion en cours d'exécution s'achève.|  
|**max_concurrent_dynamic_snapshots**||Il s’agit d’un **int** que représente le nombre maximal de sessions d’instantané pour générer des données filtrées de capture instantanée qui peut exécuter simultanément sur une publication de fusion qui utilise des filtres de lignes paramétrables. Si **0**, il n’existe aucune limite. Si ce nombre est supérieur au nombre planifié de processus d'instantané à exécuter simultanément, les travaux en trop sont placés dans une file d'attente jusqu'à ce qu'un processus de fusion en cours d'exécution s'achève.|  
|**post_snapshot_script**||Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de distribution ou de fusion exécute le script de post-instantané après que tous les autres scripts d'objets et données répliqués ont été appliqués lors d'une synchronisation initiale. La modification de cette propriété requiert un nouvel instantané.|  
|**pre_snapshot_script**||Spécifie un pointeur vers un **.sql** emplacement du fichier. L'Agent de fusion exécute le script de pré-instantané avant tous les scripts d'objets répliqués, lors de l'application d'un instantané chez un abonné. La modification de cette propriété requiert un nouvel instantané.|  
|**publication_compatibility_level**|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
||**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**publish_to_activedirectory**|**true**|Ce paramètre est déconseillé et il n'est pris en charge que pour la compatibilité descendante des scripts. Vous ne pouvez plus ajouter d'informations de publication dans Active Directory.|  
||**false**|Supprime les informations de publication d'Active Directory.|  
|**replicate_ddl**|**1**|Instructions de définition Language (DDL) de données qui sont exécutées sur le serveur de publication sont répliquées.|  
||**0**|Les instructions DDL ne sont pas répliquées.|  
|**retention**||Il s’agit d’un **int** qui représente le nombre de *retention_period_unit* unités pour lequel enregistrer les modifications pour la publication concernée. L'abonnement expire et doit être réinitialisé s'il n'est pas synchronisé pendant la période de rétention et que les modifications en attente qu'il aurait dû recevoir ont été supprimées par une opération de nettoyage sur le serveur de distribution. La période de rétention maximale autorisée correspond au nombre de jours entre la date actuelle et le 31 décembre 9999.<br /><br /> Remarque : La période de rétention pour les publications de fusion a une période de grâce de 24 heures pour prendre en charge les abonnés dans des fuseaux horaires différents.|  
|**retention_period_unit**|**day**|La période de rétention est spécifiée en jours.|  
||**week**|La période de rétention est spécifiée en semaines.|  
||**month**|La période de rétention est spécifiée en mois.|  
||**year**|La période de rétention est spécifiée en années.|  
|**snapshot_in_defaultfolder**|**true**|Les fichiers d'instantané sont stockés dans le dossier d'instantané par défaut.|  
||**false**|Fichiers d’instantanés sont stockés dans l’emplacement secondaire spécifié par *alt_snapshot_folder*. Cette combinaison indique que les fichiers d'instantané sont stockés dans les emplacements par défaut et de remplacement.|  
|**snapshot_ready**|**true**|L'instantané de la publication est disponible.|  
||**false**|L'instantané de la publication n'est pas disponible.|  
|**status**|**Active**|La publication est dans un état actif.|  
||**Inactif**|La publication est dans un état inactif.|  
|**sync_mode**|**native** ou<br /><br /> **bcp natif**|La sortie programme de la copie en bloc en mode natif de toutes les tables est utilisée pour l'instantané initial.|  
||**character**<br /><br /> ou **bcp caractère**|La sortie programme de la copie en bloc en mode caractère de toutes les tables est utilisée pour l'instantané initial, ce qui est requis pour tous les Abonnés non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**use_partition_groups**<br /><br /> Remarque : après avoir utilisé partition_groups, si vous revenez à l’utilisation **« setupbelongs »**et la valeur **use_partition_groups = false** dans **changemergearticle**, cela ne peut-être pas reflété correctement après la capture instantanée. Les déclencheurs générés par l'instantané sont conformes avec les groupes de partition.<br /><br /> La solution à ce scénario consiste à définir l’état inactif, modifiez le **use_partition_groups**, puis définissez le statut actif.|**true**|La publication utilise des partitions précalculées.|  
||**false**|La publication n'utilise pas de partitions précalculées.|  
|**validate_subscriber_info**||Répertorie les fonctions utilisées pour extraire des informations d'Abonné. Puis, valide les critères de filtrage dynamiques utilisés pour l'Abonné pour vérifier que les informations sont partitionnées régulièrement.|  
|**web_synchronization_url**||Valeur par défaut de l'URL Internet utilisée pour la synchronisation Web.|  
|NULL (par défaut)||Retourne la liste des valeurs prises en charge pour *propriété*.|  
  
 [  **@force_invalidate_snapshot =** ] *force_invalidate_snapshot*  
 Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que la modification de la publication n’invalide l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** Spécifie que la modification de la publication peut invvalidate l’instantané. S’il existe des abonnements existants qui nécessiteraient un nouvel instantané, donne l’autorisation pour l’instantané existant être marqué comme obsolète et un nouvel instantané doit être créé.  
  
 Consultez la section Notes pour les propriétés, modification requiert un nouvel instantané doit être créé.  
  
 [  **@force_reinit_subscription =** ] *force_reinit_subscription*  
 Confirme que l'action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bits** avec une valeur par défaut **0**.  
  
 **0** Spécifie que la modification de la publication ne requiert pas que les abonnements soient réinitialisés. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** Spécifie que si vous modifiez la publication entraînent la réinitialisation des abonnements existants et autorise la réinitialisation des abonnements se produise.  
  
 Voir la section Remarques pour connaître les propriétés dont la modification requiert la réinitialisation de tous les abonnements existants.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changemergepublication** est utilisé dans la réplication de fusion.  
  
 La modification des propriétés suivantes requiert qu'un nouvel instantané soit généré. Vous vous devez spécifier une valeur de **1** pour le *force_invalidate_snapshot* paramètre.  
  
-   **alt_snapshot_folder**  
  
-   **compress_snapshot**  
  
-   **dynamic_filters**  
  
-   **ftp_address**  
  
-   **ftp_login**  
  
-   **ftp_password**  
  
-   **ftp_port**  
  
-   **ftp_subdirectory**  
  
-   **post_snapshot_script**  
  
-   **publication_compatibility_level** (pour **80SP3** uniquement)  
  
-   **pre_snapshot_script**  
  
-   **snapshot_in_defaultfolder**  
  
-   **sync_mode**  
  
-   **use_partition_groups**  
  
 Vous devez réinitialiser les abonnements existants pour modifier les propriétés suivantes. Vous devez spécifier une valeur de **1** pour le *force_reinit_subscription* paramètre.  
  
-   **dynamic_filters**  
  
-   **validate_subscriber_info**  
  
 Objets de publication à Active Directory à l’aide de la *publish_to_active_directory*, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objet doit déjà être créé dans Active Directory.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_changemergepublication](../../relational-databases/replication/codesnippet/tsql/sp-changemergepublicatio_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_changemergepublication**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
