---
title: '@@TRANCOUNT (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- '@@TRANCOUNT_TSQL'
- '@@TRANCOUNT'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@TRANCOUNT function'
- number of active transactions
- connections [SQL Server], active transactions
- active transactions
ms.assetid: b2638410-e410-4bd0-9b54-90096182b2b6
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 296c3e9eb2a2c6789377442ed02c16278de37062
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="x40x40trancount-transact-sql"></a>&#x40;&#x40;TRANCOUNT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne le nombre d'instructions BEGIN TRANSACTION qui se sont produites sur la connexion actuelle.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
@@TRANCOUNT  
```  
  
## <a name="return-types"></a>Types de retour  
 **entier**  
  
## <a name="remarks"></a>Notes   
 L’instruction BEGIN TRANSACTION incrémente @@TRANCOUNT de 1. ROLLBACK TRANSACTION décrémente @@TRANCOUNT à 0, à l’exception de ROLLBACK TRANSACTION *savepoint_name*, qui n’affecte pas @@TRANCOUNT. COMMIT TRANSACTION ou COMMIT WORK décrémente @@TRANCOUNT de 1.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-showing-the-effects-of-the-begin-and-commit-statements"></a>A. Illustration des effets des instructions BEGIN et COMMIT  
 L'exemple suivant illustre l'effet des instructions `BEGIN` et `COMMIT` imbriqués sur la variable `@@TRANCOUNT`.  
  
```  
PRINT @@TRANCOUNT  
--  The BEGIN TRAN statement will increment the  
--  transaction count by 1.  
BEGIN TRAN  
    PRINT @@TRANCOUNT  
    BEGIN TRAN  
        PRINT @@TRANCOUNT  
--  The COMMIT statement will decrement the transaction count by 1.  
    COMMIT  
    PRINT @@TRANCOUNT  
COMMIT  
PRINT @@TRANCOUNT  
--Results  
--0  
--1  
--2  
--1  
--0  
```  
  
### <a name="b-showing-the-effects-of-the-begin-and-rollback-statements"></a>B. Illustration des effets des instructions BEGIN et ROLLBACK  
 L'exemple suivant illustre l'effet des instructions `BEGIN TRAN` et `ROLLBACK` imbriqués sur la variable `@@TRANCOUNT`.  
  
```  
PRINT @@TRANCOUNT  
--  The BEGIN TRAN statement will increment the  
--  transaction count by 1.  
BEGIN TRAN  
    PRINT @@TRANCOUNT  
    BEGIN TRAN  
        PRINT @@TRANCOUNT  
--  The ROLLBACK statement will clear the @@TRANCOUNT variable  
--  to 0 because all active transactions will be rolled back.  
ROLLBACK  
PRINT @@TRANCOUNT  
--Results  
--0  
--1  
--2  
--0  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)  
  
  
