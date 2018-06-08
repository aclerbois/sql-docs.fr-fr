---
title: Stratégie de basculement automatique flexible - groupe de disponibilité | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], flexible failover policy
- Availability Groups [SQL Server], failover
- flexible failover policy
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 8c504c7f-5c1d-4124-b697-f735ef0084f0
caps.latest.revision: 29
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: cbfc71239bee8c013891b13d862d799435444c83
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769115"
---
# <a name="flexible-automatic-failover-policy---availability-group"></a>Stratégie de basculement automatique flexible - groupe de disponibilité
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une stratégie de basculement flexible vous offre un contrôle granulaire sur les conditions qui entraînent le [basculement automatique](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) d’un groupe de disponibilité. En changeant les conditions d'échec qui déclenchent un basculement automatique et la fréquence des contrôles d'intégrité, vous pouvez augmenter ou diminuer la probabilité d'un basculement automatique pour assurer le contrat de niveau de service relatif à la haute disponibilité.  
  
 La stratégie de basculement flexible d'un groupe de disponibilité est définie par son niveau de condition et le seuil du délai d'attente de contrôle d'intégrité. Lorsque le dépassement du niveau de condition d'échec ou du seuil du délai d'attente de contrôle d'intégrité d'un groupe de disponibilité est détecté, la DLL de ressource du groupe de disponibilité répond au Clustering de basculement Windows Server (WSFC). Le cluster WSFC initialise un basculement automatique vers le réplica secondaire.  
  
> [!IMPORTANT]  
>  Si un groupe de disponibilité dépasse le seuil d'échec WSFC, le cluster WSFC ne tente pas un basculement automatique du groupe de disponibilité. En outre, le groupe de ressources WSFC du groupe de disponibilité reste à l'état d'échec jusqu'à ce que l'administrateur de cluster mette manuellement le groupe de ressources en ligne ou jusqu'à ce que l'administrateur de base de données exécute un basculement manuel du groupe de disponibilité. Le *seuil d'échec WSFC* est le nombre maximal d'échecs autorisés pour le groupe de disponibilité au cours d'une période donnée. La période par défaut est de six heures, et la valeur par défaut du nombre maximal d’échecs au cours de cette période est *n*-1, où *n* est le nombre de nœuds de WSFC. Pour modifier les valeurs de seuil/d'échec pour un groupe de disponibilité donné, utilisez la console du gestionnaire de basculement WSFC.  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Seuil du délai d'attente de contrôle d'intégrité](#HCtimeout)  
  
-   [Niveau de condition d'échec](#FClevel)  
  
-   [Tâches associées](#RelatedTasks)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="HCtimeout"></a> Seuil du délai d'attente de contrôle d'intégrité  
 La DLL de ressource WSFC du groupe de disponibilité exécute un *contrôle d’intégrité* du réplica principal en appelant la procédure stockée [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) sur l’instance de SQL Server qui héberge le réplica principal. **sp_server_diagnostics** retourne les résultats à un intervalle égal à 1/3 du seuil du délai d’attente de vérification d’intégrité pour le groupe de disponibilité. Le seuil par défaut du délai d’attente de vérification d’intégrité est de 30 secondes, ce qui signifie que **sp_server_diagnostics** répond à un intervalle de 10 secondes. Si la procédure **sp_server_diagnostics** est lente ou ne renvoie aucune information, la DLL de ressource attend la fin de l’intervalle du seuil du délai d’attente de vérification d’intégrité avant de déterminer que le réplica principal ne répond pas. Si le réplica principal ne répond pas, un basculement automatique est initialisé, si actuellement pris en charge.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** n’exécute pas de vérifications d’intégrité au niveau de la base de données.  
  
##  <a name="FClevel"></a> Niveau de condition d'échec  
 Le fait que les données de diagnostic et les informations d’intégrité renvoyées par **sp_server_diagnostics** justifient ou non un basculement automatique dépend du niveau de condition d’échec du groupe de disponibilité. Le *niveau de condition d’échec* spécifie les conditions d’échec qui déclenchent un basculement automatique. Il existe cinq niveaux de condition d'échec, allant du moins restrictif (niveau 1) au plus restrictif (le niveau 5). Chaque niveau comprend les niveaux moins restrictifs. Par conséquent, le niveau de condition le plus strict, le niveau 5, inclut les quatre conditions moins restrictives (1 à 4), et ainsi de suite.  
  
> [!IMPORTANT]  
>  Les bases de données endommagées et suspectes ne sont détectées par aucun niveau de condition d'échec. Par conséquent, une base de données qui est endommagée ou suspecte (que ce soit en raison d'une défaillance matérielle, de l'altération des données ou de tout autre problème) ne déclenche jamais de basculement automatique.  
  
 Le tableau suivant décrit les conditions d'échec qui correspondent à chaque niveau.  
  
|Level|Condition d'échec|[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valeur|Valeur PowerShell|  
|-----------|-----------------------|------------------------------|----------------------|  
|Un|Le serveur est arrêté. Spécifie qu’un basculement automatique est initialisé lorsque l’une des situations suivantes se produit :<br /><br /> Le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est fermé.<br /><br /> Le bail du groupe de disponibilité pour la connexion au cluster WSFC expire car aucun accusé de réception n'est reçu de l'instance de serveur. Pour plus d’informations, consultez [Fonctionnement : délai d’expiration de bail Always On SQL Server](http://blogs.msdn.com/b/psssql/archive/2012/09/07/how-it-works-sql-server-Always%20On-lease-timeout.aspx).<br /><br /> <br /><br /> Il s'agit du niveau le moins restrictif.| 1|**OnServerDown**|  
|Deux|Le serveur ne répond pas. Spécifie qu’un basculement automatique est initialisé lorsque l’une des situations suivantes se produit :<br /><br /> L'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne se connecte pas au cluster et le seuil du délai d'attente de contrôle d'intégrité spécifié par l'utilisateur pour le groupe de disponibilité est dépassé.<br /><br /> Le réplica de disponibilité est dans un état d'échec.|2|**OnServerUnresponsive**|  
|Trois|Erreur critique du serveur. Spécifie qu'un basculement automatique est initialisé en cas d'erreurs internes critiques [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , telles que les verrouillages spinlock orphelins, les violations graves d'accès en écriture, ou en cas de trop de vidages.<br /><br /> C'est le niveau par défaut.|3|**OnCriticalServerError**|  
|Quatre|Erreur de serveur modérée. Spécifie qu'un basculement automatique est initialisé en cas d'erreurs internes modérées [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , telles qu'une condition persistante de mémoire insuffisante dans le pool de ressources interne [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|4|**OnModerateServerError**|  
|Cinq|Conditions d'échec qualifiées. Spécifie qu'un basculement automatique est initialisé pour toutes les conditions d'échec qualifiées, notamment :<br /><br /> Détection d’un interblocage de Scheduler.<br /><br /> Détection d'un blocage insoluble.<br /><br /> <br /><br /> Il s'agit du niveau le plus restrictif.|5|**OnAnyQualifiedFailureConditions**|  
  
> [!NOTE]  
>  L'absence de réponse par une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aux demandes des clients n'est pas pertinente pour les groupes de disponibilité.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer un basculement automatique**  
  
-   [Modifier le mode de disponibilité d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md) (le basculement automatique nécessite le mode de disponibilité avec validation synchrone)  
  
-   [Modifier le mode de basculement d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Configurer la stratégie de basculement flexible pour contrôler les conditions du basculement automatique &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Fonctionnement : délai d’expiration de bail Always On SQL Server](http://blogs.msdn.com/b/psssql/archive/2012/09/07/how-it-works-sql-server-Always%20On-lease-timeout.aspx)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Basculement et modes de basculement &#40;groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Stratégie de basculement pour les instances de cluster de basculement](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)  
  
  
