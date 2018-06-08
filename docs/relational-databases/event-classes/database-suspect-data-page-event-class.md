---
title: Database Suspect Data Page, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- suspect_pages system table
- database mirroring [SQL Server], event notifications
- Database Suspect Data Page event class
ms.assetid: 098e1443-a8a0-425c-9311-0a479b1370ed
caps.latest.revision: 11
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 02fc3d275021a2c83bfe4573a0a462d0490e2a97
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="database-suspect-data-page-event-class"></a>classe d'événements Database Suspect Data Page
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La classe d’événements **Database Suspect Data Page** indique qu’une page a été ajoutée à la table [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) dans la base de données [msdb](../../relational-databases/databases/msdb-database.md). Incluez cette classe d'événements dans les traces qui surveillent l'occurrence de pages suspectes.  
  
> [!NOTE]  
>  Cet événement est émis de façon asynchrone à partir de l’insertion d’une ligne correspondante dans la table **suspect_pages** . Par conséquent, un travail à l’écoute de cet événement peut ne pas trouver immédiatement l’entrée **suspect_pages** correspondante.  
  
 Lorsque la classe d'événements **Database Suspect Data Page** est incluse au sein d’une trace, la surcharge relative est peu élevée. La surcharge peut être plus élevée si le nombre de pages suspectes augmente, par exemple, si un lecteur de disque rencontre des problèmes.  
  
## <a name="database-suspect-data-page-event-class-data-columns"></a>Colonnes de données de la classe d'événements Database Suspect Data Page  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**Int**|ID de la base de données pour laquelle l'événement de page suspecte a été déclenché. Il correspond à la valeur de la colonne **database_id** de la table **suspect_pages** .|3|Oui|  
|**EventClass**|**Int**|Le type de l'événement est 213.|27|non|  
|**EventSequence**|**Int**|Séquence de la classe d'événements dans le lot.|51|non|  
|**SPID**|**Int**|ID de la tâche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui a rencontré la page suspecte.|12|Oui|  
|**StartTime**|**datetime**|Heure à laquelle l'événement s'est produit.|14|Oui|  
|**ObjectID**|**Int**|ID du fichier de base de données qui contient la page suspecte. Il correspond à la valeur de la colonne **file_id** de la table **suspect_pages** .|22|Oui|  
|**ObjectID2**|**Int**|ID de la page suspecte dans le fichier. Il correspond à la valeur de la colonne **page_id** de la table **suspect_pages** .|56|Oui|  
|**Erreur**|**Int**|Type de l’erreur rencontrée. Il s’agit de la même valeur que celle de l’élément **event_type** de la page dans la table **suspect_pages** .|31|Oui|  
  
## <a name="see-also"></a> Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Gérer la table suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  
