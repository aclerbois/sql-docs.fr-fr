---
title: Sys.dm_pdw_nodes (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/07/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 93966909-d758-4d50-950b-f5066d104fa6
caps.latest.revision: 7
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 4e51b83d1e21a46dd7c7854660e033ea8afe14e6
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmpdwnodes-transact-sql"></a>Sys.dm_pdw_nodes (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Contient des informations sur tous les nœuds dans [!INCLUDE[ssAPS](../../includes/ssaps-md.md)]. Il répertorie une ligne par un nœud dans l’appliance.  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Id numérique unique associé au nœud.<br /><br /> Clé pour cette vue.|Le point d’entrée unique pour l’application, quel que soit le type.|  
|Type|**nvarchar(32)**|Type du nœud.|'COMPUTE', « CONTRÔLE », « ADMINISTRATION »|  
|name|**nvarchar(32)**|Nom logique du nœud.|Toute chaîne de longueur appropriée.|  
|address|**nvarchar(32)**|Adresse IP de ce nœud.|Dans le format [0-255]. 0-255. 0-255. 0-255.|  
|is_passive|**int**|Indique si l’ordinateur virtuel en cours d’exécution du nœud est en cours d’exécution sur le serveur affecté ou a basculé vers le serveur de secours.|0 – machine virtuelle du nœud est en cours d’exécution sur le serveur d’origine.<br /><br /> 1 – machine virtuelle du nœud est en cours d’exécution sur le serveur de secours.|  
|région|**nvarchar(32)**|La région où le nœud est en cours d’exécution.|'PDW', 'HDINSIGHT'|  
  
## <a name="see-also"></a>Voir aussi  
 [Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
