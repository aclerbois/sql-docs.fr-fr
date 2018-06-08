---
title: Le groupe de disponibilité n’est pas prêt pour le basculement automatique | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp3autofailover.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 28261014-342c-442a-bd89-6d04b8d4e8b7
caps.latest.revision: 12
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0c99a7d813b190823c28b50daa4b50f1d6b2c6b1
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771325"
---
# <a name="availability-group-is-not-ready-for-automatic-failover"></a>Le groupe de disponibilité n'est pas prêt pour le basculement automatique
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|Disponibilité du groupe de disponibilité pour le basculement automatique|  
|**Problème**|Le groupe de disponibilité n'est pas prêt pour le basculement automatique.|  
|**Catégorie**|**Critique**|  
|**Facette**|Groupe de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie vérifie que le groupe de disponibilité a au moins un réplica secondaire prêt pour le basculement. La stratégie se trouve dans un état non sain et une alerte est déclenchée lorsque le mode de basculement du réplica principal est automatique, mais qu'aucun des réplicas secondaires dans le groupe de disponibilité n'est prêt pour le basculement.  
  
 La stratégie se trouve dans un état sain lorsqu'au moins un réplica secondaire est prêt pour le basculement automatique.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], les informations sur les causes et les solutions possibles figurent sous [Le groupe de disponibilité n’est pas prêt pour le basculement automatique](http://go.microsoft.com/fwlink/p/?LinkId=220851) sur le Wiki TechNet.  
  
## <a name="possible-causes"></a>Causes possibles  
 Le groupe de disponibilité n'est pas prêt pour le basculement automatique. Le réplica principal est configuré pour le basculement automatique ; toutefois, le réplica secondaire n'est pas prêt pour le basculement automatique. Le réplica secondaire configuré pour le basculement automatique est peut-être indisponible ou son état de synchronisation des données n'est pas SYNCHRONIZED pour le moment.  
  
## <a name="possible-solutions"></a>Solutions possibles  
 Voici les solutions possibles à ce problème :  
  
-   Vérifiez qu'au moins un réplica secondaire est configuré pour le basculement automatique. Si aucun réplica secondaire n'est configuré pour le basculement automatique, mettez à jour la configuration d'un réplica secondaire de sorte qu'il soit la cible de basculement automatique avec validation synchrone.  
  
-   Utilisez la stratégie pour vérifier que les données sont dans un état de synchronisation et que la cible de basculement automatique est à l'état SYNCHRONIZED, puis pour résoudre le problème qui affecte le réplica de disponibilité.  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
