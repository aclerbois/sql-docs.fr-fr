---
title: Objet SQLServer:Cursor Manager by Type | Microsoft Docs
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
- Cursor Manager by Type object
- SQLServer:Cursor Manager by Type
ms.assetid: d67fbd8a-7554-4a16-96f1-d9ee857a95e3
caps.latest.revision: 15
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: bcf783c6ff68a2626b823d5dc5d077124f06b2bc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-cursor-manager-by-type-object"></a>Objet SQLServer:Cursor Manager by Type
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'objet **SQLServer:Cursor Manager by Type** fournit des compteurs pour surveiller les curseurs, groupés par type.  
  
 Le tableau ci-dessous décrit les compteurs **Cursor Manager by Type** de SQL Server.  
  
|Compteurs Cursor Manager by Type|Description|  
|-------------------------------------|-----------------|  
|**Curseurs actifs**|Nombre de curseurs actifs.|  
|**Taux d'accès au cache**|Rapport entre les présences dans le cache et les recherches.|  
|**Base du taux d’accès au cache**|À usage interne uniquement| 
|**Nombres de curseurs mis en cache**|Nombre de curseurs d'un type donné dans le cache.|  
|**Nombre d'utilisations du cache de curseurs/s**|Nombre d'utilisations de chaque type de curseur en cache.|  
|**Mémoire utilisée par les curseurs**|Quantité de mémoire consommée par les curseurs en kilo-octets (Ko).|  
|**Requêtes de curseurs/s**|Nombre de requêtes de curseurs SQL reçues par le serveur.|  
|**Tables de travail utilisées par les curseurs**|Nombre de tables de travail utilisées par les curseurs.|  
|**Nombre de plans de curseurs actifs**|Nombre de plans de curseurs.|  
  
 Chaque compteur de l'objet contient les instances suivantes :  
  
|Instance Cursor Manager|Description|  
|-----------------------------|-----------------|  
|**_Total**|Informations pour tous les curseurs.|  
|**API Cursor**|Informations sur le curseur API uniquement.|  
|**TSQL Global Cursor**|Informations sur le curseur global [!INCLUDE[tsql](../../includes/tsql-md.md)] uniquement.|  
|**TSQL Local Cursor**|Informations sur le curseur local [!INCLUDE[tsql](../../includes/tsql-md.md)] uniquement.|  
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
