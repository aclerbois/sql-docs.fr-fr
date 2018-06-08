---
title: PreConnect:Completed, classe d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- PreConnect:Completed Event Class
ms.assetid: 7ed2f620-6511-4985-9961-d2927c2b1759
caps.latest.revision: 18
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d46e07875a1f54cfb665128fd37be55fc45a3ede
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="preconnectcompleted-event-class"></a>PreConnect:Completed, classe d’événements
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La classe d’événements PreConnect:Completed indique quand un déclencheur LOGON ou la fonction classifieur de Resource Governor finit de s’exécuter.  
  
## <a name="preconnectcompleted-event-class-data-columns"></a>Colonnes de données de la classe d'événements PreConnect:Completed  
  
|Nom de la colonne de données|Type de données|Description|ID de la colonne|Filtrable|  
|----------------------|---------------|-----------------|---------------|----------------|  
|EventClass|**Int**|216|27|non|  
|SPID|**Int**|ID du processus serveur qui déclenche cet événement.|12|Oui|  
|EventSubClass|**Int**|1 pour la fonction classifieur définie par l'utilisateur.|21|Oui|  
|StartTime|**datetime**|Heure de démarrage de la fonction classifieur définie par l'utilisateur.|14|Oui|  
|EndTime|**datetime**|Heure de démarrage de la fonction classifieur définie par l'utilisateur.|15|Oui|  
|Duration|**bigint**|Temps en microsecondes utilisé par la fonction classifieur.|13|Oui|  
|ObjectID|**Int**|ID de l'objet classifieur défini par l'utilisateur.|22|Oui|  
|Unité centrale|**Int**|Utilisation de l'UC en millisecondes.|18|Oui|  
|Reads|**Int**|Nombre de lectures logiques.|16|Oui|  
|Writes|**Int**|Nombre d'écritures logiques.|17|Oui|  
|GroupID|**Int**|ID du groupe de charges de travail classifié.|66|Oui|  
|Error|**Int**|Dernier numéro d'erreur si la fonction classifieur définie par l'utilisateur ne peut pas s'exécuter.|31|Oui|  
|État|**Int**|État de la dernière erreur.|30|Oui|  
|TargetUserName|**sysname**|Valeur de retour (nom de groupe de charges de travail) pour la fonction classifieur définie par l'utilisateur si le système ne peut pas trouver un groupe actif correspondant. Dans le cas contraire, cette colonne a la valeur NULL.|39|Oui|  
|ObjectName|**nvarchar (256)**|Nom en deux parties de la fonction classifieur définie par l'utilisateur. Par exemple, dbo.classifier.|34|Oui|  
  
## <a name="see-also"></a> Voir aussi  
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [PreConnect:Starting, classe d'événements](../../relational-databases/event-classes/preconnect-starting-event-class.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)  
  
  
