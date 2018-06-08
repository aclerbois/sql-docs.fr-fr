---
title: SQL Server, Broker Activation (objet) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
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
- SQLServer:Broker Activation
- Broker Activation object
ms.assetid: cd9b6880-c924-42c7-b333-09c303317c0b
caps.latest.revision: 20
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 8f9ada76376c27fb2b5e4ed207df9c225d570b4c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-broker-activation-object"></a>SQL Server, Broker Activation (objet)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'objet de performance **SQLServer:BrokerActivation** contient des compteurs de performances qui fournissent des informations sur l'activation des procédures stockées. Le tableau ci-dessous répertorie les compteurs inclus dans cet objet.  
  
|Compteurs de SQL Server Broker Activation|Description|  
|-------------------------------------------|-----------------|  
|**Appels de procédures stockées/s**|Ce compteur indique le nombre total de procédures stockées d'activation appelées par seconde par tous les moniteurs de file d'attente de l'instance.|  
|**Limite de tâches atteinte**|Ce compteur indique le nombre total de fois qu'un démarrage d'une nouvelle tâche par un moniteur de file d'attente a avorté du fait que le nombre maximal de tâches pour cette file est déjà en cours d'exécution.|  
|**Limite de tâches atteinte/s**|Ce compteur indique le nombre de fois par seconde qu'un démarrage d'une nouvelle tâche par un moniteur de file d'attente a avorté du fait que le nombre maximal de tâches pour cette file est déjà en cours d'exécution.|  
|**Tâches abandonnées/s**|Ce compteur signale le nombre de tâches de procédures stockées d'activation s'étant soldées par une erreur ou ayant été abandonnées par un moniteur de file d'attente du fait de la non-réception de messages.|  
|**Tâches en cours d'exécution**|Ce compteur fait état du nombre de procédures stockées d'activation en cours d'exécution.|  
|**Tâches démarrées/s**|Ce compteur indique le nombre de procédures stockées d'activation appelées par seconde par tous les moniteurs de file d'attente de l'instance.|  
  
## <a name="see-also"></a> Voir aussi  
 [sys.dm_broker_activated_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-broker-activated-tasks-transact-sql.md)   
 [sys.dm_broker_queue_monitors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-broker-queue-monitors-transact-sql.md)   
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
