---
title: sp_helpmergearticle (Transact-SQL) | Documents Microsoft
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
- sp_helpmergearticle
- sp_helpmergearticle_TSQL
helpviewer_keywords:
- sp_helpmergearticle
ms.assetid: 0fb9986a-3c33-46ef-87bb-297396ea5a6a
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 46c8751290a1e5c08cb2f77a458aae252ec903c2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelpmergearticle-transact-sql"></a>sp_helpmergearticle (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur un article. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication ou sur la base de données d'abonnement d'un Abonné de republication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergearticle [ [ @publication = ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=**] **'***publication***'**  
 Nom de la publication sur laquelle extraire des informations. *publication*est **sysname**, avec une valeur par défaut **%**, qui retourne des informations sur tous les articles de fusion contenus dans toutes les publications dans la base de données actuelle.  
  
 [  **@article=**] **'***article***'**  
 Nom de l'article pour lequel des informations doivent être retournées. *article*est **sysname**, avec une valeur par défaut **%**, qui retourne des informations sur tous les articles de fusion de la publication donnée.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificateur de l'article|  
|**nom**|**sysname**|Nom de l'article.|  
|**source_owner**|**sysname**|Nom du propriétaire de l'objet source|  
|**source_object**|**sysname**|Nom de l'objet source à partir duquel l'article doit être ajouté.|  
|**lui**|**sysname**|Nom du propriétaire de la vue qui définit l'article publié.|  
|**sync_object**|**sysname**|Nom de l'objet personnalisé utilisé pour établir les données initiales pour la partition.|  
|**description**|**nvarchar(255)**|Description de l'article|  
|**status**|**tinyint**|État de l'article, qui peut être l'un des suivants :<br /><br /> **1** = inactif<br /><br /> **2** = actif<br /><br /> **5** = l’opération de data definition language (DDL) en attente<br /><br /> **6** = opération DDL avec un instantané nouvellement généré<br /><br /> Remarque : Lorsqu’un article est réinitialisé, les valeurs de **5** et **6** sont modifiés en **2**.|  
|**creation_script**|**nvarchar(255)**|Chemin d'accès et nom d'un script de schéma d'article facultatif utilisé pour créer l'article dans la base de données d'abonnement.|  
|**conflict_table**|**nvarchar(270)**|Nom de la table stockant les conflits d'insertion ou de mise à jour.|  
|**article_resolver**|**nvarchar(255)**|Outil de résolution personnalisé pour l'article|  
|**subset_filterclause**|**nvarchar(1000)**|Clause WHERE spécifiant le filtrage horizontal.|  
|**pre_creation_command**|**tinyint**|Méthode de précréation, qui peut être l'une des suivantes :<br /><br /> **0** = none<br /><br /> **1** = supprimer<br /><br /> **2** = suppression<br /><br /> **3** = truncate|  
|**schema_option**|**binary(8)**|Bitmap de l'option de génération de schéma pour l'article. Pour plus d’informations sur cette option de bitmap, consultez [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) ou [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md).|  
|**type**|**smallint**|Type de l'article, qui peut être l'un des suivants :<br /><br /> **10** = table<br /><br /> **32** = procédure stockée<br /><br /> **64** = vue ou la vue indexée<br /><br /> **128** = fonction définie par l’utilisateur<br /><br /> **160** = schéma de synonyme uniquement|  
|**column_tracking**|**int**|Paramètre pour effectuer le suivi au niveau des colonnes ; où **1** signifie que le suivi au niveau de la colonne est activé, et **0** signifie que le suivi au niveau de la colonne est désactivée.|  
|**resolver_info**|**nvarchar(255)**|Nom de l'outil de résolution de l'article|  
|**vertical_partition**|**bit**|Si l’article est partitionné verticalement ; où **1** signifie que l’article est partitionné verticalement, et **0** signifie qu’il n’est pas.|  
|**destination_owner**|**sysname**|Propriétaire de l'objet de destination. Applicable uniquement aux articles de schémas de fonctions utilisateur (UDF), aux vues et aux procédures stockées de fusion.|  
|**identity_support**|**int**|Si la gestion de plages d’identité automatique est activée ; où **1** est activé et **0** est désactivé.|  
|**pub_identity_range**|**bigint**|Taille de plage à utiliser lors de l'affectation de nouvelles valeurs d'identité. Pour plus d’informations, consultez la section « Réplication de fusion » de [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**identity_range**|**bigint**|Taille de plage à utiliser lors de l'affectation de nouvelles valeurs d'identité. Pour plus d’informations, consultez la section « Réplication de fusion » de [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**Seuil**|**int**|Pourcentage de valeur utilisé pour les abonnés exécutant [!INCLUDE[ssEW](../../includes/ssew-md.md)] ou des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **seuil** contrôle lorsque l’Agent de fusion affecte une nouvelle plage d’identité. Lorsque le pourcentage de valeurs spécifié dans le seuil est utilisé, l'Agent de fusion crée une nouvelle plage d'identité. Pour plus d’informations, consultez la section « Réplication de fusion » de [répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).|  
|**verify_resolver_signature**|**int**|Si une signature numérique est vérifiée avant d’utiliser un résolveur dans la réplication de fusion ; où **0** signifie que la signature n’est pas vérifiée, et **1** signifie que la signature est vérifiée pour voir si elle provient d’une source approuvée.|  
|**destination_object**|**sysname**|Nom de l'objet de destination. Applicable uniquement aux articles de schémas de fonctions utilisateur, aux vues et aux procédures stockées de fusion.|  
|**allow_interactive_resolver**|**int**|Si le résolveur interactif est utilisé sur un article ; où **1** signifie que ce programme de résolution est utilisé, et **0** signifie qu’il n’est pas utilisé.|  
|**fast_multicol_updateproc**|**int**|Active ou désactive l’Agent de fusion pour appliquer les modifications à plusieurs colonnes dans la même ligne dans une instruction UPDATE ; où **1** signifie que plusieurs colonnes sont mises à jour dans une instruction, et **0** signifie que des instructions UPDATE séparées est émises pour chaque colonne mise à jour.|  
|**check_permissions**|**int**|Valeur entière qui représente la bitmap des autorisations au niveau des tables qui sont vérifiées. Pour obtenir la liste des valeurs possibles, consultez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**processing_order**|**int**|Ordre selon lequel les modifications de données sont appliquées aux articles d'une publication.|  
|**upload_options**|**tinyint**|Définit des restrictions sur les mises à jour effectuées sur un Abonné ayant un abonnement client. Peut avoir une des valeurs suivantes.<br /><br /> **0** = il n’existe aucune restriction sur les mises à jour effectuées sur l’abonné avec un abonnement client ; toutes les modifications sont téléchargées sur le serveur de publication.<br /><br /> **1** = les modifications sont autorisées sur un abonné disposant d’un abonnement client, mais elles ne sont pas téléchargées sur le serveur de publication.<br /><br /> **2** = modifications ne sont pas autorisées sur un abonné avec un abonnement client.<br /><br /> Pour plus d’informations, consultez [Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).|  
|**identityrangemanagementoption**|**int**|Si la gestion de plages d’identité automatique est activée ; où **1** est activé et **0** est désactivé.|  
|**delete_tracking**|**bit**|Si les suppressions sont répliquées ; où **1** signifie que les suppressions sont répliquées, et **0** signifie qu’ils ne sont pas.|  
|**compensate_for_errors**|**bit**|Indique si des actions de compensation sont entreprises lorsque des erreurs sont rencontrées lors de la synchronisation ; où **1** indique que des actions de compensation sont entreprises, et **0** signifie que les actions de compensation ne sont pas prises.|  
|**partition_options**|**tinyint**|Définit le mode de partitionnement des données de l'article, ce qui permet l'optimisation des performances lorsque toutes les lignes appartiennent à une seule partition ou à un seul abonnement. *partition_options* peut prendre l’une des valeurs suivantes.<br /><br /> **0** = le filtrage de l’article, est statique ou ne produit pas un sous-ensemble unique de données pour chaque partition ; autrement dit, il est une partition en « chevauchement ».<br /><br /> **1** = les partitions se chevauchent et les mises à jour de données manipulation language (DML) effectuées sur l’abonné ne peut pas modifier la partition à laquelle appartient une ligne.<br /><br /> **2** = le filtrage de l’article génère sans chevauchement de partitions, mais plusieurs abonnés peuvent recevoir la même partition.<br /><br /> **3** = le filtrage de l’article génère des partitions sans chevauchement qui sont uniques pour chaque abonnement.|  
|**artid**|**uniqueidentifier**|Identificateur qui identifie l'article de façon unique|  
|**pubid**|**uniqueidentifier**|Identificateur qui identifie de manière unique la publication dans laquelle l'article est publié.|  
|**stream_blob_columns**|**bit**|Indique si un optimisation du flux de données est utilisée lors de la réplication de colonnes BLOB (binary large objects). **1** signifie que l’optimisation est utilisée, et **0** signifie que l’optimisation n’est pas utilisée.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergearticle** est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** fixe du rôle de base de données dans la base de données de publication, le **replmonitor** du rôle dans la base de données de distribution, ou de la liste de contrôle d’accès pour une publication peuvent exécuter **sp_helpmergearticle**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_helpmergearticle](../../relational-databases/replication/codesnippet/tsql/sp-helpmergearticle-tran_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés de l’Article](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_dropmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
