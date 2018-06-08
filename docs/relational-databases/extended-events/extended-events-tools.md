---
title: Outils associés aux événements étendus | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: xevents
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- extended events [SQL Server], using
- extended events [SQL Server], options for using
ms.assetid: d312a9ff-50ba-4721-baef-50bfd3169d38
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 445ee6ce61017507756006fdc2f2b22359b2ec0d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="extended-events-tools"></a>Outils associés aux événements étendus
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Vous pouvez utiliser les outils suivants pour créer et gérer des sessions d’événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Instructions DDL (Data Definition Language). Celles-ci vous permettent de créer et de modifier une session d'événements étendus.  
  
-   Vues de gestion dynamique, affichages catalogue et tables système. Ceux-ci vous permettent d’obtenir des données et des métadonnées de session à l’aide des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les tables système vous aident à déterminer les équivalents Événements étendus existants pour les classes d'événements Trace SQL et les colonnes.  
  
-   Nœud **Événements étendus** de l’Explorateur d’objets. Il vous permet de démarrer, d'arrêter ou de supprimer une session, ou d'importer et d'exporter des modèles de session.  
  
-   Fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Il s’agit d’un outil puissant que vous pouvez utiliser pour créer, modifier et gérer des sessions d’événements étendus. Pour plus d’informations, consultez [Utiliser le fournisseur PowerShell pour les événements étendus](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cela vous permet de créer et d'exécuter les exemples de code fournis dans les rubriques Événements étendus. Pour plus d’informations, consultez [Explorateur d’objets](http://msdn.microsoft.com/library/469ea8e2-79b9-44c8-bb6f-f0e1c5dbf0f2).  
  
 En plus des sessions que vous créez, une session de l'intégrité du système par défaut existe sur le serveur. Elle recueille des données système qui peuvent vous aider à résoudre des problèmes de performances. Pour plus d’informations, consultez [Utiliser la session system_health](../../relational-databases/extended-events/use-the-system-health-session.md).  
  
## <a name="ddl-statements"></a>Instructions DDL  
 Utilisez les instructions DDL suivantes pour créer, modifier et supprimer une session d'événements étendus.  
  
|Nom   |Description|  
|----------|-----------------|  
|[CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)|Crée un objet de session Événements étendus qui identifie la source des événements, les cibles de la session d'événements et les paramètres de la session d'événements.|  
|[ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)|Démarre ou arrête une session d'événements, ou modifie la configuration d'une session d'événements.|  
|[DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)|Supprime une session d'événements.|  
  
## <a name="catalog-views"></a>Affichages catalogue  
 Utilisez les affichages catalogue ci-dessous pour obtenir les métadonnées créées lorsque vous créez une session d'événements.  
  
|Nom   |Description|  
|----------|-----------------|  
|[sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)|Répertorie toutes les définitions de la session d'événements.|  
|[sys.server_event_session_actions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-actions-transact-sql.md)|Retourne une ligne pour chaque action d'un événement d'une session d'événements.|  
|[sys.server_event_session_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-events-transact-sql.md)|Retourne une ligne pour chaque événement d'une session d'événements.|  
|[sys.server_event_session_fields &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-fields-transact-sql.md)|Retourne une ligne pour chaque colonne personnalisable définie explicitement sur les événements et les cibles.|  
|[sys.server_event_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-targets-transact-sql.md)|Retourne une ligne pour chaque cible d'événement d'une session d'événements.|  
  
## <a name="dynamic-management-views"></a>Vues de gestion dynamique  
 Utilisez les vues de gestion dynamique ci-dessous pour obtenir des métadonnées de session et des données de session. Les métadonnées sont obtenues à partir des affichages catalogue et les données de session sont créées lorsque vous démarrez et exécutez une session d'événements.  
  
> [!NOTE]  
>  Ces vues ne contiennent pas de données de session tant qu'une session n'a pas démarré.  
  
|Nom   |Description|  
|----------|-----------------|  
|[sys.dm_os_dispatcher_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-dispatcher-pools-transact-sql.md)|Retourne des informations sur les pools de répartiteurs de la session.|  
|[sys.dm_xe_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)|Retourne une ligne pour chaque objet exposé par un package d'événement.|  
|[sys.dm_xe_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)|Retourne les informations de schéma pour tous les objets.|  
|[sys.dm_xe_packages &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-packages-transact-sql.md)|Répertorie tous les packages inscrits avec le moteur d'événements étendus.|  
|[sys.dm_xe_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-sessions-transact-sql.md)|Retourne des informations sur une session d'événements étendus active.|  
|[sys.dm_xe_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)|Retourne des informations sur les cibles de la session.|  
|[sys.dm_xe_session_events &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-events-transact-sql.md)|Retourne des informations sur les événements de la session.|  
|[sys.dm_xe_session_event_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-event-actions-transact-sql.md)|Retourne des informations sur les actions de la session d'événements.|  
|[sys.dm_xe_map_values &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-map-values-transact-sql.md)|Fournit un mappage des clés numériques internes sur du texte explicite.|  
|[sys.dm_xe_session_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-object-columns-transact-sql.md)|Indique les valeurs de configuration d'objets liés à une session.|  
  
## <a name="system-tables"></a>Tables système  
 Utilisez les tables système suivantes pour obtenir les informations à propos des équivalents Événements étendus pour les classes d'événements Trace SQL et les colonnes.  
  
|Nom   |Description|  
|----------|-----------------|  
|[trace_xe_event_map &#40;Transact-SQL&#41;](../../relational-databases/system-tables/extended-events-tables-trace-xe-event-map.md)|Contient une ligne pour chaque événement Événements étendus mappé à une classe d'événements Trace SQL.|  
|[trace_xe_action_map &#40;Transact-SQL&#41;](../../relational-databases/system-tables/extended-events-tables-trace-xe-action-map.md)|Contient une ligne pour chaque action Événements étendus mappée à un ID de colonne Trace SQL.|  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Tables des événements étendus SQL Server &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/6d52ff03-f5aa-4f0f-8c98-9b49dc76f94e)   
 [Utiliser la session system_health](../../relational-databases/extended-events/use-the-system-health-session.md)   
 [Utiliser le fournisseur PowerShell pour les événements étendus](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)  
  
  
