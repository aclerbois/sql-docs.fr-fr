---
title: Sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL) | Documents Microsoft
description: Retourne l’état actuel de sémaphores de ressource utilisé pour limiter l’optimisation des requêtes simultanées
ms.custom: ''
ms.date: 04/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_query_optimizer_memory_gateways_TSQL
- dm_exec_query_optimizer_memory_gateways
- sys.dm_exec_query_optimizer_memory_gateways_TSQL
- sys.dm_exec_query_optimizer_memory_gateways
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_memory_gateways dynamic management view
author: josack
ms.author: josack
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b9d36c4a67fab2f9f7c867a3ba6b7e7d01fc5d29
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecqueryoptimizermemorygateways-transact-sql"></a>sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

Retourne l’état actuel des sémaphores de ressource utilisé pour limiter l’optimisation des requêtes simultanées.

|Colonne|Type| Description|  
|----------|---------------|-----------------|  
|**pool_id**|**int**|ID du pool de ressources sous le gouverneur de ressources|  
|**nom**|**sysname**|Compiler le nom de la porte (petite la passerelle, moyenne, grande passerelle)|
|**max_count**|**int**|Le nombre maximal configuré de compile simultanées|
|**active_count**|**int**|Le nombre actuellement actif de compilations de cette porte|
|**waiter_count**|**int**|Le nombre d’objets waiter de cette porte|
|**threshold_factor**|**bigint**|Facteur de seuil qui définit la partie de la mémoire maximale utilisée par l’optimisation des requêtes.  Pour la passerelle petite, threshold_factor indique l’utilisation de mémoire optimiseur maximale en octets pour une requête avant qu’il soit nécessaire pour obtenir un accès dans la passerelle small.  Pour la passerelle moyenne et grande threshold_factor montre la partie de la mémoire totale du serveur disponible pour cette porte. Il est utilisé en tant que diviseur lors du calcul du seuil d’utilisation de mémoire pour la porte.|
|**Seuil**|**bigint**|Mémoire de seuil suivante en octets.  La requête est nécessaire pour accéder à cette passerelle si sa consommation de mémoire atteint ce seuil.  « -1 » si la requête n’est pas nécessaire pour un accès à cette passerelle.|
|**is_active**|**bit**|Indique si la requête est requis pour passer de la porte en cours ou non.|


## <a name="permissions"></a>Autorisations  
SQL Server requiert l’autorisation VIEW SERVER STATE sur le serveur.

Base de données SQL Azure nécessite l’autorisation VIEW DATABASE STATE dans la base de données.


## <a name="remarks"></a>Notes  
SQL Server utilise une approche de passerelle à plusieurs niveaux pour limiter le nombre de compilations simultanées autorisées.  Trois passerelles sont utilisées, y compris de petite, moyenne et big. Passerelles prévenir l’insuffisance des ressources de mémoire globale par les consommateurs de mémoire exigeant plus de compilation.

Attend sur un résultat de la passerelle dans la compilation différée. En plus des retards dans la compilation, des demandes limitées aura une RESOURCE_SEMAPHORE_QUERY_COMPILE associé à attendre l’accumulation de type. Le type d’attente RESOURCE_SEMAPHORE_QUERY_COMPILE peut indiquer que requêtes utilisent une grande quantité de mémoire pour la compilation et que la mémoire a été épuisée, ou vous pouvez également est suffisamment de mémoire disponible de manière générale, toutefois, les unités disponibles dans une passerelle spécifique ont été épuisées. La sortie de **sys.dm_exec_query_optimizer_memory_gateways** peut être utilisé pour dépanner des scénarios où la mémoire était insuffisante pour compiler un plan d’exécution de requête.  

## <a name="examples"></a>Exemples  

### <a name="a-viewing-statistics-on-resource-semaphores"></a>A. Affichage des statistiques sur les sémaphores de ressources  
Quelles sont les statistiques de passerelle optimiseur mémoire actuelle pour cette instance de SQL Server ?

```  
SELECT [pool_id], [name], [max_count], [active_count],
       [waiter_count], [threshold_factor], [threshold],
       [is_active]
FROM sys.dm_exec_query_optimizer_memory_gateways;   

```  

## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](./execution-related-dynamic-management-views-and-functions-transact-sql.md)  
[L’utilisation de la commande DBCC MEMORYSTATUS pour surveiller l’utilisation de mémoire sur SQL Server 2005](https://support.microsoft.com/help/907877/how-to-use-the-dbcc-memorystatus-command-to-monitor-memory-usage-on-sql-server-2005)
[attend de compilation de requête de grande taille sur RESOURCE_SEMAPHORE_QUERY_COMPILE dans SQL Server 2014](https://support.microsoft.com/help/3024815/large-query-compilation-waits-on-resource-semaphore-query-compile-in-sql-server-2014)
