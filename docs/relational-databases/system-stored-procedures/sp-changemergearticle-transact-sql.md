---
title: sp_changemergearticle (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/09/2015
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
- sp_changemergearticle_TSQL
- sp_changemergearticle
helpviewer_keywords:
- sp_changemergearticle
ms.assetid: 0dc3da5c-4af6-45be-b5f0-074da182def2
caps.latest.revision: 74
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b778c6bad14bb756ddb8a8e24b1d7e95a87956d4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchangemergearticle-transact-sql"></a>sp_changemergearticle (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les propriétés d'un article de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changemergearticle [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication dans laquelle existe l'article. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article à modifier. *article* est **sysname**, sans valeur par défaut.  
  
 [  **@property=**] **'***propriété***'**  
 Propriété à modifier pour l'article et la publication donnés. *propriété* est **nvarchar (30)**, et peut prendre l’une des valeurs répertoriées dans le tableau.  
  
 [  **@value=**] **'***valeur***'**  
 Est la nouvelle valeur pour la propriété spécifiée. *valeur* est **nvarchar (1000)**, et peut prendre l’une des valeurs répertoriées dans le tableau.  
  
 Le tableau ci-dessous décrit les propriétés des articles et les valeurs de ces propriétés.  
  
|Propriété|Valeurs| Description|  
|--------------|------------|-----------------|  
|**allow_interactive_resolver**|**true**|Permet d'utiliser un outil de résolution interactif pour l'article.|  
||**false**|Désactive l'utilisation d'un outil de résolution interactif pour l'article.|  
|**article_resolver**||Outil de résolution personnalisé pour l'article S’applique uniquement à un article de table.|  
|**check_permissions** (bitmap)|**0 x 00**|Les autorisations au niveau de la table ne sont pas vérifiées.|  
||**0 x 10**|Les autorisations au niveau de la table sont vérifiées dans le serveur de publication avant que les instructions INSERT effectuées sur l'Abonné ne soient appliquées dans le serveur de publication.|  
||**0 x 20**|Les autorisations au niveau de la table sont vérifiées dans le serveur de publication avant que les instructions UPDATE effectuées sur l'Abonné ne soient appliquées dans le serveur de publication.|  
||**0 x 40**|Les autorisations au niveau de la table sont vérifiées sur le serveur de publication avant que les instructions DELETE effectuées sur l'Abonné ne soient appliquées dans le serveur de publication.|  
|**column_tracking**|**true**|Active le suivi au niveau de la colonne. S’applique uniquement à un article de table.<br /><br /> Remarque : Suivi au niveau colonne ne peut pas être utilisé lors de la publication des tables avec plus de 246 colonnes.|  
||**false**|Désactive le suivi au niveau de la colonne et conserve la détection des conflits au niveau de la ligne. S’applique uniquement à un article de table.|  
|**compensate_for_errors**|**true**|Des actions de compensation sont effectuées lorsque des erreurs se produisent au cours de la synchronisation. Pour plus d’informations, consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
||**false**|Les actions de compensation ne sont pas effectuées, ce qui est le comportement par défaut. Pour plus d’informations, consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).<br /><br /> **\*\* Important \* \***  bien que les données des lignes affectées peuvent sembler être hors de convergence, dès que vous résolvez les éventuelles erreurs, modifications peuvent être appliquées et les données convergent. Si la table source pour un article est déjà publiée dans une autre publication, la valeur de *compensate_for_errors* doivent être identiques pour les deux articles.|  
|**creation_script**||Chemin d'accès et nom d'un script de schéma d'article facultatif utilisé pour créer l'article dans la base de données d'abonnement.|  
|**delete_tracking**|**true**|Les instructions DELETE sont répliquées, ce qui est le comportement par défaut.|  
||**false**|Les instructions DELETE ne sont pas répliquées.<br /><br /> **\*\* Important \* \***  paramètre **delete_tracking** à **false** entraîne une non-convergence et supprimés les lignes doivent être supprimées manuellement.|  
|**description**||Entrée descriptive de l'article|  
|**destination_owner**||Nom du propriétaire de l’objet dans la base de données abonnement, dans le cas contraire **dbo**.|  
|**identity_range**||**bigint** qui spécifie la taille de la plage à utiliser lors de l’attribution de nouvelles valeurs d’identité si l’article a **identityrangemanagementoption** la valeur **automatique** ou **auto_identity_ plage** la valeur **true**. S'applique à un article de table uniquement. Pour plus d’informations, consultez la section « Réplication de fusion » de [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**identityrangemanagementoption**|**Manuelle**|Désactive la gestion automatique des plages d'identité. Marque les colonnes d'identité en utilisant NOT FOR REPLICATION pour permettre la gestion manuelle des plages d'identité. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
||**Aucun**|Désactive toute la gestion des plages d'identité.|  
|**logical_record_level_conflict_detection**|**true**|Un conflit est détecté si des modifications sont apportées à un point quelconque de l'enregistrement logique. Requiert que **logical_record_level_conflict_resolution** avoir la valeur **true**.|  
||**false**|Détection de conflit par défaut est utilisée comme spécifié par **column_tracking**.|  
|**logical_record_level_conflict_resolution**|**true**|L'enregistrement logique gagnant complet remplace l'enregistrement logique perdant.|  
||**false**|Les lignes gagnantes ne sont pas limitées à l'enregistrement logique.|  
|**partition_options**|**0**|Le filtrage de l'article est statique ou il ne produit pas un sous-ensemble unique de données pour chaque partition, c'est-à-dire une partition en « chevauchement ».|  
||**1**|Les partitions se chevauchent, et les mises à jour DML effectuées sur l'Abonné ne peuvent pas modifier la partition à laquelle une ligne appartient.|  
||**2**|Le filtrage de l'article produit des partitions qui ne se chevauchent pas, mais plusieurs abonnés peuvent recevoir la même partition.|  
||**3**|Le filtrage de l'article produit des partitions qui ne se chevauchent pas et qui sont uniques pour chaque abonnement.<br /><br /> Remarque : Si vous spécifiez une valeur de **3** pour **partition_options**, il peut y avoir qu’un seul abonnement pour chaque partition de données dans cet article. Si un deuxième abonnement est créé dans lequel le critère de filtrage du nouvel abonnement produit la même partition que l'abonnement existant, ce dernier est supprimé.|  
|**pre_creation_command**|**Aucun**|Si la table existe déjà côté abonné, aucune action n'est effectuée.|  
||**delete**|Entraîne une suppression basée sur la clause WHERE dans le filtre de sous-ensemble.|  
||**DROP**|Supprime la table avant de la recréer.|  
||**truncate**|Tronque la table de destination.|  
|**processing_order**||**int** qui indique l’ordre de traitement des articles dans une publication de fusion.|  
|**pub_identity_range**||**bigint** qui spécifie la taille de plage allouée à un abonné disposant d’un abonnement serveur si l’article a **identityrangemanagementoption** la valeur **automatique** ou **auto_ identity_range** la valeur **true**. Cette plage d'identité est réservée à un Abonné de republication qui peut l'allouer à ses propres Abonnés. S'applique à un article de table uniquement. Pour plus d’informations, consultez la section « Réplication de fusion » de [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**published_in_tran_pub**|**true**|L'article est également publié dans une publication transactionnelle.|  
||**false**|L'article n'est pas également publié dans une publication transactionnelle.|  
|**resolver_info**||Permet de définir les informations supplémentaires requises par un outil de résolution personnalisé. Certains outils de résolution [!INCLUDE[msCoName](../../includes/msconame-md.md)] nécessitent une colonne en guise d'entrée. **resolver_info** est **nvarchar (255)**, avec NULL comme valeur par défaut. Pour plus d’informations, consultez [Programmes de résolution COM Microsoft](../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).|  
|**schema_option** (bitmap)||Pour plus d’informations, consultez la section Notes plus loin dans cette rubrique.|  
||**0 x 00**|Désactive la génération de scripts par l’Agent d’instantané et utilise le script fourni dans **creation_script**.|  
||**0 x 01**|Génère le script de création d'objet (CREATE TABLE, CREATE PROCEDURE, etc.).|  
||**0 x 10**|Génère un index cluster correspondant.|  
||**0 x 20**|Convertit les types de données définis par l'utilisateur en types de données de base auprès de l'Abonné. Cette option ne peut pas être utilisée lorsqu’il existe une contrainte CHECK ou DEFAULT sur une colonne de type défini par l’utilisateur (UDT), si une colonne UDT fait partie de la clé primaire, ou si une colonne calculée fait référence à une colonne UDT.|  
||**0 x 40**|Génère les index non-cluster correspondants.|  
||**0 x 80**|Inclut l'intégrité référentielle déclarée dans les clés primaires.|  
||**0 x 100**|Réplique les déclencheurs utilisateur, si ceux-ci sont définis, sur un article de table.|  
||**0 x 200**|Réplique les contraintes FOREIGN KEY. Si la table référencée ne fait pas partie d'une publication, aucune contrainte FOREIGN KEY appliquée à une table publiée n'est répliquée.|  
||**0 x 400**|Réplique les contraintes CHECK.|  
||**0 x 800**|Réplique les valeurs par défaut.|  
||**0 x 1000**|Réplique le classement au niveau des colonnes.|  
||**0 x 2000**|Réplique les propriétés étendues associées à l'objet source de l'article publié.|  
||**0 x 4000**|Réplique les clés uniques, si celles-ci sont définies, sur un article de table.|  
||**0 x 8000**|Génère des instructions ALTER TABLE lors de la création d'un script de contraintes.|  
||**0 x 10000**|Réplique les contraintes CHECK en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0 x 20000**|Réplique les contraintes FOREIGN KEY en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0 x 40000**|Réplique les groupes de fichiers associés à une table ou un index partitionné.|  
||**0 x 80000**|Réplique le schéma de partition d'une table partitionnée.|  
||**0 x 100000**|Réplique le schéma de partition d'un index partitionné.|  
||**0 x 200000**|Réplique les statistiques d'une table.|  
||**0 x 400000**|Réplique des liaisons par défaut|  
||**0x800000**|Réplique des liaisons de règle.|  
||**0 x 1000000**|Réplique l'index de texte intégral.|  
||**0x2000000**|Collections de schémas XML lié à **xml** colonnes ne sont pas répliquées.|  
||**0x4000000**|Réplique les index sur **xml** colonnes.|  
||**0 x 8000000**|Crée tout schéma non encore présent chez l'abonné.|  
||**0 x 10000000**|Convertit **xml** colonnes à **ntext** sur l’abonné.|  
||**0 x 20000000**|Types de données de l’objet convertit de grande taille (**nvarchar (max)**, **varchar (max)**, et **varbinary (max)**) qui ont été introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] aux types de données qui sont prises en charge sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)].|  
||**0 x 40000000**|Réplique les autorisations.|  
||**0 x 80000000**|Tente de supprimer les dépendances envers tous les objets qui ne font pas partie de la publication.|  
||**0 x 100000000**|Utilisez cette option pour répliquer l’attribut FILESTREAM s’il est spécifié sur **varbinary (max)** colonnes. Ne spécifiez pas cette option si vous répliquez des tables sur des Abonnés [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Réplication de tables qui possèdent des colonnes FILESTREAM sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] abonnés n'est pas pris en charge, quelle que soit la manière dont cette option de schéma. Consultez l’option connexe **0 x 800000000**.|  
||**0x200000000**|Convertit les types de données de date et heure (**date**, **temps**, **datetimeoffset**, et **datetime2**) qui sont introduits dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] aux types de données qui sont prises en charge dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**0x400000000**|Réplique l'option de compression pour les données et les index. Pour plus d'informations, consultez [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
||**0 x 800000000**|Définissez cette option pour stocker les données FILESTREAM dans leur propre groupe de fichiers sur l'Abonné. Si cette option n'est pas définie, les données FILESTREAM sont stockées dans le groupe de fichiers par défaut. La réplication ne crée pas de groupes de fichiers ; par conséquent, si vous définissez cette option, vous devez créer le groupe de fichiers avant d'appliquer l'instantané à l'Abonné. Pour plus d’informations sur la façon de créer des objets avant d’appliquer l’instantané, consultez [exécuter des Scripts avant et après l’instantané est appliqué](../../relational-databases/replication/execute-scripts-before-and-after-the-snapshot-is-applied.md).<br /><br /> Consultez l’option connexe **0 x 100000000**.|  
||**0x1000000000**|Convertit les types common language runtime (CLR) définis par l’utilisateur (UDT) **varbinary (max)** afin que les colonnes de type UDT puissent être répliquées sur les abonnés qui sont en cours d’exécution [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||**0 x 2000000000**|Convertit le **hierarchyid** type de données à **varbinary (max)** afin que les colonnes de type **hierarchyid** peuvent être répliquées sur les abonnés qui sont en cours d’exécution [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations sur l’utilisation de **hierarchyid** colonnes dans les tables répliquées, consultez [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
||**0x4000000000**|Réplique tous les index filtrés sur la table. Pour plus d’informations sur les index filtrés, consultez [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).|  
||**0x8000000000**|Convertit le **geography** et **geometry** types de données **varbinary (max)** afin que les colonnes de ces types peuvent être répliquées sur les abonnés qui sont en cours d’exécution [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||**0x10000000000**|Réplique les index sur des colonnes de type **geography** et **geometry**.|  
||NULL|Le système génère automatiquement une option de schéma valide pour l'article.|  
|**status**|**Active**|Exécution du script de traitement initial pour publier la table.|  
||**unsynced**|Le script de traitement initial servant à publier la table sera exécuté lors de la prochaine exécution de l'Agent d'instantané.|  
|**stream_blob_columns**|**true**|Une optimisation de flux de données est utilisée lors de la réplication de colonnes d'objets binaires volumineux (BLOB). Toutefois, certaines fonctionnalités de réplication de fusion, telles que les enregistrements logiques, peuvent tout de même empêcher d'utiliser l'optimisation de flux. *stream_blob_columns* a la valeur true lorsque FILESTREAM est activé. Cela permet la réplication des données FILESTREAM dans le but d'optimiser et de réduire l'utilisation de la mémoire. Pour forcer les articles de la table FILESTREAM à ne pas utiliser l’objet blob de diffusion en continu, définissez *stream_blob_columns* avec la valeur false.<br /><br /> **\*\* Important \* \***  l’activation de cette optimisation de la mémoire peut dégrader les performances de l’Agent de fusion lors de la synchronisation. Cette option ne doit être utilisée que lors de la réplication de colonnes contenant des mégaoctets de données.|  
||**false**|L'optimisation n'est pas utilisée lors de la réplication de colonnes BLOB.|  
|**subscriber_upload_options**|**0**|Aucune restriction sur les mises à jour effectuées sur un Abonné disposant d'un abonnement client ; les modifications sont téléchargées sur le serveur de publication. La modification de cette propriété peut exiger la réinitialisation des Abonnés existants.|  
||**1**|Les modifications sont autorisées sur un Abonné disposant d'un abonnement client, mais elles ne sont pas téléchargées sur le serveur de publication.|  
||**2**|Les modifications ne sont pas autorisées sur un Abonné disposant d'un abonnement client.|  
|**subset_filterclause**||Clause WHERE spécifiant le filtrage horizontal. S’applique uniquement à un article de table.<br /><br /> **\*\* Important \* \***  pour des raisons de performances, il est recommandé que vous pas appliquez de fonctions aux noms de colonnes dans les clauses de filtre de lignes paramétrable, tel que `LEFT([MyColumn]) = SUSER_SNAME()`. Si vous utilisez [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) dans une clause de filtre et que vous remplacez la valeur HOST_NAME, vous devrez peut-être convertir les types de données à l’aide de [convertir](../../t-sql/functions/cast-and-convert-transact-sql.md). Pour plus d’informations sur les meilleures pratiques pour ce cas, consultez la section « Substitution de la valeur de HOST_NAME() » dans [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).|  
|**Seuil**||Pourcentage de valeur utilisé pour les abonnés exécutant [!INCLUDE[ssEW](../../includes/ssew-md.md)] ou des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **seuil** contrôle lorsque l’Agent de fusion affecte une nouvelle plage d’identité. Lorsque le pourcentage de valeurs spécifié dans le seuil est utilisé, l'Agent de fusion crée une nouvelle plage d'identité. Utilisé lorsque **identityrangemanagementoption** a la valeur **automatique** ou **auto_identity_range** a la valeur **true**. S'applique à un article de table uniquement. Pour plus d’informations, consultez la section « Réplication de fusion » de [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**verify_resolver_signature**|**1**|La signature numérique d'un outil de résolution personnalisé est vérifiée pour déterminer s'il provient d'une source approuvée.|  
||**0**|La signature numérique d'un outil de résolution personnalisé n'est pas vérifiée pour déterminer s'il provient d'une source approuvée.|  
|NULL (par défaut)||Retourne la liste des valeurs prises en charge pour *propriété*.|  
  
 [  **@force_invalidate_snapshot =** ] *force_invalidate_snapshot*  
 Signale que l'action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications apportées à l’article de fusion n’invalident pas l’instantané n’est pas valide. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** signifie que les modifications apportées à l’article de fusion peuvent invalider l’instantané n’est pas valide, et s’il existe des abonnements nécessitant un nouvel instantané, donne l’autorisation de l’instantané existant soit marqué comme obsolète et de générer un nouvel instantané.  
  
 Consultez la section Remarques pour connaître les propriétés dont la modification nécessite la génération d'un nouvel instantané.  
  
 [  **@force_reinit_subscription =** ] *force_reinit_subscription*  
 Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications apportées à l’article de fusion ne provoquent pas la réinitialisation des abonnements. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** signifie que les modifications à l’article de fusion entraînent la réinitialisation des abonnements existants et autorise la réinitialisation des abonnements se produise.  
  
 Voir la section Remarques pour connaître les propriétés dont la modification requiert la réinitialisation de tous les abonnements existants.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changemergearticle** est utilisé dans la réplication de fusion.  
  
 Étant donné que **sp_changemergearticle** est utilisée pour modifier les propriétés de l’article initialement spécifiées à l’aide de [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), reportez-vous à [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour plus d’informations sur ces propriétés.  
  
 Modifier les propriétés suivantes nécessite la génération d’un nouvel instantané, et vous devez spécifier une valeur de **1** pour le *force_invalidate_snapshot* paramètre :  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **schema_options**  
  
-   **subset_filterclause**  
  
 Modifier les propriétés suivantes nécessite existants réinitialisation des abonnements, et vous devez spécifier une valeur de **1** pour le *force_reinit_subscription* paramètre :  
  
-   **check_permissions**  
  
-   **column_tracking**  
  
-   **destination_owner**  
  
-   **pre_creation_command**  
  
-   **identityrangemanagementoption**  
  
-   **subscriber_upload_options**  
  
-   **subset_filterclause**  
  
-   **creation_script**  
  
-   **schema_option**  
  
-   **logical_record_level_conflict_detection**  
  
-   **logical_record_level_conflict_resolution**  
  
 Lorsque vous spécifiez la valeur 3 pour **partition_options**, métadonnées sont nettoyées chaque fois que l’Agent de fusion s’exécute et l’instantané partitionné expire plus rapidement. Lorsque vous utilisez cette option, pensez à activer l'instantané partitionné requis par l'abonné. Pour plus d’informations, voir [Instantanés des publications de fusion avec des filtres paramétrés](../../relational-databases/replication/snapshots-for-merge-publications-with-parameterized-filters.md).  
  
 Lors de la définition du **column_tracking** propriété, si la table est déjà publiée dans d’autres publications de fusion, la colonne de suivi doit être identique à la valeur utilisée par des articles existants basés sur cette table. Ce paramètre concerne uniquement les articles de table.  
  
 Si plusieurs publications publient des articles basés sur la même table sous-jacente, modifiez le **delete_tracking** propriété ou le **compensate_for_errors** propriété pour un article entraîne la même modification à apporter aux autres articles qui reposent sur la même table.  
  
 Si la connexion d'accès/le compte d'utilisateur du serveur de publication utilisé par le processus de fusion ne possède pas les autorisations de table appropriées, les modifications non valides sont enregistrées en tant que conflits.  
  
 Lorsque vous modifiez la valeur de **schema_option**, le système n’effectue pas une mise à jour au niveau du bit. Cela signifie que lorsque vous définissez **schema_option** à l’aide de **sp_changemergearticle**existante les paramètres bits peuvent être désactivés. Pour conserver les paramètres existants, vous devez effectuer [& (Bitwise AND)](../../t-sql/language-elements/bitwise-and-transact-sql.md) entre la valeur que vous définissez et la valeur actuelle de *schema_option*, qui peut être déterminée en exécutant [sp_helpmergearticle](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md).  
  
> [!CAUTION]  
>  Lorsque vous nombreuses (peut-être des centaines) des articles dans une publication et que vous exécutez **sp_changemergearticle** pour l’un des articles, il peut prendre beaucoup de temps pour terminer l’exécution.  
  
## <a name="valid-schema-option-table"></a>Tableau des options de schéma valides  
 Le tableau suivant décrit autorisées *schema_option*valeurs, en fonction du type d’article.  
  
|Type de l'article|Valeurs de l'option de schéma|  
|------------------|--------------------------|  
|**schéma Func uniquement**|**0 x 01** et **0 x 2000**|  
|**schéma de vue indexée uniquement**|**0 x 01**, **0 x 040**, **0 x 0100**, **0 x 2000**, **0 x 40000**, **0 x 1000000**et **0 x 200000**|  
|**schéma de procédure uniquement**|**0 x 01** et **0 x 2000**|  
|**table**|Toutes les options.|  
|**schéma de vue uniquement**|**0 x 01**, **0 x 040**, **0 x 0100**, **0 x 2000**, **0 x 40000**, **0 x 1000000**et **0 x 200000**|  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_changemergearticle](../../relational-databases/replication/codesnippet/tsql/sp-changemergearticle-tr_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_changemergearticle**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés de l’Article](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
