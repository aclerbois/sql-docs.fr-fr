---
title: sys.dm_exec_sql_text (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_exec_sql_text
- sys.dm_exec_sql_text
- sys.dm_exec_sql_text_TSQL
- dm_exec_sql_text_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sql_text dynamic management function
ms.assetid: 61b8ad6a-bf80-490c-92db-58dfdff22a24
caps.latest.revision: 36
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 21b22b837cc4e46bdd5169b0c669e7dde74c029c
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecsqltext-transact-sql"></a>sys.dm_exec_sql_text (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne le texte de l’instruction SQL du lot qui est identifié par l’objet *sql_handle*. Cette fonction remplace la fonction système **fn_get_sql**.  
  
 
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_exec_sql_text(sql_handle | plan_handle)  
```  
  
## <a name="arguments"></a>Arguments  
*sql_handle*  
Handle SQL du lot à rechercher. *sql_handle* est **varbinary(64)**. *sql_handle* peut être obtenu à partir des objets de gestion dynamique suivants :  
  
-   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)  
  
-   [sys.dm_exec_xml_handles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)  
  
-   [sys.dm_exec_query_memory_grants](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)  
  
*plan_handle*  
Identifie de façon univoque un plan de requête pour un traitement en cache ou en cours d'exécution. *plan_handle* est **varbinary(64)**. *plan_handle* peut être obtenu à partir des objets de gestion dynamique suivants :  
  
-   [sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|ID de base de données.<br /><br /> Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.|  
|**objectid**|**int**|ID d’objet.<br /><br /> Est NULL pour les instructions SQL ad hoc et préparées.|  
|**number**|**smallint**|Pour une procédure stockée numérotée, cette colonne retourne le numéro de la procédure stockée. Pour plus d’informations, consultez [fonctionnalité sys.numbered_procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-numbered-procedures-transact-sql.md).<br /><br /> Est NULL pour les instructions SQL ad hoc et préparées.|  
|**Chiffré**|**bit**|1 = le texte SQL est chiffré.<br /><br /> 0 = le texte SQL n'est pas chiffré.|  
|**texte**|**nvarchar(max** **)**|Texte de la requête SQL.<br /><br /> NULL pour les objets chiffrés.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE` sur le serveur.  
  
## <a name="remarks"></a>Notes  
Pour les requêtes ad hoc, les handles SQL sont des valeurs de hachage en fonction du texte SQL qui est envoyé au serveur et peuvent provenir d’une base de données. 

Pour des objets de base de données tels que des procédures stockées, des déclencheurs ou des fonctions, les handles SQL sont dérivés de l'ID de la base de données, de l'ID de l'objet et du numéro de l'objet. 

Descripteur de plan est une valeur de hachage dérivée du plan compilé du lot entier. 

> [!NOTE]
> **dbid** ne peut pas être déterminé à partir de *sql_handle* pour les requêtes ad hoc. Pour déterminer **dbid** pour les requêtes ad hoc, utilisez *plan_handle* à la place.
  
## <a name="examples"></a>Exemples 

### <a name="a-conceptual-example"></a>A. Exemple conceptuel
Voici un exemple pour illustrer le passage de base un **sql_handle** soit directement, soit via **CROSS APPLY**.
  1.  Créer l’activité.  
Exécutez le code T-SQL suivant dans une nouvelle fenêtre de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].   
      ```sql
      -- Identify current spid (session_id)
      SELECT @@SPID;
      GO
  
      -- Create activity
        WAITFOR DELAY '00:02:00';
      ```
      
    2.  À l’aide de **CROSS APPLY**.  
    La colonne sql_handle de **sys.dm_exec_requests** seront passés à **sys.dm_exec_sql_text** à l’aide de **CROSS APPLY**. Ouvrez une nouvelle fenêtre de requête et passer le spid identifié à l’étape 1. Dans cet exemple, le spid se trouve être `59`.

        ```sql
        SELECT t.*
        FROM sys.dm_exec_requests AS r
        CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) AS t
        WHERE session_id = 59 -- modify this value with your actual spid
         ```      
 
    2.  Passage **sql_handle** directement.  
Acquérir le **sql_handle** de **sys.dm_exec_requests**. Ensuite, passez le **sql_handle** directement à **sys.dm_exec_sql_text**. Ouvrez une nouvelle fenêtre de requête et de passer le spid identifié à l’étape 1 pour **sys.dm_exec_requests**. Dans cet exemple, le spid se trouve être `59`. Passez ensuite retourné **sql_handle** en tant qu’argument à **sys.dm_exec_sql_text**.

        ```sql
        -- acquire sql_handle
        SELECT sql_handle FROM sys.dm_exec_requests WHERE session_id = 59  -- modify this value with your actual spid
        
        -- pass sql_handle to sys.dm_exec_sql_text
        SELECT * FROM sys.dm_exec_sql_text(0x01000600B74C2A1300D2582A2100000000000000000000000000000000000000000000000000000000000000) -- modify this value with your actual sql_handle
         ```      
    
  
### <a name="b-obtain-information-about-the-top-five-queries-by-average-cpu-time"></a>B. Obtenir des informations sur les cinq premières requêtes par temps processeur moyen  
 L'exemple suivant retourne le texte de l'instruction SQL et le temps processeur moyen pour les cinq premières requêtes.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
    SUBSTRING(st.text, (qs.statement_start_offset/2)+1,   
        ((CASE qs.statement_end_offset  
          WHEN -1 THEN DATALENGTH(st.text)  
         ELSE qs.statement_end_offset  
         END - qs.statement_start_offset)/2) + 1) AS statement_text  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
ORDER BY total_worker_time/execution_count DESC;  
```  
  
### <a name="c-provide-batch-execution-statistics"></a>C. Fournir des statistiques d’exécution du lot  
 L'exemple suivant retourne le texte des requêtes SQL qui sont exécutées par traitements et fournit des informations statistiques à leur sujet.  
  
```sql  
SELECT s2.dbid,   
    s1.sql_handle,    
    (SELECT TOP 1 SUBSTRING(s2.text,statement_start_offset / 2+1 ,   
      ( (CASE WHEN statement_end_offset = -1   
         THEN (LEN(CONVERT(nvarchar(max),s2.text)) * 2)   
         ELSE statement_end_offset END)  - statement_start_offset) / 2+1))  AS sql_statement,  
    execution_count,   
    plan_generation_num,   
    last_execution_time,     
    total_worker_time,   
    last_worker_time,   
    min_worker_time,   
    max_worker_time,  
    total_physical_reads,   
    last_physical_reads,   
    min_physical_reads,    
    max_physical_reads,    
    total_logical_writes,   
    last_logical_writes,   
    min_logical_writes,   
    max_logical_writes    
FROM sys.dm_exec_query_stats AS s1   
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS s2    
WHERE s2.objectid is null   
ORDER BY s1.sql_handle, s1.statement_start_offset, s1.statement_end_offset;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [Sys.dm_exec_cursors &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)   
 [sys.dm_exec_xml_handles &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [À l’aide d’appliquer](../../t-sql/queries/from-transact-sql.md#using-apply)   
 [sys.dm_exec_text_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  

