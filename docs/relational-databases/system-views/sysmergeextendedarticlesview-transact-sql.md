---
title: sysmergeextendedarticlesview (Transact-SQL) | Documents Microsoft
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
- sysmergeextendedarticlesview
- sysmergeextendedarticlesview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeextendedarticlesview view
ms.assetid: bd5c8414-5292-41fd-80aa-b55a50ced7e2
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0556aeedb68d19b9b616bf776f6846fdfcce985b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysmergeextendedarticlesview-transact-sql"></a>sysmergeextendedarticlesview (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **sysmergeextendedarticlesview** expose des informations de l’article de vue. Cette vue est stockée dans la base de données de publication sur le serveur de publication et dans la base de données d'abonnement au niveau de l'Abonné.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Le nom de l’article.|  
|**type**|**tinyint**|Indique le type d'article, qui peut être l'un des suivants :<br /><br /> **10** = table.<br /><br /> **32** = Proc schema uniquement.<br /><br /> **64** = schéma de vue uniquement ou schéma de vue indexée uniquement.<br /><br /> **128** = schéma de fonction uniquement.<br /><br /> **160** = schéma de synonyme uniquement.|  
|**objid**|**int**|Identificateur de l'objet du serveur de publication.|  
|**sync_objid**|**int**|Identificateur de la vue représentant le dataset synchronisé.|  
|**view_type**|**tinyint**|Type de vue :<br /><br /> **0** ne = pas d’une vue ; utilisez l’intégralité de l’objet de base.<br /><br /> **1** = vue permanente.<br /><br /> **2** = vue temporaire.|  
|**artid**|**uniqueidentifier**|Numéro d'identification unique de l'article donné.|  
|**description**|**nvarchar(255)**|Brève description de l'article.|  
|**pre_creation_command**|**tinyint**|L’action par défaut à effectuer lorsque l’article est créé dans la base de données d’abonnement :<br /><br /> **0** = aucune - si la table existe déjà sur l’abonné, aucune action n’est effectuée.<br /><br /> **1** = Supprimer - supprime la table avant de créer à nouveau.<br /><br /> **2** = supprimer - entraîne une suppression basée sur la clause WHERE dans le filtre de sous-ensemble.<br /><br /> **3** = Truncate : identique à 2, mais supprime des pages au lieu de lignes. Toutefois, n'accepte pas la clause WHERE.|  
|**pubid**|**uniqueidentifier**|ID de la publication à laquelle appartient l'article actif.|  
|**Surnom**|**int**|Le mappage de surnom pour l'identification de l'article.|  
|**column_tracking**|**int**|Indique si la fonction de suivi des colonnes est implémentée pour l'article.|  
|**status**|**tinyint**|Indique l'état de l'article, qui peut être l'un des suivants :<br /><br /> **1** = Unsynced - le script de traitement initial pour publier la table sera exécuté lors de la prochaine exécution de l’Agent d’instantané.<br /><br /> **2** = active - le script de traitement initial pour publier la table a été exécuté.<br /><br /> **5** = New_inactive - à ajouter.<br /><br /> **6** = New_active - à ajouter.|  
|**conflict_table**|**sysname**|Nom de la table locale qui contient les enregistrements en conflit pour l'article actif. Cette table est fournie à titre d'information uniquement et son contenu peut être modifié ou supprimé à l'aide des routines personnalisées de résolution de conflits ou directement par l'administrateur.|  
|**creation_script**|**nvarchar(255)**|Script de création de l'article.|  
|**conflict_script**|**nvarchar(255)**|Script de conflit de l'article.|  
|**article_resolver**|**nvarchar(255)**|Outil personnalisé de résolution des conflits au niveau ligne pour l'article.|  
|**ins_conflict_proc**|**sysname**|La procédure utilisée pour écrire les conflits dans **conflict_table**.|  
|**insert_proc**|**sysname**|Procédure utilisée par l'outil de résolution des conflits pour insérer des lignes lors de la synchronisation.|  
|**update_proc**|**sysname**|Procédure utilisée par l'outil de résolution des conflits pour mettre à jour des lignes lors de la synchronisation.|  
|**select_proc**|**sysname**|Nom de la procédure stockée générée automatiquement que l'Agent de fusion utilise pour effectuer le verrouillage et rechercher les colonnes et les lignes d'un article.|  
|**schema_option**|**binary(8)**|Pour les valeurs prises en charge de *schema_option*, consultez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nom de la table créée sur l'Abonné.|  
|**resolver_clsid**|**nvarchar(50)**|ID de l'outil personnalisé de résolution des conflits|  
|**subset_filterclause**|**nvarchar(1000)**|Clause de filtre de l'article.|  
|**missing_col_count**|**int**|Nombre de colonnes manquantes.|  
|**missing_cols**|**varbinary(128)**|Bitmap des colonnes manquantes.|  
|**columns**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**resolver_info**|**nvarchar(255)**|Espace de stockage réservé aux informations complémentaires requises par les outils de résolution des conflits personnalisés.|  
|**view_sel_proc**|**nvarchar(290)**|Nom de la procédure stockée utilisée par l'Agent de fusion pour effectuer le remplissage initial d'un article dans une publication filtrée dynamiquement et pour énumérer les lignes modifiées d'une publication filtrée.|  
|**gen_cur**|**int**|Numéro de génération des modifications locales apportées à la table de base d'un article.|  
|**excluded_cols**|**varbinary(128)**|Bitmap des colonnes exclues de l'article lors de son envoi à l'Abonné.|  
|**excluded_col_count**|**int**|Nombre de colonnes exclues.|  
|**vertical_partition**|**int**|Indique si le filtrage de colonne est activé sur un article de table. **0** indique l’absence de filtrage vertical et publie toutes les colonnes.|  
|**identity_support**|**int**|Spécifie si la gestion automatique de plages d'identités est activée. **1** signifie que les plages d’identités est activée, et **0** signifie qu’il n’existe aucune identité de la plage prise en charge.|  
|**destination_owner**|**sysname**|Le nom du propriétaire de l’objet de destination.|  
|**before_image_objid**|**int**|ID d'objet de la table de suivi. La table de suivi contient certaines valeurs de colonnes clés lorsqu'une publication est configurée pour activer les optimisations de modifications de partitions.|  
|**before_view_objid**|**int**|ID d'objet d'une table de vue. La vue est associée à une table qui détermine si une ligne appartenait à un Abonné particulier avant sa suppression ou sa mise à jour. S’applique uniquement lorsqu’une publication est créée avec *@keep_partition_changes*  =  **true**.|  
|**verify_resolver_signature**|**int**|Spécifie si une signature numérique est vérifiée avant d'utiliser un résolveur dans une réplication de fusion :<br /><br /> **0** = signature n’est pas vérifiée.<br /><br /> **1** = signature est vérifiée pour déterminer s’il est d’une source approuvée.|  
|**allow_interactive_resolver**|**bit**|Spécifie si l'utilisation du composant résolveur interactif sur un article est activée. **1** Spécifie que le résolveur interactif est utilisé sur l’article.|  
|**fast_multicol_updateproc**|**bit**|Spécifie si l'Agent de fusion est activé pour appliquer des modifications à plusieurs colonnes d'une même ligne à partir d'une seule instruction UPDATE.<br /><br /> **0** = émet une instruction UPDATE distincte pour chaque colonne modifiée.<br /><br /> **1** = émis à l’instruction UPDATE qui provoque des mises à jour à plusieurs colonnes dans une instruction.|  
|**check_permissions**|**int**|L’image bitmap des autorisations au niveau des tables qui seront vérifiées lorsque l’Agent de fusion applique les modifications au serveur de publication. *check_permissions* peut avoir l’une des valeurs suivantes :<br /><br /> **0 x 00** = les autorisations ne sont pas vérifiées.<br /><br /> **0 x 10** = les autorisations sont vérifiées sur le serveur de publication avant de peuvent télécharger les insertions effectuées sur l’abonné.<br /><br /> **0 x 20** = les autorisations sont vérifiées sur le serveur de publication avant que les mises à jour effectuées sur l’abonné puissent être téléchargées.<br /><br /> **0 x 40** = les autorisations sont vérifiées sur le serveur de publication avant que les suppressions effectuées sur l’abonné puissent être téléchargées.|  
|**maxversion_at_cleanup**|**int**|Génération la plus élevée pour laquelle les métadonnées sont nettoyées.|  
|**processing_order**|**int**|Indique l’ordre de traitement des articles dans une publication de fusion ; où la valeur **0** indiqué que l’article n’est pas ordonné, et les articles sont traités dans l’ordre de la plus petite à la plus élevée. Si deux articles ont la même valeur, ils sont traités simultanément. Pour plus d’informations, consultez [Spécifier l’ordre de traitement d’articles de fusion](../../relational-databases/replication/merge/specify-the-processing-order-of-merge-articles.md).|  
|**published_in_tran_pub**|**bit**|Indique qu'un article d'une publication de fusion est également publié dans une publication transactionnelle.<br /><br /> **0** = l’article n’est pas publiée dans un article transactionnel.<br /><br /> **1** = l’article est également publié dans un article transactionnel.|  
|**upload_options**|**tinyiny**|Indique si des modifications peuvent être effectuées sur l'Abonné ou téléchargés à partir de l'Abonné ; peut prendre l'une des valeurs suivantes :<br /><br /> **0** = il n’existe aucune restriction sur les mises à jour effectuées sur l’abonné ; toutes les modifications sont téléchargées sur le serveur de publication.<br /><br /> **1** = les modifications sont autorisées sur l’abonné, mais elles ne sont pas téléchargées sur le serveur de publication.<br /><br /> **2** = les modifications ne sont pas autorisées sur l’abonné.|  
|**légère**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**delete_proc**|**sysname**|Procédure utilisée par l'outil de résolution des conflits pour supprimer des lignes lors de la synchronisation.|  
|**before_upd_view_objid**|**int**|ID de la vue d'une table avant les mises à jour.|  
|**delete_tracking**|**bit**|Indique si les suppressions sont répliquées.<br /><br /> **0** = suppressions ne sont pas répliquées.<br /><br /> **1** = les suppressions sont répliquées, ce qui est le comportement par défaut pour la réplication de fusion.<br /><br /> Lorsque la valeur de *delete_tracking* est **0**, de lignes supprimées sur l’abonné doivent être supprimées manuellement sur le serveur de publication et de lignes supprimées sur le serveur de publication doivent être supprimées manuellement sur l’abonné.<br /><br /> Remarque : Une valeur de **0** entraîne une non-convergence.|  
|**compensate_for_errors**|**bit**|Indique si des actions de compensation interviennent lorsque des erreurs se produisent pendant la synchronisation.<br /><br /> **0** = Compensating actions sont désactivées.<br /><br /> **1** = les modifications qui ne peut pas être appliquées à un abonné ou un serveur de publication entraînent toujours des actions de compensation pour annuler ces modifications, qui est le comportement par défaut pour la réplication de fusion.<br /><br /> Remarque : Une valeur de **0** entraîne une non-convergence.|  
|**pub_range**|**bigint**|Taille de la plage d'identité du serveur de publication.|  
|**range**|**bigint**|Taille des valeurs d'identité consécutives qui seraient affectées aux abonnés dans le cas d'un ajustement.|  
|**Seuil**|**int**|Seuil de la plage d'identité exprimé en pourcentage.|  
|**metadata_select_proc**|**sysname**|Le nom de la procédure stockée générée automatiquement utilisée pour accéder aux métadonnées dans les tables système de réplication de fusion.|  
|**stream_blob_columns**|**bit**|Spécifie si une optimisation de flux de données est utilisée lors de la réplication de colonnes d’objets binaires volumineux. **1** signifie que l’optimisation sera tentée.|  
|**preserve_rowguidcol**|**bit**|Indique si la réplication utilise une colonne rowguid existante. La valeur **1** signifie qu’une colonne ROWGUIDCOL existante est utilisée. **0** signifie que la réplication a ajouté la colonne ROWGUIDCOL.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [sysmergearticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysmergearticles-transact-sql.md)  
  
  
