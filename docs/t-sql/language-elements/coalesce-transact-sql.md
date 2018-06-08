---
title: COALESCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- COALESCE
- COALESCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], nonnull
- COALESCE function
- first nonnull expressions [SQL Server]
- nonnull expressions
ms.assetid: fafc0dba-f8a8-4aad-9b7f-908e34b74d88
caps.latest.revision: 52
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: cce959876ded6e2815260e82b0c9e909e804fd5a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="coalesce-transact-sql"></a>COALESCE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Évalue les arguments dans l’ordre et retourne la valeur actuelle de la première expression qui ne prend pas initialement la valeur `NULL`. Par exemple, `SELECT COALESCE(NULL, NULL, 'third_value', 'fourth_value');` renvoie la troisième valeur, car il s’agit de la première valeur qui n’est pas Null. 
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
COALESCE ( expression [ ,...n ] )   
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type.  
  
## <a name="return-types"></a>Types de retour  
 Retourne le type de données de l’*expression* dont la priorité est la plus élevée. Si aucune des expressions n'acceptent les valeurs NULL, le résultat est typé comme n'acceptant pas les valeurs NULL.  
  
## <a name="remarks"></a>Notes   
 Si tous les arguments ont la valeur `NULL`, `COALESCE` retourne `NULL`. Au moins une des valeurs Null doit être une valeur `NULL` typée.  
  
## <a name="comparing-coalesce-and-case"></a>Comparaison de COALESCE et de CASE  
 L’expression `COALESCE` est un raccourci syntaxique de l’expression `CASE`.  Autrement dit, le code `COALESCE`(*expression1*,*...n*) est réécrit par l’optimiseur de requête sous la forme de l’expression `CASE` suivante :  
  
 ```sql  
 CASE  
 WHEN (expression1 IS NOT NULL) THEN expression1  
 WHEN (expression2 IS NOT NULL) THEN expression2  
 ...  
 ELSE expressionN  
 END  
 ```  
  
 Cela signifie que les valeurs d’entrée (*expression1*, *expression2*, *expressionN*, etc.) sont évaluées plusieurs fois. En outre, conformément à la norme SQL, une expression de valeur qui contient une sous-requête est considérée comme non déterministe et la sous-requête est évaluée deux fois. Dans l'un et l'autre cas, différents résultats peuvent être retournés entre la première évaluation et les suivantes.  
  
 Par exemple, lorsque le code `COALESCE((subquery), 1)` est exécuté, la sous-requête est évaluée deux fois. Par conséquent, vous pouvez obtenir des résultats différents selon le niveau d'isolement de la requête. Par exemple, le code peut retourner la valeur `NULL` avec le niveau d’isolement `READ COMMITTED` dans un environnement multi-utilisateurs. Pour garantir des résultats stables, utilisez le niveau d’isolement `SNAPSHOT ISOLATION` ou remplacez `COALESCE` par la fonction `ISNULL`. Vous pouvez également réécrire la requête pour envoyer (push) la sous-requête dans une sous-sélection, comme le montre l’exemple suivant :  
  
```sql  
SELECT CASE WHEN x IS NOT NULL THEN x ELSE 1 END  
from  
(  
SELECT (SELECT Nullable FROM Demo WHERE SomeCol = 1) AS x  
) AS T;  
  
```  
  
## <a name="comparing-coalesce-and-isnull"></a>Comparaison de COALESCE et de ISNULL  
 La fonction `ISNULL` et l’expression `COALESCE` ont un objectif similaire, mais peuvent se comporter différemment.  
  
1.  `ISNULL` étant une fonction, elle est évaluée une seule fois.  Comme décrit ci-dessus, les valeurs d’entrée pour l’expression `COALESCE` peuvent être évaluées plusieurs fois.  
  
2.  La détermination du type de données de l'expression obtenue est différente. `ISNULL` utilise le type de données du premier paramètre, `COALESCE` suit les règles de l’expression `CASE` et retourne le type de données de la valeur ayant la priorité la plus élevée.  
  
3.  La possibilité de valeurs Null de l’expression de résultat est différente pour `ISNULL` et `COALESCE`. La valeur renvoyée `ISNULL` est toujours considérée comme n’acceptant PAS la valeur NULL (en supposant que la valeur renvoyée est non-NULL), tandis que `COALESCE` avec des paramètres non-NULL est considéré comme `NULL`. Les expressions `ISNULL(NULL, 1)` et `COALESCE(NULL, 1)` ont ainsi des possibilités de valeur NULL différentes bien qu’elles soient équivalentes. Cela a son importance si vous utilisez ces expressions dans des colonnes calculées, créez des contraintes de clé ou rendez déterministe la valeur renvoyée d’une fonction définie par l’utilisateur scalaire afin qu’elle puisse être indexée, comme le montre l’exemple suivant :  
  
    ```sql  
    USE tempdb;  
    GO  
    -- This statement fails because the PRIMARY KEY cannot accept NULL values  
    -- and the nullability of the COALESCE expression for col2   
    -- evaluates to NULL.  
    CREATE TABLE #Demo   
    (   
    col1 integer NULL,   
    col2 AS COALESCE(col1, 0) PRIMARY KEY,   
    col3 AS ISNULL(col1, 0)   
    );   
  
    -- This statement succeeds because the nullability of the   
    -- ISNULL function evaluates AS NOT NULL.  
  
    CREATE TABLE #Demo   
    (   
    col1 integer NULL,   
    col2 AS COALESCE(col1, 0),   
    col3 AS ISNULL(col1, 0) PRIMARY KEY   
    );  
    ```  
  
4.  Les validations pour `ISNULL` et `COALESCE` sont également différentes. Par exemple, une valeur `NULL` pour `ISNULL` est convertie en **int** tandis que pour `COALESCE`, vous devez fournir un type de données.  
  
5.  `ISNULL` accepte uniquement deux paramètres alors que `COALESCE` en accepte un nombre.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-running-a-simple-example"></a>A. Exécution d'un exemple simple  
 L'exemple suivant montre comment `COALESCE` sélectionne les données de la première colonne qui a une valeur non Null. Cet exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT Name, Class, Color, ProductNumber,  
COALESCE(Class, Color, ProductNumber) AS FirstNotNull  
FROM Production.Product;  
```  
  
### <a name="b-running-a-complex-example"></a>B. Exécution d'un exemple complexe  
 Dans l'exemple suivant, la table `wages` comporte trois colonnes qui contiennent des informations sur les salaires annuels des employés : salaire horaire, salaire et commission. Cependant, chaque employé ne perçoit qu'un seul type de salaire. Pour déterminer le montant total versé à tous les employés, utilisez `COALESCE` afin de recevoir seulement la valeur non NULL trouvée dans `hourly_wage`, `salary` et `commission`.  
  
```sql  
SET NOCOUNT ON;  
GO  
USE tempdb;  
IF OBJECT_ID('dbo.wages') IS NOT NULL  
    DROP TABLE wages;  
GO  
CREATE TABLE dbo.wages  
(  
    emp_id        tinyint   identity,  
    hourly_wage   decimal   NULL,  
    salary        decimal   NULL,  
    commission    decimal   NULL,  
    num_sales     tinyint   NULL  
);  
GO  
INSERT dbo.wages (hourly_wage, salary, commission, num_sales)  
VALUES  
    (10.00, NULL, NULL, NULL),  
    (20.00, NULL, NULL, NULL),  
    (30.00, NULL, NULL, NULL),  
    (40.00, NULL, NULL, NULL),  
    (NULL, 10000.00, NULL, NULL),  
    (NULL, 20000.00, NULL, NULL),  
    (NULL, 30000.00, NULL, NULL),  
    (NULL, 40000.00, NULL, NULL),  
    (NULL, NULL, 15000, 3),  
    (NULL, NULL, 25000, 2),  
    (NULL, NULL, 20000, 6),  
    (NULL, NULL, 14000, 4);  
GO  
SET NOCOUNT OFF;  
GO  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS money) AS 'Total Salary'   
FROM dbo.wages  
ORDER BY 'Total Salary';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```   
 Total Salary  
 ------------  
 10000.00  
 20000.00  
 20800.00  
 30000.00  
 40000.00  
 41600.00  
 45000.00  
 50000.00  
 56000.00  
 62400.00  
 83200.00  
 120000.00  
  
 (12 row(s) affected)
 ```  
  
### <a name="c-simple-example"></a>C : Exemple simple  
 L’exemple suivant montre comment `COALESCE` sélectionne les données de la première colonne qui a une valeur non-Null. Cet exemple suppose que la table `Products` contient ces données :  
  
 ```  
 Name         Color      ProductNumber  
 ------------ ---------- -------------  
 Socks, Mens  NULL       PN1278  
 Socks, Mens  Blue       PN1965  
 NULL         White      PN9876
 ```  
  
 Nous exécutons ensuite la requête COALESCE suivante :  
  
```sql  
SELECT Name, Color, ProductNumber, COALESCE(Color, ProductNumber) AS FirstNotNull   
FROM Products ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 Name         Color      ProductNumber  FirstNotNull  
 ------------ ---------- -------------  ------------  
 Socks, Mens  NULL       PN1278         PN1278  
 Socks, Mens  Blue       PN1965         Blue  
 NULL         White      PN9876         White
 ```  
  
 Notez que dans la première ligne, la valeur `FirstNotNull` est `PN1278`, pas `Socks, Mens`. En effet, la colonne `Name` n’a pas été spécifiée en tant que paramètre pour `COALESCE` dans l’exemple.  
  
### <a name="d-complex-example"></a>D : Exemple complexe  
 L’exemple suivant utilise `COALESCE` pour comparer les valeurs de trois colonnes et retourner uniquement la valeur non-Null trouvée dans les colonnes.  
  
```sql  
CREATE TABLE dbo.wages  
(  
    emp_id        tinyint   NULL,  
    hourly_wage   decimal   NULL,  
    salary        decimal   NULL,  
    commission    decimal   NULL,  
    num_sales     tinyint   NULL  
);  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (1, 10.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (2, 20.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (3, 30.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (4, 40.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (5, NULL, 10000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (6, NULL, 20000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (7, NULL, 30000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (8, NULL, 40000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (9, NULL, NULL, 15000, 3);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (10,NULL, NULL, 25000, 2);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (11, NULL, NULL, 20000, 6);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (12, NULL, NULL, 14000, 4);  
  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS decimal(10,2)) AS TotalSalary   
FROM dbo.wages  
ORDER BY TotalSalary;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Total Salary  
 ------------  
 10000.00  
 20000.00  
 20800.00  
 30000.00  
 40000.00  
 41600.00  
 45000.00  
 50000.00  
 56000.00  
 62400.00  
 83200.00  
 120000.00
 ```  
  
## <a name="see-also"></a> Voir aussi  
 [ISNULL &#40;Transact-SQL&#41;](../../t-sql/functions/isnull-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  
