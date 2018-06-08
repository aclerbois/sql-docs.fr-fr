---
title: + (Plus unaire) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- Azure SQL Database
- SQL Server (starting with 2008)
f1_keywords:
- + (positive)
- +
- positive
dev_langs:
- TSQL
helpviewer_keywords:
- + (positive operator)
- positive operator (+)
- positive values [SQL Server]
ms.assetid: 0f31c5cc-3078-4f6a-9870-7eb1a98053fb
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 9ce8eabbf2cf1d585ffc68e0904d2e44a5add04f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unary-operators---positive"></a>Opérateurs unaires - Positif
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Retourne la valeur d'une expression numérique (un opérateur unaire). Les opérateurs unaires effectuent une opération sur une seule expression de n'importe quel type de données de la catégorie des types numériques.   
  
|Opérateur|Signification|  
|--------------|-------------|  
|[+ (Positif)](../../t-sql/language-elements/unary-operators-positive.md)|La valeur numérique est positive.|  
|[- (Négatif)](../../t-sql/language-elements/unary-operators-negative.md)|La valeur numérique est négative.|  
|[~ (NOT au niveau du bit)](../../t-sql/language-elements/bitwise-not-transact-sql.md)|Renvoie le complément à un du nombre.|  
  
 Les opérateurs + (positif) et - (négatif) peuvent s'utiliser dans toute expression de n'importe quel type de données de la catégorie des types numériques. L'opérateur ~ (NOT au niveau du bit) ne peut s'utiliser que dans des expressions dont le type de données appartient à la catégorie des types entiers.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
+ numeric_expression  
```  
  
## <a name="arguments"></a>Arguments  
 *numeric_expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de l’un des types de données de la catégorie numérique, sauf les types de données **datetime** et **smalldatetime**.  
  
## <a name="result-types"></a>Types des résultats  
 Retourne le type de données *numeric_expression*.  
  
## <a name="remarks"></a>Notes   
 Bien qu'un plus unaire puisse apparaître avant n'importe quelle expression, il n'effectue aucune opération sur la valeur retournée de l'expression. Plus précisément, il ne retourne pas la valeur positive d'une expression négative. Pour retourner la valeur positive d’une expression négative, utilisez la fonction [ABS](../../t-sql/functions/abs-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-a-variable-to-a-positive-value"></a>A. Affectation d'une valeur positive à une variable  
 Cet exemple assigne à une variable une valeur positive.  
  
```  
DECLARE @MyNumber decimal(10,2);  
SET @MyNumber = +123.45;  
SELECT @MyNumber;  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
-----------   
123.45            
  
(1 row(s) affected)  
```  
  
### <a name="b-using-the-unary-plus-operator-with-a-negative-value"></a>B. Utilisation de l'opérateur unaire plus avec une valeur négative  
 Cet exemple montre l'utilisation du plus unaire avec une expression négative et la fonction ABS() sur la même expression négative. Le plus unaire n'affecte pas l'expression, mais la fonction ABS retourne la valeur positive de l'expression.  
  
```  
USE tempdb;  
GO  
DECLARE @Num1 int;  
SET @Num1 = -5;  
SELECT +@Num1, ABS(@Num1);  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
----------- -----------  
-5          5  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [ABS &#40;Transact-SQL&#41;](../../t-sql/functions/abs-transact-sql.md)  
  
  
