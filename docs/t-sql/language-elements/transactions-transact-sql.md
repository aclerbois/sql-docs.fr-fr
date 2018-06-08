---
title: Transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- Transactions
- Transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transactions [SQL Server]
- transactions [SQL Server], about transactions
- UOW [SQL Server]
- unit of work [SQL Server]
ms.assetid: 1485c375-921a-42af-a871-bb333cc08d3e
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 458f1a04b28db78b7931b08b27ad110be034c5e9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transactions-transact-sql"></a>Transactions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Une transaction est une unité de travail. Lorsqu'une transaction aboutit, toutes les modifications de données apportées lors de la transaction sont validées et intégrées de façon permanente à la base de données. Si une transaction rencontre des erreurs et doit être annulée ou restaurée, toutes les modifications de données sont supprimées.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les modes de transaction disponibles sont les suivants :  
  
 Transactions en mode de validation automatique  
 Chaque instruction individuelle est une transaction.  
  
 Transactions explicites  
 Chaque transaction est explicitement lancée par l'instruction BEGIN TRANSACTION et explicitement terminée par une instruction COMMIT ou ROLLBACK.  
  
 Transactions implicites  
 Une nouvelle transaction est implicitement lancée lorsque la transaction précédente est terminée, mais chaque transaction est explicitement terminée par une instruction COMMIT ou ROLLBACK.  
  
 Transaction dont l'étendue est définie par lot  
 Uniquement applicable aux ensembles de résultats MARS (Multiple Active Result Sets), une transaction [!INCLUDE[tsql](../../includes/tsql-md.md)] explicite ou implicite qui démarre sous une session MARS devient une transaction dont l'étendue est définie par traitement. Une transaction dont l'étendue est définie par traitement qui n'est pas validée ou restaurée à la fin du traitement est automatiquement restaurée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

> [!NOTE] 
> Pour les considérations spéciales relatives aux produits Data Warehouse, consultez [Transactions (SQL Data Warehouse)](transactions-sql-data-warehouse.md).   

## <a name="in-this-section"></a>Dans cette section  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit les instructions de transaction suivantes :  
  
|||  
|-|-|  
|[BEGIN DISTRIBUTED TRANSACTION](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)|[ROLLBACK TRANSACTION](../../t-sql/language-elements/rollback-transaction-transact-sql.md)|  
|[BEGIN TRANSACTION](../../t-sql/language-elements/begin-transaction-transact-sql.md)|[ROLLBACK WORK](../../t-sql/language-elements/rollback-work-transact-sql.md)|  
|[COMMIT TRANSACTION](../../t-sql/language-elements/commit-transaction-transact-sql.md)|[SAVE TRANSACTION](../../t-sql/language-elements/save-transaction-transact-sql.md)|  
|[COMMIT WORK](../../t-sql/language-elements/commit-work-transact-sql.md)||  
  
## <a name="see-also"></a> Voir aussi  
 [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
  
