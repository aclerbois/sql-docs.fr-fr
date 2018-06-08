---
title: MSmerge_identity_range_allocations (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSmerge_identity_range_allocations
- MSmerge_identity_range_allocations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_identity_range_allocations system table
ms.assetid: 6362e35e-0ab3-4638-855b-1ce013f5fd6d
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 9a7d2e628f8bd70b5e71b294b64674214dd2a0f0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmergeidentityrangeallocations-transact-sql"></a>MSmerge_identity_range_allocations (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_identity_range_allocations** table est utilisée pour effectuer le suivi de l’historique des affectations plage identité, à des éditeurs et les abonnés, pour les articles publiés. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|L’ID du serveur de publication.|  
|**publisher_db**|**nvarchar(128)**|Le nom de la base de données de publication.|  
|**Publication**|**nvarchar(128)**|Nom de la publication.|  
|**article**|**nvarchar(128)**|Le nom de l’article.|  
|**subscriber** (Abonné)|**nvarchar(128)**|Nom de l'Abonné.|  
|**bd_abonné**|**nvarchar(128)**|Le nom de la base de données d’abonnement.|  
|**is_pub_range**|**bit**|Indique si la plage d'identité est affectée à un serveur de publication.|  
|**ranges_allocated**|**tinyint**|Nombre de plages d'identité affectées.|  
|**range_begin**|**numeric(38)**|La valeur de départ de la plage.|  
|**range_end**|**numeric(38)**|La dernière valeur dans la plage.|  
|**next_range_begin**|**numeric(38)**|Valeur de début de la plage suivante à affecter.|  
|**next_range_end**|**numeric(38)**|Valeur de fin de la plage suivante à affecter.|  
|**max_used**|**numeric(38)**|Valeur d'identité la plus élevée utilisée.|  
|**time_of_allocation**|**datetime**|Heure à laquelle l'affectation a été faite.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
