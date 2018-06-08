---
title: PDW services - état du système de plateforme Analytique | Documents Microsoft
description: État des services de système de plateforme Analytique Parallel Data Warehouse (PDW).
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: e2252bb821f9522515f1625b0fc118323cb50d1f
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="parallel-data-warehouse-services-status-for-analytics-platform-system"></a>État des services Parallel Data Warehouse pour système de plateforme Analytique
Parallel Data Warehouse **l’état des Services** page dans Microsoft Analytique plateforme système Configuration Manager affiche l’état actuel de tous les services SQL Server PDW et offre la possibilité d’arrêter et démarrer les services PDW. Il s’agit de la seule méthode prise en charge pour démarrer et arrêter les services PDW. Notez que des composants ou services ne peut pas être redémarrées de manière indépendante.  
  
#### <a name="to-start-or-stop-the-appliance-services"></a>Pour démarrer ou arrêter les services d’application  
  
1.  Pour démarrer les services d’application, cliquez sur **démarrer une Appliance**.  
  
2.  Pour arrêter les services d’application, cliquez sur **arrêter l’Appliance**.  
  
Il n’est pas nécessaire de cliquer sur **appliquer** lors du démarrage et arrêt des services d’application à l’aide de **démarrer une Appliance** et **arrêter l’Appliance**.  
  
![Services de PDW des appliances DWConfig](./media/pdw-services-status/SQL_Server_PDW_DWConfig_ApplPDWServices.png "SQL_Server_PDW_DWConfig_ApplPDWServices")  
  
> [!NOTE]  
> L’arrêt de la région PDW arrête également l’agent PDW (sqldwagent) sur les nœuds de la HDInsight région. La HDInsight région fonctionne toujours, mais le contrôle d’intégrité ne sera pas disponible. (L’agent PDW nécessite le nœud de contrôle PDW pour signaler le contrôle d’intégrité).  
  
## <a name="see-also"></a>Voir aussi  
[Le dispositif de points d’accès de l’alimentation ou désactiver &#40;Analytique plate-forme système&#41;](power-the-aps-appliance-on-or-off.md)  
  
