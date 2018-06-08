---
title: IHextendedArticleView (Transact-SQL) | Documents Microsoft
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
- IHextendedArticleView_TSQL
- IHextendedArticleView
dev_langs:
- TSQL
helpviewer_keywords:
- IHextendedArticleView view
ms.assetid: 19ef0a12-3214-4bb0-9c25-a665897e65a2
caps.latest.revision: 11
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2e0bcc294189f4902fd8d39de230166e6956dd53
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ihextendedarticleview-transact-sql"></a>IHextendedArticleView (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **IHextendedArticleView** vue expose des informations sur les articles dans une publication non SQL Server. Cette vue est stockée dans le **distribution** base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Identificateur unique du serveur de publication.|  
|**publication_id**|**int**|Identificateur unique de la publication.|  
|**article**|**sysname**|Nom de l'article.|  
|**destination_object**|**sysname**|Nom de l'objet publié sur l'Abonné.|  
|**source_owner**|**sysname**|Propriétaire de l'objet publié sur le serveur de publication.|  
|**source_object**|**sysname**|Nom de l'objet publié sur le serveur de publication.|  
|**description**|**nvarchar(255)**|Description de l'article.|  
|**creation_script**|**nvarchar(255)**|Script de création du schéma de l'article.|  
|**del_cmd**|**nvarchar(255)**|Commande exécutée pour une opération DELETE.|  
|**Filter**|**int**|Identificateur de la procédure stockée utilisée pour définir la partition horizontale.|  
|**filter_clause**|**ntext**|Clause WHERE utilisée pour filtrer horizontalement l'article.|  
|**ins_cmd**|**nvarchar(255)**|Commande exécutée pour une opération INSERT.|  
|**pre_creation_cmd**|**tinyint**|Commande de pré-création pour les instructions DROP TABLE, DELETE TABLE ou TRUNCATE :<br /><br /> **0** = none.<br /><br /> **1** = SUPPRIMER.<br /><br /> **2** = SUPPRESSION.<br /><br /> **3** = TRUNCATE.|  
|**status**|**tinyint**|Masque de bits de l'état et des options d'article, qui peut être le résultat OR logique au niveau du bit d'au moins l'une des valeurs suivantes :<br /><br /> **1** = l’article est actif.<br /><br /> **8** = inclut le nom de colonne dans les instructions INSERT.<br /><br /> **16** = utilise des instructions paramétrée.<br /><br /> **24** = inclut le nom de colonne dans les instructions INSERT et utilise des instructions paramétrées.<br /><br /> Par exemple, un article actif utilisant des instructions paramétrables posséderait la valeur de **17** dans cette colonne. La valeur **0** signifie que l’article est inactif et qu’aucune des propriétés supplémentaires ne sont définies.|  
|**type**|**tinyint**|Type d'article :<br /><br /> **1** = article basé sur le journal.<br /><br /> **3** = article basé sur journal avec filtre manuel.<br /><br /> **5** = article basé sur le journal avec vue manuelle.<br /><br /> **7** = article basé sur le journal avec filtre manuel et vue manuelle.|  
|**upd_cmd**|**nvarchar(255)**|Commande exécutée pour une opération UPDATE.|  
|**schema_option**|**binaire**|Indique ce qui doit faire l'objet d'un script. Consultez [sp_addarticle &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) pour obtenir la liste des options de schéma pris en charge.|  
|**dest_owner**|**sysname**|Propriétaire de l'objet publié dans la base de données de destination.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
