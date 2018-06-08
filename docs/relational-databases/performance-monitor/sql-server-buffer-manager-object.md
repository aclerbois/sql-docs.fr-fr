---
title: SQL Server, objet Buffer Manager | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
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
- Buffer Manager object
- SQLServer:Buffer Manager
ms.assetid: 9775ebde-111d-476c-9188-b77805f90e98
caps.latest.revision: 36
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: dc709ccddddbd80955cc1f5a27899495517db169
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-buffer-manager-object"></a>SQL Server, objet Buffer Manager
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'objet **Buffer Manager** fournit des compteurs pour analyser comment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise :  
  
-   la mémoire pour stocker des pages de données ;  
  
-   Compteurs pour analyser les E/S physiques lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lit et écrit des pages de base de données.  
  
-   Extension du pool de mémoires tampons pour étendre le cache des tampons à l'aide d'une mémoire non volatile rapide comme les disques SSD.  
  
 L'analyse de la mémoire et des compteurs utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous aide à déterminer :  
  
-   Si des goulots d'étranglement sont créés par de la mémoire physique inadéquate. Si vous ne pouvez pas stocker dans le cache les données fréquemment sollicitées, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit les récupérer sur disque.   
  
-   Si les performances des requêtes peuvent être améliorées en ajoutant de la mémoire ou en mettant plus de mémoire à la disposition du cache des données ou des structures internes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   La fréquence de lecture de données à partir du disque par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Comparées aux autres opérations, comme les accès mémoire, les E/S physiques consomment beaucoup de temps. La diminution des E/S physiques permet d'améliorer les performances des requêtes.  
  
## <a name="buffer-manager-performance-objects"></a>Objets de performance du gestionnaire de tampons  
 Ce tableau décrit les objets de performance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Gestionnaire de tampons** .  
  
|Compteurs du gestionnaire de tampons de SQL Server|Description|  
|----------------------------------------|-----------------|  
|**Pages d’enregistreur en arrière-plan/s**|Nombre de pages vidées pour permettre l’application des paramètres d’intervalle de récupération.| 
|**Taux d'accès au cache des tampons**|Indique le pourcentage des pages retrouvées dans le cache des tampons sans devoir être lues sur le disque. Ce rapport correspond au nombre total de présences dans le cache divisé par le nombre total de recherches dans le cache au cours des quelques derniers milliers d'accès aux pages. Au bout d'un certain temps, ce rapport change peu. Comme la lecture à partir du cache est beaucoup moins coûteuse que la lecture à partir du disque, ce rapport devrait être élevé. En général, il est possible d'augmenter le taux de présence dans le cache en augmentant la quantité de mémoire mise à la disposition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou en utilisant la fonctionnalité d'extension du pool de mémoires tampons.|  
|**Base du taux d’accès au cache des tampons**|À usage interne uniquement|
|**Pages de points de contrôle/s**|Indique le nombre de pages vidées sur le disque par seconde par point de contrôle ou autre opération impliquant le vidage des pages de modifications.|  
|**Pages de base de données**|Indique le nombre de pages dans le pool de mémoires tampons avec le contenu de la base de données.|  
|**Pages affectées à l'extension**|Nombre total de pages de cache occupé dans le fichier d'extension du pool de mémoires tampons.|  
|**Pages hors extension**|Nombre total de pages de cache libre dans le fichier d'extension du pool de mémoires tampons.|  
|**Extension utilisée en pourcentage**|Pourcentage du fichier de pagination de l'extension du pool de mémoires tampons occupé par les pages du gestionnaire de tampons.|  
|**Compteur d'E/S en attente d'extension**|Longueur de la file d'attente d'E/S pour le fichier d'extension du pool de mémoires tampons.|  
|**Évictions de pages d'extension par seconde**|Nombre de pages expulsées du fichier d'extension du pool de mémoires tampons par seconde.|  
|**Lectures de pages d'extension par seconde**|Nombre de pages lues sur le fichier d'extension du pool de mémoires tampons par seconde.|  
|**Heure non référencée de la page d'extension**|Nombre moyen de secondes pendant lesquelles une page est conservée dans l'extension du pool de mémoires tampons sans être référencée.|  
|**Écritures de pages d'extension par seconde**|Nombre de pages écrites sur le fichier d'extension du pool de mémoires tampons par seconde.|  
|**Piles de liste libre/s**|Indique le nombre de requêtes par seconde qui ont dû attendre une page libre.|  
|**Pente intégrale du contrôleur**|Pente que le contrôleur intégral a utilisé la dernière fois pour le pool de mémoires tampons, fois -10 milliards.| 
|**Écritures différées/s**|Indique le nombre de tampons écrits par seconde par l'outil d'écriture différée du gestionnaire de tampons. L’outil d’ *écriture différée* est un processus système dont le rôle consiste à vider les traitements de tampons modifiés ou âgés (tampons qui contiennent des modifications devant être réécrites sur le disque avant que le tampon puisse être réutilisé pour une page différente) et à les rendre disponibles pour les processus utilisateur. L'outil d'écriture différée élimine le besoin de fréquents points de contrôle pour créer des tampons disponibles.|  
|**Espérance de vie d'une page**|Indique le nombre de secondes pendant lesquelles une page est conservée dans le pool de mémoires tampons sans références.|  
|**Recherches de pages/s**|Indique le nombre de requêtes par seconde pour rechercher une page dans le pool de mémoires tampons.|  
|**Lectures de pages/s**|Indique le nombre de lectures de pages de base de données physiques effectuées par seconde. Cette statistique affiche le nombre total de lectures physiques de pages sur toutes les bases de données. Les E/S physiques étant coûteuses en terme de temps machine, vous pouvez les minimiser en utilisant un cache de données plus important, des index intelligents et des requêtes plus efficaces, ou en modifiant la structure de la base de données.|  
|**Écritures de pages/s**|Indique le nombre d'écritures de pages de base de données physiques effectuées par seconde.|  
|**Pages lues par anticipation/s**|Indique le nombre de requêtes de pages lues par seconde par anticipation d'utilisation.|  
|**Durée d’anticipation/s**|Temps (en microsecondes) par anticipation d’émission|
|**Pages cibles**|Nombre idéal de pages dans le pool de mémoires tampons.|

  
## <a name="see-also"></a> Voir aussi  
 [SQL Server:Buffer Node](../../relational-databases/performance-monitor/sql-server-buffer-node.md)   
 [Mémoire du serveur (option de configuration de serveur)](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [SQL Server - Objet Plan Cache](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)   
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [Extension du pool de mémoires tampons](../../database-engine/configure-windows/buffer-pool-extension.md)  
  
  
