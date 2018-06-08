---
title: Sys.dm_exec_input_buffer (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 10/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_exec_input_buffer
- sys.dm_exec_input_buffer _tsql
- dm_exec_input_buffer
- dm_exec_input_buffer_tsql
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_input_buffer dynamic management function
ms.assetid: fb34a560-bde9-4ad9-aa96-0d4baa4fc104
caps.latest.revision: 12
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: da708fb9606b5e5d52165680af8dff74a9201115
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmexecinputbuffer-transact-sql"></a>Sys.dm_exec_input_buffer (Transact-SQL)
[!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)]

  Retourne des informations sur les instructions envoyées à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_exec_input_buffer ( session_id , request_id )
```  
  
## <a name="arguments"></a>Arguments  
*session_id*  
L’id de session exécute le lot à rechercher. *session_id* est **smallint**. *session_id* peut être obtenu à partir des objets de gestion dynamique suivants :  
  
-   [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
-   [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)  
  
-   [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)   
  
*request_id*  
Request_id de [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md). *request_id* est **int**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**event_type**|**nvarchar (256)**|Le type d’événement dans le tampon d’entrée pour le spid donné.|  
|**parameters**|**smallint**|Tous les paramètres fournis pour l’instruction.|  
|**event_info**|**nvarchar(max)**|Le texte de l’instruction dans le tampon d’entrée pour le spid donné.|  
  
## <a name="permissions"></a>Autorisations  
 Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si l’utilisateur a l’autorisation VIEW SERVER STATE, l’utilisateur voit les sessions de tout en cours d’exécution sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; sinon, l’utilisateur voit uniquement la session active.  
  
 Sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)], si l’utilisateur est le propriétaire de la base de données, l’utilisateur s’affiche en cours d’exécution toutes les sessions sur le [!INCLUDE[ssSDS](../../includes/sssds-md.md)]; sinon, l’utilisateur voit uniquement la session active.  
  
## <a name="remarks"></a>Notes  
 Cette fonction de gestion dynamique peut être utilisée conjointement avec sys.dm_exec_sessions ou sys.dm_exec_requests faisant **CROSS APPLY**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example"></a>A. Exemple simple  
 L’exemple suivant illustre le passage d’un id de session (SPID) et un id de demande à la fonction.  
  
```sql  
SELECT * FROM sys.dm_exec_input_buffer (52, 0);
GO
```  
  
### <a name="b-using-cross-apply-to-additional-information"></a>B. À l’aide de s’appliquent à des informations supplémentaires  
 L’exemple suivant répertorie le tampon d’entrée pour les sessions avec l’id de session supérieur à 50.  
  
```sql  
SELECT es.session_id, ib.event_info   
FROM sys.dm_exec_sessions AS es  
CROSS APPLY sys.dm_exec_input_buffer(es.session_id, NULL) AS ib  
WHERE es.session_id > 50;
GO
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)  
