---
title: Types d’attentes des groupes de disponibilité Always On (SQL Server) | Microsoft Docs
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: ''
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: afa8caff-f325-48d9-a8ef-a30beab60389
caps.latest.revision: 6
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 1eee6003c8697be620fd69a0dc137d12c14620b8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="always-on-availability-groups-wait-types"></a>Types d’attentes des groupes de disponibilité Always On
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Quand vous tentez de résoudre les problèmes de latence des groupes de disponibilité Always On, vous pouvez monitorer l’accumulation des statistiques d’attente à l’aide des types d’attentes spécifiques aux groupes de disponibilité dans la DMV (vue de gestion dynamique) [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
 Pour obtenir des informations générales sur l’utilisation des statistiques d’attente, consultez [Attentes et files d’attente dans SQL Server 2005](https://technet.microsoft.com/library/cc966413.aspx). Ce document a été écrit pour SQL Server 2005, mais les informations qu’il contient s’appliquent aux versions ultérieures de SQL Server.  
  
## <a name="query-for-availability-groups-wait-types"></a>Requête permettant d’obtenir les types d’attentes des groupes de disponibilité  
 Utilisez la requête T-SQL ci-dessous pour récupérer toutes les statistiques d’attente avec les types d’attentes des groupes de disponibilité :  
  
```sql  
SELECT * FROM sys.dm_os_wait_stats   
WHERE wait_type LIKE '%hadr%'  
ORDER BY wait_time_ms DESC  
```  
  
 Pour monitorer les statistiques d’attente en capturant des événements étendus, utilisez la commande T-SQL suivante.  
  
```sql
CREATE EVENT SESSION [alwayson] ON SERVER   
ADD EVENT sqlos.wait_info(  
    WHERE ([wait_type]=(758) OR [wait_type]=(776) OR [wait_type]=(853) OR [wait_type]=(833)))  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,  
MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)  
GO  
```  
  
 Pour afficher le mappage clé-valeur du type d’attente, exécutez la requête suivante :  
  
```sql
SELECT * FROM sys.dm_xe_map_values   
WHERE name='wait_types' AND map_value LIKE '%hadr%'   
ORDER BY map_key ASC  
```  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Types d’attentes](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md#WaitTypes)  
  
  