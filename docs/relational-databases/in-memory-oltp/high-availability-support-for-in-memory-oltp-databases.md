---
title: Prise en charge de la haute disponibilité pour les bases de données OLTP en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 08/31/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2113a916-3b1e-496c-8650-7f495e492510
caps.latest.revision: 11
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: a3e1697d99eed38fbfcca2a3f0539537fa088cdd
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="high-availability-support-for-in-memory-oltp-databases"></a>Prise en charge de la haute disponibilité pour les bases de données OLTP en mémoire
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les bases de données contenant des tables optimisées en mémoire, avec ou sans procédures stockées compilées natives, sont entièrement prises en charge avec les groupes de disponibilité Always On.  Il n’existe aucune différence dans la configuration et la prise en charge des bases de données contenant des objets [!INCLUDE[hek_2](../../includes/hek-2-md.md)] et celles n’en comportant pas.  
  
 Quand une base de données OLTP en mémoire est déployée dans une configuration de groupe de disponibilité Always On, les modifications apportées aux tables optimisées en mémoire sur le réplica principal sont appliquées dans la mémoire aux tables sur les réplicas secondaires, au moment où la phase de restauration par progression est appliquée. Cela signifie que le basculement vers un réplica secondaire peut être très rapide, étant donné que les données sont déjà en mémoire. En outre, les tables sont disponibles pour les requêtes sur les réplicas secondaires qui ont été configurés pour un accès en lecture.  
  
## <a name="always-on-availability-groups-and-in-memory-oltp-databases"></a>Groupes de disponibilité Always On et bases de données OLTP en mémoire  
 La configuration des bases de données avec des composants [!INCLUDE[hek_2](../../includes/hek-2-md.md)] fournit les éléments suivants :  
  
-   **Expérience entièrement intégrée**   
    Vous pouvez configurer vos bases de données contenant des tables optimisées en mémoire à l’aide du même assistant et avec le même niveau de prise en charge pour les réplicas secondaires synchrones et asynchrones. En outre, le contrôle d’intégrité est fourni par le tableau de bord Always On que vous connaissez dans SQL Server Management Studio.  
  
-   **Temps de basculement comparable**   
    Les réplicas secondaires maintiennent l’état en mémoire des tables optimisées en mémoire durables. En cas de basculement automatique ou forcé, le temps de basculement vers le nouveau réplica principal est comparable aux tables sur disque car aucune récupération n'est nécessaire. Les tables mémoire optimisées créées en tant que SCHEMA_ONLY sont prises en charge dans cette configuration. Toutefois, les modifications apportées à ces tables ne sont pas enregistrées et par conséquent, aucune donnée n'existera dans ces tables sur le réplica secondaire.  
  
-   **Secondaire accessible en lecture**   
    Vous pouvez accéder aux tables optimisées en mémoire sur le réplica secondaire et les interroger si le réplica secondaire a été configuré pour un accès en lecture. Dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], l’horodateur de lecture sur le réplica secondaire est étroitement synchronisé avec l’horodateur de lecture sur le réplica principal, ce qui signifie que les modifications apportées sur le réplica principal sont rapidement visibles sur le réplica secondaire. Ce comportement de synchronisation étroite est différent d’OLTP en mémoire [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] .  
  
## <a name="failover-clustering-instance-fci-and-in-memory-oltp-databases"></a>Instance de clustering de basculement (FCI) et bases de données OLTP en mémoire  
 Pour bénéficier d’une haute disponibilité dans une configuration de stockage partagé, vous pouvez configurer le clustering de basculement sur les instances comportant une ou plusieurs bases de données avec des tables mémoire optimisées. Vous devez tenir compte des facteurs suivants dans le cadre de la configuration d'une instance FCI.  
  
-   **Objectif de temps de récupération**   
    Le temps de basculement est susceptible d’être plus élevé, car les tables optimisées en mémoire doivent être chargées en mémoire avant que la base de données ne soit disponible.  
  
-   **Tables SCHEMA_ONLY**   
    Sachez que les tables SCHEMA_ONLY seront vides et ne comporteront aucune ligne après le basculement. C’est l’application qui conçoit et définit cela. Le comportement est identique quand vous redémarrez une base de données [!INCLUDE[hek_2](../../includes/hek-2-md.md)] avec une ou plusieurs tables SCHEMA_ONLY.  
  
## <a name="support-for-transaction-replication-in-in-memory-oltp"></a>Prise en charge de la réplication des transactions dans OLTP en mémoire  
 Les tables agissant comme des abonnés de réplication transactionnelle, à l'exclusion de la réplication transactionnelle d'égal à égal, peuvent être configurées en tant que tables mémoire optimisées. Les autres configurations de réplication ne sont pas compatibles avec les tables mémoire optimisées.  Pour plus d’informations, consultez [Abonnés à la réplication de tables optimisées en mémoire](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Groupes de disponibilité Always On (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Secondaires actifs : réplicas secondaires accessibles en lecture (groupes de disponibilité Always On)](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Abonnés à la réplication de tables optimisées en mémoire](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md)  
  
  
