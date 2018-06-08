---
title: Sys.dm_tran_session_transactions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_tran_session_transactions
- sys.dm_tran_session_transactions
- sys.dm_tran_session_transactions_TSQL
- dm_tran_session_transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_session_transactions dynamic management view
ms.assetid: c7157491-58c2-49fe-87d7-0c9723113adf
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: edc9d4295d6a5e02de73d6a7c7096e8bfd288324
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmtransessiontransactions-transact-sql"></a>sys.dm_tran_session_transactions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie les informations de corrélation des transactions et des sessions associées.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_tran_session_transactions**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|session_id|**int**|ID de la session dans laquelle la transaction est en cours d'exécution.|  
|transaction_id|**bigint**|ID de la transaction.|  
|transaction_descriptor|**binary(8)**|ID de la transaction utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la communication avec le pilote du client.|  
|enlist_count|**int**|Nombre de requêtes actives dans la session qui effectue la transaction.|  
|is_user_transaction|**bit**|1 = La transaction a été lancée par une requête utilisateur.<br /><br /> 0 = Transaction système.|  
|is_local|**bit**|1 = Transaction locale.<br /><br /> 0 = Transaction distribuée ou transaction sur une session liée par une inscription.|  
|is_enlisted|**bit**|1 = Transaction distribuée inscrite.<br /><br /> 0 = Transaction distribuée non inscrite.|  
|is_bound|**bit**|1 = La transaction est active sur la session par l'intermédiaire de sessions liées.<br /><br /> 0 = La transaction n'est pas active sur la session par l'intermédiaire de sessions liées.|  
|open_transaction_count||Le nombre de transactions ouvertes pour chaque session.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="remarks"></a>Notes  
 Par l'intermédiaire de sessions liées et distribuées, il est possible à une transaction de s'exécuter dans plusieurs sessions. Dans ces cas, les transactions sys.dm_tran_session_transactions affichent plusieurs lignes pour le même identificateur transaction_id : une pour chaque session dans laquelle la transaction s'exécute.  
  
 En exécutant plusieurs requêtes en mode validation automatique utilisant plusieurs ensembles de résultats actifs (MARS), il est possible d'avoir plusieurs transactions actives sur une seule session. Dans ces cas, les transactions sys.dm_tran_session_transactions affichent plusieurs lignes pour le même identificateur session_id : une pour chaque transaction qui s'exécute dans cette session.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


