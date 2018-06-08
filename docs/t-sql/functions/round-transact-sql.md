---
title: ROUND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ROUND_TSQL
- ROUND
dev_langs:
- TSQL
helpviewer_keywords:
- rounding expressions
- ROUND function [Transact-SQL]
ms.assetid: 23921ed6-dd6a-4c9e-8c32-91c0d44fe4b7
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 40be3c733cdaf83cdc3aa747aef4015aa8176aa7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="round-transact-sql"></a>ROUND (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne une valeur numérique, arrondie à la longueur ou à la précision indiquée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ROUND ( numeric_expression , length [ ,function ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *numeric_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de la catégorie de type de données numérique exacte ou approximative, à l’exception du type de données **bit**.  
  
 *length*  
 Précision avec laquelle l’expression *numeric_expression* doit être arrondie. *length* doit être une expression de type **tinyint**, **smallint** ou **int**. Quand *length* est un nombre positif, l’expression *numeric_expression* est arrondie au nombre de décimales indiqué par *length*. Quand *length* est un nombre négatif, l’expression *numeric_expression* est arrondie à gauche de la virgule décimale, comme indiqué par *length*.  
  
 *function*  
 Type d'opération à effectuer. *function* doit être de type **tinyint**, **smallint** ou **int**. Lorsque *function* est omis ou a la valeur 0 (valeur par défaut), l’expression *numeric_expression* est arrondie. Lorsqu’une valeur autre que 0 est spécifiée, l’expression *numeric_expression* est tronquée.  
  
## <a name="return-types"></a>Types de retour  
 Retourne les types de données suivants.  
  
|Résultat de l'expression|Type de retour|  
|-----------------------|-----------------|  
|**tinyint**|**Int**|  
|**smallint**|**Int**|  
|**Int**|**Int**|  
|**bigint**|**bigint**|  
|Catégorie **decimal** et **numeric** (p, s)|**decimal(p, s)**|  
|Catégorie **money** et **smallmoney**|**money**|  
|Catégorie **float** et **real**|**float**|  
  
## <a name="remarks"></a>Notes   
 ROUND retourne toujours une valeur. Si *length* est une valeur négative supérieure au nombre de chiffres placés avant la virgule décimale, ROUND retourne 0.  
  
| Exemple|Résultats|  
|-------------|------------|  
|ROUND(748.58, -4)|0|  
  
 ROUND retourne une expression *numeric_expression* arrondie, quel que soit le type de données, lorsque *length* est un nombre négatif.  
  
|Exemples|Résultats|  
|--------------|------------|  
|ROUND(748.58, -1)|750.00|  
|ROUND(748.58, -2)|700.00|  
|ROUND(748.58, -3)|Le résultat est un dépassement arithmétique car la valeur par défaut de 748.58 est une décimale (5,2) qui ne peut pas retourner 1000.00.|  
|Pour arrondir jusqu'à 4 chiffres, modifiez le type de données de l'entrée. Exemple :<br /><br /> `SELECT ROUND(CAST (748.58 AS decimal (6,2)),-3);`|1000.00|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-round-and-estimates"></a>A. Utilisation de ROUND et des estimations  
 Cet exemple contient deux expressions qui montrent qu'avec `ROUND`, le dernier chiffre est toujours une estimation.  
  
```  
SELECT ROUND(123.9994, 3), ROUND(123.9995, 3);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -----------  
123.9990    124.0000      
```  
  
### <a name="b-using-round-and-rounding-approximations"></a>B. Utilisation de ROUND et des approximations d'arrondi  
 Cet exemple montre des arrondis et des approximations.  
  
```  
SELECT ROUND(123.4545, 2), ROUND(123.45, -2);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

 ```
--------  ----------
123.45    100.00
```
  
### <a name="c-using-round-to-truncate"></a>C. Utilisation de ROUND pour une troncature  
 Cet exemple utilise deux instructions `SELECT` pour démontrer la différence entre l'arrondi et la troncature. La première instruction arrondit le résultat. La seconde instruction tronque le résultat.  
  
```  
SELECT ROUND(150.75, 0);  
GO  
SELECT ROUND(150.75, 0, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
151.00  
  
(1 row(s) affected)  
  
--------  
150.00  
  
(1 row(s) affected)  
```
  
## <a name="see-also"></a> Voir aussi  
 [CEILING &#40;Transact-SQL&#41;](../../t-sql/functions/ceiling-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FLOOR &#40;Transact-SQL&#41;](../../t-sql/functions/floor-transact-sql.md)   
 [Fonctions mathématiques &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)
