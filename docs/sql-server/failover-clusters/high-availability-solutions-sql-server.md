---
title: Solutions haute disponibilité (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- high availability [SQL Server], solutions
- Database Engine [SQL Server], availability
- database availability [SQL Server]
- availability [SQL Server]
- server availability [SQL Server]
ms.assetid: b2eda634-0f8e-4703-801b-7ba895544ff5
caps.latest.revision: 84
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ef76ce9bfa54ad7e3c87b492644a74e8e7fa9fb3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="high-availability-solutions-sql-server"></a>Solutions haute disponibilité (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique présente plusieurs solutions haute disponibilité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui améliorent la disponibilité des serveurs ou des bases de données. Une solution à haute disponibilité masque l'impact d'une défaillance matérielle ou logicielle et gère la disponibilité des applications pour réduire au maximum le temps mort que perçoit l'utilisateur.    
    
   
>  **Remarque** vous voulez savoir quelles éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prennent en charge une solution haute disponibilité donnée ? Consultez la section « Haute disponibilité (Always On) » de l’article [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).    
     
    
##  <a name="TermsAndDefinitions"></a> Présentation des solutions haute disponibilité SQL Server    
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit plusieurs options permettant de garantir un haut niveau de disponibilité pour un serveur ou une base de données. Les options de haute disponibilité sont les suivantes :    
    
*  Instances de cluster de basculement Always On    
 Dans le cadre de l’offre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Always On, les instances de cluster de basculement Always On exploitent la fonctionnalité de clustering de basculement Windows Server (WSFC) pour fournir une haute disponibilité locale grâce à la redondance au niveau de l’instance de serveur, une *instance de cluster de basculement* (FCI). Une instance FCI est une instance unique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installée sur plusieurs nœuds WSFC (clustering de basculement Windows Server) et, éventuellement, sur plusieurs sous-réseaux. Sur le réseau, une instance de cluster de basculement FCI apparaît en tant qu'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécutant sur un ordinateur unique, mais elle permet le basculement d'un nœud WSFC vers un autre en cas d'indisponibilité du nœud actuel.    
    
 Pour plus d'informations, consultez [Instances de cluster de basculement Always On &#40;SQL Server&#41;](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).    
    
*  [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]    
 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] est une solution haute disponibilité et de récupération d’urgence au niveau de l’entreprise introduite dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] pour vous permettre d’optimiser la disponibilité d’une ou de plusieurs bases de données utilisateur. [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] exige que les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] résident sur des nœuds de clustering de basculement Windows Server (WSFC). Pour plus d’informations, consultez [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).    
    
  
>  **Remarque** Une instance FCI peut tirer parti de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] pour permettre une récupération d'urgence à distance au niveau de la base de données. Pour plus d’informations, consultez [Clustering de basculement et groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).    
    
*  Mise en miroir de bases de données. **Remarque** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Nous vous recommandons d'utiliser [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] à la place.     
La mise en miroir de base de données est une solution qui permet d'optimiser la disponibilité d'une base de données en utilisant presque instantanément le basculement. La mise en miroir de base de données peut être utilisée pour gérer une base de données de secours ou une *base de données miroir*pour une base de données de production correspondante désignée sous le nom de *base de données principale*. Pour plus d’informations, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).    
    
*  Copie des journaux de transaction    
 Comme [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] et la mise en miroir de bases de données, la copie des journaux de transaction fonctionne au niveau de la base de données. Vous pouvez utiliser la copie des journaux de transactions pour gérer une ou plusieurs bases de données de secours semi-automatique (appelées *bases de données secondaires*) pour une base de données de production unique correspondante appelée *base de données primaire*. Pour plus d’informations sur la copie des journaux de transactions, consultez [À propos de la copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).    
    
##  <a name="RecommendedSolutions"></a> Solutions recommandées pour l'utilisation de SQL Server pour protéger des données    
 Voici les mesures que nous préconisons pour assurer la protection des données de votre environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :    
    
-   Pour la protection des données via une solution tierce de disque partagé (réseau SAN), nous vous recommandons d’utiliser des instances de cluster de basculement Always On.    
    
-   Pour la protection des données via [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nous vous recommandons d'utiliser [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].    
    
       >  Nous vous recommandons d’utiliser la copie des journaux de transaction si vous exécutez une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne prend pas en charge [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Pour plus d’informations sur les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui prennent en charge [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], consultez la section « Haute disponibilité (Always On) » de l’article [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).    
    
## <a name="see-also"></a> Voir aussi    
 [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)     
 [Mise en miroir de bases de données : interopérabilité et coexistence &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-interoperability-and-coexistence-sql-server.md)     
 [Fonctionnalités du moteur de base de données déconseillées dans SQL Server 2016](../../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)    
    
  

