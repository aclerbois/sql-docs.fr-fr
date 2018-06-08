---
title: sp_set_session_context (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- Azure SQL Database
- SQL Server 2016 Preview
f1_keywords:
- sp_set_session_context
- sp_set_session_context_TSQL
- sys.sp_set_session_context
- sys.sp_set_session_context_TSQL
helpviewer_keywords:
- sp_set_session_context
ms.assetid: 7a3a3b2a-1408-4767-a376-c690e3c1fc5b
caps.latest.revision: 7
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2d1396ef79eb69b96a40f075c50cd38b6ad77d24
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsetsessioncontext-transact-sql"></a>sp_set_session_context (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

Définit une paire clé-valeur dans le contexte de session.  
  

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_set_session_context [ @key= ] 'key', [ @value= ] 'value'  
    [ , [ @read_only = ] { 0 | 1 } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @key=] 'key'  
 La clé définie, de type **sysname**. La taille de clé maximale est de 128 octets.  
  
 [ @value=] 'value'  
 La valeur de la clé spécifiée, de type **sql_variant**. Définition d’une valeur null libère la mémoire. La taille maximale est de 8 000 octets.  
  
 [ @read_only= ] { 0 | 1 }  
 Un indicateur de type **bits**. La valeur 1, la valeur de la clé spécifiée ne peut pas être modifiée à nouveau sur cette connexion logique. Si 0 (valeur par défaut), alors que la valeur peut être modifiée.  
  
## <a name="permissions"></a>Autorisations  
 N’importe quel utilisateur peut définir un contexte de session pour leur session.  
  
## <a name="remarks"></a>Notes  
 Comme les autres procédures stockées, uniquement des littéraux et des variables (pas d’expressions ou des appels de fonction) peuvent être passés comme paramètres.  
  
 La taille totale du contexte de session est limitée à 256 Ko. Si vous définissez une valeur qui entraîne le dépassement de cette limite, l’instruction échoue. Vous pouvez surveiller l’utilisation mémoire globale dans [sys.dm_os_memory_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).  
  
 Vous pouvez surveiller l’utilisation de la mémoire globale en interrogeant [sys.dm_os_memory_cache_counters &#40;Transact-SQL&#41; ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md) comme suit : `SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT';`  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant montre comment définir et revenir ensuite une clé de contexte de sessions nommée language avec une valeur de l’anglais.  
  
```  
EXEC sp_set_session_context 'language', 'English';  
SELECT SESSION_CONTEXT(N'language');  
```  
  
 L’exemple suivant illustre l’utilisation de l’indicateur facultatif en lecture seule.  
  
```  
EXEC sp_set_session_context 'user_id', 4, @read_only = 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)   
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO  &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
