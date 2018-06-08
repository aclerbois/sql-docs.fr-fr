---
title: sp_changearticle (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/28/2015
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
- sp_changearticle
- sp_changearticle_TSQL
helpviewer_keywords:
- sp_changearticle
ms.assetid: 24c33ca5-f03a-4417-a267-131ca5ba6bb5
caps.latest.revision: 77
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1f68944355ce8af106d46ebba123e4c1fff05b04
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchangearticle-transact-sql"></a>sp_changearticle (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les propriétés d'un article dans une publication transactionnelle ou d'instantané. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changearticle [ [@publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication contenant l'article. *publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article dont la propriété doit être modifiée. *article* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@property=**] **'***propriété***'**  
 Propriété de l'article à modifier. *propriété* est **nvarchar (100)**.  
  
 [  **@value=**] **'***valeur***'**  
 Nouvelle valeur de la propriété d'article. *valeur* est **nvarchar (255)**.  
  
 Le tableau ci-dessous décrit les propriétés des articles et les valeurs de ces propriétés.  
  
|Propriété|Valeurs| Description|  
|--------------|------------|-----------------|  
|**creation_script**||Chemin d'accès et nom d'un script de schéma d'article utilisé pour créer des tables cibles. La valeur par défaut est NULL.|  
|**del_cmd**||Instruction DELETE à exécuter ; à défaut, elle sera élaborée à partir du journal.|  
|**description**||Nouvelle entrée descriptive de l'article.|  
|**dest_object**||Fourni pour la compatibilité ascendante. Utilisez **dest_table**.|  
|**dest_table**||Nouvelle table de destination.|  
|**destination_owner**||Nom du propriétaire de l’objet de destination.|  
|**Filter**||Nouvelle procédure stockée à utiliser pour filtrer la table (filtrage horizontal). La valeur par défaut est NULL. Ne peut pas être modifié pour les publications dans la réplication d'égal à égal.|  
|**fire_triggers_on_snapshot**|**true**|Les déclencheurs de l'utilisateur répliqués sont exécutés lorsque l'instantané initial est appliqué.<br /><br /> Remarque : pour les déclencheurs de réplication, la valeur de masque de bits de *schema_option* doit inclure la valeur **0 x 100**.|  
||**false**|Les déclencheurs de l'utilisateur répliqués ne sont pas exécutés lorsque l'instantané initial est appliqué.|  
|**identity_range**||Contrôle la taille des plages d'identité affectées à l'abonné. Non pris en charge pour la réplication d'égal à égal.|  
|**ins_cmd**||Instruction INSERT à exécuter ; à défaut, elle sera élaborée à partir du journal.|  
|**pre_creation_cmd**||Définit une commande de précréation pouvant supprimer, effacer ou tronquer la table de destination avant l'application de la synchronisation.|  
||**Aucun**|N'utilise pas de commande.|  
||**DROP**|Supprime la table de destination.|  
||**delete**|Détruit la table de destination.|  
||**truncate**|Tronque la table de destination.|  
|**pub_identity_range**||Contrôle la taille des plages d'identité affectées à l'abonné. Non pris en charge pour la réplication d'égal à égal.|  
|**schema_option**||Spécifie le bitmap de l'option de génération de schéma pour l'article considéré. *schema_option* est **Binary (8)**. Pour plus d’informations, consultez la section Notes plus loin dans cette rubrique.|  
||**0 x 00**|Désactive les scripts de l'Agent d'instantané.|  
||**0 x 01**|Génère la création d'objets (CREATE TABLE, CREATE PROCEDURE, etc.).|  
||**0 x 02**|Génère les procédures stockées qui propagent les modifications pour l'article, si elles sont définies.|  
||**0 x 04**|Les colonnes d'identité font l'objet d'un script utilisant la propriété IDENTITY.|  
||**0 x 08**|Répliquer **timestamp** colonnes. Si ce n’est pas définie, **timestamp** les colonnes sont répliquées en tant que **binaire**.|  
||**0 x 10**|Génère un index cluster correspondant.|  
||**0 x 20**|Convertit les types de données définis par l'utilisateur (UDT) en types de données de base auprès de l'Abonné. Vous ne pouvez pas utiliser cette option lorsqu'il existe une contrainte CHECK ou DEFAULT sur une colonne de type défini par l'utilisateur (UDT), si une colonne UDT fait partie de la clé primaire, ou si une colonne calculée désigne une colonne UDT. Non pris en charge pour les serveurs de publication Oracle.|  
||**0 x 40**|Génère les index non-cluster correspondants.|  
||**0 x 80**|Inclut l'intégrité référentielle déclarée dans les clés primaires.|  
||**0 x 100**|Réplique les déclencheurs utilisateur, si ceux-ci sont définis, sur un article de table.|  
||**0 x 200**|Réplique les contraintes FOREIGN KEY. Si la table référencée ne fait pas partie d'une publication, aucune contrainte FOREIGN KEY appliquée à une table publiée n'est répliquée.|  
||**0 x 400**|Réplique les contraintes CHECK.|  
||**0 x 800**|Réplique les valeurs par défaut.|  
||**0 x 1000**|Réplique le classement au niveau des colonnes.|  
||**0 x 2000**|Réplique les propriétés étendues associées à l'objet source de l'article publié.|  
||**0 x 4000**|Réplique les clés uniques, si celles-ci sont définies, sur un article de table.|  
||**0 x 8000**|Réplique la clé primaire et les clés uniques sur un article de table sous forme de contraintes, à l'aide d'instructions ALTER TABLE.<br /><br /> Remarque : Cette option est déconseillée. Utilisez **0 x 80** et **0 x 4000** à la place.|  
||**0 x 10000**|Réplique les contraintes CHECK en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0 x 20000**|Réplique les contraintes FOREIGN KEY en tant que NOT FOR REPLICATION afin que les contraintes ne soient pas appliquées durant la synchronisation.|  
||**0 x 40000**|Réplique les groupes de fichiers associés à une table ou un index partitionné.|  
||**0 x 80000**|Réplique le schéma de partition d'une table partitionnée.|  
||**0 x 100000**|Réplique le schéma de partition d'un index partitionné.|  
||**0 x 200000**|Réplique les statistiques d'une table.|  
||**0 x 400000**|Liaisons par défaut|  
||**0x800000**|Liaisons de règle|  
||**0 x 1000000**|Index de recherche en texte intégral|  
||**0x2000000**|Collections de schémas XML lié à **xml** colonnes ne sont pas répliquées.|  
||**0x4000000**|Réplique les index sur **xml** colonnes.|  
||**0 x 8000000**|Crée tout schéma non encore présent chez l'abonné.|  
||**0 x 10000000**|Convertit **xml** colonnes à **ntext** sur l’abonné.|  
||**0 x 20000000**|Types de données de l’objet convertit de grande taille (**nvarchar (max)**, **varchar (max)**, et **varbinary (max)**) qui ont été introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] aux types de données qui sont prises en charge sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)].|  
||**0 x 40000000**|Réplique les autorisations.|  
||**0 x 80000000**|Tente de supprimer les dépendances envers tous les objets qui ne font pas partie de la publication.|  
||**0 x 100000000**|Utilisez cette option pour répliquer l’attribut FILESTREAM s’il est spécifié sur **varbinary (max)** colonnes. Ne spécifiez pas cette option si vous répliquez des tables sur des Abonnés [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Réplication de tables qui possèdent des colonnes FILESTREAM sur [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] abonnés n'est pas pris en charge, quelle que soit la manière dont cette option de schéma.<br /><br /> Consultez l’option connexe **0 x 800000000**.|  
||**0x200000000**|Convertit les types de données de date et heure (**date**, **temps**, **datetimeoffset**, et **datetime2**) qui ont été introduits dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] aux types de données qui sont prises en charge dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
||**0x400000000**|Réplique l'option de compression pour les données et les index. Pour plus d'informations, consultez [Data Compression](../../relational-databases/data-compression/data-compression.md).|  
||**0 x 800000000**|Définissez cette option pour stocker les données FILESTREAM dans leur propre groupe de fichiers sur l'Abonné. Si cette option n'est pas définie, les données FILESTREAM sont stockées dans le groupe de fichiers par défaut. La réplication ne crée pas de groupes de fichiers ; par conséquent, si vous définissez cette option, vous devez créer le groupe de fichiers avant d'appliquer l'instantané à l'Abonné. Pour plus d’informations sur la façon de créer des objets avant d’appliquer l’instantané, consultez [exécuter des Scripts avant et après l’instantané est appliqué](../../relational-databases/replication/execute-scripts-before-and-after-the-snapshot-is-applied.md).<br /><br /> Consultez l’option connexe **0 x 100000000**.|  
||**0x1000000000**|Convertit les types common language runtime (CLR) définis par l’utilisateur (UDT) supérieurs à 8 000 octets pour **varbinary (max)** afin que les colonnes de type UDT puissent être répliquées sur les abonnés qui sont en cours d’exécution [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||**0 x 2000000000**|Convertit le **hierarchyid** type de données à **varbinary (max)** afin que les colonnes de type **hierarchyid** peuvent être répliquées sur les abonnés qui sont en cours d’exécution [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations sur l’utilisation de **hierarchyid** colonnes dans les tables répliquées, consultez [hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md).|  
||**0x4000000000**|Réplique tous les index filtrés sur la table. Pour plus d’informations sur les index filtrés, consultez [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).|  
||**0x8000000000**|Convertit le **geography** et **geometry** types de données **varbinary (max)** afin que les colonnes de ces types peuvent être répliquées sur les abonnés qui sont en cours d’exécution [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||**0x10000000000**|Réplique les index sur des colonnes de type **geography** et **geometry**.|  
||**0x20000000000**|Réplique l'attribut SPARSE pour les colonnes. Pour plus d’informations sur cet attribut, consultez [utiliser des colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).|  
||**0 x 40000000000**|Activer la génération de scripts par l’agent d’instantané pour créer la table optimisée en mémoire sur l’abonné.|  
||**0x80000000000**|Convertit les index cluster en index non cluster pour les articles de la mémoire optimisées.|  
|**status**||Spécifie le nouvel état de la propriété.|  
||**partitions horizontales de DTS**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
||**inclure les noms de colonne**|Les noms de colonnes sont inclus dans l'instruction INSERT répliquée.|  
||**Aucun nom de colonne**|Les noms de colonnes ne sont pas inclus dans l'instruction INSERT répliquée.|  
||**pas de partitions dts horizontales**|La partition horizontale pour l'article n'est pas définie par un abonnement transformable.|  
||**Aucun**|Efface toutes les options d’état dans le [sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md) de table et marque l’article comme étant inactif.|  
||**parameters**|Les modifications sont propagées vers l'abonné à l'aide de commandes paramétrables. Il s'agit du paramètre par défaut pour un nouvel article.|  
||**Littéraux de chaîne**|Les modifications sont propagées vers l'abonné à l'aide de valeurs littérales de chaîne.|  
|**sync_object**||Nom de la table ou de la vue utilisée pour produire un fichier de sortie de synchronisation. La valeur par défaut est NULL. Non pris en charge pour les serveurs de publication Oracle.|  
|**Espace disque logique**||Identifie l'espace de table utilisé par la table de journalisation pour un article publié à partir d'une base de données Oracle. Pour plus d’informations, consultez [Gérer des espaces disque logiques Oracle](../../relational-databases/replication/non-sql/manage-oracle-tablespaces.md).|  
|**Seuil**||Valeur de pourcentage qui contrôle le moment où l'Agent de distribution affecte une nouvelle plage d'identité. Non pris en charge pour la réplication d'égal à égal.|  
|**type**||Non pris en charge pour les serveurs de publication Oracle.|  
||**logbased**|Article basé sur le journal.|  
||**logbased manualboth**|Article reposant sur un journal, avec filtre manuel et vue manuelle. Cette option requiert le *sync_object* et *filtre* propriétés également être définies. Non pris en charge pour les serveurs de publication Oracle.|  
||**logbased manualfilter**|Article reposant sur un journal, avec filtre manuel. Cette option requiert le *sync_object* et *filtre* propriétés également être définies. Non pris en charge pour les serveurs de publication Oracle.|  
||**logbased manualview**|Article reposant sur un journal, avec vue manuelle. Cette option requiert le *sync_object* propriété également être définie. Non pris en charge pour les serveurs de publication Oracle.|  
||**viewlogbased indexé**|Article de vue indexée reposant sur un journal. Non pris en charge pour les serveurs de publication Oracle. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément.|  
||**viewlogbased indexée manualboth**|Article de vue indexée reposant sur un journal avec filtre manuel et vue manuelle. Cette option requiert le *sync_object* et *filtre* propriétés également être définies. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
||**viewlogbased indexée manualfilter**|Article de vue indexée reposant sur un journal avec filtre manuel. Cette option requiert le *sync_object* et *filtre* propriétés également être définies. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
||**viewlogbased indexée manualview**|Article de vue indexée reposant sur un journal avec vue manuelle. Cette option requiert le *sync_object* propriété également être définie. Pour ce type d'article, il n'est pas nécessaire de publier la table de base séparément. Non pris en charge pour les serveurs de publication Oracle.|  
|**upd_cmd**||Instruction UPDATE à exécuter ; à défaut, elle sera élaborée à partir du journal.|  
|NULL|NULL|Renvoie une liste de propriétés d'articles modifiables.|  
  
 [  **@force_invalidate_snapshot =** ] *force_invalidate_snapshot*  
 Signale que l'action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bits**, avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications de l’article n’invalident pas l’instantané n’est pas valide. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article peuvent invalider l’instantané n’est pas valide, s’il existe des abonnements nécessitant un nouvel instantané pour générer un nouvel instantané et de l’instantané existant soit marqué comme obsolète.  
  
 Consultez la section Remarques pour connaître les propriétés dont la modification nécessite la génération d'un nouvel instantané.  
  
 [**@force_reinit_subscription=] *** force_reinit_subscription*  
 Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bits** avec une valeur par défaut **0**.  
  
 **0** Spécifie que les modifications de l’article ne provoquent pas la réinitialisation des abonnements. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que les modifications apportées à l’article entraînent la réinitialisation des abonnements existants et autorise la réinitialisation des abonnements se produise.  
  
 Voir la section Remarques pour connaître les propriétés dont la modification requiert la réinitialisation de tous les abonnements existants.  
  
 [ **@publisher**=] **'***publisher***'**  
 Spécifie un serveur de publication non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé lors de la modification des propriétés de l’article sur un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changearticle** est utilisé dans la réplication de capture instantanée et la réplication transactionnelle.  
  
 Lorsqu’un article appartient à une publication qui prend en charge la réplication transactionnelle d’égal à égal, vous ne pouvez modifier le **description**, **ins_cmd**, **upd_cmd**, et **del_cmd** propriétés.  
  
 La modification des propriétés suivantes nécessite la génération d’un nouvel instantané, et vous devez spécifier une valeur de **1** pour le *force_invalidate_snapshot* paramètre :  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **ins_cmd**  
  
-   **pre_creation_cmd**  
  
-   **schema_options**  
  
-   **upd_cmd**  
  
 La modification des propriétés suivantes nécessite existants réinitialisation des abonnements, et vous devez spécifier une valeur de **1** pour le *force_reinit_subscription* paramètre.  
  
-   **del_cmd**  
  
-   **dest_table**  
  
-   **destination_owner**  
  
-   **Filter**  
  
-   **ins_cmd**  
  
-   **status**  
  
-   **upd_cmd**  
  
 Dans une publication existante, vous pouvez utiliser **sp_changearticle** pour modifier un article sans devoir supprimer et recréer la publication entière.  
  
> [!NOTE]  
>  Lorsque vous modifiez la valeur de *schema_option*, le système n’effectue pas une mise à jour au niveau du bit. Cela signifie que lorsque vous définissez *schema_option* à l’aide de **sp_changearticle**existante les paramètres bits peuvent être désactivés. Pour conserver les paramètres existants, vous devez effectuer [| (OR au niveau du bit) ](../../t-sql/language-elements/bitwise-or-transact-sql.md) entre la valeur que vous définissez et la valeur actuelle de *schema_option*, qui peut être déterminée en exécutant [sp_helparticle](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md).  
  
## <a name="valid-schema-options"></a>Options de schéma valides  
 Le tableau suivant décrit les valeurs autorisées de *schema_option* basé sur le type de réplication (du haut) et le type d’article (la première colonne).  
  
|Type de l'article|Type de réplication||  
|------------------|----------------------|------|  
||Transactionnelle|Snapshot|  
|**logbased**|Toutes les options|Toutes les options mais **0 x 02**|  
|**logbased manualfilter**|Toutes les options|Toutes les options mais **0 x 02**|  
|**logbased manualview**|Toutes les options|Toutes les options mais **0 x 02**|  
|**la vue indexée logbased**|Toutes les options|Toutes les options mais **0 x 02**|  
|**la vue indexée logbased manualfilter**|Toutes les options|Toutes les options mais **0 x 02**|  
|**la vue indexée logbased manualview**|Toutes les options|Toutes les options mais **0 x 02**|  
|**la vue indexée logbase manualboth**|Toutes les options|Toutes les options mais **0 x 02**|  
|**proc exec**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|  
|**serializable proc exec**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|  
|**schéma de procédure uniquement**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|  
|**schéma de vue uniquement**|**0 x 01**, **0x010**, **0x020**, **0 x 040**, **0 x 0100**, **0 x 2000**, **0 x 40000**, **0 x 100000**, **0 x 200000**, **0 x 400000**, **0x800000**,  **0x2000000**, **0 x 8000000**, **0 x 40000000**, et **0 x 80000000**|**0 x 01**, **0x010**, **0x020**, **0 x 040**, **0 x 0100**, **0 x 2000**, **0 x 40000**, **0 x 100000**, **0 x 200000**, **0 x 400000**, **0x800000**,  **0x2000000**, **0 x 8000000**, **0 x 40000000**, et **0 x 80000000**|  
|**schéma Func uniquement**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|**0 x 01**, **0 x 20**, **0 x 2000**, **0 x 400000**, **0x800000**, **0x2000000**, **0 x 8000000**, **0 x 10000000**, **0 x 20000000**, **0 x 40000000**, et **0 x 80000000**|  
|**schéma de vue indexée uniquement**|**0 x 01**, **0x010**, **0x020**, **0 x 040**, **0 x 0100**, **0 x 2000**, **0 x 40000**, **0 x 100000**, **0 x 200000**, **0 x 400000**, **0x800000**,  **0x2000000**, **0 x 8000000**, **0 x 40000000**, et **0 x 80000000**|**0 x 01**, **0x010**, **0x020**, **0 x 040**, **0 x 0100**, **0 x 2000**, **0 x 40000**, **0 x 100000**, **0 x 200000**, **0 x 400000**, **0x800000**,  **0x2000000**, **0 x 8000000**, **0 x 40000000**, et **0 x 80000000**|  
  
> [!NOTE]  
>  Pour les publications avec mise à jour en file d’attente, le *schema_option* valeur **0 x 80** doit être activée. La prise en charge *schema_option* des valeurs non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publications sont : **0 x 01**, **0 x 02**, **0 x 10**, **0 x 40**, **0 x 80**, **0 x 1000** et **0 x 4000**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_changetranarticle](../../relational-databases/replication/codesnippet/tsql/sp-changearticle-transac_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_changearticle**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés de l’Article](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)  
  
  
