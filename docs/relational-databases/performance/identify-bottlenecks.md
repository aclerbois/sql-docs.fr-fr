---
title: Identifier les goulots d’étranglement | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- resource bottlenecks [SQL Server]
- database monitoring [SQL Server], bottlenecks
- performance [SQL Server], bottlenecks
- tuning databases [SQL Server], bottlenecks
- monitoring server performance [SQL Server], bottlenecks
- monitoring performance [SQL Server], bottlenecks
- database performance [SQL Server], bottlenecks
- server performance [SQL Server], bottlenecks
- bottlenecks [SQL Server]
- identifying bottlenecks [SQL Server]
ms.assetid: db079e65-ee80-4105-aec9-f8230d0d6635
caps.latest.revision: 18
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 10c7dcbfb817a234aaed83da9eac4c91fb1650fc
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="identify-bottlenecks"></a>Identifier les goulots d'étranglement
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  L'accès simultané aux ressources partagées provoque des goulots d'étranglement. En général, les goulots d'étranglement sont inévitables et existent dans tous les systèmes logiciels. Toutefois, des demandes excessives sur les ressources partagées engendrent un temps de réponse médiocre, qui impose de les identifier et de les ajuster.  
  
 Les causes des goulots d'étranglement sont notamment les suivantes :  
  
-   ressources insuffisantes nécessitant l'ajout ou la mise à niveau de composants ;  
  
-   répartition inégale des charges de travail entre les ressources de même type (ce qui peut être le cas lorsqu'un disque est monopolisé) ;  
  
-   ressources défectueuses ;  
  
-   ressources configurées incorrectement.  
  
## <a name="analyzing-bottlenecks"></a>Analyse des goulots d'étranglement  
 La durée excessive de divers événements représente un indicateur des goulots d'étranglement susceptibles d'être ajustés.  
  
 Exemple :  
  
-   un composant peut empêcher le chargement d'un autre composant, augmentant ainsi le temps nécessaire pour terminer le chargement ;  
  
-   les demandes de client peuvent prendre plus longtemps en raison d'encombrements sur le réseau.  
  
 Voici cinq domaines clés à surveiller lors du suivi des performances du serveur pour identifier les goulots d'étranglement.  
  
|Domaine possible de goulet d'étranglement|Effets sur le serveur|  
|------------------------------|---------------------------|  
|Utilisation de la mémoire|Une quantité de mémoire insuffisante, allouée ou disponible pour Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dégrade les performances. Les données doivent être lues sur le disque plutôt que directement à partir du cache de données. Les systèmes d'exploitation Microsoft Windows font un usage excessif des fichiers d'échange : ils transfèrent des données en provenance et à destination du disque chaque fois que les pages sont nécessaires.|  
|Utilisation du processeur|Un taux d'utilisation processeur régulièrement élevé peut indiquer que les requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] doivent être ajustées ou que l'unité centrale doit être mise à niveau.|  
|Entrées/Sorties disque (E/S)|[!INCLUDE[tsql](../../includes/tsql-md.md)] Les requêtes peuvent être ajustées pour éviter les E/S superflues, par exemple en utilisant des index.|  
|Connexions utilisateur|Un nombre trop important d'utilisateurs peuvent accéder au serveur en même temps, provoquant une dégradation des performances.|  
|Verrous bloquants|Des applications mal conçues peuvent provoquer des blocages et nuire à la simultanéité, provoquant ainsi des temps de réponse plus longs et des débits de transactions plus faibles.|  
  
## <a name="see-also"></a> Voir aussi  
 [Surveiller l'utilisation de l'UC](../../relational-databases/performance-monitor/monitor-cpu-usage.md)   
 [Surveiller l'utilisation du disque](../../relational-databases/performance-monitor/monitor-disk-usage.md)   
 [Surveiller l'utilisation de la mémoire](../../relational-databases/performance-monitor/monitor-memory-usage.md)   
 [SQL Server, objet General Statistics](../../relational-databases/performance-monitor/sql-server-general-statistics-object.md)   
 [SQL Server, objet Locks](../../relational-databases/performance-monitor/sql-server-locks-object.md)  
  
  
