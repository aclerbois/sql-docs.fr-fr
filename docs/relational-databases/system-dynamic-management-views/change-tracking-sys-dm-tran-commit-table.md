---
title: Sys.dm_tran_commit_table (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_tran_commit_table
- dm_tran_commit_table_TSQL
- sys.dm_tran_commit_table
- sys.dm_tran_commit_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_commit_table dynamic management view
ms.assetid: 732d23c5-1f6c-4e96-bc85-8f29b520cf0e
caps.latest.revision: 17
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e8eb01a2be2c65905cc2c97fb703c2a673d52d86
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="change-tracking---sysdmtrancommittable"></a>Suivi - sys.dm_tran_commit_table
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Affiche une ligne pour chaque transaction validée pour une table suivie par le suivi des modifications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La vue de gestion sys.dm_tran_commit_table, fournie à des fins de prise en charge, expose les informations relatives aux transactions que le suivi des modifications stocke dans la table système sys.syscommittab. La table sys.syscommittab fournit un mappage persistant efficace d'un ID de transaction spécifique à la base de données au numéro séquentiel dans le journal de validation (LSN) et l'horodateur de validation de la transaction. Les données stockées dans la table sys.syscommittab et exposées dans cette vue de gestion sont soumises au nettoyage en fonction de la période de rétention spécifiée lors de la configuration du suivi des modifications.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_tran_commit_table**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|commit_ts|**bigint**|Nombre à croissance monotone servant d'horodateur spécifique à la base de données pour chaque transaction validée.|  
|xdes_id|**bigint**|ID interne spécifique à la base de données pour la transaction.|  
|commit_lbn|**bigint**|Numéro du bloc de journal qui contient l'enregistrement du journal de validation de la transaction.|  
|commit_csn|**bigint**|Numéro séquentiel de validation spécifique à l'instance pour la transaction.|  
|commit_time|**smalldatetime**|Heure de la validation de la transaction.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [À propos du suivi des modifications &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
  
  


