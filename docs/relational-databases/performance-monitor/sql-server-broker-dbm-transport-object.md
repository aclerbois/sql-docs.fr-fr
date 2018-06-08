---
title: SQL Server, objet Broker - DBM Transport | Microsoft Docs
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
- Broker / DBM Transport object
- SQLServer:Broker/DBM Transport
ms.assetid: eddb60b6-20a9-416c-adf3-4bc1687944fa
caps.latest.revision: 34
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: dc79e7ca28f76cb2a2e81995448ea6f645d058a5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-broker---dbm-transport-object"></a>SQL Server, objet Broker - DBM Transport
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L’objet de performance **Broker / DBM Transport** contient des compteurs de performances qui recueillent des informations concernant l’activité réseau relative à Service Broker et à la mise en miroir de bases de données. Le tableau ci-dessous répertorie les compteurs inclus dans cet objet.  
  
|Compteur SQL Server Broker/DBM Transport|Description|  
|------------------------------------------------|-----------------|  
|**Octets actuels pour les E/S reçues**|Ce compteur donne le nombre d'octets à lire par les opérations de réception de transport en cours d'exécution.|  
|**Octets actuels pour les E/S envoyées**|Ce compteur retrace le nombre d'octets composant les fragments des messages faisant l'objet d'un envoi en cours sur le réseau.|  
|**Fragments de message actuels pour les E/S envoyées**|Ce compteur reprend le nombre total de fragments de message faisant l'objet d'un envoi en cours sur le réseau.|  
|**Fragments de message P1 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 1 envoyés sur le réseau par seconde.|  
|**Fragments de message P2 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 2 envoyés sur le réseau par seconde.|  
|**Fragments de message P3 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 3 envoyés sur le réseau par seconde.|  
|**Fragments de message P4 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 4 envoyés sur le réseau par seconde.|  
|**Fragments de message P5 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 5 envoyés sur le réseau par seconde.|  
|**Fragments de message P6 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 6 envoyés sur le réseau par seconde.|  
|**Fragments de message P7 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 7 envoyés sur le réseau par seconde.|  
|**Fragments de message P8 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 8 envoyés sur le réseau par seconde.|  
|**Fragments de message P9 envoyés/s**|Ce compteur indique le nombre de fragments de message de priorité 9 envoyés sur le réseau par seconde.|  
|**Fragments de message P10 envoyés/s**|Ce compteur désigne le nombre de fragments de message de priorité 10 envoyés sur le réseau par seconde.|  
|**Fragments de message reçus/s**|Ce compteur reprend le nombre de fragments de message reçus sur le réseau par seconde.|   
|**Fragments de message envoyés/s**|Ce compteur désigne le nombre de fragments de message de toutes les priorités envoyés sur le réseau par seconde.|  
|**Taille moyenne des fragments de message reçus**|Ce compteur précise la taille moyenne des fragments de message reçus sur le réseau.|  
|**Base de taille moyenne des fragments de message reçus**|À usage interne uniquement| 
|**Taille moyenne des fragments de message envoyés**|Ce compteur précise la taille moyenne des fragments de message envoyés sur le réseau.|  
|**Base de taille moyenne des fragments de message envoyés**|À usage interne uniquement|
|**Nombre de connexions ouvertes**|Ce compteur désigne le nombre de connexions réseau que le Service Broker a d'ouvertes.|  
|**Octets en attente pour les E/S reçues**|Ce compteur indique le nombre d'octets contenus dans les fragments de message reçus du réseau mais n'ayant pas encore été placés dans la file d'attente ou ayant été abandonnés.|  
|**Octets en attente pour les E/S envoyées**|Ce compteur reprend le nombre total d'octets dans les fragments de message prêts à être envoyés sur le réseau.|  
|**Fragments de message en attente pour les E/S reçues**|Ce compteur précise le nombre de fragments de message reçus du réseau mais n'ayant pas encore été placés dans la file d'attente ou ayant été abandonnés.|  
|**Fragments de message en attente pour les E/S envoyées**|Ce compteur retrace le nombre total d'octets dans les fragments de message prêts à être envoyés sur le réseau.|  
|**Octets d'E/S reçus/s**|Ce compteur reprend le nombre total d'octets reçus par seconde sur le réseau par les points de terminaison Service Broker et de mise en miroir de bases de données.|  
|**Total des octets d'E/S reçus**|Ce compteur indique le nombre total d'octets reçus sur le réseau par les points de terminaison Service Broker et de mise en miroir de bases de données.|  
|**Longueur moyenne des E/S reçues**|Ce compteur désigne le nombre moyen d'octets pour une opération de réception de transport.|  
|**Base de longueur moyenne des E/S reçues**|À usage interne uniquement|
|**E/S reçues/s**|Ce compteur indique le nombre d'opérations d'E/S du transport en réception par seconde que la couche Service Broker/DBM transport a effectuées. Il se peut qu'une opération de réception du transport contienne plusieurs fragments de message.|  
|**Octets de copies de tampon par les E/S reçues/s**|Vitesse à laquelle les opérations d’E/S de transport reçues ont dû déplacer des fragments de tampon en mémoire.|
|**Nombre de copies de tampon par les E/S reçues**|Nombre de fois où les opérations d’E/S de transport reçues ont dû déplacer des fragments de tampon en mémoire.| 
|**Octets d'E/S envoyés/s**|Ce compteur reprend le nombre total d'octets envoyés par seconde sur le réseau par les points de terminaison Service Broker et de mise en miroir de bases de données.|   
|**Total des octets d'E/S envoyés**|Ce compteur indique le nombre total d'octets transmis sur le réseau par les points de terminaison Service Broker et de mise en miroir de bases de données.| 
|**Longueur moyenne des E/S envoyées**|Ce compteur précise la taille moyenne en octets des opérations d'envoi de transport. Il se peut qu'une opération d'envoi du transport contienne plusieurs fragments de message.|  
|**Base de longueur moyenne des E/S envoyées**|À usage interne uniquement|
|**E/S envoyées/s**|Ce compteur indique le nombre d'opérations d'E/S du transport en envoi par seconde qui ont été effectuées. Il se peut qu'une opération d'envoi du transport contienne plusieurs fragments de message.|  
  
## <a name="see-also"></a> Voir aussi  
 [sys.dm_broker_forwarded_messages &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-broker-forwarded-messages-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
