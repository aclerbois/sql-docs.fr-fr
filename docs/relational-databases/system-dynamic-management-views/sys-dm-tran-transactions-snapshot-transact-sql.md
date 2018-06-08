---
title: Sys.dm_tran_transactions_snapshot (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_transactions_snapshot
- dm_tran_transactions_snapshot
- sys.dm_tran_transactions_snapshot_TSQL
- dm_tran_transactions_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_transactions_snapshot dynamic management view
ms.assetid: 03f64883-07ad-4092-8be0-31973348c647
caps.latest.revision: 37
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7e862da4552d605993a5dde7913fe1bec5856603
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmtrantransactionssnapshot-transact-sql"></a>sys.dm_tran_transactions_snapshot (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une table virtuelle pour le **sequence_number** de transactions sont actives lors de chaque transaction d’instantané démarre. Les informations retournées par cette vue peuvent vous être utiles pour les opérations suivantes :  
  
-   Rechercher le nombre de transactions d'instantané actives.  
  
-   Identifier les modifications de données qui sont ignorées par une transaction d'instantané spécifique. Pour une transaction qui est active lorsqu'une transaction d'instantané démarre, toutes les modifications de données effectuées par cette transaction (même après sa validation) sont ignorées par la transaction d'instantané.  
  
 Par exemple, considérez la sortie suivante de **sys.dm_tran_transactions_snapshot**:  
  
```  
transaction_sequence_num snapshot_id snapshot_sequence_num  
------------------------ ----------- ---------------------  
59                       0           57  
59                       0           58  
60                       0           57  
60                       0           58  
60                       0           59  
60                       3           57  
60                       3           58  
60                       3           59  
60                       3           60  
```  
  
 La colonne `transaction_sequence_num` identifie le numéro de séquence de transaction (XSN) des transactions d'instantané actives. Le résultat indique deux valeurs : `59` et `60`. La colonne `snapshot_sequence_num` identifie le numéro de séquence des transactions qui sont actives au moment où chaque transaction d'instantané démarre.  
  
 Le résultat indique que la transaction d'instantané XSN-59 démarre alors que deux transactions actives, XSN-57 et XSN-58, sont en cours d'exécution. Si XSN-57 ou XSN-58 effectue des modifications de données, XSN-59 ignore les changements et utilise le contrôle de version de ligne pour assurer une vue cohérente, d'un point de vue transactionnel, de la base de données.  
  
 La transaction d'instantané XSN-60 ignore les modifications de données apportées par XSN-57, XSN-58 et également XSN 59.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
dm_tran_transactions_snapshot  
```  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Numéro de séquence (XSN) d'une transaction d'instantané.|  
|**snapshot_id**|**int**|Identificateur d'instantané pour chaque instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] lancée en mode de lecture validée à l'aide du contrôle de version de ligne. Cette valeur permet de générer une vue cohérente, d'un point de vue transactionnel, de la base de données prenant en charge chaque requête qui est exécutée en mode de lecture validée à l'aide du contrôle de version de ligne.|  
|**snapshot_sequence_num**|**bigint**|Numéro de séquence d'une transaction qui était active lorsque la transaction d'instantané a commencé.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="remarks"></a>Notes  
 Lorsqu'une transaction d'instantané démarre, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] enregistre toutes les transactions qui sont actives à ce moment précis. **Sys.dm_tran_transactions_snapshot** fournit ces informations pour toutes les transactions d’instantané actuellement actives.  
  
 Chaque transaction est identifiée par un numéro de séquence qui lui est affecté au moment où elle commence. Les transactions commencent au moment où une instruction BEGIN TRANSACTION ou BEGIN WORK est exécutée. Toutefois, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] affecte la séquence de transaction numérique avec l’exécution de la première [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction qui accède aux données après l’instruction BEGIN TRANSACTION ou BEGIN WORK. Les numéros de séquence des transactions sont incrémentés d'une unité à la fois.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  

