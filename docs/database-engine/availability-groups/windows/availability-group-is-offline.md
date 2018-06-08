---
title: Le groupe de disponibilité est hors connexion | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp2online.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 093c5208-bf7a-49f4-a546-72b48197cadf
caps.latest.revision: 14
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 7dc3b221631d07291a4965f5bb37dca54f406703
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34768595"
---
# <a name="availability-group-is-offline"></a>Le groupe de disponibilité est hors connexion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="introduction"></a>Introduction  
  
|||  
|-|-|  
|**Nom de la stratégie**|État en ligne du groupe de disponibilité|  
|**Problème**|Le groupe de disponibilité est hors connexion.|  
|**Catégorie**|**Critique**|  
|**Facette**|Groupe de disponibilité|  
  
## <a name="description"></a>Description  
 Cette stratégie vérifie l'état (en ligne ou hors connexion) du groupe de disponibilité. La stratégie se trouve dans un état non sain et une alerte est générée lorsque la ressource de cluster du groupe de disponibilité est hors connexion ou que le groupe de disponibilité n'a pas de réplica principal.  
  
 L'état de la stratégie est sain lorsque la ressource de cluster du groupe de disponibilité est en ligne et que le groupe de disponibilité a un réplica principal.  
  
> [!NOTE]  
>  Pour cette version de [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], les informations sur les causes et les solutions possibles sont situées sous [Le groupe de disponibilité est hors connexion](http://go.microsoft.com/fwlink/p/?LinkId=220850) sur le Wiki TechNet.  
  
## <a name="possible-causes"></a>Causes possibles  
 Ce problème peut être dû à un échec de l'instance de serveur qui héberge le réplica principal ou à la mise hors connexion de la ressource de groupe de disponibilité du cluster de basculement Windows Server (WSFC). Voici les causes possibles de mise hors connexion du groupe de disponibilité :  
  
-   Le groupe de disponibilité n'est pas configuré avec le mode de basculement automatique. Le réplica principal va bientôt être indisponible et le rôle de tous les réplicas du groupe de disponibilité est RESOLVING.  
  
    -   Le service de l'instance du réplica principal est arrêté ou ne répond pas.  
  
    -   Le groupe de disponibilité rencontre un problème de connectivité avec le cluster.  
  
-   Le groupe de disponibilité est configuré avec le mode de basculement automatique et ne se termine pas correctement.  
  
    -   Pendant le basculement automatique, le contrôle de disponibilité du réplica principal sur le réplica cible échoue, et aucun réplica n'est disponible pour devenir le nouveau réplica principal.  
  
-   La ressource du groupe de disponibilité dans le cluster est en train de passer hors connexion.  
  
    -   Une ressource de cluster dépendante rencontre un problème critique et est en train de passer hors connexion. La ressource du groupe de disponibilité reste également hors connexion jusqu'à ce que la ressource dépendante soit en ligne.  
  
    -   Un problème critique au niveau du cluster désactive la ressource du groupe de disponibilité.  
  
-   Un basculement automatique, manuel ou forcé est en cours pour le groupe de disponibilité.  
  
## <a name="possible-solutions"></a>Solutions possibles  
 Voici les solutions possibles à ce problème :  
  
-   Si l'instance SQL Server du réplica principal est arrêtée, redémarrez le serveur, puis vérifiez que le groupe de disponibilité repasse à un état sain.  
  
-   Si le basculement automatique a échoué, vérifiez que les bases de données du réplica sont synchronisées avec le réplica principal précédemment connu, puis basculez vers le réplica principal. Si les bases de données ne sont pas synchronisées, sélectionnez un réplica avec une perte minimale de données, puis repassez en mode de basculement.  
  
-   Si la ressource du cluster est hors connexion alors que les instances SQL Server semblent être saines, utilisez le Gestionnaire du cluster de basculement pour vérifier l'intégrité de cluster ou détecter d'autres problèmes de cluster sur le serveur. Vous pouvez également utiliser le Gestionnaire du cluster de basculement pour tenter de remettre en ligne la ressource du groupe de disponibilité.  
  
-   Si un basculement est en cours, attendez qu'il soit achevé.  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
