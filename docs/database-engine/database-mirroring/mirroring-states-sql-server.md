---
title: États de la mise en miroir (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.component: database-mirroring
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- states [SQL Server], database mirroring
- PENDING_FAILOVER state
- mirroring states [SQL Server]
- DISCONNECTED state
- SYNCHRONIZING state
- SYNCHRONIZED state
- SUSPENDED state
- database mirroring [SQL Server], states
ms.assetid: 90062917-74f9-471b-b49e-bc153ae1a468
caps.latest.revision: 39
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 7be58689ed97e72a9e8f887cb7ac303cd2b20513
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mirroring-states-sql-server"></a>États de la mise en miroir (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Pendant une session de mise en miroir de bases de données, la base de données mise en miroir est toujours dans un état spécifique (l’ *état de la mise en miroir*). L'état de la base de données reflète l'état de communication, le flux de données et les différences dans des données entre les partenaires. Une session de mise en miroir de bases de données adopte le même état que la base de données principale.  
  
 Pendant tout le déroulement d'une session de mise en miroir de bases de données, les instances de serveur s'analysent mutuellement. Les partenaires utilisent l'état de la mise en miroir pour surveiller la base de données. À l'exception de l'état PENDING_FAILOVER, les bases de données principale et miroir sont toujours dans le même état. Si un témoin est défini pour la session, chacun des partenaires surveille le témoin en utilisant son état de connexion (CONNECTED ou DISCONNECTED).  
  
 Les états de la mise en miroir possibles sont les suivants :  
  
|état de la mise en miroir|Description|  
|---------------------|-----------------|  
|SYNCHRONIZING|Le contenu de la base de données miroir accuse un retard par rapport au contenu de la base de données principale. Le serveur principal envoie des enregistrements de journal au serveur miroir, qui applique les modifications à la base de données miroir pour la restaurer par progression.<br /><br /> Lors du démarrage d'une session de mise en miroir de base de données, la base de données est dans l'état SYNCHRONIZING. Le serveur principal sert la base de données et le serveur miroir essaie de rattraper son retard.|  
|SYNCHRONIZED|Lorsque le serveur miroir a rattrapé suffisamment de retard par rapport au serveur principal, l'état de la mise en miroir devient SYNCHRONIZED. La base de données reste dans cet état aussi longtemps que le serveur principal continue d'envoyer des modifications au serveur miroir et que le serveur miroir continue d'appliquer les modifications à la base de données miroir.<br /><br /> Si la sécurité des transactions a la valeur FULL, le basculement automatique et le basculement manuel sont tous les deux pris en charge dans l'état SYNCHRONIZED ; aucune perte de données ne se produit après un basculement.<br /><br /> Si la sécurité des transactions est désactivée, une perte de données est toujours possible, même dans l'état SYNCHRONIZED.|  
|SUSPENDED|La copie miroir de la base de données n'est pas disponible. La base de données principale fonctionne sans envoyer de journaux au serveur miroir, condition qualifiée d’ *exécution exposée*. Cet état est observé après un basculement.<br /><br /> Une session peut également prendre l'état SUSPENDED à la suite d'erreurs de répétition ou si l'administrateur suspend la session.<br /><br /> L'état SUSPENDED est un état permanent qui persiste lors des arrêts et des démarrages des partenaires.|  
|PENDING_FAILOVER|Cet état est observé uniquement sur le serveur principal lorsqu'un basculement a commencé, mais que le serveur n'est pas passé par le rôle de miroir.<br /><br /> Une fois le basculement déclenché, la base de données principale passe à l'état PENDING_FAILOVER, met rapidement un terme à toutes les connexions utilisateur et adopte le rôle de miroir peu de temps après.|  
|DISCONNECTED|Le partenaire a perdu la communication avec l'autre partenaire.|  
  
## <a name="see-also"></a> Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
