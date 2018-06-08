---
title: Variables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
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
ms.assetid: f372ae86-a003-40af-92de-fa52e3eea13f
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8088c643bc8d3b811fbad55710062b4cc1eb2b6d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="variables-transact-sql"></a>Variables (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Une variable locale Transact-SQL est un objet pouvant posséder une valeur de données unique d’un type donné. Les variables contenues dans les traitements et les scripts sont généralement utilisées : 

* pour compter ou vérifier le nombre de fois qu'une boucle est réalisée ;
* pour retenir une valeur de données à tester par une instruction de contrôle de flux ;
* pour enregistrer une valeur de données que doit retourner le code de retour d'une procédure stockée ou la valeur de retour d'une fonction.

> [!NOTE]
> Le nom de certaines fonctions système Transact-SQL commence par deux *arobases* (@@). Bien que dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les fonctions de type @@functions soient considérées comme des variables globales, ce n’en sont pas et elles n’en ont pas le comportement. Elles sont en réalité des fonctions système, et leur syntaxe suit les mêmes règles que celle des fonctions normales.

Le script suivant crée une petite table test et lui attribue 26 lignes. Il utilise une variable pour effectuer trois actions : 

* vérifier le nombre de lignes insérées en contrôlant combien de fois la boucle est exécutée ;
* fournir la valeur insérée dans la colonne INT ;
* faire partie de l'expression qui génère les lettres devant être insérées dans la colonne CHAR.  

```sql
-- Create the table.
CREATE TABLE TestTable (cola int, colb char(3));
GO
SET NOCOUNT ON;
GO
-- Declare the variable to be used.
DECLARE @MyCounter int;

-- Initialize the variable.
SET @MyCounter = 0;

-- Test the variable to see if the loop is finished.
WHILE (@MyCounter < 26)
BEGIN;
   -- Insert a row into the table.
   INSERT INTO TestTable VALUES
       -- Use the variable to provide the integer value
       -- for cola. Also use it to generate a unique letter
       -- for each row. Use the ASCII function to get the
       -- integer value of 'a'. Add @MyCounter. Use CHAR to
       -- convert the sum back to the character @MyCounter
       -- characters after 'a'.
       (@MyCounter,
        CHAR( ( @MyCounter + ASCII('a') ) )
       );
   -- Increment the variable to count this iteration
   -- of the loop.
   SET @MyCounter = @MyCounter + 1;
END;
GO
SET NOCOUNT OFF;
GO
-- View the data.
SELECT cola, colb
FROM TestTable;
GO
DROP TABLE TestTable;
GO
```

## <a name="declaring-a-transact-sql-variable"></a>Déclaration d'une variable Transact-SQL
L’instruction DECLARE initialise une variable Transact-SQL de la manière suivante : 
* Affectation d'un nom. Celui-ci doit avoir comme premier caractère un @ unique.
* Affectation d'un type de données système ou défini par l'utilisateur, ainsi que d'une taille. Pour les variables numériques, la précision et l'échelle doivent également être affectées. Pour les variables de type XML, une collection de schéma facultative peut être affectée.
* Affectation de la valeur NULL à la variable.

Par exemple, l’instruction **DECLARE** ci-dessous crée une variable locale appelée **@mycounter** avec un type de données int.  
```sql
DECLARE @MyCounter int;
```
Pour déclarer plusieurs variables locales, utilisez une virgule après la première variable locale définie, puis indiquez le nom et le type de données de la variable locale suivante.

Par exemple, l’instruction **DECLARE** suivante crée trois variables locales nommées **@LastName**, **@FirstName** et **@StateProvince**, puis initialise chacune sur la valeur NULL :  
```sql
DECLARE @LastName nvarchar(30), @FirstName nvarchar(20), @StateProvince nchar(2);
```

L’étendue d’une variable correspond à la plage des instructions Transact-SQL pouvant référencer cette variable. La portée d'une variable commence au point où elle est déclarée et se termine à la fin du traitement ou de la procédure stockée où elle est déclarée. Le script suivant, par exemple, génère une erreur de syntaxe car la variable est déclarée dans un lot et référencée dans un autre :  
```sql
USE AdventureWorks2014;
GO
DECLARE @MyVariable int;
SET @MyVariable = 1;
-- Terminate the batch by using the GO keyword.
GO 
-- @MyVariable has gone out of scope and no longer exists.

-- This SELECT statement generates a syntax error because it is
-- no longer legal to reference @MyVariable.
SELECT BusinessEntityID, NationalIDNumber, JobTitle
FROM HumanResources.Employee
WHERE BusinessEntityID = @MyVariable;
```

Les variables ont une portée locale et elles ne sont visibles que dans le traitement ou la procédure où elles sont définies. Dans l'exemple suivant, la portée imbriquée créée pour l'exécution de sp_executesql n'a pas accès à la variable déclarée dans la portée supérieure et elle retourne une erreur.  

```sql
DECLARE @MyVariable int;
SET @MyVariable = 1;
EXECUTE sp_executesql N'SELECT @MyVariable'; -- this produces an error
```

## <a name="setting-a-value-in-a-transact-sql-variable"></a>Affectation d'une valeur à une variable Transact-SQL

Quand une variable est déclarée pour la première fois, elle prend la valeur NULL. Pour affecter une valeur à une variable, utilisez l'instruction SET. C'est la méthode recommandée pour affecter une valeur à une variable. Il est également possible d'affecter une valeur à une variable en y faisant référence dans la liste de sélection d'une instruction SELECT.

Pour affecter une valeur à une variable à l'aide de l'instruction SET, indiquez le nom et la valeur à affecter à la variable. C'est la méthode recommandée pour affecter une valeur à une variable. Le lot suivant, par exemple, déclare deux variables, leur affecte une valeur et les utilise dans la clause `WHERE` d'une instruction `SELECT` :  

```sql
USE AdventureWorks2014;
GO
-- Declare two variables.
DECLARE @FirstNameVariable nvarchar(50),
   @PostalCodeVariable nvarchar(15);

-- Set their values.
SET @FirstNameVariable = N'Amy';
SET @PostalCodeVariable = N'BA5 3HX';

-- Use them in the WHERE clause of a SELECT statement.
SELECT LastName, FirstName, JobTitle, City, StateProvinceName, CountryRegionName
FROM HumanResources.vEmployee
WHERE FirstName = @FirstNameVariable
   OR PostalCode = @PostalCodeVariable;
GO
```

Vous pouvez également affecter une valeur à une variable en y faisant référence dans une liste de sélection. Si une variable est référencée dans une liste de sélection, une valeur scalaire doit lui être affectée, sinon l'instruction SELECT ne retournera qu'une seule ligne. Exemple :  

```sql
USE AdventureWorks2014;
GO
DECLARE @EmpIDVariable int;

SELECT @EmpIDVariable = MAX(EmployeeID)
FROM HumanResources.Employee;
GO
```

> [!WARNING]
> S'il y a plusieurs clauses d'affectation dans une seule instruction SELECT, SQL Server ne garantit pas l'ordre d'évaluation des expressions. Notez que les effets ne sont visibles que s'il existe des références parmi les affectations.

Si une instruction SELECT retourne plusieurs lignes et si la variable fait référence à une expression non scalaire, la variable prend la valeur retournée par l’expression figurant à la dernière ligne de l’ensemble de résultats. Dans le lot suivant, par exemple, **@EmpIDVariable** prend la valeur **BusinessEntityID** de la dernière ligne retournée, c’est-à-dire 1 :  

```sql
USE AdventureWorks2014;
GO
DECLARE @EmpIDVariable int;

SELECT @EmpIDVariable = BusinessEntityID
FROM HumanResources.Employee
ORDER BY BusinessEntityID DESC;

SELECT @EmpIDVariable;
GO
```

## <a name="see-also"></a> Voir aussi  
 [Declare @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
 [SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
 [SELECT @local_variable](../../t-sql/language-elements/select-local-variable-transact-sql.md)  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
  
  
