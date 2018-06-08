---
title: Sys.resource_governor_resource_pools (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_resource_pools
- resource_governor_resource_pools
- sys.resource_governor_resource_pools_TSQL
- resource_governor_resource_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_resource_pools catalog view
ms.assetid: 56793e9c-aa90-452e-88c6-d9b799239888
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: bb68e1efdd1261c5bc6204a96b398c55cc80cc43
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysresourcegovernorresourcepools-transact-sql"></a>sys.resource_governor_resource_pools (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne la configuration de pool de ressources stockée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Chaque ligne de la vue détermine la configuration d'un pool.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|ID unique du pool de ressources. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom du pool de ressources. N'accepte pas la valeur NULL.|  
|min_cpu_percent|**int**|Bande passante processeur moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|  
|max_cpu_percent|**int**|Bande passante processeur moyenne maximale pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|  
|min_memory_percent|**int**|Quantité de mémoire garantie pour toutes les demandes dans le pool de ressources. Cette valeur n'est pas partagée avec d'autres pools de ressources. N'accepte pas la valeur NULL.|  
|max_memory_percent|**int**|Pourcentage de la mémoire totale du serveur qui peut être utilisé par les demandes dans ce pool de ressources. N'accepte pas la valeur NULL. La valeur maximale effective dépend des valeurs minimales du pool. Par exemple, max_memory_percent peut avoir la valeur 100, alors que sa valeur maximale effective est inférieure.|  
|cap_cpu_percent|**int**|**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Limite maximale d'utilisation fixe sur la bande passante de l'UC que toutes les demandes dans le pool de ressources recevront. Limite la bande passante maximale de l'UC au niveau spécifié. La plage autorisée pour la valeur est comprise entre 1 et 100.|  
|min_iops_per_volume|**int**|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Opérations d'E/S minimales par seconde (IOPS) par paramètre de volume de ce pool. 0 = aucune réservation. Ne peut pas avoir la valeur null.|  
|max_iops_per_volume|**int**|**S'applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Opérations d'E/S maximales par seconde (IOPS) par paramètre de volume de ce pool. 0 = illimitées. Ne peut pas avoir la valeur null.|  
  
## <a name="remarks"></a>Notes  
 L'affichage catalogue affiche les métadonnées stockées. Pour consulter la configuration en mémoire, utilisez la vue de gestion dynamique correspondante, [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW ANY DEFINITION pour consulter le contenu, requiert l'autorisation CONTROL SERVER pour modifier le contenu.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue du gouverneur de ressources &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)  
  
  
