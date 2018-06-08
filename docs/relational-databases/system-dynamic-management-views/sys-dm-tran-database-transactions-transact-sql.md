---
title: Sys.dm_tran_database_transactions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_tran_database_transactions
- sys.dm_tran_database_transactions_TSQL
- dm_tran_database_transactions_TSQL
- sys.dm_tran_database_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_database_transactions dynamic management view
ms.assetid: 82a44295-4cbc-4a5b-891a-8ebaf307b8f5
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3054cc3cc8378e34b44d97720692b040e1b1bade
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmtrandatabasetransactions-transact-sql"></a>sys.dm_tran_database_transactions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne les informations concernant les transactions au niveau de la base de données.  
  
> [!NOTE]  
>  Pour appeler cette DMV à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_tran_database_transactions**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|ID de la transaction au niveau de l'instance, et non au niveau de la base de données. Il n'est unique que dans les bases de données d'une instance, pas dans toutes les instances du serveur.|  
|database_id|**int**|ID de la base de données associée à la transaction.|  
|database_transaction_begin_time|**datetime**|Heure à laquelle la base de données a été impliquée dans la transaction. Il s'agit plus précisément de l'heure du premier enregistrement de journal dans la base de données pour la transaction.|  
|database_transaction_type|**int**|1 = transaction en lecture/écriture<br /><br /> 2 = transaction en lecture seule<br /><br /> 3 = transaction système|  
|database_transaction_state|**int**|1 = la transaction n'a pas été initialisée.<br /><br /> 3 = la transaction a été initialisée, mais n'a produit aucun enregistrement de journal.<br /><br /> 4 = la transaction a produit des enregistrements de journal.<br /><br /> 5 = la transaction a été préparée.<br /><br /> 10 = la transaction a été validée.<br /><br /> 11 = la transaction a été restaurée.<br /><br /> 12 = la transaction est en cours de validation. (L’enregistrement du journal est généré, mais n'a pas été matérialisé ou persistant.)|  
|database_transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|database_transaction_log_record_count|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre d'enregistrements de journal produits dans la base de données pour la transaction.|  
|database_transaction_replicate_record_count|**int**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre d’enregistrements de journal générés dans la base de données de la transaction qui est répliquée.|  
|database_transaction_log_bytes_used|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre d'octets utilisés jusqu'alors dans la base de données pour la transaction.|  
|database_transaction_log_bytes_reserved|**bigint**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre d'octets réservés à l'utilisation du journal de la base de données pour la transaction.|  
|database_transaction_log_bytes_used_system|**int**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre d'octets utilisés jusqu'alors dans le journal de la base de données pour les transactions système pour le compte de la transaction.|  
|database_transaction_log_bytes_reserved_system|**int**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Nombre d'octets réservés à l'utilisation du journal de la base de données pour les transactions système pour le compte de la transaction.|  
|database_transaction_begin_lsn|**numeric(25,0)**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Numéro séquentiel dans le journal (NSE) du premier enregistrement concernant la transaction dans le journal de la base de données.|  
|database_transaction_last_lsn|**numeric(25,0)**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Numéro de séquence d'enregistrement (NSE) de l'enregistrement le plus récent concernant la transaction dans le journal de la base de données.|  
|database_transaction_most_recent_savepoint_lsn|**numeric(25,0)**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Numéro de séquence d'enregistrement du plus récent point d'enregistrement pour la transaction dans le journal de la base de données.|  
|database_transaction_commit_lsn|**numeric(25,0)**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Numéro de séquence d'enregistrement (NSE) de l'enregistrement du journal de validation concernant la transaction dans le journal de la base de données.|  
|database_transaction_last_rollback_lsn|**numeric(25,0)**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Numéro de séquence d'enregistrement utilisé la dernière fois pour une restauration. Si aucune restauration n’a eu lieu, la valeur est MaxLSN.|  
|database_transaction_next_undo_lsn|**numeric(25,0)**|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].<br /><br /> Numéro de séquence d'enregistrement du prochain enregistrement à annuler.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="see-also"></a>Voir aussi  
 [sys.dm_tran_active_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-transactions-transact-sql.md)   
 [sys.dm_tran_session_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


