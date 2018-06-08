---
title: Analyse de l’appliance - système de plateforme Analytique | Documents Microsoft
description: Ce guide analyse appliance décrit les outils et les tâches de surveillance de l’application de système de plateforme Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: f87734a14337e7e35655439ddf70f0a126147eb7
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="appliance-monitoring-for-analytics-platform-system"></a>Dispositif d’analyse pour le système de plateforme d’Analytique
Ce guide analyse appliance décrit les outils et les tâches de surveillance de l’application de système de plateforme Analytique.  
  
## <a name="Basics"></a>Principes de base et les outils d’analyse  
Les valeurs et les informations qui peuvent être surveillées sur le dispositif de SQL Server PDW sont étendues. Par exemple, les éléments suivants sont classiques de surveillance des tâches.  
  
-   Recherchez toute alerte émise par SQL Server PDW.  
  
-   Analyse pour le matériel défaillant.  
  
-   Analyse des problèmes de connectivité réseau.  
  
-   Vérifiez les erreurs retournées aux utilisateurs pendant le traitement des requêtes.  
  
-   Afficher le nombre de sessions actives et des requêtes.  
  
-   Vérifiez l’état de la charge, les sauvegardes et restaurations.  
  
### <a name="appliance-monitoring-tools"></a>Outils d’analyse des appliances  
Il existe plusieurs outils disponibles pour surveiller l’application.  
  
Console Administration  
SQL Server PDW a une Console d’administration. Il s’agit d’un outil web qui affiche des informations sur les requêtes, charges, sauvegarde et restauration, les verrous, les sessions, alertes et état des appliances. La Console d’administration s’exécute sur l’application ; les utilisateurs se connecter à la Console d’administration via Internet Explorer. Pour plus d'informations, consultez :  
  
-   [Contrôler le matériel à l’aide de la Console d’administration &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-the-admin-console.md)  
  
![Alertes de la Console d’administration PDW](./media/appliance-monitoring/SQL_Server_PDW_AdminConsol_Queries.png "SQL_Server_PDW_AdminConsol_Queries")  
  
Vues système  
SQL Server PDW inclut des vues système complète qui vous permettent d’obtenir des informations détaillées sur le contrôle d’intégrité de l’équipement, état et les performances. Pour obtenir la liste de vues système pour l’analyse des tâches, consultez :  
  
-   [Surveiller l’application à l’aide de vues système &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-system-views.md)  
  
System Center Operations Manager (SCOM)  
SQL Server PDW offre une intégration complète avec Systems Center Operations Manager. Les packs d’administration pour SQL Server PDW sont disponibles en téléchargement gratuit. Pour plus d’informations sur l’utilisation de System Center pour surveiller SQL Server PDW, consultez les rubriques suivantes :  
  
-   [Surveiller l’application à l’aide de System Center Operations Manager &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-system-center-operations-manager.md)  
  
Solutions personnalisées  
Pour les situations lorsque System Center n’est pas disponible avec votre centre de données outils d’analyse, vous pouvez surveiller l’appliance à l’aide d’une solution d’analyse tiers. Installation d’agents de logiciels externes n’est actuellement pas pris en charge dans PDW, mais la plupart des solutions de surveillance prend en charge Transact\-intégration SQL, l’administrateur système peut implémenter Transact direct\-des requêtes SQL sur votre PDW équipement.  
  
Si votre solution d’analyse ne prend pas en charge directe Transact\-requêtes SQL, ou vous n’avez pas un outil de surveillance, vous pouvez ensuite utiliser des scripts pour effectuer des tâches de surveillance, telles que l’envoi de courrier électronique lorsqu’une alerte se produit.  Le wiki TechNet contient un exemple de solution de surveillance sous forme de script.  
  
-   [Exemple d’analyse pour SQL Server PDW de Power Shell](http://go.microsoft.com/fwlink/?LinkId=248020)  
   
## <a name="Tasks"></a>Liées de tâches de surveillance  
  
|Tâche d’analyse| Description|  
|-------------------|---------------|  
|Contrôler le matériel à l’aide de la Console d’administration.|[Contrôler le matériel à l’aide de la Console d’administration &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-the-admin-console.md)|  
|Contrôler le matériel à l’aide de vues système.|[Surveiller l’application à l’aide de vues système &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-system-views.md)|  
|Surveiller l’application à l’aide de System Center|[Surveiller l’application à l’aide de System Center Operations Manager &#40;Analytique plate-forme système&#41;](monitor-the-appliance-by-using-system-center-operations-manager.md)|  
|Surveiller l’état de l’application.|[État de contrôle d’intégrité de l’analyse &#40;Analytique plate-forme système&#41;](monitor-appliance-health-state.md)|  
|Analyse des pulsations.|[Envoyer des commentaires de télémétrie à Microsoft &#40;SQL Server PDW&#41;](send-telemetry-feedback-to-microsoft-sql-server-pdw.md)|  
|Suivi des alertes de l’appliance.|[Suivi des alertes de l’Appliance &#40;Analytique plate-forme système&#41;](track-appliance-alerts.md)|  
|Déterminer la capacité est utilisée.|[Afficher l’utilisation de la capacité &#40;Analytique plate-forme système&#41;](view-capacity-utilization.md)|  
|Déterminer la fréquence d’interrogation de l’application.|[Déterminer la fréquence d’interrogation &#40;Analytique plate-forme système&#41;](determine-polling-frequency.md)|  
|En cas de défaillance d’un cluster, déterminer quel cluster nœud a échoué.|[Déterminer quel nœud du Cluster a échoué &#40;Analytique plate-forme système&#41;](determine-which-cluster-node-failed.md)|  


<!-- MISSING LINKS |Monitor loads.|[Monitor Loads &#40;SQL Server PDW&#41;](../sqlpdw/monitor-loads-sql-server-pdw.md)|  -->  
<!-- MISSING LINKS |Monitor backups and restores.|[Monitor Backups and Restores &#40;SQL Server PDW&#41;](../sqlpdw/monitor-backups-and-restores-sql-server-pdw.md)|  -->  
<!-- MISSING LINKS |Monitor the active queries.|[Monitoring Active Queries &#40;SQL Server PDW&#41;](../sqlpdw/monitoring-active-queries-sql-server-pdw.md)|  -->  
  
## <a name="see-also"></a>Voir aussi  
<!-- MISSING LINKS [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
[Tâches de gestion appliance &#40;Analytique plate-forme système&#41;](appliance-management-tasks.md)  
  
