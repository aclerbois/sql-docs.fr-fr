---
title: UNION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- UNION
- UNION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UNION queries
- combining query results
- UNION operator [SQL Server]
ms.assetid: 607c296f-8a6a-49bc-975a-b8d0c0914df7
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 942e0613e4a3c2c540a1a01148e4d9969ff23078
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-operators---union-transact-sql"></a>Opérateurs de jeu - UNION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Associe les résultats de plusieurs requêtes en un seul jeu de résultats qui inclut toutes les lignes appartenant aux requêtes de l'union. Cette opération diffère de l'utilisation des jointures combinant les colonnes des deux tables.  
  
 Voici les règles de base pour combiner les jeux de résultats de deux requêtes à l'aide de la procédure UNION :  
  
-   Le nombre et l'ordre des colonnes doivent être identiques dans toutes les requêtes.  
  
-   Les types de données doivent être compatibles.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
    { <query_specification> | ( <query_expression> ) }   
  UNION [ ALL ]   
  <query_specification | ( <query_expression> )   
 [ UNION [ ALL ] <query_specification> | ( <query_expression> )   
    [ ...n ] ]   
```  
  
## <a name="arguments"></a>Arguments  
\<query_specification> | ( \<query_expression> ) Spécification ou expression de requête qui retourne les données à associer aux données d’une autre spécification ou expression de requête. Les définitions des colonnes faisant partie d'une opération UNION ne doivent pas forcément être identiques, mais elles doivent être compatibles via une conversion implicite. Lorsque les types de données diffèrent, le type de données résultant est déterminé en fonction des règles de [priorité des types de données](../../t-sql/data-types/data-type-precedence-transact-sql.md). Si les types sont les mêmes mais diffèrent en terme de précision, d'échelle ou de longueur, le résultat se détermine d'après les mêmes règles de combinaison d'expressions. Pour plus d’informations, consultez [Précision, échelle et longueur &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
 Les colonnes de type de données **xml** doivent être équivalentes. Toutes les colonnes doivent être typées selon un schéma XML ou être non typées. Si elles sont typées, elles doivent l'être par rapport à la même collection de schémas XML.  
  
 UNION  
 Indique que les jeux de résultats multiples doivent être associés et retournés dans un seul jeu de résultats.  
  
 ALL  
 Incorpore toutes les lignes dans les résultats, y compris les doublons. S'il n'est pas spécifié, tous les doublons de lignes sont supprimés.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-a-simple-union"></a>A. Utilisation de l'opérateur UNION simple  
 Dans l'exemple suivant, le jeu de résultats comprend le contenu des colonnes `ProductModelID` et `Name` des deux tables `ProductModel` et `Gloves`.  
  
```  
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Here is the simple union.  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
```  
  
### <a name="b-using-select-into-with-union"></a>B. Utilisation de SELECT INTO avec UNION  
 Dans l'exemple suivant, la clause `INTO` de la seconde instruction `SELECT` indique que la table nommée `ProductResults` contient le jeu de résultats final de l'union des colonnes désignées des tables `ProductModel` et `Gloves`. Notez que la table `Gloves` est créée dans la première instruction `SELECT`.  
  
```  
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.ProductResults', 'U') IS NOT NULL  
DROP TABLE dbo.ProductResults;  
GO  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
INTO dbo.ProductResults  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
SELECT ProductModelID, Name   
FROM dbo.ProductResults;  
  
```  
  
### <a name="c-using-union-of-two-select-statements-with-order-by"></a>C. Utilisation de l'opérateur UNION dans deux instructions SELECT avec ORDER BY  
 L'ordre de certains paramètres utilisés avec la clause UNION est important. L'exemple suivant illustre l'utilisation incorrecte et correcte de `UNION` dans deux instructions `SELECT` où une colonne doit être renommée dans le résultat.  
  
```  
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.Gloves', 'U') IS NOT NULL  
DROP TABLE dbo.Gloves;  
GO  
-- Create Gloves table.  
SELECT ProductModelID, Name  
INTO dbo.Gloves  
FROM Production.ProductModel  
WHERE ProductModelID IN (3, 4);  
GO  
  
/* INCORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
ORDER BY Name  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves;  
GO  
  
/* CORRECT */  
-- Uses AdventureWorks  
  
SELECT ProductModelID, Name  
FROM Production.ProductModel  
WHERE ProductModelID NOT IN (3, 4)  
UNION  
SELECT ProductModelID, Name  
FROM dbo.Gloves  
ORDER BY Name;  
GO  
  
```  
  
### <a name="d-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>D. Utilisation de l'opérateur UNION dans trois instructions SELECT pour illustrer les effets de ALL et des parenthèses  
 Les exemples suivants utilisent `UNION` pour combiner les résultats de trois tables, ayant chacune les 5 lignes de données identiques. Le premier exemple utilise `UNION ALL` pour montrer les doublons d'enregistrement et retourne l'ensemble des 15 lignes. Le deuxième exemple utilise `UNION` sans `ALL` pour éliminer les doublons de ligne des résultats combinés des trois instructions `SELECT` et retourne 5 lignes.  
  
 Le troisième exemple utilise `ALL` avec la première clause `UNION` et met entre parenthèses la seconde clause `UNION` qui n'utilise pas `ALL`. La seconde clause `UNION` est traitée en premier, car elle est entre parenthèses. Elle retourne 5 lignes car l'option `ALL` n'est pas utilisée et les doublons sont supprimés. Ces 5 lignes sont combinées avec les résultats de la première instruction `SELECT` à l'aide des mots clés `UNION ALL`. Cela ne supprime pas les doublons entre les deux ensembles de 5 lignes. Le résultat final contient 10 lignes.  
  
```  
-- Uses AdventureWorks  
  
IF OBJECT_ID ('dbo.EmployeeOne', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeOne;  
GO  
IF OBJECT_ID ('dbo.EmployeeTwo', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeTwo;  
GO  
IF OBJECT_ID ('dbo.EmployeeThree', 'U') IS NOT NULL  
DROP TABLE dbo.EmployeeThree;  
GO  
  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeOne  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeTwo  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
SELECT pp.LastName, pp.FirstName, e.JobTitle   
INTO dbo.EmployeeThree  
FROM Person.Person AS pp JOIN HumanResources.Employee AS e  
ON e.BusinessEntityID = pp.BusinessEntityID  
WHERE LastName = 'Johnson';  
GO  
-- Union ALL  
SELECT LastName, FirstName, JobTitle  
FROM dbo.EmployeeOne  
UNION ALL  
SELECT LastName, FirstName ,JobTitle  
FROM dbo.EmployeeTwo  
UNION ALL  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle  
FROM dbo.EmployeeOne  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION   
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree;  
GO  
  
SELECT LastName, FirstName,JobTitle   
FROM dbo.EmployeeOne  
UNION ALL  
(  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeTwo  
UNION  
SELECT LastName, FirstName, JobTitle   
FROM dbo.EmployeeThree  
);  
GO  
  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-a-simple-union"></a>E. Utilisation de l'opérateur UNION simple  
 Dans l’exemple suivant, le jeu de résultats comprend le contenu des colonnes `CustomerKey` des deux tables `FactInternetSales` et `DimCustomer`. Étant donné que le mot clé ALL n’est pas utilisé, les doublons sont exclus des résultats.  
  
```  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="f-using-union-of-two-select-statements-with-order-by"></a>F. Utilisation de l'opérateur UNION dans deux instructions SELECT avec ORDER BY  
 Lorsqu’une instruction SELECT dans une instruction UNION comprend une clause ORDER BY, cette clause doit être placée après toutes les instructions SELECT. L’exemple suivant illustre l’utilisation incorrecte et correcte de `UNION` dans deux instructions `SELECT` où une colonne doit être ordonnée avec ORDER BY.  
  
```  
-- Uses AdventureWorks  
  
-- INCORRECT  
SELECT CustomerKey   
FROM FactInternetSales    
ORDER BY CustomerKey  
UNION   
SELECT CustomerKey   
FROM DimCustomer  
ORDER BY CustomerKey;  
  
-- CORRECT   
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales    
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="g-using-union-of-two-select-statements-with-where-and-order-by"></a>G. Utilisation de l’opérateur UNION dans deux instructions SELECT avec WHERE et ORDER BY  
 L’exemple suivant montre l’utilisation incorrecte et correcte de `UNION` dans deux instructions `SELECT` où WHERE et ORDER BY sont nécessaires.  
  
```  
-- Uses AdventureWorks  
  
-- INCORRECT   
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
ORDER BY CustomerKey   
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
  
-- CORRECT  
USE AdventureWorksPDW2012;  
  
SELECT CustomerKey   
FROM FactInternetSales   
WHERE CustomerKey >= 11000  
UNION   
SELECT CustomerKey   
FROM DimCustomer   
ORDER BY CustomerKey;  
```  
  
### <a name="h-using-union-of-three-select-statements-to-show-effects-of-all-and-parentheses"></a>H. Utilisation de l’opérateur UNION dans trois instructions SELECT pour illustrer les effets de ALL et des parenthèses  
 Les exemples suivants utilisent `UNION` pour combiner les résultats de **la même table** afin d’illustrer les effets de ALL et des parenthèses lorsque vous utilisez `UNION`.  
  
 Le premier exemple utilise `UNION ALL` pour afficher les enregistrements en double et retourne chaque ligne dans la table source trois fois. Le deuxième exemple utilise `UNION` sans `ALL` pour éliminer les doublons de ligne des résultats combinés des trois instructions `SELECT` et retourne uniquement les lignes qui ne sont pas en double de la table source.  
  
 Le troisième exemple utilise `ALL` avec le premier opérateur `UNION` et des parenthèses autour du deuxième opérateur `UNION` qui n’utilise pas `ALL`. Le deuxième opérateur `UNION` est traité en premier, car il est entre parenthèses. Il retourne uniquement les lignes de la table qui ne sont pas en double, car l’option `ALL` n’est pas utilisée et les doublons sont supprimés. Ces lignes sont combinées avec les résultats de la première instruction `SELECT` à l’aide des mots clés `UNION ALL`. Cela ne supprime pas les doublons entre les deux ensembles.  
  
```  
-- Uses AdventureWorks  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer;  
  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION ALL  
(  
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
UNION   
SELECT CustomerKey, FirstName, LastName  
FROM DimCustomer  
);  
```  
  
## <a name="see-also"></a> Voir aussi  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Exemples SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)  
  
  

