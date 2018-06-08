---
title: SQL Server Agent, objet Alerts | Microsoft Docs
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
- Alerts object
- SQLAgent:Alerts
ms.assetid: e5e37f74-ee88-46d0-ad8f-71fd1b1fa64a
caps.latest.revision: 24
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ea269c45f065e03f5583e62ca98f0084e2fb4772
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-agent-alerts-object"></a>SQL Server Agent, objet Alerts
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'objet de performance **Alerts** de l'Agent SQL Server contient des compteurs de performance qui donnent des informations sur les alertes de l'Agent SQL Server. Le tableau ci-dessous répertorie les compteurs inclus dans cet objet.  
  
 Le tableau suivant contient les compteurs **SQLAgent:Alerts** .  
  
|Nom   |Description|  
|----------|-----------------|  
|**Alertes activées**|Ce compteur donne le nombre total d'alertes que l'Agent SQL Server a activées depuis son dernier redémarrage.|  
|**Alertes activées/minute**|Ce compteur donne le nombre d'alertes que l'Agent SQL Server a activées au cours de la dernière minute.|  
  
> [!NOTE]  
>  Pour utiliser cet objet de l’Agent SQL Server, les utilisateurs doivent être membres du rôle de serveur fixe **sysadmin** .  
  
## <a name="see-also"></a> Voir aussi  
 [Alerts](http://msdn.microsoft.com/library/3f57d0f0-4781-46ec-82cd-b751dc5affef)   
 [Utiliser des objets de performance](http://msdn.microsoft.com/library/830b843a-6b2a-4620-a51b-98358e9fc54b)   
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
