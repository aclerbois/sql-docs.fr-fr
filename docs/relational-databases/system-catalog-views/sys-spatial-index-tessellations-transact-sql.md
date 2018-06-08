---
title: Sys.spatial_index_tessellations (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- spatial_index_tessellations
- sys.spatial_index_tessellations_TSQL
- spatial_index_tessellations_TSQL
- sys.spatial_index_tessellations
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_index_tessellations catalog view
ms.assetid: 8b17a9a4-b57f-4220-8138-fc73581b1670
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 766b756ab0b00c3e5239a2fe5063e22eed230861
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspatialindextessellations-transact-sql"></a>sys.spatial_index_tessellations (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

 Représente les informations sur le schéma de pavage et les paramètres de chaque index spatial.  
  
> [!NOTE]  
>  Pour plus d’informations sur le pavage, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID de l'objet sur lequel l'index est défini. Chaque (object_id, index_id) paire a une entrée correspondante [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md).|  
|index_id|**int**|ID de l'index spatial où la colonne indexée est définie.|  
|tessellation_scheme|**sysname**|Nom du schéma de pavage, un des : la valeur GEOMETRY_GRID, GEOGRAPHY_GRID|  
|bounding_box_xmin|**float(53)**|Coordonnée X du coin inférieur gauche de la délimitation boîte, des : NULL = non applicable pour un schéma de pavage donné (tel que GEOGRAPHY_GRID) *n* = si tessellation_scheme a la valeur GEOMETRY_GRID, la valeur de coordonnée x-min.                     **Remarque :** les coordonnées définies par les paramètres du rectangle englobant sont interprétées pour chaque objet en fonction de son [identificateur de référence spatiale (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).|  
|bounding_box_ymin|**float(53)**|Coordonnée Y du coin inférieur gauche de la délimitation boîte, des : NULL = non applicable pour un schéma de pavage donné (tel que GEOGRAPHY_GRID) *n* = si tessellation_scheme a la valeur GEOMETRY_GRID, la valeur de coordonnée y-min|  
|bounding_box_xmax|**float(53)**|Coordonnée X du coin supérieur droit d’englobant boîte, des : NULL = non applicable pour un schéma de pavage donné (tel que GEOGRAPHY_GRID) *n* = si tessellation_scheme a la valeur GEOMETRY_GRID, la valeur de coordonnée x-max|  
|bounding_box_ymax|**float(53)**|Coordonnée Y du coin supérieur droit de la délimitation boîte, des : NULL = non applicable pour un schéma de pavage donné (tel que GEOGRAPHY_GRID) *n* = si tessellation_scheme a la valeur GEOMETRY_GRID, la valeur de coordonnée y-max|  
|level_1_grid|**smallint**|Densité de la grille pour la grille de niveau supérieur. Si tessellation_scheme a la valeur GEOMETRY_GRID ou GEOGRAPHY_GRID, un des : 16 = grille 4 par 4 (faible) 64 = 8 par grille 8 (MEDIUM) 256 = 16 par 16 grille (élevé) NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_1_grid_desc|**nvarchar(60)**|Densité de grille pour la grille de niveau supérieur, parmi : faible moyenne élevée NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_2_grid|**smallint**|Densité de la grille pour la grille de 2e niveau. Si tessellation_scheme a la valeur GEOMETRY_GRID ou GEOGRAPHY_GRID, un des : 16 = grille 4 par 4 (faible) 64 = 8 par grille 8 (MEDIUM) 256 = 16 par 16 grille (élevé) NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_2_grid_desc|**nvarchar(60)**|Densité de grille pour la grille de 2e niveau, parmi : faible moyenne élevée NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_3_grid|**smallint**|Densité de la grille pour la grille de 3e niveau.   Si tessellation_scheme a la valeur GEOMETRY_GRID ou GEOGRAPHY_GRID, un des : 16 = grille 4 par 4 (faible) 64 = 8 par grille 8 (MEDIUM) 256 = 16 par 16 grille (élevé) NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_3_grid_desc|**nvarchar(60)**|Densité de grille pour la grille de 3e niveau, parmi : faible moyenne élevée NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_4_grid|**smallint**|Densité de la grille pour la grille de 4e niveau. Si tessellation_scheme a la valeur GEOMETRY_GRID ou GEOGRAPHY_GRID, un des : 16 = grille 4 par 4 (faible) 64 = 8 par grille 8 (MEDIUM) 256 = 16 par 16 grille (élevé) NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|level_4_grid_desc|**nvarchar(60)**|Densité de grille pour la grille de 4e niveau, parmi : < faible moyenne élevée NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial.  NULL est retourné lorsque le nouveau pavage de SQL Server 11 est utilisé.|  
|cells_per_object|**int**|Nombre de cellules par objet spatial, un des : si tessellation_scheme a la valeur GEOMETRY_GRID ou GEOGRAPHY_GRID, *n* = nombre de cellules par objet NULL = non applicable pour donné le schéma de pavage ou de type d’index spatial|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
