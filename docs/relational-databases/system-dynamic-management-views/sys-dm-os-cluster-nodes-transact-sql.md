---
title: Sys.dm_os_cluster_nodes (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes_TSQL
- dm_os_cluster_nodes
- sys.dm_os_cluster_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_cluster_nodes dynamic management view
ms.assetid: 92fa804e-2d08-42c6-a36f-9791544b1d42
caps.latest.revision: 36
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 6fb5b957cfdaa340ec90a9bb30a5e9db6a54ca31
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosclusternodes-transact-sql"></a>sys.dm_os_cluster_nodes (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque nœud de la configuration de l'instance de cluster de basculement. Si l’instance actuelle est une instance de cluster de basculement, il retourne une liste de nœuds sur lequel cette instance de cluster de basculement (anciennement « serveur virtuel ») a été définie. Si l'instance de serveur active n'est pas une instance de cluster de basculement, elle retourne un ensemble de lignes vides.  
  
> **Remarque :** à appeler à partir [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_cluster_nodes**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**NodeName**|**sysname**|Nom d'un nœud de la configuration de l'instance de cluster de basculement (serveur virtuel) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|status|**int**|État du nœud dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance de cluster de basculement : 0, 1, 2, 3, -1. Pour plus d’informations, consultez [fonction GetClusterNodeState](http://go.microsoft.com/fwlink/?LinkId=204794).|  
|status_description|**nvarchar(20)**|Description de l'état du nœud de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 0 = actif<br /><br /> 1 = inactif<br /><br /> 2 = en pause<br /><br /> 3 = en cours de jointure<br /><br /> -1 = inconnu|  
|is_current_owner|bit|1 signifie que ce nœud est le propriétaire actuel de la ressource du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="remarks"></a>Notes  
 Lorsque le clustering de basculement est activé, l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être exécutée sur n'importe quel nœud du cluster de basculement faisant partie de la configuration de l'instance de cluster de basculement (serveur virtuel) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> **Remarque :** cette vue remplace la fonction fn_virtualservernodes, qui sera déconseillée dans une version ultérieure.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW SERVER STATE sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise sys. dm_os_cluster_nodes pour retourner les nœuds d'une instance de serveur cluster.  
  
```  
SELECT NodeName, status, status_description, is_current_owner   
FROM sys.dm_os_cluster_nodes;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|NodeName|status|status_description|is_current_owner|  
|--------------|------------|-------------------------|------------------------|  
|node1|0|up|1|  
|node2|0|up|0|  
|Node3|1|down|0|  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.dm_os_cluster_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-properties-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [sys.fn_virtualservernodes &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-virtualservernodes-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  



