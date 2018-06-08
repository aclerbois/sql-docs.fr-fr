---
title: Surveillance et dépannage gérés les objets de base de données | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], performance
- monitoring [CLR integration]
- performance [CLR integration]
ms.assetid: a7b589ac-104d-4b68-b4aa-9f5fc192b13d
caps.latest.revision: 18
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: e5797bbbf25138658fd5db595a2cc1203046dd32
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitoring-and-troubleshooting-managed-database-objects"></a>Surveillance et dépannage des objets de base de données managés
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique fournit des informations sur les outils à l'aide desquels vous pouvez surveiller et dépanner des objets de base de données et des assemblys managés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="profiler-trace-events"></a>Événements de trace du Générateur de profils  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut SQL Trace et des notifications d'événements qui permettent de surveiller les événements survenant dans le moteur de base de données. En enregistrant des événements spécifiques, Trace SQL permet d'améliorer les performances, d'analyser l'activité de la base de données, de collecter des échantillons de données pour un environnement de test, de déboguer les procédures stockées et les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et de collecter des données pour les outils d'analyse des performances. Pour plus d’informations, consultez [SQL Trace](../../relational-databases/sql-trace/sql-trace.md) et [événements étendus](../../relational-databases/extended-events/extended-events.md).  
  
|Événement| Description|  
|-----------|-----------------|  
|[Classe d'événements Assembly Load](http://msdn.microsoft.com/library/cfb0b69d-4ce0-4067-a3df-d82775e57886)|Utilisé pour surveiller des demandes de chargement d'assembly (succès et échecs).|  
|[Classe d’événements SQL : BatchStarting](../../relational-databases/event-classes/sql-batchstarting-event-class.md), [classe d’événements SQL : BatchCompleted](../../relational-databases/event-classes/sql-batchcompleted-event-class.md)|Fournit des informations sur des lots [!INCLUDE[tsql](../../includes/tsql-md.md)] qui ont démarré ou ont été finalisés.|  
|[SP : Starting, classe d’événements](../../relational-databases/event-classes/sp-starting-event-class.md), [SP : Completed événement (classe)](../../relational-databases/event-classes/sp-completed-event-class.md)|Utilisé pour surveiller l'exécution des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|[Classe d’événements SQL : StmtStarting](../../relational-databases/event-classes/sql-stmtstarting-event-class.md), [classe d’événements SQL : StmtCompleted](../../relational-databases/event-classes/sql-stmtcompleted-event-class.md)|Utilisé pour surveiller l'exécution du CLR (Common Language Runtime) et des routines [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
  
## <a name="performance-counters"></a>Compteurs de performances  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des objets et des compteurs qui peuvent être utilisés par le Moniteur système pour analyser l'activité des ordinateurs exécutant une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un objet peut être n'importe quelle ressource [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telle qu'un verrou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un processus Windows. Chaque objet contient un ou plusieurs compteurs qui déterminent divers aspects de l'objet à surveiller. Pour plus d’informations, consultez [Utiliser des objets SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
|Objet| Description|  
|------------|-----------------|  
|[SQL Server, objet CLR](../../relational-databases/performance-monitor/sql-server-clr-object.md)|Durée d'exécution totale dans le CLR.|  
  
## <a name="windows-system-monitor-perfmonexe-counters"></a>Compteurs du Moniteur système Windows (PERFMON.EXE)  
 L'outil Moniteur système (PERFMON.EXE) de Windows dispose de plusieurs compteurs de performances que vous pouvez utiliser pour surveiller les applications d'intégration du CLR. Les compteurs de performances CLR .NET peuvent être filtrés d'après le nom du processus « sqlservr » afin de contrôler les applications d'intégration du CLR en cours d'exécution.  
  
|Objet de performance| Description|  
|------------------------|-----------------|  
|SqlServer:CLR|Fournit au serveur des statistiques sur l'UC.|  
|Exceptions CLR .NET|Suit le nombre d'exceptions par seconde.|  
|Chargement CLR .NET|Fournit des informations sur les AppDomains et les assemblys chargés dans le serveur.|  
|Mémoire CLR .NET|Fournit des informations sur l'utilisation de la mémoire CLR. Cet objet peut être employé pour émettre des alertes si l'utilisation de la mémoire de la mémoire devient trop importante.|  
|Fournisseur de données .NET pour SQL Server|Suit le nombre de connexions et de déconnexions par seconde. Cet objet peut être utilisé pour surveiller le niveau d'activité de la base de données.|  
  
## <a name="catalog-views"></a>Affichages catalogue  
 Les affichages catalogue retournent des informations qui sont exploitées par le moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il est conseillé d'utiliser les affichages catalogue puisqu'ils représentent l'interface la plus générale vers les métadonnées de catalogue et le moyen le plus efficace pour obtenir, transformer et présenter des formulaires personnalisés de ces informations. Toutes les métadonnées de catalogue accessibles à l'utilisateur sont exposées dans des affichages catalogue. Pour plus d’informations, consultez [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).  
  
|Vue de catalogue| Description|  
|------------------|-----------------|  
|[Sys.Assemblies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)|Retourne des informations sur des assemblys inscrits dans une base de données.|  
|[Sys.assembly_references &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-references-transact-sql.md)|Identifie des assemblys qui référencent d'autres assemblys.|  
|[sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)|Retourne des informations sur chaque fonction, procédure stockée et déclencheur définis dans un assembly.|  
|[sys.assembly_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-files-transact-sql.md)|Retourne des informations sur les fichiers d'assembly inscrits dans la base de données.|  
|[sys.assembly_types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-types-transact-sql.md)|Identifie les types définis par l'utilisateur (UDT) définis par un assembly.|  
|[sys.module_assembly_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-module-assembly-usages-transact-sql.md)|Identifie les assemblys dans lesquels les modules CLR sont définis.|  
|[sys.parameter_type_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameter-type-usages-transact-sql.md)|Retourne des informations sur les paramètres correspondant à des types définis par l'utilisateur.|  
|[sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)|Identifie l'assembly dans lequel un déclencheur CLR est défini.|  
|[sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)|Identifie sur le serveur les déclencheurs DDL de niveau serveur, y compris les déclencheurs CLR.|  
|[sys.type_assembly_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-type-assembly-usages-transact-sql.md)|Identifie les assemblys dans lesquels les types définis par l'utilisateur sont définis.|  
|[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)|Retourne les types définis par l'utilisateur et système inscrits dans la base de données.|  
  
## <a name="dynamic-management-views"></a>Vues de gestion dynamique  
 Les fonctions et les vues de gestion dynamique renvoient des informations sur l'état du serveur qu'il est possible d'utiliser pour surveiller l'état d'une instance du serveur, diagnostiquer des problèmes et améliorer les performances. Pour plus d’informations, consultez [fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
|Vue de gestion dynamique| Description|  
|---------|-----------------|  
|[sys.dm_clr_appdomains &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-appdomains-transact-sql.md)|Fournit des informations sur chaque domaine d'application sur le serveur.|  
|[Sys.dm_clr_loaded_assemblies &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)|Identifie chaque assembly managé inscrit sur le serveur.|  
|[Sys.dm_clr_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-properties-transact-sql.md)|Retourne des informations sur le CLR hébergé.|  
|[Sys.dm_clr_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-tasks-transact-sql.md)|Identifie toutes les tâches du CLR en cours d'exécution.|  
|[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)|Retourne des informations sur les plans d'exécution de requêtes mis en cache par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une exécution plus rapide des requêtes.|  
|[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)|Retourne les statistiques sur les performances des agrégats pour les plans de requêtes mis en cache.|  
|[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|Retourne des informations sur chaque demande qui s'exécute dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)|Retourne l'ensemble des régisseurs de mémoire actuellement actifs dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y compris ceux du CLR.|  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de programmation pour l’intégration du CLR &#40;Common Language Runtime&#41;](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md)  
  
  
