---
title: '@@ROWCOUNT (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- '@@ROWCOUNT_TSQL'
- '@@ROWCOUNT'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@ROWCOUNT function'
- number of rows affected by statement
- row affected by statements [SQL Server]
- statements [SQL Server], last statement
- counting rows
ms.assetid: 97a47998-81d9-4331-a244-9eb8b6fe4a56
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: dca98f41e55d0b55d7a3eb74725f3eeb9acec92a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="x40x40rowcount-transact-sql"></a>&#x40;&#x40;ROWCOUNT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne le nombre de lignes affectées par la dernière instruction. Si le nombre de lignes est supérieur à 2 milliards, utilisez [ROWCOUNT_BIG](../../t-sql/functions/rowcount-big-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
@@ROWCOUNT  
```  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
## <a name="remarks"></a>Notes   
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent définir la valeur de @@ROWCOUNT comme suit :  
  
-   En attribuant à @@ROWCOUNT le nombre de lignes affectées ou lues. Les lignes peuvent être, ou ne pas être, envoyées au client.  
  
-   En conservant la valeur de @@ROWCOUNT issue de l’exécution de l’instruction précédente.  
  
-   En réinitialisant @@ROWCOUNT à 0 sans renvoyer la valeur au client.  
  
 Les instructions qui réalisent une affectation simple attribuent toujours à @@ROWCOUNT la valeur 1. Aucune ligne n'est envoyée au client. Parmi ces instructions figurent SET @*local_variable*, RETURN, READTEXT, ainsi que les instructions de sélection sans requête telles que SELECT GETDATE() ou SELECT **'***Generic Text***'**.  
  
 Les instructions qui réalisent une affectation dans une requête ou utilisent RETURN dans une requête attribuent à @@ROWCOUNT le nombre de lignes affectées ou lues par la requête, par exemple SELECT @*local_variable* = c1 FROM t1.  
  
 Les instructions DML (Data Manipulation Language) attribuent à @@ROWCOUNT le nombre de lignes affectées par la requête et renvoient cette valeur au client. Les instructions DML peuvent ne pas envoyer de lignes au client.  
  
 DECLARE CURSOR et FETCH attribuent à @@ROWCOUNT la valeur 1.  
  
 Les instructions EXECUTE conservent la valeur @@ROWCOUNT précédente.  
  
 Les instructions telles que USE, SET \<option>, DEALLOCATE CURSOR, CLOSE CURSOR, BEGIN TRANSACTION ou COMMIT TRANSACTION réinitialisent la valeur ROWCOUNT à 0.  
  
 Les procédures stockées compilées en mode natif conservent la valeur @@ROWCOUNT précédente. Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à l’intérieur des procédures stockées compilées en mode natif ne définissent pas @@ROWCOUNT. Pour plus d’informations, consultez [Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant exécute une instruction `UPDATE` et utilise `@@ROWCOUNT` pour déterminer si des lignes ont été modifiées.  
  
```  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee   
SET JobTitle = N'Executive'  
WHERE NationalIDNumber = 123456789  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were updated';  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)  
  
  
