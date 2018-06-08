---
title: sysmergepartitioninfoview (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- sysmergepartitioninfoview
- sysmergepartitioninfoview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfoview view
ms.assetid: 714e2935-1bc7-4901-aea2-64b1bbda03d6
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b3a1362e62d6e078b7847a8d37af31614a082162
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysmergepartitioninfoview-transact-sql"></a>sysmergepartitioninfoview (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **sysmergepartitioninfoview** affichage expose les informations de partitionnement pour les articles de table. Cette vue est stockée dans la base de données de publication sur le serveur de publication et dans la base de données d'abonnement au niveau de l'Abonné.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Le nom de l’article.|  
|**type**|**tinyint**|Indique le type d'article, qui peut être l'un des suivants :<br /><br /> **0x0A** = table.<br /><br /> **0 x 20** = schéma de procédure uniquement.<br /><br /> **0 x 40** = schéma de vue uniquement ou schéma de vue indexée uniquement.<br /><br /> **0 x 80** = schéma de fonction uniquement.|  
|**objid**|**int**|Identificateur de l'objet publié.|  
|**sync_objid**|**int**|ID d'objet de la vue représentant l'ensemble de données synchronisées.|  
|**view_type**|**tinyint**|Type de vue :<br /><br /> **0** ne = pas d’une vue ; utilisez l’intégralité de l’objet de base.<br /><br /> **1** = vue permanente.<br /><br /> **2** = vue temporaire.|  
|**artid**|**uniqueidentifier**|Numéro d'identification unique de l'article donné.|  
|**description**|**nvarchar(255)**|Brève description de l'article.|  
|**pre_creation_command**|**tinyint**|L’action par défaut à effectuer lorsque l’article est créé dans la base de données d’abonnement :<br /><br /> **0** = aucune - si la table existe déjà sur l’abonné, aucune action n’est effectuée.<br /><br /> **1** = Supprimer - supprime la table avant de la recréer.<br /><br /> **2** = supprimer - entraîne une suppression basée sur la clause WHERE dans le filtre de sous-ensemble.<br /><br /> **3** = Truncate : identique à 2, mais supprime des pages au lieu de lignes. Toutefois, n'accepte pas la clause WHERE.|  
|**pubid**|**uniqueidentifier**|ID de la publication à laquelle appartient l'article actif.|  
|**Surnom**|**int**|Le mappage de surnom pour l'identification de l'article.|  
|**column_tracking**|**int**|L’indique si le suivi des colonnes est implémentée pour l’article.|  
|**status**|**tinyint**|Indique l'état de l'article, qui peut être l'un des suivants :<br /><br /> **1** = Unsynced - le script de traitement initial pour publier la table sera exécuté lors de la prochaine exécution de l’Agent d’instantané.<br /><br /> **2** = active - le script de traitement initial pour publier la table a été exécuté.|  
|**conflict_table**|**sysname**|Nom de la table locale qui contient les enregistrements en conflit pour l'article actif. Cette table est fournie à titre d'information uniquement et son contenu peut être modifié ou supprimé à l'aide des routines personnalisées de résolution de conflits ou directement par l'administrateur.|  
|**creation_script**|**nvarchar(255)**|Script de création de l'article.|  
|**conflict_script**|**nvarchar(255)**|Script de conflit de l'article.|  
|**article_resolver**|**nvarchar(255)**|Outils de résolution des conflits pour l'article.|  
|**ins_conflict_proc**|**sysname**|Procédure utilisée pour écrire des informations de conflit dans la table de conflits.|  
|**insert_proc**|**sysname**|Procédure utilisée pour insérer des lignes pendant la synchronisation.|  
|**update_proc**|**sysname**|Procédure utilisée pour mettre à jour des lignes pendant la synchronisation.|  
|**select_proc**|**sysname**|Nom de la procédure stockée générée automatiquement que l'Agent de fusion utilise pour effectuer les verrouillages et rechercher des colonnes et des lignes pour un article.|  
|**metadata_select_proc**|**sysname**|Nom de la procédure stockée générée automatiquement et utilisée pour accéder aux métadonnées des tables système de réplication de fusion.|  
|**delete_proc**|**sysname**|Procédure utilisée pour supprimer des lignes pendant la synchronisation.|  
|**schema_option**|**binary(8)**|Bitmap de l'option de génération de schéma pour l'article donné. Pour plus d’informations sur pris en charge *schema_option* valeurs, consultez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nom de la table créée sur l'Abonné.|  
|**destination_owner**|**sysname**|Le nom du propriétaire de l’objet de destination.|  
|**resolver_clsid**|**nvarchar(50)**|ID de l'outil personnalisé de résolution des conflits Cette valeur est NULL dans le cas d'un gestionnaire de logique métier.|  
|**subset_filterclause**|**nvarchar(1000)**|Clause de filtre de l'article.|  
|**missing_col_count**|**int**|Nombre de colonnes publiées manquantes dans l'article.|  
|**missing_cols**|**varbinary(128)**|Bitmap décrivant les colonnes manquant dans l'article.|  
|**excluded_cols**|**varbinary(128)**|Bitmap des colonnes exclues de l'article.|  
|**excluded_col_count**|**int**|Nombre de colonnes exclues de l'article.|  
|**columns**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deleted_cols**|**varbinary(128)**|Bitmap décrivant les colonnes supprimées de l'article.|  
|**resolver_info**|**nvarchar(255)**|Espace de stockage réservé aux informations complémentaires nécessaires aux outils personnalisés de résolution des conflits.|  
|**view_sel_proc**|**nvarchar(290)**|Nom de la procédure stockée utilisée par l'Agent de fusion pour effectuer le remplissage initial d'un article dans une publication filtrée dynamiquement et pour énumérer les lignes modifiées dans une publication filtrée.|  
|**gen_cur**|**bigint**|Génère le nombre de modifications locales apportées à la table de base d'un article.|  
|**vertical_partition**|**int**|Indique si le filtrage de colonne est activé sur un article de table. **0** indique l’absence de filtrage vertical et publie toutes les colonnes.|  
|**identity_support**|**int**|Spécifie si la gestion automatique de plages d'identités est activée. **1** signifie que les plages d’identités est activée, et **0** signifie qu’il n’existe aucune identité de la plage prise en charge.|  
|**before_image_objid**|**int**|ID d'objet de la table de suivi. La table de suivi contient certaines valeurs de colonne clé lorsque l'optimisation des modifications de partition est activée pour la publication.|  
|**before_view_objid**|**int**|ID d'objet d'une table de vue. La vue est associée à une table qui détermine si une ligne appartenait à un Abonné particulier avant sa suppression ou sa mise à jour. Ceci n'est vrai que lorsque l'optimisation des modifications de partition est activée pour la publication.|  
|**verify_resolver_signature**|**int**|Spécifie si une signature numérique est vérifiée avant d'utiliser un résolveur dans une réplication de fusion :<br /><br /> **0** = signature n’est pas vérifiée.<br /><br /> **1** = signature est vérifiée pour déterminer s’il est d’une source approuvée.|  
|**allow_interactive_resolver**|**bit**|Spécifie si l'utilisation du composant résolveur interactif sur un article est activée. **1** signifie que le composant résolveur interactif peut être utilisé sur l’article.|  
|**fast_multicol_updateproc**|**bit**|Spécifie si l'Agent de fusion est activé pour appliquer des modifications à plusieurs colonnes d'une même ligne à partir d'une seule instruction UPDATE.<br /><br /> **0** = émet une instruction UPDATE distincte pour chaque colonne modifiée.<br /><br /> **1** = émis à l’instruction UPDATE qui provoque des mises à jour à plusieurs colonnes dans une instruction.|  
|**check_permissions**|**int**|L’image bitmap des autorisations au niveau des tables qui seront vérifiées lorsque l’Agent de fusion applique les modifications au serveur de publication. *check_permissions* peut avoir l’une des valeurs suivantes :<br /><br /> **0 x 00** = les autorisations ne sont pas vérifiées.<br /><br /> **0 x 10** = vérifie les autorisations sur le serveur de publication avant que les insertions sont faites à un abonné peuvent être téléchargées.<br /><br /> **0 x 20** = les autorisations sont vérifiées sur le serveur de publication avant que les mises à jour effectuées sur l’abonné puissent être téléchargées.<br /><br /> **0 x 40** = les autorisations sont vérifiées sur le serveur de publication avant que les suppressions effectuées sur l’abonné puissent être téléchargées.|  
|**maxversion_at_cleanup**|**int**|Génération maximale faisant l'objet d'un nettoyage lors de la prochaine exécution de l'Agent de fusion.|  
|**processing_order**|**int**|Indique l’ordre de traitement des articles dans une publication de fusion ; où la valeur **0** indique que l’article n’est pas ordonné, et les articles sont traités dans l’ordre de la plus petite à la plus élevée. Si deux articles ont la même valeur, ils sont traités simultanément. Pour plus d’informations, consultez [Spécifier l’ordre de traitement d’articles de fusion](../../relational-databases/replication/merge/specify-the-processing-order-of-merge-articles.md).|  
|**upload_options**|**tinyint**|Indique si des modifications peuvent être effectuées sur l'Abonné ou téléchargés à partir de l'Abonné ; peut prendre l'une des valeurs suivantes :<br /><br /> **0** = il n’existe aucune restriction sur les mises à jour effectuées sur l’abonné ; toutes les modifications sont téléchargées sur le serveur de publication.<br /><br /> **1** = les modifications sont autorisées sur l’abonné, mais elles ne sont pas téléchargées sur le serveur de publication.<br /><br /> **2** = les modifications ne sont pas autorisées sur l’abonné.|  
|**published_in_tran_pub**|**bit**|Indique qu'un article d'une publication de fusion est également publié dans une publication transactionnelle.<br /><br /> **0** = l’article n’est pas publié dans un article transactionnel.<br /><br /> **1** = l’article est également publié dans un article transactionnel.|  
|**légère**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**procname_postfix**|**nchar(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**well_partitioned_lightweight**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**before_upd_view_objid**|**int**|ID de la vue de la table avant les mises à jour.|  
|**delete_tracking**|**bit**|Indique si les suppressions sont répliquées.<br /><br /> **0** = suppressions ne sont pas répliquées.<br /><br /> **1** = les suppressions sont répliquées, ce qui est le comportement par défaut pour la réplication de fusion.<br /><br /> Lorsque la valeur de *delete_tracking* est **0**, de lignes supprimées sur l’abonné doivent être supprimées manuellement sur le serveur de publication et de lignes supprimées sur le serveur de publication doivent être supprimées manuellement sur l’abonné.<br /><br /> Remarque : Une valeur de **0** entraîne une non-convergence.|  
|**compensate_for_errors**|**bit**|Indique si des actions de compensation interviennent lorsque des erreurs se produisent pendant la synchronisation.<br /><br /> **0** = Compensating actions sont désactivées.<br /><br /> **1** = les modifications qui ne peut pas être appliquées à un abonné ou un serveur de publication entraînent toujours des actions de compensation pour annuler ces modifications, qui est le comportement par défaut pour la réplication de fusion.<br /><br /> Remarque : Une valeur de **0** entraîne une non-convergence.|  
|**pub_range**|**bigint**|Taille de la plage d'identité du serveur de publication.|  
|**range**|**bigint**|Taille des valeurs d'identité consécutives qui seraient affectées aux abonnés dans le cas d'un ajustement.|  
|**Seuil**|**int**|Seuil de la plage d'identité exprimé en pourcentage.|  
|**stream_blob_columns**|**bit**|Indique si la fonction d'optimisation de diffusion est utilisée pour les colonnes d'objets binaires volumineux. **1** signifie que l’optimisation est tentée.|  
|**preserve_rowguidcol**|**bit**|Indique si la réplication utilise une colonne rowguid existante. La valeur **1** signifie qu’une colonne ROWGUIDCOL existante est utilisée. **0** signifie que la réplication a ajouté la colonne ROWGUIDCOL.|  
|**partition_view_id**|**int**|Identifie la vue définissant une partition d'abonné.|  
|**repl_view_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**partition_deleted_view_rule**|**sysname**|Instruction utilisée à l'intérieur d'un déclencheur de réplication de fusion pour extraire l'ID de partition pour chaque ligne supprimée ou mise à jour en fonction de ses anciennes valeurs de colonne.|  
|**partition_inserted_view_rule**|**sysname**|Instruction utilisée à l'intérieur d'un déclencheur de réplication de fusion pour extraire l'ID de partition pour chaque ligne insérée ou mise à jour en fonction de ses anciennes valeurs de colonne.|  
|**membership_eval_proc_name**|**sysname**|Le nom de la procédure qui évalue les ID de partition actuel de lignes dans [MSmerge_contents &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-contents-transact-sql.md).|  
|**column_list**|**sysname**|Liste séparée par des virgules des colonnes publiées dans un article.|  
|**column_list_blob**|**sysname**|Liste séparée par des virgules des colonnes publiées dans un article, y compris les colonnes d'objets binaires volumineux.|  
|**expand_proc**|**sysname**|Nom de la procédure qui réévalue les ID de partition de toutes les lignes enfants d'une ou plusieurs lignes parents nouvellement insérées ayant fait l'objet d'une modification de partition ou qui ont été supprimées.|  
|**logical_record_parent_nickname**|**int**|Surnom du parent de niveau supérieur d'un article donné dans un enregistrement logique.|  
|**logical_record_view**|**int**|Vue qui génère la colonne rowguid d'article de parent de niveau supérieur correspondant à chaque colonne rowguid enfant.|  
|**logical_record_deleted_view_rule**|**sysname**|Semblable à **logical_record_view**, sauf qu’il affiche les lignes enfants de la table « supprimée » dans mettre à jour et supprimer les déclencheurs.|  
|**logical_record_level_conflict_detection**|**bit**|Indique si les conflits doivent être détectés au niveau des enregistrements logiques ou au niveau des lignes ou des colonnes.<br /><br /> **0** = niveau ligne ou colonne-détection des conflits.<br /><br /> **1** = logique détection de conflit d’enregistrement est utilisée, où une modification d’une ligne sur le serveur de publication et de modification dans une fonction de lignes la même logique enregistrement au niveau de l’abonné est gérée comme un conflit.<br /><br /> Lorsque cette valeur est égale à 1, seule la résolution de conflits au niveau de l'enregistrement logique peut être utilisée.|  
|**logical_record_level_conflict_resolution**|**bit**|Indique si les conflits doivent être résolus au niveau de l'enregistrement logique ou au niveau de la ligne ou de la colonne.<br /><br /> **0** = niveau ligne ou colonne-résolution est utilisée.<br /><br /> **1** = en cas de conflit, l’enregistrement logique entière du vainqueur du remplace l’intégralité enregistrement logique côté perdant.<br /><br /> La valeur 1 peut-être utilisée aussi bien pour la détection au niveau de l'enregistrement logique que pour la détection au niveau de la ligne ou de la colonne.|  
|**partition_options**|**tinyint**|Définit le mode de partitionnement des données de l'article, ce qui permet l'optimisation des performances lorsque toutes les lignes appartiennent à une seule partition ou à un seul abonnement. Le *partition_options* peut prendre l’une des valeurs suivantes.<br /><br /> **0** = le filtrage pour l’article est statique ou ne génère pas un sous-ensemble unique de données pour chaque partition, c'est-à-dire une partition en « chevauchement ».<br /><br /> **1** = les partitions se chevauchent et les mises à jour DML effectuées sur l’abonné ne peut pas modifier la partition à laquelle appartient une ligne.<br /><br /> **2** = le filtrage de l’article génère sans chevauchement de partitions, mais plusieurs abonnés peuvent recevoir la même partition.<br /><br /> **3** = le filtrage de l’article génère des partitions sans chevauchement qui sont uniques pour chaque abonnement.|  
|**nom**|**sysname**|Nom d'une partition.|  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer les Partitions d’une Publication de fusion avec des filtres paramétrés](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_helpmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md)  
  
  
