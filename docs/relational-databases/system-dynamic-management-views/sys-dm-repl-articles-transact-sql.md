---
title: Sys.dm_repl_articles (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_repl_articles_TSQL
- dm_repl_articles
- dm_repl_articles_TSQL
- sys.dm_repl_articles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_articles dynamic management function
ms.assetid: 794d514e-bacd-432e-a8ec-3a063a97a37b
caps.latest.revision: 16
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 12a9e842c8ff0ebbf74e9d1126de52224980b473
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmreplarticles-transact-sql"></a>sys.dm_repl_articles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les objets de base de données publiés sous forme d'articles dans une topologie de réplication.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**artcache_db_address**|**varbinary(8)**|Adresse en mémoire de la structure de base de données du cache pour la base de données de publication.|  
|**artcache_table_address**|**varbinary(8)**|Adresse en mémoire de la structure de table du cache pour l'article de table publié.|  
|**artcache_schema_address**|**varbinary(8)**|Adresse en mémoire de la structure d'article du cache pour un article de table publié.|  
|**artcache_article_address**|**varbinary(8)**|Adresse en mémoire de la structure d’article mis en cache pour un article de table publié.|  
|**artid**|**bigint**|Identifie sans équivoque chaque entrée de cette table.|  
|**artfilter**|**bigint**|ID de la procédure stockée utilisée pour filtrer horizontalement l'article.|  
|**artobjid**|**bigint**|ID de l’objet publié.|  
|**artpubid**|**bigint**|Identificateur de la publication à laquelle appartient l'article.|  
|**artstatus**|**tinyint**|Masque binaire des options et de l'état de l'article, lequel peut être le résultat OR logique au niveau du bit et peut prendre une ou plusieurs des valeurs suivantes :<br /><br /> **1** = l’article est actif.<br /><br /> **8** = inclut le nom de colonne dans les instructions INSERT.<br /><br /> **16** = utilise des instructions paramétrée.<br /><br /> **24** = inclut le nom de colonne dans les instructions INSERT et utilise des instructions paramétrées.<br /><br /> Par exemple, un article actif utilisant des instructions paramétrables posséderait la valeur 17 dans cette colonne. La valeur 0 signifie que l'article est inactif et qu'aucune propriété supplémentaire n'est définie.|  
|**arttype**|**tinyint**|Type d'article :<br /><br /> **1** = article basé sur le journal.<br /><br /> **3** = article basé sur journal avec filtre manuel.<br /><br /> **5** = article basé sur le journal avec vue manuelle.<br /><br /> **7** = article basé sur le journal avec filtre manuel et vue manuelle.<br /><br /> **8** = exécution d’une procédure stockée.<br /><br /> **24** = exécution d’une procédure stockée sérialisable.<br /><br /> **32** = procédure stockée (schéma uniquement).<br /><br /> **64** = vue (schéma uniquement).<br /><br /> **128** = fonction (schéma uniquement).|  
|**wszArtdesttable**|**nvarchar(514)**|Nom de l'objet publié à la destination.|  
|**wszArtdesttableowner**|**nvarchar(514)**|Propriétaire de l'objet publié à la destination.|  
|**wszArtinscmd**|**nvarchar(510)**|Commande ou procédure stockée utilisée pour les insertions.|  
|**cmdTypeIns**|**int**|Syntaxe d'appel pour la procédure stockée d'insertion, pouvant être une de ces valeurs.<br /><br /> **1** = APPEL<br /><br /> **2** = SQL<br /><br /> **3** = NONE<br /><br /> **7** = INCONNU|  
|**wszArtdelcmd**|**nvarchar(510)**|Commande ou procédure stockée utilisée pour les suppressions.|  
|**cmdTypeDel**|**int**|Syntaxe d'appel pour la procédure stockée de suppression, pouvant être une de ces valeurs.<br /><br /> **0** = XCALL<br /><br /> **1** = APPEL<br /><br /> **2** = SQL<br /><br /> **3** = NONE<br /><br /> **7** = INCONNU|  
|**wszArtupdcmd**|**nvarchar(510)**|Commande ou procédure stockée utilisée pour les mises à jour.|  
|**cmdTypeUpd**|**int**|Syntaxe d'appel pour la procédure stockée de mise à jour, pouvant être une de ces valeurs.<br /><br /> **0** = XCALL<br /><br /> **1** = APPEL<br /><br /> **2** = SQL<br /><br /> **3** = NONE<br /><br /> **4** = MCALL<br /><br /> **5** = VCALL<br /><br /> **6** = SCALL<br /><br /> **7** = INCONNU|  
|**wszArtpartialupdcmd**|**nvarchar(510)**|Commande ou procédure stockée utilisée pour les mises à jour partielles.|  
|**cmdTypePartialUpd**|**int**|Syntaxe d'appel pour la procédure stockée de mise à jour partielle, pouvant être une de ces valeurs.<br /><br /> **2** = SQL|  
|**numCol**|**int**|Nombre de colonnes dans la partition pour un article filtré verticalement.|  
|**artcmdtype**|**tinyint**|Type de commande actuellement répliqué, pouvant être une de ces valeurs.<br /><br /> **1** = INSERTION<br /><br /> **2** = SUPPRESSION<br /><br /> **3** = MISE À JOUR<br /><br /> **4** = UPDATETEXT<br /><br /> **5** = none<br /><br /> **6** = pour usage interne uniquement<br /><br /> **7** = pour usage interne uniquement<br /><br /> **8** = mise à jour partielle|  
|**artgeninscmd**|**nvarchar(510)**|Modèle de commande INSERT reposant sur les colonnes incluses dans l'article.|  
|**artgendelcmd**|**nvarchar(510)**|Modèle de commande DELETE, qui peut inclure la clé primaire ou les colonnes incluses dans l'article, selon la syntaxe d'appel utilisée.|  
|**artgenupdcmd**|**nvarchar(510)**|Modèle de commande UPDATE, qui peut inclure la clé primaire, les colonnes mises à jour ou une liste complète de colonnes, selon la syntaxe d'appel utilisée.|  
|**artpartialupdcmd**|**nvarchar(510)**|Modèle de commande UPDATE partielle, qui inclut la clé primaire et les colonnes mises à jour.|  
|**artupdtxtcmd**|**nvarchar(510)**|Modèle de commande UPDATETEXT, qui inclut la clé primaire et les colonnes mises à jour.|  
|**artgenins2cmd**|**nvarchar(510)**|Modèle de commande INSERT utilisé lors de l'harmonisation d'un article au cours d'un traitement d'instantanés simultanés.|  
|**artgendel2cmd**|**nvarchar(510)**|Modèle de commande DELETE utilisé lors de l'harmonisation d'un article au cours d'un traitement d'instantanés simultanés.|  
|**fInReconcile**|**tinyint**|Indique si un article est en cours d'harmonisation lors d'un traitement d'instantanés simultanés.|  
|**fPubAllowUpdate**|**tinyint**|Indique si la publication autorise la mise à jour d'abonnements.|  
|**intPublicationOptions**|**bigint**|Image précisant les options de publication supplémentaires, où les valeurs des options au niveau du bit peuvent être :<br /><br /> **0 x 1** - activées pour la réplication d’égal à égal.<br /><br /> **0 x 2** -publier les modifications locales uniquement.<br /><br /> **0 x 4** - activée pour les abonnés non SQL Server.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation VIEW DATABASE STATE sur la base de données de publication pour appeler **dm_repl_articles**.  
  
## <a name="remarks"></a>Notes  
 Les informations ne sont retournées que pour les objets de base de données répliqués actuellement chargés dans le cache des articles de réplication.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la réplication &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)  
  
  

