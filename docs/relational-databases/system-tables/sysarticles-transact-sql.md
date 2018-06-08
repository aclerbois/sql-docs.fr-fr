---
title: sysarticles (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sysarticles
- sysarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticles system table
ms.assetid: 9d9d5d51-6d8f-4e42-84a9-82e58eb0301e
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a89895f89e55daa70ca1357aabba73dc58dffd31
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysarticles-transact-sql"></a>sysarticles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque article défini dans la base de données locale. Cette table est stockée dans la base de données publiée.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Colonne d'identité fournissant un numéro d'identification unique pour l'article|  
|**creation_script**|**nvarchar(255)**|Script du schéma de l'article.|  
|**del_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des suppressions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**description**|**nvarchar(255)**|L’entrée descriptive de l’article.|  
|**dest_table**|**sysname**|Nom de la table de destination|  
|**Filter**|**int**|Identificateur de la procédure stockée, utilisé pour la partition horizontale.|  
|**filter_clause**|**ntext**|Clause WHERE de l'article utilisée pour le filtrage horizontal.|  
|**ins_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des insertions avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**nom**|**sysname**|Nom associé à l'article et unique dans la publication|  
|**objid**|**int**|Identificateur de l'objet de la table publiée|  
|**pubid**|**int**|Identificateur de la publication à laquelle appartient l'article|  
|**pre_creation_cmd**|**tinyint**|Commande de précréation pour les instructions DROP TABLE, DELETE TABLE ou TRUNCATE :<br /><br /> **0** = none.<br /><br /> **1** = SUPPRIMER.<br /><br /> **2** = SUPPRESSION.<br /><br /> **3** = TRUNCATE.|  
|**status**|**tinyint**|Masque de bits de l'état et des options d'article, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **1** = l’article est actif.<br /><br /> **8** = inclut le nom de colonne dans les instructions INSERT.<br /><br /> **16** = utilise des instructions paramétrée.<br /><br /> **24** = inclut le nom de colonne dans les instructions INSERT et utilise des instructions paramétrées.<br /><br /> **64** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> Par exemple, un article actif utilisant des instructions paramétrables posséderait la valeur de **17** dans cette colonne. La valeur **0** signifie que l’article est inactif et qu’aucune des propriétés supplémentaires ne sont définies.|  
|**sync_objid**|**int**|Identificateur de la table ou de la vue représentant la définition de l'article.|  
|**type**|**tinyint**|Type d'article :<br /><br /> **1** = article basé sur le journal.<br /><br /> **3** = article basé sur journal avec filtre manuel.<br /><br /> **5** = article basé sur le journal avec vue manuelle.<br /><br /> **7** = article basé sur le journal avec filtre manuel et vue manuelle.<br /><br /> **8** = exécution d’une procédure stockée.<br /><br /> **24** = exécution d’une procédure stockée sérialisable.<br /><br /> **32** = procédure stockée (schéma uniquement).<br /><br /> **64** = vue (schéma uniquement).<br /><br /> **128** = fonction (schéma uniquement).|  
|**upd_cmd**|**nvarchar(255)**|Type de commande de réplication utilisé pour répliquer des mises à jour avec des articles de table. Pour plus d’informations, consultez [Spécifier le mode de propagation des modifications des articles transactionnels](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**schema_option**|**binary(8)**|Masque de bits des options de génération de schéma pour l'article, qui déterminent quelles sont les parties du schéma d'article devant donner lieu à un script pour la remise à l'Abonné. Pour plus d’informations sur les options de schéma, consultez [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Propriétaire de la table dans la base de données de destination|  
|**ins_scripting_proc**|**int**|Procédure stockée ou script personnalisé inscrit exécuté lorsqu'une instruction INSERT est répliquée.|  
|**del_scripting_proc**|**int**|Procédure stockée ou script personnalisé inscrit exécuté lorsqu'une instruction DELETE est répliquée.|  
|**upd_scripting_proc**|**int**|Procédure stockée ou script personnalisé inscrit exécuté lorsqu'une instruction UPDATE est répliquée.|  
|**custom_script**|**nvarchar(2048)**|Procédure stockée ou script personnalisé inscrit exécuté à la fin du déclencheur DDL.|  
|**fire_triggers_on_snapshot**|**bit**|Indique si les répliquées déclencheurs sont exécutés lorsque l’instantané est appliqué, ce qui peut être une des valeurs suivantes :<br /><br /> **0** = les déclencheurs ne sont pas exécutées.<br /><br /> **1** = les déclencheurs sont exécutés.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)  
  
  
