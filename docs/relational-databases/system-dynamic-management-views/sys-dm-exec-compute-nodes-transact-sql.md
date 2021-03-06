---
title: sys.dm_exec_compute_nodes (Transact-SQL) | Microsoft Docs
ms.custom: 
ms.date: 03/15/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.service: 
ms.component: dmv's
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- DM_EXEC_COMPUTE_NODES_TSQL
- DM_EXEC_COMPUTE_NODES
- SYS.DM_EXEC_COMPUTE_NODES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_compute_nodes management view
- PolyBase, views
- PolyBase management views
- dm_exec_compute_nodes management view
ms.assetid: 0de4b7a4-401f-4e2d-9ab0-c54587e05154
caps.latest.revision: 
author: stevestein
ms.author: sstein
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: cdd765d5f7b46e1189357e588d8b28ddd7acd029
ms.sourcegitcommit: c556eaf60a49af7025db35b7aa14beb76a8158c5
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="sysdmexeccomputenodes-transact-sql"></a>sys.dm_exec_compute_nodes (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]

  Contient des informations sur les nœuds utilisé avec la gestion des données PolyBase. Il répertorie une ligne par nœud.  
  
 Utilisez cette vue de gestion dynamique pour afficher la liste de tous les nœuds du cluster de la montée en puissance parallèle avec leur rôle, le nom et l’adresse IP.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|compute_node_id|**int**|Id numérique unique associé au nœud. Clé pour cette vue.|Unique sur le cluster de montée en puissance parallèle, quelle que soit le type.|  
|type|**nvarchar(32)**|Type du nœud.|'COMPUTE', 'PRINCIPAL'|  
|name|**nvarchar(32)**|Nom logique du nœud.|Toute chaîne de longueur appropriée.|  
|address|**nvarchar(32)**|Adresse P de ce nœud.|Plage d’adresses IP|  
  
## <a name="see-also"></a>Voir aussi  
 [PolyBase, résolution des problèmes avec les vues de gestion dynamique](http://msdn.microsoft.com/library/ce9078b7-a750-4f47-b23e-90b83b783d80)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Base de données associés des vues de gestion dynamique &#40; Transact-SQL &#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
  
