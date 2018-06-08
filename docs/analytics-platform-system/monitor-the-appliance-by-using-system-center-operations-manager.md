---
title: Moniteur avec SCOM - système de plateforme Analytique | Documents Microsoft
description: System Center Operations Manager (SCOM) permet de surveiller le matériel du système de plateforme Analytique (APS).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: c2b26462ab37cf7d63960ff7db6e20c57e8290bb
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="monitor-with-system-center-operations-manager---analytics-platform-system"></a>Moniteur avec System Center Operations Manager - système de plateforme Analytique
System Center Operations Manager (SCOM) permet de surveiller le matériel du système de plateforme Analytique (APS).
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="prerequisites"></a>Configuration requise  
  
1.  System Center Operations Manager 2007 R2, 2012 ou 2012 SP1 doit être installé et en cours d’exécution.  
  
2.  SQL Server 2008 R2 Native Client ou SQL Server 2012 Native Client doit être installé.  
  
3.  Les packs d’administration pour analyser SQL Server PDW et HDInsight doivent être installés, importés et configurés. Utilisez les articles suivants pour obtenir des instructions pour effectuer ces tâches.  
  
    -   [Installer les Packs d’administration SCOM &#40;Analytique plate-forme système&#41;](install-the-scom-management-packs.md)  
  
    -   [Importez le Pack d’administration SCOM pour PDW &#40;Analytique plate-forme système&#41;](import-the-scom-management-pack-for-pdw.md) 
    
    -   [Configurer SCOM pour surveiller le système de plateforme Analytique &#40;Analytique plate-forme système&#41;](configure-scom-to-monitor-analytics-platform-system.md)
  
<!-- MISSING LINKS    -   [Import the SCOM Management Pack for HDInsight &#40;Analytics Platform System&#41;](import-the-scom-management-pack-for-hdinsight.md)  -->  
   
  
## <a name="to-monitor-sql-server-pdw-with-scom"></a>Pour surveiller SQL Server PDW avec SCOM  
Après avoir configuré les Packs d’administration SCOM, cliquez sur le volet analyse de SCOM et accéder à **SQL Server Appliance** , puis **Microsoft SQL Server Parallel Data Warehouse**. Sous Microsoft SQL Server Parallel Data Warehouse, il existe quatre options : alertes, appareils, diagramme d’application et nœuds.  
  
### <a name="alerts"></a>Alertes  
Les alertes sont où vous pouvez rechercher des alertes en cours à gérer.  
  
![Alerts](./media/monitor-the-appliance-by-using-system-center-operations-manager/SCOM_SCOM.png "SCOM_SCOM")  
  
### <a name="appliances"></a>Appliances  
Appareils sont où vous trouverez les appareils actuellement détectés et analysés SQL Server PDW dans votre environnement. Si une appliance n’affiche pas ici, et vous avez créé la connexion ODBC pour celle-ci, puis il peut être un problème avec votre compte PDWWatcher. Si elles apparaissent comme « Non analysé », peut avoir un problème avec votre compte PDWMonitor. Soyez patient car SCOM ne pas apporte des modifications en temps réel, mais vérifie périodiquement pour les nouveaux équipements à surveiller et envoie périodiquement des requêtes pour les appareils pour l’analyse.  
  
![Appliances](./media/monitor-the-appliance-by-using-system-center-operations-manager/SCOM_SCOM2.png "SCOM_SCOM2")  
  
### <a name="appliances-diagram"></a>Diagramme des appliances  
La Page de diagramme des Appliances est où vous pouvez obtenir les examiner l’intégrité de votre application avec une vue d’arborescence :  
  
![Diagramme des appliances](./media/monitor-the-appliance-by-using-system-center-operations-manager/SCOM_SCOM3.png "SCOM_SCOM3")  
  
### <a name="nodes"></a>Nœuds  
Enfin, l’affichage de nœuds permet de voir l’intégrité de votre application sur chaque nœud :  
  
![Nodes](./media/monitor-the-appliance-by-using-system-center-operations-manager/SCOM_SCOM4.png "SCOM_SCOM4")  
  
## <a name="see-also"></a>Voir aussi  
<!-- MISSING LINKS [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
[Alertes de la Console Administration de présentation &#40;Analytique plate-forme système&#41;](understanding-admin-console-alerts.md)  
  
