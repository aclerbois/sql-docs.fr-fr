---
title: Curseurs XTP SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 84bf4654-3ef7-4d7f-a269-c8bb4ed4acad
caps.latest.revision: 6
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 0da2434b2f8b50e06a667129d2de1076341a587e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-xtp-cursors"></a>Curseurs XTP SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  L’objet de performance Curseurs XTP SQL Server contient des compteurs liés aux curseurs internes du moteur OLTP en mémoire. Les curseurs constituent les blocs de construction de bas niveau que le moteur OLTP en mémoire utilise pour traiter les requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] . Par conséquent, vous ne pouvez pas les contrôler directement.  
  
 Ce tableau décrit les compteurs **Curseurs XTP SQL Server** .  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Suppressions des curseurs par seconde**|Nombre de suppressions des curseurs (en moyenne), par seconde.|  
|**Insertions des curseurs par seconde**|Nombre d'insertions des curseurs (en moyenne), par seconde.|  
|**Analyses des curseurs démarrées par seconde**|Nombre d'analyses des curseurs démarrées (en moyenne), par seconde.|  
|**Violations de contrainte unique des curseurs par seconde**|Nombre de violations de contrainte unique (en moyenne), par seconde.|  
|**Mises à jour des curseurs par seconde**|Nombre de mises à jour des curseurs (en moyenne), par seconde.|  
|**Conflits d'écriture des curseurs par seconde**|Nombre de conflits d'écriture-écriture vers la même version de ligne (en moyenne), par seconde.|  
|**Nouvelles tentatives d'analyse d'angles inutilisés par seconde (sortie utilisateur)**|Nombre de tentatives d'analyse dues à des conflits d'écriture lors des nettoyages d'angles inutilisés indiqué par une analyse de table complète de l'utilisateur (en moyenne), par seconde. Il s'agit d'un compteur de très bas niveau, non destiné au client.|  
|**Lignes expirées supprimées par seconde**|Nombre de lignes expirées supprimées par les curseurs (en moyenne), par seconde.|  
|**Lignes expirées touchées par seconde**|Nombre de lignes expirées touchées par les curseurs (en moyenne), par seconde.|  
|**Lignes retournées par seconde**|Nombre de lignes retournées par les curseurs (en moyenne), par seconde.|  
|**Lignes touchées par seconde**|Nombre de lignes touchées par les curseurs (en moyenne), par seconde.|  
|**Lignes en cours de suppression, touchées par seconde**|Nombre de lignes expirant touchées par les curseurs (en moyenne), par seconde. Une ligne expire si la transaction qui l’a supprimée est toujours active (c’est-à-dire qu’elle n’a pas encore été validée ou annulée.)|  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server XTP &#40;OLTP en mémoire&#41;, compteurs de performances](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
