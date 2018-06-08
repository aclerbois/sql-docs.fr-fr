---
title: SQL Server, objet FileTable | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:FileTable
ms.assetid: 325f5e58-1095-450f-9321-dfacfe6fd55f
caps.latest.revision: 3
author: dagiro
ms.author: v-dagir
manager: craigg
ms.openlocfilehash: d8011be879fd610a8a7f42fb78f6dd6d464f88d4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-filetable-object"></a>SQL Server, objet FileTable
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
L’objet de performance **SQLServer:FileTable** fournit des compteurs de statistiques associés à FileTable et à l’accès non transactionnel.

Le tableau suivant décrit les objets de performance **FileTable** SQL Server.

|**Compteurs FileTable SQL Server**|Description|  
|-------------|-----------------|  
|**Avg time delete FileTable item**|Temps moyen (en millisecondes) nécessaires à la suppression d’un élément FileTable.|
|**Avg time FileTable enumeration**|Temps moyen (en millisecondes) nécessaire pour une demande d’énumération FileTable.|
|**Avg time FileTable handle kill**|Temps moyen (en millisecondes) nécessaire pour supprimer un handle FileTable.|
|**Avg time move FileTable item**|Temps moyen (en millisecondes) nécessaire pour déplacer un élément FileTable.|
|**Avg time per file I/O request**|Temps moyen (en millisecondes) consacré à la gestion d’une demande d’E/S de fichier entrant.|
|**Avg time per file I/O response**|Temps moyen (en millisecondes) consacré à la gestion d’une réponse d’E/S de fichier sortant.|
|**Avg time rename FileTable item**|Temps moyen (en millisecondes) nécessaire pour renommer un élément FileTable.|
|**Avg time to get FileTable item**|Temps moyen (en millisecondes) nécessaire pour récupérer un élément FileTable.|
|**Avg time update FileTable item**|Temps moyen (en millisecondes) nécessaire pour mettre à jour un élément FileTable.|
|**FileTable db operations/sec**|Nombre total d’événements opérationnels de base de données traités par le composant de stockage FileTable par seconde.|
|**FileTable enumeration reqs/sec**|Nombre total de demandes d’énumération FileTable par seconde.|
|**FileTable file I/O requests/sec**|Nombre total de demandes d’E/S de fichier FileTable entrant par seconde.|
|**FileTable file I/O response/sec**|Nombre total de réponses d’E/S de fichier sortant par seconde.|
|**FileTable item delete reqs/sec**|Nombre total de demandes de suppression d’élément FileTable par seconde.|
|**FileTable item get requests/sec**|Nombre total de demandes de récupération d’élément FileTable par seconde.|
|**FileTable item move reqs/sec**|Nombre total de demandes de déplacement d’élément FileTable par seconde.|
|**FileTable item rename reqs/sec**|Nombre total de demandes de changement de nom d’élément FileTable par seconde.|
|**FileTable item update reqs/sec**|Nombre total de demandes de mise à jour d’élément FileTable par seconde.|
|**FileTable kill handle ops/sec**|Nombre total d’opérations de suppression de handle FileTable par seconde.|
|**FileTable table operations/sec**|Nombre total d’événements opérationnels de table traités par le composant de stockage FileTable par seconde.|
|**Temps de suppression de l’élément FileTable (BASE)**|À usage interne uniquement|
|**Time FileTable enumeration BASE**|À usage interne uniquement|
|**Time FileTable handle kill BASE**|À usage interne uniquement|
|**Time move FileTable item BASE**|À usage interne uniquement|
|**Time per file I/O request BASE**|À usage interne uniquement|
|**Time per file I/O response BASE**|À usage interne uniquement|
|**Time rename FileTable item BASE**|À usage interne uniquement|
|**Time to get FileTable item BASE**|À usage interne uniquement|
|**Time update FileTable item BASE**|À usage interne uniquement| 
 
## <a name="see-also"></a> Voir aussi  
[Analyser l'utilisation des ressources (Moniteur système)](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)
