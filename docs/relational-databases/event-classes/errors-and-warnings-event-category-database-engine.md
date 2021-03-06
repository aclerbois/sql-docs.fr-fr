---
title: "Catégorie d’événements Erreurs et avertissements (moteur de base de données) | Microsoft Docs"
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database
ms.service: 
ms.component: event-classes
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- Errors and Warnings event category [SQL Server]
- SQL Server event classes, Errors and Warnings event category
- event classes [SQL Server], Errors and Warnings event category
ms.assetid: 249c19b5-af68-4433-80f6-337395176641
caps.latest.revision: 
author: stevestein
ms.author: sstein
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: f6eaa88dfe9f1d01b72d63e44f30c9850e846f20
ms.sourcegitcommit: 37f0b59e648251be673389fa486b0a984ce22c81
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
# <a name="errors-and-warnings-event-category-database-engine"></a>Catégorie d'événements Erreurs et avertissements (moteur de base de données)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
La catégorie d’événements **Erreurs et avertissements** contient des événements généraux d’erreurs et d’avertissements.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Attention, classe d’événements](../../relational-databases/event-classes/attention-event-class.md)|Indique qu’un événement **Attention** s’est produit.|  
|[Classe d'événements Background Job Error](../../relational-databases/event-classes/background-job-error-event-class.md)|Indique qu'un travail en arrière-plan s'est terminé anormalement.|  
|[Classe d'événements Bitmap Warning](../../relational-databases/event-classes/bitmap-warning-event-class.md)|Indique que le filtrage Bitmap a été désactivé dans une requête.|  
|[Classe d'événements Blocked Process Report](../../relational-databases/event-classes/blocked-process-report-event-class.md)|Indique qu'une tâche a été bloquée plus longtemps qu'une période spécifiée.|  
|[CPU Threshold Exceeded, classe d’événements](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md)|Indique que Resource Governor détecte une requête qui dépasse le seuil de l'UC spécifié.|  
|[Classe d'événements ErrorLog](../../relational-databases/event-classes/errorlog-event-class.md)|Indique que les événements d'erreurs ont été enregistrés dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Classe d'événements EventLog](../../relational-databases/event-classes/eventlog-event-class.md)|Indique que les événements ont été consignés dans le journal des événements de Windows.|  
|[Classe d'événements Exception](../../relational-databases/event-classes/exception-event-class.md)|Indique qu'une exception s'est produite dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Classe d'événements Exchange Spill](../../relational-databases/event-classes/exchange-spill-event-class.md)|Indique que des tampons de communication dans un plan de requête parallèle ont été écrits dans la base de données tempdb.|  
|[Classe d'événements Execution Warnings](../../relational-databases/event-classes/execution-warnings-event-class.md)|Indique que des avertissements d'allocation de mémoire se sont produits lors de l'exécution d'une instruction ou d'une procédure stockée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Classe d'événements Hash Warning](../../relational-databases/event-classes/hash-warning-event-class.md)|Indique qu'une récurrence ou une interruption de hachage s'est produite pendant une opération de hachage.|  
|[Classe d'événements Missing Column Statistics](../../relational-databases/event-classes/missing-column-statistics-event-class.md)|Indique que des statistiques de colonne qui auraient pu être utiles pour l'optimiseur ne sont pas disponibles.|  
|[Classe d'événements Missing Join Predicate](../../relational-databases/event-classes/missing-join-predicate-event-class.md)|Indique qu'une requête en cours d'exécution est sans prédicat de jointure.|  
|[Classe d'événements Sort Warnings](../../relational-databases/event-classes/sort-warnings-event-class.md)|Indique que des opérations de tri ne peuvent pas être effectuées en mémoire.|  
|[Classe d'événements User Error Message](../../relational-databases/event-classes/user-error-message-event-class.md)|Affiche des messages d'erreur vus par l'utilisateur.|  
  
## <a name="see-also"></a> Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
