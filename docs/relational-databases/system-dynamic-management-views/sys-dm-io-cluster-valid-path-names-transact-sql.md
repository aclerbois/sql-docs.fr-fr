---
title: Sys.dm_io_cluster_valid_path_names (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_io_cluster_valid_path_names
- dm_io_cluster_valid_path_names_TSQL
- sys.dm_io_cluster_valid_path_names_TSQL
- dm_io_cluster_valid_path_names
dev_langs:
- TSQL
helpviewer_keywords:
- dm_io_cluster_valid_path_names
- sys.dm_io_cluster_valid_path_names
- cluster valid path names
- csv name
- cluster shared volume names
ms.assetid: 5bc8a0e5-6c72-425b-8c58-f276eb9add2c
caps.latest.revision: 6
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 73be0586ae8f12633fff733f763f021f7133d119
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmioclustervalidpathnames-transact-sql"></a>sys.dm_io_cluster_valid_path_names (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur tous les disques partagés valides, y compris les volumes partagés de cluster, pour une instance de cluster de basculement SQL Server. Si l'instance n'est pas en cluster, un ensemble de lignes vide est retourné.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom_chemin**|**Nvarchar(512)**|Point de montage de volume ou chemin d'accès de lecteur pouvant être utilisé comme répertoire racine pour la base de données et les fichiers journaux. N'accepte pas la valeur NULL.|  
|**cluster_owner_node**|**Nvarchar(64)**|Propriétaire actuel du disque. Pour les volumes partagés de cluster (CSV), le propriétaire est le nœud qui héberge le serveur de métadonnées. N'accepte pas la valeur NULL.|  
|**is_cluster_shared_volume**|**Bit**|Retourne 1 si le lecteur sur lequel ce chemin d'accès se trouve est un volume partagé de cluster ; sinon retourne 0.|  
  
## <a name="remarks"></a>Notes  
 Une instance de cluster de basculement SQL Server doit utiliser le stockage partagé sur tous ses nœuds pour le stockage de fichiers de données et de fichiers journaux. Les disques répertoriés dans cette vue sont ceux qui se trouvent dans le groupe de ressources de cluster associé à l'instance et sont les seuls disques pouvant être utilisés pour le stockage de fichiers de données et de fichiers journaux.  
  
> [!NOTE]  
>  Cette vue remplace [sys.dm_io_cluster_shared_drives &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md) dans une version ultérieure.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation VIEW SERVER STATE pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise sys.dm_io_cluster_valid_path_names pour déterminer les lecteurs partagés d'une instance de serveur cluster :  
  
```  
SELECT * FROM sys.dm_io_cluster_valid_path_names;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sys.dm_os_cluster_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.dm_io_cluster_shared_drives &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-cluster-shared-drives-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

