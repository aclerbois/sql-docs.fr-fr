---
title: MSsubscription_articles (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 03/03/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-tables
ms.reviewer: 
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: 
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSsubscription_articles
- MSsubscription_articles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_articles system table
ms.assetid: dbc1737f-261e-4017-b9cd-703b9fc4ac78
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 8c2b344c12cc7bd13ee9782c8ae1ebafa91a6c17
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="mssubscriptionarticles-transact-sql"></a>MSsubscription_articles (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSsubscription_articles** table contient des informations sur les articles d’un abonnement en file d’attente. La table est remplie seulement pour les types de réplications Mise à jour en attente et Mise à jour immédiate avec la mise à jour en file d'attente comme mode de basculement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**éléments agent_id**|**int**|ID de l'Agent qui fournit des services à cet article.|  
|**artid**|**int**|L’ID d’article de la **sysarticles** table.|  
|**article**|**sysname**|Le nom de l’article à partir de la **sysarticles** table.|  
|**dest_table**|**sysname**|Le nom de la table de destination à partir de la **sysarticles** table.|  
|**propriétaire**|**sysname**|Propriétaire de l'abonnement.|  
|**cft_table**|**sysname**|Nom de la table de conflits de l'article pour le type de réplication Mise à jour en attente.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40; Transact-SQL &#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
