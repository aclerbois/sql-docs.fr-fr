---
title: DATEADD (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DATEADD
- DATEADD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- add interval to date or time [SQL Server]
- subtract interval from date or time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], intervals
- date and time [SQL Server], DATEADD
- DATEADD function [SQL Server]
ms.assetid: 89c5ae32-89c6-47e1-979e-15d97908b9f1
caps.latest.revision: 71
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8dcbc7ccfc8c94c28f3c8ba0df32b915440d76e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dateadd-transact-sql"></a>DATEADD (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Renvoie une *date* spécifiée avec l’intervalle *number* spécifié (entier signé) ajouté à une partie *datepart* spécifiée de cette *date*.
  
Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DATEADD (datepart , number , date )  
```  
  
## <a name="arguments"></a>Arguments  
*datepart*  
Partie de *date* à laquelle un **nombre***entier* est ajouté. Le tableau suivant répertorie tous les arguments *datepart* valides. Les équivalents de variables définis par l'utilisateur ne sont pas valides.
  
|*datepart*|Abréviations|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**, **w**|  
|**hour**|**hh**|  
|**minute**|**mi**, **n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
  
*nombre*  
Expression qui peut être résolue en une valeur [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) qui est ajoutée à une valeur *datepart* de *date*. Les variables définies par l'utilisateur sont valides.  
Si vous spécifiez une valeur avec une fraction décimale, la fraction est tronquée et n'est pas arrondie.
  
*date*  
Expression qui peut être résolue en valeur **time**, **date**, **smalldatetime**, **datetime**, **datetime2** ou **datetimeoffset**. *date* peut être une expression, une expression de colonne, une variable définie par l’utilisateur ou un littéral de chaîne. Si l’expression est un littéral de chaîne, elle doit être résolue en **datetime**. Pour éviter toute ambiguïté, représentez les années à l'aide de quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration de serveur d’année de coupure à deux chiffres](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-types"></a>Types de retour
Le type des données renvoyées est le type de données de l’argument *date*, à l’exception des littéraux de chaîne.
Le type des données renvoyées pour un littéral de chaîne est **datetime**. Une erreur sera déclenchée si l'échelle des secondes du littéral de chaîne comprend plus de trois positions (.nnn) ou contient la partie du décalage de fuseau horaire.
  
## <a name="return-value"></a>Valeur retournée  
  
## <a name="datepart-argument"></a>Argument datepart  
**dayofyear**, **day** et **weekday** renvoient la même valeur.
  
Chaque *datepart* et ses abréviations renvoient la même valeur.
  
Si *datepart* est **month** et que le mois de *date* comprend plus de jours que le mois renvoyé et que le jour de *date* n’existe pas dans le mois renvoyé, le dernier jour du mois renvoyé est renvoyé. Par exemple, septembre compte 30 jours ; par conséquent, les deux instructions suivantes retournent 2006-09-30 00:00:00.000:
  
```sql
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '20060831');
```
  
## <a name="number-argument"></a>Argument number  
L’argument *number* ne peut pas dépasser la plage des **int**. Dans les instructions suivantes, l’argument pour *number* dépasse la plage des **int** de 1. Le message d'erreur suivant est retourné :`Msg 8115, Level 16, State 2, Line 1. Arithmetic overflow error converting expression to data type int."`
  
```sql
SELECT DATEADD(year,2147483648, '20060731');  
SELECT DATEADD(year,-2147483649, '20060731');  
```  
  
## <a name="date-argument"></a>Argument date  
L’argument *date* ne peut pas être incrémenté à une valeur située hors de la plage de son type de données. Dans les instructions suivantes, la valeur *number* qui est ajoutée à la valeur *date* dépasse la plage du type de données *date*. Le message d'erreur suivant est retourné :`Msg 517, Level 16, State 1, Line 1 Adding a value to a 'datetime' column caused overflow`.
  
```sql
SELECT DATEADD(year,2147483647, '20060731');  
SELECT DATEADD(year,-2147483647, '20060731');  
```  
  
## <a name="return-values-for-a-smalldatetime-date-and-a-second-or-fractional-seconds-datepart"></a>Valeurs retournées pour une date smalldatetime et une partie de date seconde ou fractions de seconde  
La partie des secondes d’une valeur [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) est toujours 00. Si *date* est de type **smalldatetime**, les remarques suivantes s’appliquent :
-   Si *datepart* est **second** et que *number* est compris entre-30 et +29, aucun ajout n’est effectué.  
-   Si *datepart* est **second** et que *number* est inférieur à -30 ou supérieur à +29, l’ajout est effectué en commençant à une minute.  
-   Si *datepart* est **millisecond** et que *number* est compris entre-30001 et +29998, aucun ajout n’est effectué.  
-   Si *datepart* est **millisecond** et que *number* est inférieur à -30001 ou supérieur à +29998, l’ajout est effectué en commençant à une minute.  
  
## <a name="remarks"></a>Notes   
DATEADD peut être utilisé dans les clauses SELECT \<list>, WHERE, HAVING, GROUP BY et ORDER BY.
  
## <a name="fractional-seconds-precision"></a>Précision en fractions de seconde
L’ajout n’est pas autorisé pour une valeur *datepart* égale à **microsecond** ou **nanosecond** pour une valeur *date* de type de données **smalldatetime**, **date** ou **datetime**.
  
Les millisecondes ont une échelle de 3 (0,123). Les microsecondes ont une échelle de 6 (0,123456). Les nanosecondes ont une échelle de 9 (0,123456789). Les types de données **time**, **datetime2** et **datetimeoffset** ont une échelle maximale de 7 (.1234567). Si *datepart* est **nanosecond**, *number* doit être 100 avant que les fractions de seconde de *date* augmentent. Une valeur *number* comprise entre 1 et 49 est arrondie à 0 et une valeur comprise entre 50 et 99 est arrondie à 100.
  
Les instructions suivantes ajoutent une valeur *datepart* égale à **millisecond**, **microsecond** ou **nanosecond**.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT '1 millisecond', DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT '2 milliseconds', DATEADD(millisecond,2,@datetime2)  
UNION ALL  
SELECT '1 microsecond', DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT '2 microseconds', DATEADD(microsecond,2,@datetime2)  
UNION ALL  
SELECT '49 nanoseconds', DATEADD(nanosecond,49,@datetime2)  
UNION ALL  
SELECT '50 nanoseconds', DATEADD(nanosecond,50,@datetime2)  
UNION ALL  
SELECT '150 nanoseconds', DATEADD(nanosecond,150,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
1 millisecond     2007-01-01 13:10:10.1121111  
2 milliseconds    2007-01-01 13:10:10.1131111  
1 microsecond     2007-01-01 13:10:10.1111121  
2 microseconds    2007-01-01 13:10:10.1111131  
49 nanoseconds    2007-01-01 13:10:10.1111111  
50 nanoseconds    2007-01-01 13:10:10.1111112  
150 nanoseconds   2007-01-01 13:10:10.1111113  
```  
  
## <a name="time-zone-offset"></a>Décalage de fuseau horaire
L'ajout n'est pas autorisé pour le décalage de fuseau horaire.
  
## <a name="examples"></a>Exemples  

### <a name="a-incrementing-datepart-by-an-interval-of-1"></a>A. Incrémentation d'une partie de date d'un intervalle de 1  
Chacune des instructions suivantes incrémente *datepart* d’un intervalle de 1.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT 'year', DATEADD(year,1,@datetime2)  
UNION ALL  
SELECT 'quarter',DATEADD(quarter,1,@datetime2)  
UNION ALL  
SELECT 'month',DATEADD(month,1,@datetime2)  
UNION ALL  
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)  
UNION ALL  
SELECT 'day',DATEADD(day,1,@datetime2)  
UNION ALL  
SELECT 'week',DATEADD(week,1,@datetime2)  
UNION ALL  
SELECT 'weekday',DATEADD(weekday,1,@datetime2)  
UNION ALL  
SELECT 'hour',DATEADD(hour,1,@datetime2)  
UNION ALL  
SELECT 'minute',DATEADD(minute,1,@datetime2)  
UNION ALL  
SELECT 'second',DATEADD(second,1,@datetime2)  
UNION ALL  
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Year         2008-01-01 13:10:10.1111111  
quarter      2007-04-01 13:10:10.1111111  
month        2007-02-01 13:10:10.1111111  
dayofyear    2007-01-02 13:10:10.1111111  
day          2007-01-02 13:10:10.1111111  
week         2007-01-08 13:10:10.1111111  
weekday      2007-01-02 13:10:10.1111111  
hour         2007-01-01 14:10:10.1111111  
minute       2007-01-01 13:11:10.1111111  
second       2007-01-01 13:10:11.1111111  
millisecond  2007-01-01 13:10:10.1121111  
microsecond  2007-01-01 13:10:10.1111121  
nanosecond   2007-01-01 13:10:10.1111111  
```  
  
### <a name="b-incrementing-more-than-one-level-of-datepart-in-one-statement"></a>B. Incrémentation de plusieurs niveaux d'une partie de date dans une instruction  
Chacune des instructions suivantes incrémente *datepart* d’une valeur *number* assez grande pour incrémenter également la valeur *datepart* immédiatement supérieure de *date*.
  
```sql
DECLARE @datetime2 datetime2;  
SET @datetime2 = '2007-01-01 01:01:01.1111111';  
--Statement                                 Result     
-------------------------------------------------------------------   
SELECT DATEADD(quarter,4,@datetime2);     --2008-01-01 01:01:01.110  
SELECT DATEADD(month,13,@datetime2);      --2008-02-01 01:01:01.110  
SELECT DATEADD(dayofyear,365,@datetime2); --2008-01-01 01:01:01.110  
SELECT DATEADD(day,365,@datetime2);       --2008-01-01 01:01:01.110  
SELECT DATEADD(week,5,@datetime2);        --2007-02-05 01:01:01.110  
SELECT DATEADD(weekday,31,@datetime2);    --2007-02-01 01:01:01.110  
SELECT DATEADD(hour,23,@datetime2);       --2007-01-02 00:01:01.110  
SELECT DATEADD(minute,59,@datetime2);     --2007-01-01 02:00:01.110  
SELECT DATEADD(second,59,@datetime2);     --2007-01-01 01:02:00.110  
SELECT DATEADD(millisecond,1,@datetime2); --2007-01-01 01:01:01.110  
```  
  
### <a name="c-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>C. Utilisation d'expressions comme arguments pour les paramètres de date et numériques  
Les exemples suivants utilisent différents types d’expressions comme arguments pour les paramètres *number* et *date*. Ces exemples utilisent la base de données AdventureWorks.
  
#### <a name="specifying-a-column-as-date"></a>Spécification d'une colonne comme date  
L'exemple suivant ajoute `2` jours à chaque valeur figurant dans la colonne `OrderDate` pour dériver une nouvelle colonne nommée `PromisedShipDate`.
  
```sql
SELECT SalesOrderID  
    ,OrderDate   
    ,DATEADD(day,2,OrderDate) AS PromisedShipDate  
FROM Sales.SalesOrderHeader;  
```  
  
Voici un jeu de résultats partiel.
  
```sql
SalesOrderID OrderDate               PromisedShipDate  
------------ ----------------------- -----------------------  
43659        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43660        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43661        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
...  
43702        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43703        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43704        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43705        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43706        2005-07-03 00:00:00.000 2005-07-05 00:00:00.000  
...  
43711        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
43712        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
...  
43740        2005-07-11 00:00:00.000 2005-07-13 00:00:00.000  
43741        2005-07-12 00:00:00.000 2005-07-14 00:00:00.000  
  
```  
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>Spécification de variables définies par l'utilisateur comme nombre et date  
L’exemple suivant spécifie des variables définies par l’utilisateur comme arguments pour *number* et *date*.
  
```sql
DECLARE @days int = 365,   
        @datetime datetime = '2000-01-01 01:01:01.111'; /* 2000 was a leap year */;  
SELECT DATEADD(day, @days, @datetime);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
-----------------------  
2000-12-31 01:01:01.110  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-system-function-as-date"></a>Spécification de la fonction système scalaire comme date  
L’exemple suivant spécifie `SYSDATETIME` pour *date*.
  
```sql
SELECT DATEADD(month, 1, SYSDATETIME());  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
---------------------------  
2013-02-06 14:29:59.6727944  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>Spécification de sous-requêtes scalaires et de fonctions scalaires comme nombre et date  
L’exemple suivant utilise des sous-requêtes scalaires, `MAX(ModifiedDate)`, comme arguments pour *number* et *date*. `(SELECT TOP 1 BusinessEntityID FROM Person.Person)` est un argument artificiel pour le paramètre number, utilisé pour illustrer la sélection d’un argument *number* dans une liste de valeurs.
  
```sql
SELECT DATEADD(month,(SELECT TOP 1 BusinessEntityID FROM Person.Person),  
    (SELECT MAX(ModifiedDate) FROM Person.Person));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>Spécification d'expressions numériques et de fonctions système scalaires comme nombre et date  
L’exemple suivant utilise une expression numérique (-`(10/2))`, des [opérateurs unaires](../../mdx/unary-operators.md) (`-`), un [opérateur arithmétique](../../mdx/arithmetic-operators.md) (`/`) et des fonctions système scalaires (`SYSDATETIME`) comme arguments pour *number* et *date*.
  
```sql
SELECT DATEADD(month,-(10/2), SYSDATETIME());  
```  
  
#### <a name="specifying-ranking-functions-as-number"></a>Spécification de fonctions de classement comme nombre  
L’exemple suivant utilise une fonction de classement comme arguments pour *number*.
  
```sql
SELECT p.FirstName, p.LastName  
    ,DATEADD(day,ROW_NUMBER() OVER (ORDER BY  
        a.PostalCode),SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
#### <a name="specifying-an-aggregate-window-function-as-number"></a>Spécification d'une fonction d'agrégation comme nombre  
L’exemple suivant utilise une fonction d’agrégation comme argument pour *number*.
  
```sql
SELECT SalesOrderID, ProductID, OrderQty  
    ,DATEADD(day,SUM(OrderQty)   
        OVER(PARTITION BY SalesOrderID),SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

