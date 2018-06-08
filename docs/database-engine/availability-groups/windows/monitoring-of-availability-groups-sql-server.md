---
title: Surveillance des groupes de disponibilité (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/08/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], troubleshooting
ms.assetid: 1d5e3291-0d0a-45a1-88e5-1fc242d17210
caps.latest.revision: 27
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 39d3c9694ab25320bc2326f7464bf0948a3abc88
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771435"
---
# <a name="monitoring-of-availability-groups-sql-server"></a>Surveillance des groupes de disponibilité (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Pour surveiller les propriétés et l’état d’un groupe de disponibilité Always On, vous pouvez utiliser les outils suivants.  
  
|Outil|Brève description|Liens|  
|----------|-----------------------|-----------|  
|Pack d'analyse System Center pour SQL Server|Le pack d'analyse pour SQL Server (SQLMP) est la solution recommandée pour la surveillance des groupes de disponibilité, des réplicas de disponibilité et des bases de données de disponibilité pour les administrateurs informatiques. Les fonctionnalités d'analyse particulièrement appropriées pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] incluent les suivantes :<br /><br /> Découverte automatique des groupes de disponibilité, des réplicas de disponibilité et des bases de données de disponibilité entre des centaines d'ordinateurs. Cette opération vous permet de suivre facilement votre inventaire [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .<br /><br /> Fonctionnalités complètes d'alertes et tickets System Center Operations Manager (SCOM). Ces fonctionnalités offrent des connaissances détaillées qui permettent de résoudre plus rapidement un problème.<br /><br /> Extension personnalisée à l’analyse d’intégrité Always On à l’aide de la gestion basée sur des stratégies.<br /><br /> Intégrité regroupée des bases de données de disponibilité aux réplicas de disponibilité.<br /><br /> Tâches personnalisées qui gèrent [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans la console System Center Operations Manager.|Pour télécharger le pack d’analyse (SQLServerMP.msi) et *le Guide SQL Server Management Pack pour System Center Operations Manager* (SQLServerMPGuide.doc), consultez :<br /><br /> [Pack d'analyse System Center pour SQL Server](http://www.microsoft.com/download/details.aspx?displaylang=en&id=10631)|  
|[!INCLUDE[tsql](../../../includes/tsql-md.md)]|[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] fournissent une quantité d'informations sur vos groupes de disponibilité et leurs réplicas, bases de données, écouteurs et environnement de cluster WSFC.|[Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|Le volet **Détails de l'Explorateur d'objets** affiche des informations de base sur les groupes de disponibilité hébergés sur l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté.<br /><br /> **\*\* Conseil \*\*** Utilisez ce volet pour sélectionner plusieurs groupes de disponibilité, réplicas ou bases de données et effectuer des tâches d’administration courantes sur les objets sélectionnés, comme la suppression de plusieurs réplicas de disponibilité ou bases de données dans un groupe de disponibilité.|[Utiliser les détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|Les boîtes de dialogue**Propriétés** vous permettent d'afficher les propriétés des groupes de disponibilité, les réplicas ou les écouteurs et, dans certains cas, de modifier leurs valeurs.|-   [Afficher les propriétés d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)<br />-   [Afficher les propriétés d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)<br />-   [Afficher les propriétés d’écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)|  
|Moniteur système|L’objet de performance **SQLServer:Availability Replica** intègre des compteurs de performances chargés de fournir des informations sur les réplicas de disponibilité.|[SQL Server, réplica de disponibilité](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)|  
|Moniteur système|L’objet de performance **SQLServer:Database Replica** contient des compteurs de performances qui signalent des informations concernant les bases de données secondaires sur un réplica secondaire donné.<br /><br /> L'objet **SQLServer:Databases** dans SQL Server contient des compteurs de performances pour surveiller les activités du journal des transactions, entre autres choses. Les compteurs suivants sont particulièrement appropriés pour surveiller l’activité des journaux des transactions sur des bases de données de disponibilité : **Temps d’attente de vidage du journal (ms)**, **Vidages du journal/s**, **Journaliser les absences dans le cache/s du pool**, **Journaliser les lectures du disque/s du pool**et **Journaliser les requêtes/s du pool**.|[SQL Server, réplica de base de données](../../../relational-databases/performance-monitor/sql-server-database-replica.md) et [SQL Server, objet Databases](../../../relational-databases/performance-monitor/sql-server-databases-object.md)|  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   **Blogs :**  
  
     [The Always On Health Model Part 1 -- Health Model Architecture (Modèle d’intégrité Always On Partie 1 -- Architecture du modèle d’intégrité)](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/08/the-alwayson-health-model-part-1-health-model-architecture/)  
  
     [The Always On Health Model Part 2 -- Extending the Health Model (Modèle d’intégrité Always On Partie 2 -- Extension du modèle d’intégrité)](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/13/the-alwayson-health-model-part-2-extending-the-health-model/)  
  
     [Surveillance de l’intégrité Always On avec PowerShell - Partie 1 : Vue d’ensemble des applets de commande de base](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/13/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview/)  
  
     [Surveillance de l’intégrité Always On avec PowerShell - Partie 2 : Utilisation des applets de commande avancées](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/13/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage/)  
  
     [Surveillance de l’intégrité Always On avec PowerShell - Partie 3 : Application de surveillance simple](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/14/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application/)  
  
     [Surveillance de l’intégrité Always On avec PowerShell - Partie 4 : Intégration à l’Agent SQL Server](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/15/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent/)  
  
     [Blogs de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](https://blogs.msdn.microsoft.com/sqlalwayson/)  
  
     [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](https://blogs.msdn.microsoft.com/psssql/)  
  
-   **Livres blancs :**  
  
     [Livres blancs de Microsoft pour SQL Server 2012](http://msdn.microsoft.com/library/hh403491.aspx)  
  
     [Livres blancs de l'équipe de consultants clients de SQL Server](http://sqlcat.com/)  
  
## <a name="see-also"></a> Voir aussi  
 [Affichages catalogue des groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Vues et fonctions de gestion dynamiques de groupes de disponibilité Always On &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Stratégie flexible pour le basculement automatique d’un groupe de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/flexible-automatic-failover-policy-availability-group.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Réparation de page automatique &#40;groupes de disponibilité : mise en miroir de bases de données&#41;](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
