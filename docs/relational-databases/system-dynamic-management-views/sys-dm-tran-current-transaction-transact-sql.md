---
title: Sys.dm_tran_current_transaction (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_current_transaction
- sys.dm_tran_current_transaction_TSQL
- dm_tran_current_transaction_TSQL
- dm_tran_current_transaction
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_current_transaction dynamic management view
ms.assetid: 75d5697d-b390-4963-99b8-fa0b4244a40c
caps.latest.revision: 37
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5992091b644a2861a25218a6cf0b824f6d12a97d
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmtrancurrenttransaction-transact-sql"></a>sys.dm_tran_current_transaction (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une seule ligne qui affiche des informations sur l'état de la transaction dans la session active.  
  
> [!NOTE]  
>  Pour appeler cette de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_tran_current_transaction**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_tran_current_transaction  
```  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**transaction_id**|**bigint**|ID de transaction de l'instantané actif.|  
|**transaction_sequence_num**|**bigint**|Numéro de séquence de la transaction qui produit la version de l'enregistrement.|  
|**transaction_is_snapshot**|**bit**|État d'isolement de l'instantané. Cette valeur est 1 si la transaction a démarré sous l'isolement d'instantané. Dans le cas contraire, la valeur est 0.|  
|**first_snapshot_sequence_num**|**bigint**|Il s'agit du plus petit numéro de séquence des transactions qui étaient actives lors de la création d'un instantané. Lors de l'exécution, une transaction d'instantané prend un instantané de toutes les transactions actives présentes. Pour les transactions non liées à des instantanés, la valeur 0 est affichée dans cette colonne.|  
|**last_transaction_sequence_num**|**bigint**|Numéro de séquence global. Cette valeur représente le dernier numéro de séquence de transaction généré par le système.|  
|**first_useful_sequence_num**|**bigint**|Numéro de séquence global. Cette valeur représente le plus ancien numéro de séquence de la transaction qui possède des versions de ligne devant être conservées dans la banque des versions. Les versions de ligne qui ont été créées par des transactions antérieures peuvent être supprimées.|  
|**pdw_node_id**|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre un scénario de test dans lequel quatre transactions simultanées, chacune étant identifiée par un numéro de séquence de transaction, sont exécutées dans une base de données où les options ALLOW_SNAPSHOT_ISOLATION et READ_COMMITTED_SNAPSHOT sont définies à ON. Les transactions suivantes sont exécutées :  
  
-   XSN-57 est une opération Update exécutée avec le niveau d'isolement sérialisable.  
  
-   XSN-58 est identique à XSN-57.  
  
-   XSN-59 est une opération Select exécutée avec le niveau d'isolement d'instantané.  
  
-   XSN-60 est identique à XSN-59.  
  
 La requête suivante est exécutée dans l'étendue de chaque transaction.  
  
```  
SELECT   
    transaction_id  
   ,transaction_sequence_num  
   ,transaction_is_snapshot  
   ,first_snapshot_sequence_num  
   ,last_transaction_sequence_num  
   ,first_useful_sequence_num  
  FROM sys.dm_tran_current_transaction;  
```  
  
 Voici le résultat de XSN-59.  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9387                 59                       1                         
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
57                               61                        
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 La sortie indique que XSN-59 est une transaction d'instantané qui utilise XSN-57 comme première transaction active lorsque XSN-59 a démarré. Ceci signifie que XSN-59 lit les données validées par les transactions possédant un numéro de séquence de transaction inférieur à XSN-57.  
  
 Voici le résultat de XSN-57.  
  
```  
transaction_id       transaction_sequence_num transaction_is_snapshot  
-------------------- ------------------------ -----------------------  
9295                 57                       0  
  
first_snapshot_sequence_num last_transaction_sequence_num  
--------------------------- -----------------------------  
NULL                        61  
  
first_useful_sequence_num  
-------------------------  
57  
```  
  
 Étant donné que XSN-57 n'est pas une transaction d'instantané, `first_snapshot_sequence_num` est `NULL`.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


