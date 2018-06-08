---
title: Suivi des alertes de l’appliance - système de plateforme Analytique | Documents Microsoft
description: Suivi des alertes de matériel dans le système de plateforme d’Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 82803e6f20e4a710f317e2e7a541c4a1c72ed08d
ms.sourcegitcommit: 056ce753c2d6b85cd78be4fc6a29c2b4daaaf26c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2018
---
# <a name="track-appliance-alerts-in-analytics-platform-system"></a>Suivi des alertes d’application dans le système de plateforme Analytique
Cette rubrique explique comment utiliser la Console d’administration et les vues système pour effectuer le suivi des alertes dans un dispositif de SQL Server PDW.  
  
## <a name="to-track-appliance-alerts"></a>Pour effectuer le suivi des alertes de l’Appliance  
SQL Server PDW crée des alertes pour les problèmes matériels et logiciels qui nécessitent une attention. Chaque alerte contient un titre et une description du problème.  
  
SQL Server PDW consigne les alertes dans le [sys.dm_pdw_component_health_alerts](../relational-databases/system-dynamic-management-views/sys-dm-pdw-component-health-alerts-transact-sql.md) DMV. Le système conserve une limite de 10 000 alertes et supprime les alertes les plus anciennes tout d’abord si la limite est atteinte.  
  
### <a name="view-alerts-by-using-the-admin-console"></a>Afficher les alertes à l’aide de la Console d’administration  
Il existe un **alertes** onglet pour la région PDW, la région HDI et pour la région de l’infrastructure de l’application. Une fois le basculement se produit, l’événement de basculement est inclus dans le nombre d’alertes sur la page. Il existe une page de la région PDW, la région HDI et pour la région de l’infrastructure de l’application. Chaque page de contrôle d’intégrité a un onglet. Pour en savoir plus sur une alerte, cliquez sur le **intégrité** page, le **alertes** onglet, puis cliquez sur une alerte.  
  
![Alertes de la Console d’administration PDW](./media/track-appliance-alerts/SQL_Server_PDW_AdminConsole_AlertsV2.png "SQL_Server_PDW_AdminConsole_AlertsV2")  
  
Sur le **alertes** page :  
  
-   Pour afficher l’historique des alertes, cliquez sur le **l’historique de révision alerte** lien.  
  
-   Pour afficher le composant d’alerte et de ses valeurs de propriété actuelles, cliquez sur la ligne d’alerte.  
  
-   Pour afficher des détails sur le nœud qui a déclenché l’alerte, cliquez sur le nom du nœud.  
  
### <a name="view-alerts-by-using-the-system-views"></a>Afficher les alertes en utilisant les vues système  
Pour afficher les alertes à l’aide de vues système, interrogez [sys.dm_pdw_component_health_active_alerts](../relational-databases/system-dynamic-management-views/sys-dm-pdw-component-health-active-alerts-transact-sql.md). Cette DMV affiche les alertes qui n’ont pas été corrigées. Pour plus d’informations triage des alertes et les erreurs, utilisez le [sys.dm_pdw_errors](../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md) DMV.  
  
L’exemple suivant est une requête courante pour l’affichage des alertes en cours.  
  
```sql  
SELECT   
    aa.[pdw_node_id],  
    n.[name] AS [node_name],  
    g.[group_name] ,  
    c.[component_name] ,  
    aa.[component_instance_id] ,   
    a.[alert_name] ,  
    a.[state] ,  
    a.[severity] ,  
    aa.[current_value] ,  
    aa.[previous_value] ,  
    aa.[create_time] ,  
    a.[description]   
FROM [sys].[dm_pdw_component_health_active_alerts] AS aa  
    INNER JOIN sys.dm_pdw_nodes AS n   
        ON aa.[pdw_node_id] = n.[pdw_node_id]  
    INNER JOIN [sys].[pdw_health_components] AS c   
        ON aa.[component_id] = c.[component_id]  
    INNER JOIN [sys].[pdw_health_component_groups] AS g   
        ON c.[group_id] = g.[group_id]  
    INNER JOIN [sys].[pdw_health_alerts] AS a   
        ON aa.[alert_id] = a.[alert_id] and aa.[component_id] = c.[component_id]  
ORDER BY  
    a.alert_id ,  
    aa.[pdw_node_id];  
```  
  
## <a name="see-also"></a>Voir aussi  
<!-- MISSING LINKS [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->
[Surveillance de l’appliance &#40;Analytique plate-forme système&#41;](appliance-monitoring.md)  
  
