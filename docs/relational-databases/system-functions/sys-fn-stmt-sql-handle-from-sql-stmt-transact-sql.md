---
title: Sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 6794e073-0895-4507-aba3-c3545acc843f
caps.latest.revision: 9
author: rothja
ms.author: jroth
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a109d7f23ad475fa9d8f1229be5011495f94354f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysfnstmtsqlhandlefromsqlstmt-transact-sql"></a>sys.fn_stmt_sql_handle_from_sql_stmt (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Obtient le **stmt_sql_handle** pour un [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction sous type de paramétrage (simple ou forcé) donné. Cela vous permet de faire référence à des requêtes stockées dans le magasin de requêtes à l’aide de leurs **stmt_sql_handle** lorsque vous connaissez leur texte.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.fn_stmt_sql_handle_from_sql_stmt   
(  
    'query_sql_text',   
    [ query_param_type   
) [;]  
```  
  
## <a name="arguments"></a>Arguments  
 *query_sql_text*  
 Est le texte dans le magasin de requêtes que vous souhaitez que le handle de la requête. *query_sql_text* est un **nvarchar (max)**, sans valeur par défaut.  
  
 *query_param_type*  
 Est le type de paramètre de la requête. *query_param_type* est un **tinyint**. Les valeurs possibles sont :  
  
-   NULL : valeur par défaut est 0  
  
-   0 – aucun  
  
-   1 = utilisateur  
  
-   2 – simple  
  
-   3 – forcé  
  
## <a name="columns-returned"></a>Colonnes retournées  
 Le tableau suivant répertorie les colonnes qui sys.fn_stmt_sql_handle_from_sql_stmt retourne.  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|**statement_sql_handle**|**varbinary(64)**|Handle SQL.|  
|**query_sql_text**|**nvarchar(max)**|Le texte de la [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction.|  
|**query_parameterization_type**|**tinyint**|Le type de paramétrage de requête.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
  
## <a name="permissions"></a>Autorisations  
 Requiert le **EXECUTE** sur la base de données, et **supprimer** autorisation sur les affichages catalogue du magasin de requête.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant exécute une instruction, puis utilise `sys.fn_stmt_sql_handle_from_sql_stmt` pour retourner le handle SQL de cette instruction.  
  
```  
SELECT * FROM sys.databases;   
SELECT * FROM sys.fn_stmt_sql_handle_from_sql_stmt('SELECT * FROM sys.databases', NULL);  
```  
  
 Utilisez la fonction pour mettre en corrélation les données de magasin de requêtes avec d’autres vues de gestion dynamique. L’exemple suivant :  
  
```  
SELECT qt.query_text_id, q.query_id, qt.query_sql_text, qt.statement_sql_handle,  
q.context_settings_id, qs.statement_context_id   
FROM sys.query_store_query_text AS qt  
JOIN sys.query_store_query AS q   
    ON qt.query_text_id = q.query_id  
CROSS APPLY sys.fn_stmt_sql_handle_from_sql_stmt (qt.query_sql_text, null) AS fn_handle_from_stmt  
JOIN sys.dm_exec_query_stats AS qs   
    ON fn_handle_from_stmt.statement_sql_handle = qs.statement_sql_handle;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_plan &#40;Transct-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_flush_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)   
 [sp_query_store_remove_query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [Affichages catalogue du Magasin des requêtes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Analyse des performances à l'aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
  
  
