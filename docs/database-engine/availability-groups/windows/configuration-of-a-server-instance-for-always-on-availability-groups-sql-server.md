---
title: Configuration d’une instance de SQL Server pour les groupes de disponibilité Always On | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], about
ms.assetid: fad8db32-593e-49d5-989c-39eb8399c416
caps.latest.revision: 18
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: b348d73669905489b6fdb1e3c68f87e23fb7ab40
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769085"
---
# <a name="configuration-of-a-server-instance-for-always-on-availability-groups-sql-server"></a>Configuration d’une instance de serveur pour les groupes de disponibilité Always On (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique contient des informations sur les conditions requises pour configurer une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour la prise en charge de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
> [!IMPORTANT]  
>  Pour obtenir des informations de base sur la configuration requise pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] et les restrictions pour les nœuds de clustering de basculement Windows Server (WSFC) et pour les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Conditions préalables, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 **Dans cette rubrique :**  
  
-   [Termes et définitions](#TermsAndDefinitions)  
  
-   [Pour configurer une instance de serveur pour la prise en charge des groupes de disponibilité Always On](#ConfigSI)  
  
-   [Tâches associées](#RelatedTasks)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="TermsAndDefinitions"></a> Termes et définitions  
 [Groupes de disponibilité AlwaysOn](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
 Solution de haute disponibilité et de récupération d'urgence qui fournit une alternative au niveau de l'entreprise à la mise en miroir de bases de données. Un *groupe de disponibilité* prend en charge un environnement de basculement pour un ensemble discret de bases de données utilisateur, appelées *bases de données de disponibilité*, qui basculent de concert.  
  
 réplica de disponibilité  
 Instanciation d'un groupe de disponibilité hébergé par une instance spécifique de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et qui conserve une copie locale de chaque base de données de disponibilité appartenant au groupe de disponibilité. Il existe deux types de réplicas de disponibilité : un seul *réplica principal* et un à quatre *réplicas secondaires*. Les instances de serveur qui hébergent les réplicas de disponibilité pour un groupe de disponibilité donné doivent résider sur différents nœuds d'un même cluster WSFC (Clustering de basculement Windows Server).  
  
 [point de terminaison de mise en miroir de bases de données](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
 Un point de terminaison est un objet SQL Server qui permet à SQL Server de communiquer sur le réseau. Pour participer à la mise en miroir de bases de données et/ou à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , une instance de serveur requiert un point de terminaison spécial et dédié. Toutes les connexions de groupe de disponibilité et de mise en miroir sur une instance de serveur utilisent le même point de terminaison de mise en miroir de bases de données. Ce point de terminaison a un objectif spécifique et permet exclusivement de recevoir ces connexions provenant d'autres instances de serveur.  
  
##  <a name="ConfigSI"></a> Pour configurer une instance de serveur pour la prise en charge des groupes de disponibilité Always On  
 Pour prendre en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], une instance de serveur doit résider sur un nœud dans le cluster de basculement WSFC qui héberge le groupe de disponibilité, être compatible avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] et posséder un point de terminaison pour la mise en miroir de bases de données.  
  
1.  Activez la fonctionnalité Groupes de disponibilité Always On sur chaque instance de serveur devant participer à un ou plusieurs groupes de disponibilité. Une instance de serveur donnée peut héberger un seul réplica de disponibilité pour un groupe de disponibilité donné.  
  
2.  Veillez à ce que l'instance de serveur possède un point de terminaison de mise en miroir de bases de données.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Pour activer les groupes de disponibilité Always On**  
  
-   [Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **Pour déterminer si un point de terminaison de mise en miroir de bases de données existe**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
 **Pour créer un point de terminaison pour la mise en miroir de bases de données**  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour l’authentification Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   **Blogs :**  
  
     [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](http://blogs.msdn.com/b/psssql/archive/2012/05/17/Always%20On-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases.aspx)  
  
     [Blogs de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](https://blogs.msdn.microsoft.com/sqlalwayson/)  
  
     [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](http://blogs.msdn.com/b/psssql/)  
  
-   **Vidéos :**  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1: Introducing the Next Generation High Availability Solution (Présentation de la solution haute disponibilité de nouvelle génération)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 2: Building a Mission-Critical High Availability Solution Using Always On (Génération d’une solution haute disponibilité critique à l’aide d’Always On)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Livres blancs :**  
  
     [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence)](http://go.microsoft.com/fwlink/?LinkId=227600)  
  
     [Livres blancs de Microsoft pour SQL Server 2012](http://msdn.microsoft.com/library/hh403491.aspx)  
  
     [Livres blancs de l'équipe de consultants clients de SQL Server](http://sqlcat.com/)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Groupes de disponibilité Always On : interopérabilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Clustering de basculement et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Instances de cluster de basculement Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
  
