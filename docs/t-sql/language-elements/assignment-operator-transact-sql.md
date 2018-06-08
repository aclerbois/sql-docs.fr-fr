---
title: = (Opérateur d’assignation) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], assignment
- assignment operators [Transact-SQL]
- headings [SQL Server columns]
- relationships [SQL Server], assignment operators
- column headings [SQL Server]
ms.assetid: c3040db6-21d6-40ac-a783-82c98ec006cc
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 51e8c7fb8f85eabe4f4e8fd30da6c95bf548e60f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="-assignment-operator-transact-sql"></a>= (Opérateur d’assignation) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all_md](../../includes/tsql-appliesto-ss2012-all-md.md)]

  Le signe égal (=) est le seul opérateur d'affectation de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Dans l'exemple qui suit, la variable `@MyCounter` est créée, puis l'opérateur d'affectation attribue à `@MyCounter` une valeur renvoyée par une expression.  
  
```  
DECLARE @MyCounter INT;  
SET @MyCounter = 1;  
```  
  
 L'opérateur d'affectation peut également être utilisé pour définir la relation entre l'en-tête d'une colonne et l'expression définissant les valeurs de la colonne. L'exemple suivant affiche les en-têtes de colonne `FirstColumnHeading` et `SecondColumnHeading`. La chaîne `xyz` s'affiche dans l'en-tête de colonne `FirstColumnHeading` pour toutes les lignes. Ensuite, chaque ID de produit extrait de la table `Product` est présenté dans l'en-tête de colonne `SecondColumnHeading`.  
  
```  
-- Uses AdventureWorks  
  
SELECT FirstColumnHeading = 'xyz',  
       SecondColumnHeading = ProductID  
FROM Production.Product;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
