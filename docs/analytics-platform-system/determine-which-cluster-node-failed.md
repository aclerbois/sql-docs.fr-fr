---
title: Déterminer le nœud de cluster défaillant - système de plateforme Analytique | Documents Microsoft
description: Cet article décrit comment déterminer le nom du nœud système de plateforme Analytique (APS) qui ont échoué après un basculement de cluster s’est produite et une alerte de basculement de cluster a été déclenchée. Dans le cadre du dépannage d’un cluster de basculement, vous devez déterminer le nom du nœud qui a échoué avant de contacter Microsoft pour aider à résoudre le problème.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 031c8033e91d7a7f74ca8c4409bc02296a22ebcf
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="determine-which-cluster-node-failed-for-analytics-platform-system"></a>Déterminer quel cluster nœud a échoué pour le système de plateforme Analytique
Cette rubrique explique comment déterminer le nom du nœud système de plateforme Analytique (APS) qui ont échoué après un basculement de cluster s’est produite et une alerte de basculement de cluster a été déclenchée. Dans le cadre du dépannage d’un cluster de basculement, vous devez déterminer le nom du nœud qui a échoué avant de contacter Microsoft pour aider à résoudre le problème.  
  
## <a name="Background"></a>En arrière-plan  
Pour la haute disponibilité dans SQL Server PDW, le nœud de contrôle et les nœuds de calcul sont configurés en tant que composants actifs ou passifs de clusters de basculement Windows. Lorsqu’un serveur actif ne répond pas aux demandes du système critiques, le serveur passif bascule et exécute les fonctions du serveur qui a échoué.  
  
Après un basculement de cluster, lorsque SQL Server PDW de rapports sur l’état du nœud, le serveur passif a un échec sur l’état. Toutefois, il n’est pas évidente serveur ou du nœud a échoué, en particulier si le serveur qui a échoué est encore en ligne. Pour résoudre le problème de cluster, vous devez déterminer le nom du nœud qui a basculé.  
  
## <a name="AdminConsoleSolution"></a>Solution de la Console d’administration  
  
#### <a name="to-find-the-name-of-the-node-that-failed"></a>Pour rechercher le nom du nœud qui a échoué  
  
1.  Ouvrez la Console d’administration. Pour plus d’informations sur la Console d’administration, consultez [contrôler le matériel à l’aide de la Console d’administration &#40;système de plateforme Analytique&#41;](monitor-the-appliance-by-using-the-admin-console.md). Une fois le basculement se produit, l’événement de basculement est inclus dans le nombre d’alertes sur le **intégrité** page. Il existe un **intégrité** page pour la région PDW, la région HDI et pour la région de l’infrastructure de l’application. Chaque page de contrôle d’intégrité a un **alertes** onglet. Pour en savoir plus sur une alerte, cliquez sur la page de contrôle d’intégrité, l’onglet alertes et puis cliquez sur une alerte.  
  
## <a name="SystemView"></a>Solution de vue du système  
L’instruction SQL suivante montre comment utiliser le [sys.dm_pdw_component_health_active_alerts](../relational-databases/system-dynamic-management-views/sys-dm-pdw-component-health-active-alerts-transact-sql.md) vue système pour rechercher le nom du serveur qui a échoué.  
  
```sql  
SELECT  
SUBSTRING( component_instance_id, 2, charindex(' ', component_instance_id, 1)-2) AS failed_node_name,  
create_time AS failover_time  
FROM sys.dm_pdw_component_health_active_alerts  
WHERE alert_id = 500139  
ORDER BY failed_node_name;  
```  
  
