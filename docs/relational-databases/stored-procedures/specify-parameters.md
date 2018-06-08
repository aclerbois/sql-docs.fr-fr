---
title: Spécifier les paramètres | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stored-Procs
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- parameters [SQL Server], stored procedures
- stored procedures [SQL Server], parameters
- output parameters [SQL Server]
- input parameters [SQL Server]
ms.assetid: 902314fe-5f9c-4d0d-a0b7-27e67c9c70ec
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8eae73f1a426bd615b0867c7651c00cda08afa58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-parameters"></a>Spécifier les paramètres
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  En spécifiant les paramètres de la procédure, les programmes appelants peuvent passer des valeurs dans le corps de la procédure. Ces valeurs peuvent être utilisées à plusieurs fins pendant l'exécution de la procédure. Les paramètres de la procédure peuvent également retourner des valeurs au programme appelant si le paramètre est marqué comme paramètre OUTPUT.  
  
 Une procédure peut contenir jusqu'à 2100 paramètres ; chacun ayant un nom, un type de données et une direction. Éventuellement, les paramètres peuvent avoir des valeurs par défaut.  
  
 La section suivante fournit des informations sur la transmission des valeurs dans les paramètres et sur la façon dont chacun des attributs de paramètre est utilisé lors d'un appel de procédure.  
  
## <a name="passing-values-into-parameters"></a>Transmission de valeurs dans les paramètres  
 Les valeurs des paramètres fournies avec un appel de procédure doivent être des constantes ou une variable ; un nom de fonction ne peut pas être utilisé comme valeur de paramètre. Les variables peuvent être des variables système ou des variables définies par l’utilisateur, telles que @@spid.  
  
 Les exemples suivants illustrent la transmission de valeurs de paramètres à la procédure `uspGetWhereUsedProductID`. Ils indiquent comment transmettre des paramètres sous forme de constantes et de variables. Ils décrivent également comment utiliser une variable pour transmettre la valeur d'une fonction.  
  
```  
USE AdventureWorks2012;  
GO  
-- Passing values as constants.  
EXEC dbo.uspGetWhereUsedProductID 819, '20050225';  
GO  
-- Passing values as variables.  
DECLARE @ProductID int, @CheckDate datetime;  
SET @ProductID = 819;  
SET @CheckDate = '20050225';  
EXEC dbo.uspGetWhereUsedProductID @ProductID, @CheckDate;  
GO  
-- Try to use a function as a parameter value.  
-- This produces an error message.  
EXEC dbo.uspGetWhereUsedProductID 819, GETDATE();  
GO  
-- Passing the function value as a variable.  
DECLARE @CheckDate datetime;  
SET @CheckDate = GETDATE();  
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;  
GO  
```  
  
## <a name="specifying-parameter-names"></a>Spécification des noms de paramètres  
 Lors de la création d'une procédure et de la déclaration d'un nom de paramètre, le nom doit commencer par un seul caractère @ et être unique dans l'étendue de la procédure.  
  
 En nommant explicitement les paramètres et en attribuant les valeurs appropriées à chaque paramètre dans un appel de procédure, il est possible de fournir les paramètres dans n'importe quel ordre. Par exemple, si la procédure **my_proc** attend trois paramètres nommés **@first**, **@second**et **@third**, les valeurs qui lui sont transmises peuvent être assignées aux noms de paramètres de la façon suivante : `EXECUTE my_proc @second = 2, @first = 1, @third = 3;`  
  
> [!NOTE]  
>  Si une valeur de paramètre est fournie sous la forme **@parameter =***value*, tous les paramètres suivants doivent être fournis de cette manière. Si les valeurs des paramètres ne sont pas transmises sous la forme **@parameter =***value*, elles doivent être fournies dans le même ordre (de gauche à droite) que les paramètres répertoriés dans l’instruction CREATE PROCEDURE.  
  
> [!WARNING]  
>  Un paramètre transmis sous la forme **@parameter =***value*, mais mal orthographié, génère une erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et empêche l’exécution de la procédure.  
  
## <a name="specifying-parameter-data-types"></a>Spécification des types de données de paramètre  
 Les paramètres doivent être définis avec un type de données lorsqu'ils sont déclarés dans une instruction CREATE PROCEDURE. Le type de données d'un paramètre détermine le type et la plage de valeurs admis pour le paramètre lorsque la procédure est appelée. Par exemple, si vous définissez un paramètre avec le type de données **tinyint** , seules les valeurs numériques comprises entre 0 et 255 sont acceptées lorsqu'elles sont passées à ce paramètre. Une erreur est renvoyée lorsqu'une procédure est exécutée avec une valeur incompatible avec le type de données.  
  
## <a name="specifying-parameter-default-values"></a>Spécification des valeurs de paramètres par défaut  
 Un paramètre est considéré comme facultatif si une valeur par défaut est spécifiée lorsqu'il est déclaré. Il n'est pas nécessaire de fournir une valeur pour un paramètre optionnel dans un appel de procédure.  
  
 La valeur par défaut d'un paramètre est utilisée lorsque :  
  
-   Aucune valeur n'est spécifiée pour le paramètre dans l'appel de procédure.  
  
-   Le mot clé DEFAULT est spécifié comme valeur dans l'appel de procédure.  
  
> [!NOTE]  
>  Si la valeur par défaut est une chaîne de caractères contenant des espaces ou des signes de ponctuation ou si elle débute par un nombre (par exemple, 6xxx), elle doit figurer entre guillemets simples.  
  
 Si aucune valeur ne peut être spécifiée correctement comme valeur par défaut pour le paramètre, spécifiez NULL comme valeur par défaut. Il est judicieux que la procédure retourne un message personnalisé lorsqu'elle est exécutée sans valeur pour le paramètre.  
  
 L'exemple suivant crée la procédure `uspGetSalesYTD` avec un paramètre d'entrée, `@SalesPerson`. La valeur NULL est désignée comme valeur par défaut pour le paramètre et est utilisée dans les instructions de gestion des erreurs afin de retourner un message d'erreur personnalisé lorsque la procédure est exécutée sans qu'une valeur n'ait été spécifiée pour le paramètre `@SalesPerson` .  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.uspGetSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.uspGetSalesYTD;  
GO  
CREATE PROCEDURE Sales.uspGetSalesYTD  
@SalesPerson nvarchar(50) = NULL  -- NULL default value  
AS   
    SET NOCOUNT ON;   
  
-- Validate the @SalesPerson parameter.  
IF @SalesPerson IS NULL  
BEGIN  
   PRINT 'ERROR: You must specify the last name of the sales person.'  
   RETURN  
END  
-- Get the sales for the specified sales person and   
-- assign it to the output parameter.  
SELECT SalesYTD  
FROM Sales.SalesPerson AS sp  
JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
WHERE LastName = @SalesPerson;  
RETURN  
GO  
  
```  
  
 L'exemple suivant exécute la procédure. La première instruction exécute la procédure sans spécifier de valeur d'entrée. Les instructions de gestion des erreurs dans la procédure retournent le message d'erreur personnalisé. La deuxième instruction fournit une valeur d'entrée et retourne l'ensemble de résultats attendu.  
  
```  
-- Run the procedure without specifying an input value.  
EXEC Sales.uspGetSalesYTD;  
GO  
-- Run the procedure with an input value.  
EXEC Sales.uspGetSalesYTD N'Blythe';  
GO  
```  
  
 Bien que vous puissiez omettre des paramètres ayant des valeurs par défaut, la liste des paramètres peut seulement être tronquée. Par exemple, si une procédure a cinq paramètres, le quatrième et le cinquième peuvent être omis. Toutefois, le quatrième ne peut pas être ignoré tant que le cinquième est inclus, sauf si les paramètres sont fournis sous la forme **@parameter =***value*.  
  
## <a name="specifying-parameter-direction"></a>Spécification de la direction du paramètre  
 La direction d'un paramètre peut être une entrée (une valeur passée dans le corps de la procédure) ou une sortie (la procédure retourne une valeur au programme appelant). La valeur par défaut est un paramètre d'entrée.  
  
 Pour spécifier un paramètre de sortie, vous devez indiquer le mot clé OUTPUT dans la définition du paramètre contenue dans l'instruction CREATE PROCEDURE. La procédure retourne la valeur actuelle du paramètre de sortie au programme appelant lorsqu'elle se termine. Le programme appelant doit également utiliser le mot clé OUTPUT lorsqu'il exécute la procédure pour enregistrer la valeur du paramètre dans une variable, qu'il pourra ensuite utiliser.  
  
 L'exemple suivant crée la procédure `Production.usp_GetList` qui retourne la liste des produits dont le prix ne dépasse pas un montant spécifié. Cet exemple représente l'utilisation de plusieurs instructions SELECT et de plusieurs paramètres OUTPUT. Les paramètres OUTPUT permettent à une procédure externe, à un traitement ou à plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] d'accéder à un ensemble de valeurs pendant l'exécution de la procédure.  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'Production.uspGetList', 'P' ) IS NOT NULL   
    DROP PROCEDURE Production.uspGetList;  
GO  
CREATE PROCEDURE Production.uspGetList @Product varchar(40)   
    , @MaxPrice money   
    , @ComparePrice money OUTPUT  
    , @ListPrice money OUT  
AS  
    SET NOCOUNT ON;  
    SELECT p.[Name] AS Product, p.ListPrice AS 'List Price'  
    FROM Production.Product AS p  
    JOIN Production.ProductSubcategory AS s   
      ON p.ProductSubcategoryID = s.ProductSubcategoryID  
    WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice;  
-- Populate the output variable @ListPprice.  
SET @ListPrice = (SELECT MAX(p.ListPrice)  
        FROM Production.Product AS p  
        JOIN  Production.ProductSubcategory AS s   
          ON p.ProductSubcategoryID = s.ProductSubcategoryID  
        WHERE s.[Name] LIKE @Product AND p.ListPrice < @MaxPrice);  
-- Populate the output variable @compareprice.  
SET @ComparePrice = @MaxPrice;  
GO  
  
```  
  
 Exécutez `usp_GetList` afin de retourner la liste des produits [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] (vélos) qui coûtent moins de 700 $. Les paramètres OUTPUT **@cost** et **@compareprices** sont utilisés en conjonction avec un langage de contrôle de flux afin de retourner un message dans la fenêtre **Messages** .  
  
> [!NOTE]  
>  La variable OUTPUT doit être définie pendant la création de la procédure et pendant l'utilisation de la variable. Le nom du paramètre et celui de la variable ne doivent pas nécessairement correspondre. En revanche, le type de données et la position du paramètre doivent correspondre (sauf si vous utilisez **@listprice=** *variable*).  
  
```  
DECLARE @ComparePrice money, @Cost money ;  
EXECUTE Production.uspGetList '%Bikes%', 700,   
    @ComparePrice OUT,   
    @Cost OUTPUT  
IF @Cost <= @ComparePrice   
BEGIN  
    PRINT 'These products can be purchased for less than   
    $'+RTRIM(CAST(@ComparePrice AS varchar(20)))+'.'  
END  
ELSE  
    PRINT 'The prices for all products in this category exceed   
    $'+ RTRIM(CAST(@ComparePrice AS varchar(20)))+'.';  
  
```  
  
 Voici le jeu de résultats partiel :  
  
```  
Product                                            List Price  
-------------------------------------------------- ------------------  
Road-750 Black, 58                                 539.99  
Mountain-500 Silver, 40                            564.99  
Mountain-500 Silver, 42                            564.99  
...  
Road-750 Black, 48                                 539.99  
Road-750 Black, 52                                 539.99  
  
(14 row(s) affected)  
  
These items can be purchased for less than $700.00.  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
  
