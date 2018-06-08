---
title: Objet SQL Server:Broker Statistics | Microsoft Docs
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
- SQLServer:Broker Statistics
- Broker Statistics object
ms.assetid: e9e36f01-93f6-4e6e-90c6-c7f3fd121737
caps.latest.revision: 32
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: a3d221c9f2d3b7e9e1c91d9b11d21afc537856c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-broker-statistics-object"></a>Objet SQL Server:Broker Statistics
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'objet de performance SQLServer : Statistiques de Service Broker contient des compteurs de performance qui rendent compte d'informations générales sur [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Le tableau ci-dessous répertorie les compteurs que cet objet contient.  
  
|Compteurs statistiques de Service Broker de SQL Server|Description|  
|-------------------------------------------|-----------------|  
|**Total des erreurs d'activation**|Nombre de fois où une procédure stockée d'activation de [!INCLUDE[ssSB](../../includes/sssb-md.md)] s'est terminée avec une erreur.|  
|**Restaurations de transactions Service Broker**|Nombre de transactions restaurées qui contenaient des instructions de type DML relatives à [!INCLUDE[ssSB](../../includes/sssb-md.md)], comme SEND et RECEIVE.|  
|**Total des messages endommagés**|Nombre de messages endommagés reçus dans l'instance.|  
|**Messages TransmissionQ supprimés de la file d'attente/s**|Nombre de messages supprimés de la file d'attente des transmissions du [!INCLUDE[ssSB](../../includes/sssb-md.md)] par seconde.|  
|**Nombre d'événements d'horloge de dialogue**|Nombre d'événements d'horloge actifs dans la couche protocole du dialogue. Ce nombre correspond au nombre de dialogues actifs.|  
|**Total des messages supprimés**|Nombre de messages reçus dans l'instance mais n'ayant pu être remis.|  
|**Total des messages locaux empilés**|Nombre de messages placés dans les files d'attente de l'instance, en ne prenant en compte que les messages non arrivés sur le réseau.|  
|**Messages locaux empilés/s**|Nombre de messages placés dans les files d'attente de l'instance par seconde, en ne prenant en compte que les messages non arrivés sur le réseau.|  
|**Total des messages empilés**|Nombre total de messages ayant été mis dans les files d'attente de l'instance.|  
|**Messages empilés/s**|Nombre de messages ayant été mis dans les files d'attente de l'instance par seconde. Cela inclut les messages de tous les niveaux de priorité.|  
|**Messages P1 empilés/s**|Nombre de messages de priorité 1 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P2 empilés/s**|Nombre de messages de priorité 2 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P3 empilés/s**|Nombre de messages de priorité 3 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P4 empilés/s**|Nombre de messages de priorité 4 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P5 empilés/s**|Nombre de messages de priorité 5 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P6 empilés/s**|Nombre de messages de priorité 6 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P7 empilés/s**|Nombre de messages de priorité 7 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P8 empilés/s**|Nombre de messages de priorité 8 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P9 empilés/s**|Nombre de messages de priorité 9 ayant été mis dans les files d’attente de l’instance par seconde.|  
|**Messages P10 empilés/s**|Nombre de messages de priorité 10 ayant été mis dans les files d'attente de l'instance par seconde.|  
|**Messages TransmissionQ empilés/s**|Nombre de messages placés dans la file d'attente des transmissions du [!INCLUDE[ssSB](../../includes/sssb-md.md)] par seconde.|  
|**Total des fragments de message du transport empilés**|Nombre de fragments de messages placés dans les files d'attente de l'instance, en ne prenant en compte que les messages arrivés sur le réseau.|  
|**Fragments de message du transport empilés/s**|Nombre de fragments de messages ayant été mis dans les files d'attente de l'instance par seconde.|  
|**Total des messages du transport empilés**|Nombre de messages placés dans les files d'attente de l'instance, en ne prenant en compte que les messages arrivés sur le réseau.|  
|**Messages du transport empilé/s**|Nombre de messages placés dans les files d'attente de l'instance par seconde, en ne prenant en compte que les messages arrivés sur le réseau.|  
|**Total des messages transférés**|Nombre total de messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] transférés par l'ordinateur en question.|  
|**Messages transférés/s**|Nombre de messages transférés par seconde par l'ordinateur en question.|  
|**Total des octets de messages transférés**|Taille totale des messages transférés par l'ordinateur en question, exprimée en octets.|  
|**Octets de messages transférés/s**|Taille exprimée en octets des messages transférés par seconde par l'ordinateur en question.|  
|**Total des messages transférés et ignorés**|Nombre de messages reçus par cet ordinateur dans le but de leur transfert mais qui n'ont pas été transférés correctement.|  
|**Messages transférés ignorés/s**|Nombre de messages reçus par seconde par cet ordinateur dans le but de leur transfert mais qui n'ont pas été transférés correctement.|  
|**Octets de messages transférés en attente**|Taille totale des messages actuellement conservés et destinés à être transférés.|  
|**Nombre de messages transférés en attente**|Nombre total des messages actuellement conservés et destinés à être transférés.|  
|**Total SQL RECEIVE**|Nombre total d'instructions RECEIVE [!INCLUDE[tsql](../../includes/tsql-md.md)] traitées.|  
|**SQL RECEIVE/s**|Nombre d'instructions RECEIVE [!INCLUDE[tsql](../../includes/tsql-md.md)] traitées par seconde.|  
|**Total SQL SEND**|Nombre total d'instructions SEND [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutées.|  
|**SQL SEND/s**|Nombre d'instructions SEND [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutées par seconde.|  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
