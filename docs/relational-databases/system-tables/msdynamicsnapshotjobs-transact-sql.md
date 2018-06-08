---
title: MSdynamicsnapshotjobs (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
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
- MSdynamicsnapshotjobs_TSQL
- MSdynamicsnapshotjobs
dev_langs:
- TSQL
helpviewer_keywords:
- MSdynamicsnapshotjobs system table
ms.assetid: 4f36a325-0e3c-46c4-aeeb-416346cce0bc
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 8cdfc74bad45bcb3e77bfdf8b438df45e1491633
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msdynamicsnapshotjobs-transact-sql"></a>MSdynamicsnapshotjobs (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSdynamicsnapshotjobs** table effectue le suivi des informations sur les filtres de lignes paramétrable appliquées pour générer un instantané de données filtrées. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de la tâche d'instantané des données filtrées.|  
|**nom**|**sysname**|Nom de la tâche d'instantané des données filtrées.|  
|**pubid**|**uniqueidentifier**|Numéro d'identification unique de cette publication.|  
|**job_id**|**uniqueidentifier**|L’ID du travail de l’Agent SQL Server sur le serveur de distribution.|  
|**agent_id**|**int**|ID de l’Agent SQL Server.|  
|**dynamic_filter_login**|**sysname**|La valeur utilisée pour évaluer la [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) fonction dans les filtres de lignes paramétrable défini pour la publication.|  
|**dynamic_filter_hostname**|**sysname**|La valeur utilisée pour évaluer la [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) fonction dans les filtres de lignes paramétrable défini pour la publication.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Chemin d'accès au dossier à partir duquel les fichiers d'instantané sont lus en cas d'utilisation d'un instantané de données filtrées.|  
|**partition_id**|**int**|ID de la partition de données à laquelle la tâche appartient.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
