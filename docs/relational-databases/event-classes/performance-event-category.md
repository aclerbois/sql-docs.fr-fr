---
title: Performance, catégorie d’événement | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server event classes, Performance event category
- Performance event category [SQL Server]
- event classes [SQL Server], Performance event category
ms.assetid: 708f3585-d8be-4980-bbff-672d7c59397e
caps.latest.revision: 32
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c5ed62e4bea21f7028e0ea7ad49ac3d682d3bfbe
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="performance-event-category"></a>Performance, catégorie d'événements
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Utilisez les classes d’événements **Performance** pour surveiller les classes d’événements **Showplan** et celles produites par l’exécution des opérateurs du langage de manipulation des données SQL (DML, Data Manipulation Language).  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Classe d'événements Auto Stats](../../relational-databases/event-classes/auto-stats-event-class.md)|Indique qu'une mise à jour automatique des statistiques d'index et de colonne a eu lieu.|  
|[Degree of Parallelism &#40;7.0 Insert&#41;, classe d’événements](../../relational-databases/event-classes/degree-of-parallelism-7-0-insert-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a exécuté une instruction SELECT, INSERT, UPDATE ou DELETE en utilisant un plan série ou parallèle. Le nombre d'unités centrales utilisées pour effectuer l'opération est également indiqué.|  
|[Classe d'événements Performance Statistics](../../relational-databases/event-classes/performance-statistics-event-class.md)|Contrôle les performances des requêtes exécutées.|  
|[Classe d'événements Showplan All](../../relational-databases/event-classes/showplan-all-event-class.md)|Identifie des opérateurs **Showplan** dans une instruction SQL.|  
|[Classe d'événements Showplan All for Query Compile](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md)|Affiche des données temporelles de compilation pour des opérateurs **Showplan** .|  
|[Classe d'événements Showplan Statistics Profile](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md)|Affiche le coût estimé d'une requête.|  
|[Classe d'événements Showplan XML](../../relational-databases/event-classes/showplan-xml-event-class.md)|Identifie les opérateurs **Showplan** dans une instruction SQL. La classe d'événements stocke chaque événement sous la forme d'un document XML bien défini.|  
|[Classe d'événements Showplan XML for Query Compile](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)|Affiche des données temporelles de compilation pour des opérateurs **Showplan** au format XML.|  
|[Classe d'événements Showplan XML Statistics Profile](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md)|Identifie les opérateurs **Showplan** associés à une instruction SQL. La sortie est un document XML.|  
|[Classe d'événements SQL:FullTextQuery](../../relational-databases/event-classes/sql-fulltextquery-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a exécuté une requête de texte intégral.|  
|[Classe d'événements Plan Guide Successful](../../relational-databases/event-classes/plan-guide-successful-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a pu produire un plan d'exécution pour une requête ou un lot qui contenait un repère de plan.|  
|[Classe d'événements Plan Guide Unsuccessful](../../relational-databases/event-classes/plan-guide-unsuccessful-event-class.md)|Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'a pas pu produire un plan d'exécution pour une requête ou un lot qui contenait un repère de plan.|  
  
## <a name="see-also"></a> Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
