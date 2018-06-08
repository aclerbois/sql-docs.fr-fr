---
title: Le réplica de disponibilité n’a pas un rôle sain | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.arp1rolehealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: ebb2c9f4-2097-4688-b4fb-8f0571047317
caps.latest.revision: 13
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: efe3fa11a9f009e8251f9c559df5855717754c23
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771565"
---
# <a name="availability-replica-does-not-have-a-healthy-role"></a>Le réplica de disponibilité n'a pas un rôle sain
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|État du rôle du réplica de disponibilité|  
|**Problème**|Le réplica de disponibilité n'a pas un rôle sain.|  
|**Catégorie**|**Critique**|  
|**Facette**|Réplica de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie vérifie l'état du rôle du réplica de disponibilité. La stratégie se trouve dans un état non sain lorsque le rôle du réplica de disponibilité n'est ni principal ni secondaire. Autrement, l'état de la stratégie est sain.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], des informations sur les causes et les solutions possibles se trouvent dans [Le réplica de disponibilité n’a pas un rôle sain](http://go.microsoft.com/fwlink/p/?LinkId=220856) sur Wiki TechNet.  
  
## <a name="possible-causes"></a>Causes possibles  
 Le rôle de ce réplica de disponibilité n'est pas sain. Le réplica n'a pas le rôle principal ou secondaire.  
  
## <a name="possible-solution-informationstilltocome"></a>Solution possible : Information_à_venir  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
