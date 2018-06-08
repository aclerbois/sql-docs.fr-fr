---
title: Sys.elastic_pool_resource_stats (de base de données SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 04/06/2018
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-catalog-views
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- Azure SQL Database
f1_keywords:
- sys.elastic_pool_resource_stats catalog view
helpviewer_keywords:
- sys.elastic_pool_resource_stats_TSQL
- sys.elastic_pool_resource_stats
- elastic_pool_resource_stats_TSQL
- elastic_pool_resource_stats
ms.assetid: f242c1bd-3cc8-4c8b-8aaf-c79b6a8a0329
caps.latest.revision: 18
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: a04b60738a48ddbe09db3eb8d7032d2f08b4ba9c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syselasticpoolresourcestats-azure-sql-database"></a>Sys.elastic_pool_resource_stats (de base de données SQL Azure)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Retourne les statistiques d’utilisation de ressources pour tous les pools de base de données élastique dans un serveur logique. Pour chaque pool élastique de base de données, il existe une ligne pour chaque fenêtre (quatre lignes par minute) de déclaration de 15 secondes. Cela inclut l’utilisation du processeur, e/s, journal, la consommation du stockage et demande/sessions simultanées par toutes les bases de données dans le pool. Ces données sont conservées pendant 14 jours. 
  
||  
|-|  
|**S’applique aux**: [!INCLUDE[ssSDS](../../includes/sssds-md.md)] V12.|  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**start_time**|**datetime2**|Heure UTC indiquant le début de l’intervalle de rapports de 15 secondes.|  
|**end_time**|**datetime2**|Heure UTC indiquant la fin de l’intervalle de rapports de 15 secondes.|  
|**elastic_pool_name**|**nvarchar(128)**|Nom du pool élastique de base de données.|  
|**avg_cpu_percent**|**Decimal(5,2)**|Utilisation de calcul moyenne en pourcentage de la limite du pool.|  
|**avg_data_io_percent**|**Decimal(5,2)**|Utilisation d’e/s moyenne en pourcentage de la limite du pool.|  
|**avg_log_write_percent**|**Decimal(5,2)**|L’utilisation des ressources d’écriture moyenne en pourcentage de la limite du pool.|  
|**avg_storage_percent**|**Decimal(5,2)**|Moyenne de l’utilisation du stockage en pourcentage de la limite de stockage du pool.|  
|**max_worker_percent**|**Decimal(5,2)**|Traitements simultanés maximales (demandes) en pourcentage de la limite du pool.|  
|**max_session_percent**|**Decimal(5,2)**|Nombre maximal de sessions simultané en pourcentage de la limite du pool.|  
|**elastic_pool_dtu_limit**|**int**|Max pool élastique DTU paramètre actuel de ce pool élastique pendant cet intervalle.|  
|**elastic_pool_storage_limit_mb**|**bigint**|Limite de stockage maximale du pool élastique actuelle définition pour ce pool élastique en mégaoctets pendant cet intervalle.|  
  
## <a name="remarks"></a>Notes  
 Cette vue existe dans la base de données master du serveur logique. Vous devez être connecté à la base de données master pour la requête **sys.elastic_pool_resource_stats**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **dbmanager** rôle.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne les données d’utilisation de ressources classées par l’heure la plus récente pour tous les pools de base de données élastique dans le serveur logique actif.  
  
```  
SELECT * FROM sys.elastic_pool_resource_stats   
ORDER BY end_time DESC;  
```  
  
 L’exemple suivant calcule le pourcentage de consommation DTU moyenne pour un pool donné.  
  
```  
SELECT start_time, end_time,      
  (SELECT Max(v)      
FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS value(v)) AS [avg_DTU_percent]    
FROM sys.elastic_pool_resource_stats   
WHERE elastic_pool_name = '<your pool name>'   
ORDER BY end_time DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Réduire croissance des bases de données élastiques](https://azure.microsoft.com/documentation/articles/sql-database-elastic-pool/)   
 [Créer et gérer un pool élastique de base de données SQL (version préliminaire)](https://azure.microsoft.com/documentation/articles/sql-database-elastic-pool-portal/)   
 [Sys.resource_stats &#40;base de données SQL Azure&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [Sys.dm_db_resource_stats &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md)  
  
  
