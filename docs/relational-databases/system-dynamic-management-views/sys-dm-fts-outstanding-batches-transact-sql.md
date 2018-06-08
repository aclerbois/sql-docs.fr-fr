---
title: Sys.dm_fts_outstanding_batches (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_fts_outstanding_batches
- dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches
dev_langs:
- TSQL
helpviewer_keywords:
- troubleshooting [SQL Server], full-text search
- sys.dm_fts_outstanding_batches dynamic management view
ms.assetid: c4d697ed-c906-4c28-b137-036a25e13c84
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2eb67cfc9ae23c9779efdfccb436157b9a14349f
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmftsoutstandingbatches-transact-sql"></a>sys.dm_fts_outstanding_batches (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations à propos de chaque lot d'indexation de texte intégral.  
  
  |Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données|  
|catalog_id|**int**|ID du catalogue de texte intégral|  
|table_id|**int**|ID de l'ID de table qui contient l'index de texte intégral|  
|batch_id|**int**|ID de lot|  
|memory_address|**varbinary(8)**|Adresse mémoire de l'objet de lot|  
|crawl_memory_address|**varbinary(8)**|Adresse mémoire de l'objet d'analyse (objet parent)|  
|memregion_memory_address|**varbinary(8)**|Adresse mémoire de la région mémoire de la mémoire de partage sortante de l'hôte de démon de filtre (fdhost.exe)|  
|hr_batch|**int**|Code d'erreur le plus récent pour le lot|  
|is_retry_batch|**bit**|Indique s'il s'agit d'une nouvelle tentative de lot :<br /><br /> 0 = Non<br /><br /> 1 = Oui|  
|retry_hints|**int**|Type de nouvelle tentative nécessaire pour le lot :<br /><br /> 0 = pas de nouvelle tentative<br /><br /> 1 = nouvelle tentative multithread<br /><br /> 2 = nouvelle tentative à thread unique<br /><br /> 3 = nouvelle tentative à thread unique et multithread<br /><br /> 5 = dernière tentative multithread<br /><br /> 6 = dernière tentative à thread unique<br /><br /> 7 = dernière tentative à thread unique et multithread|  
|retry_hints_description|**nvarchar(120)**|Description du type de nouvelle tentative nécessaire :<br /><br /> pas de nouvelle tentative<br /><br /> nouvelle tentative multithread<br /><br /> nouvelle tentative à thread unique<br /><br /> nouvelle tentative à thread unique et multithread<br /><br /> dernière tentative multithread<br /><br /> dernière tentative à thread unique<br /><br /> dernière tentative à thread unique et multithread|  
|doc_failed|**bigint**|Nombre de documents ayant échoué dans le lot|  
|batch_timestamp|**timestamp**|Valeur d'horodatage obtenue lorsque le lot a été créé|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L'exemple suivant recherche le nombre de lots traités actuellement pour chaque table dans l'instance de serveur.  
  
```  
SELECT database_id, table_id, COUNT(*) AS batch_count FROM sys.dm_fts_outstanding_batches GROUP BY database_id, table_id ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)  
  
  
