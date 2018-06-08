---
title: IHcolumns (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- IHcolumns_TSQL
- IHcolumns
dev_langs:
- TSQL
helpviewer_keywords:
- IHcolumns system table
ms.assetid: 5bb027e5-5279-487b-9c33-5f402987253c
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d713dbd76921955aab6066b51d163a17ca7cba9a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ihcolumns-transact-sql"></a>IHcolumns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **IHcolumns** (table système) contient une ligne pour chaque colonne publiée. Cette table est utilisée pour définir comment types de données de colonne depuis le SQL Server de publication non seront représentées lors de la publication, qui mappe les types de données entre un systèmes de gestion de base de données (SGBD) non SQL Server et SQL Server. Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**column_id**|**int**|Identifie une colonne publiée.|  
|**publishercolumn_id**|**int**|Associe une colonne publiée avec les métadonnées de colonne stockées dans le [IHpublishercolumns](../../relational-databases/system-tables/ihpublishercolumns-transact-sql.md) (table système).|  
|**nom**|**sysname**|Spécifie le nom de la colonne.|  
|**article_id**|**int**|Identifie l'article auquel appartient la colonne.|  
|**column_ordinal**|**int**|Identifie la colonne par ordre.|  
|**mapped_type**|**tinyint**|Type de données colonne de la colonne de destination chez l'abonné.|  
|**mapped_length**|**bigint**|Longueur de la colonne chez l'abonné.|  
|**mapped_prec**|**int**|Précision de la colonne chez l'abonné.|  
|**mapped_scale**|**int**|Échelle de la colonne chez l'abonné.|  
|**mapped_nullable**|**bit**|Indique si la colonne sur l’abonné accepte les valeurs NULL, où **1** signifie que les valeurs NULL sont acceptées.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sysarticlecolumns &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)   
 [sysarticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
