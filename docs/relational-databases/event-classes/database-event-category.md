---
title: Base de données, catégorie d’événement | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- event classes [SQL Server], Database event category
- Database event category [SQL Server]
- SQL Server event classes, Database event category
ms.assetid: b61af738-f144-4992-b0b2-d44cb7240991
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e5e7163438232987bc15cd655612da23653d6e59
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="database-event-category"></a>Catégorie d'événement Base de données
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La catégorie d’événement **Base de données** contient des classes d’événements pour surveiller le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Classe d'événements Data File Auto Grow](../../relational-databases/event-classes/data-file-auto-grow-event-class.md)|Indique que la taille du fichier de données a augmenté automatiquement. Cet événement n'est pas déclenché si la taille du fichier de données augmente de manière explicite par le biais d'ALTER DATABASE.|  
|[Data File Auto Shrink, classe d’événements](../../relational-databases/event-classes/data-file-auto-shrink-event-class.md)|Indique que la taille du fichier de données a diminué.|  
|[Classe d'événements de connexion de mise en miroir de bases de données](../../relational-databases/event-classes/database-mirroring-connection-event-class.md)|Événement généré pour signaler l'état d'une connexion de transport de la mise en miroir de bases de données.|  
|[Classe d'événements Database Mirroring State Change](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md)|Indique l'état des modifications de la base de données en miroir.|  
|[Classe d'événements Database Suspect Data Page](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)|Indique qu’une page est ajoutée à la table **suspect_pages** dans la base de données **msdb** .|  
|[Classe d'événements Log File Auto Grow](../../relational-databases/event-classes/log-file-auto-grow-event-class.md)|Indique que la taille du fichier journal croît automatiquement. Cet événement n'est pas déclenché si le fichier journal a été augmenté de manière explicite par le biais d'ALTER DATABASE.|  
|[Log File Auto Shrink, classe d’événements](../../relational-databases/event-classes/log-file-auto-shrink-event-class.md)|Indique que la taille du fichier journal croît automatiquement. Cet événement n'est pas déclenché si la taille du fichier journal diminue explicitement par le biais d'ALTER DATABASE.|  
  
## <a name="see-also"></a> Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
