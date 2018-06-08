---
title: Retour de données à partir d’une procédure stockée | Microsoft Docs
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
- stored procedures [SQL Server], returning data
- returning data from stored procedure
ms.assetid: 7a428ffe-cd87-4f42-b3f1-d26aa8312bf7
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4fdedc0d70a226547c80c2fadbe418401814fd6d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="return-data-from-a-stored-procedure"></a>Retour de données à partir d'une procédure stockée
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [Retour de données à partir d’une procédure stockée](https://msdn.microsoft.com/en-US/library/ms188655(SQL.120).aspx).

  Il existe trois méthodes permettant de retourner des données depuis une procédure vers un programme appelant : les jeux de résultats, les paramètres de sortie et les codes de retour. Cette rubrique fournit des informations sur les trois approches.  
  
  ## <a name="returning-data-using-result-sets"></a>Retour de données avec des jeux de résultats
 Si vous incluez une instruction SELECT dans le corps d’une procédure stockée (mais pas une instruction SELECT... INTO ou INSERT... SELECT), les lignes spécifiées par l’instruction SELECT sont envoyées directement au client.  Pour les jeux de résultats volumineux, l’exécution de la procédure stockée ne passe pas à l’instruction tant que le jeu de résultat n’a pas été entièrement envoyé au client.  Pour les jeux de résultats de petite taille, les résultats sont spoulés pour être retournés au client et l’exécution se poursuit.  Si plusieurs de ces instructions SELECT sont exécutées pendant l’exécution de la procédure stockée, plusieurs jeux de résultats sont envoyés au client.  Ce comportement s’applique également aux lots TSQL imbriqués, aux procédures stockées imbriquées et aux lots TSQL du plus haut niveau.
 
 
 ### <a name="examples-of-returning-data-using-a-result-set"></a>Exemples de retour de données avec un jeu de résultats 
  L’exemple suivant montre une procédure stockée qui retourne les valeurs LastName et SalesYTD pour toutes les lignes de SalesPerson qui apparaissent aussi dans la vue vEmployee.
  
 ```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.uspGetEmployeeSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.uspGetEmployeeSalesYTD;  
GO  
CREATE PROCEDURE Sales.uspGetEmployeeSalesYTD  
AS    
  
    SET NOCOUNT ON;  
    SELECT LastName, SalesYTD  
    FROM Sales.SalesPerson AS sp  
    JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
    
RETURN  
GO  
  
```  

  
## <a name="returning-data-using-an-output-parameter"></a>Retour de données à l'aide d'un paramètre de sortie  
 Si vous spécifiez le mot clé OUTPUT pour un paramètre dans la définition de procédure, la procédure peut retourner la valeur actuelle du paramètre au programme appelant lors de la sortie de la procédure. Pour enregistrer la valeur du paramètre dans une variable afin que le programme appelant puisse l'utiliser, ce dernier doit inclure le mot clé OUTPUT lorsqu'il exécute la procédure. Pour plus d’informations sur les types de données qui peuvent être utilisés comme paramètres de sortie, consultez [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
### <a name="examples-of-output-parameter"></a>Exemples de paramètre de sortie  
 L'exemple ci-dessous illustre une procédure avec un paramètre d'entrée et un paramètre de sortie. Le paramètre `@SalesPerson` doit recevoir une valeur d'entrée spécifiée par le programme appelant. L’instruction SELECT utilise la valeur passée dans le paramètre d’entrée pour obtenir la valeur `SalesYTD` correcte. L’instruction SELECT affecte également la valeur au paramètre de sortie `@SalesYTD` , qui retourne la valeur au programme appelant quand la procédure se termine.  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.uspGetEmployeeSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.uspGetEmployeeSalesYTD;  
GO  
CREATE PROCEDURE Sales.uspGetEmployeeSalesYTD  
@SalesPerson nvarchar(50),  
@SalesYTD money OUTPUT  
AS    
  
    SET NOCOUNT ON;  
    SELECT @SalesYTD = SalesYTD  
    FROM Sales.SalesPerson AS sp  
    JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
    WHERE LastName = @SalesPerson;  
RETURN  
GO  
  
```  
  
 L’exemple suivant appelle la procédure créée dans le premier exemple et enregistre la valeur de sortie retournée par la procédure appelée dans la variable `@SalesYTD` , qui est locale pour le programme appelant.  
  
```  
-- Declare the variable to receive the output value of the procedure.  
DECLARE @SalesYTDBySalesPerson money;  
-- Execute the procedure specifying a last name for the input parameter  
-- and saving the output value in the variable @SalesYTDBySalesPerson  
EXECUTE Sales.uspGetEmployeeSalesYTD  
    N'Blythe', @SalesYTD = @SalesYTDBySalesPerson OUTPUT;  
-- Display the value returned by the procedure.  
PRINT 'Year-to-date sales for this employee is ' +   
    convert(varchar(10),@SalesYTDBySalesPerson);  
GO  
  
```  
  
 Des valeurs d'entrée peuvent également être définies pour les paramètres OUTPUT lorsque la procédure est exécutée. Ainsi, la procédure peut recevoir une valeur du programme appelant, la modifier ou l'utiliser pour exécuter des opérations, puis retourner la nouvelle valeur au programme appelant. Dans l’exemple précédent, la variable `@SalesYTDBySalesPerson` peut recevoir une valeur avant que le programme appelle la procédure `Sales.uspGetEmployeeSalesYTD` . L’instruction d’exécution passe alors la valeur de la variable `@SalesYTDBySalesPerson` au paramètre OUTPUT `@SalesYTD` . Ensuite, dans le corps de la procédure, la valeur peut être utilisée pour des calculs qui génèrent une nouvelle valeur. La nouvelle valeur est repassée hors de la procédure par le paramètre OUTPUT, mettant à jour la valeur dans la variable `@SalesYTDBySalesPerson` quand la procédure se termine. Ce mécanisme est souvent appelé « capacité de passage par référence ».  
  
 Si vous spécifiez OUTPUT pour un paramètre lorsque vous appelez une procédure alors que le paramètre n'est pas défini avec OUTPUT dans la définition de la procédure, vous obtiendrez un message d'erreur. Il est néanmoins possible d'exécuter une procédure avec des paramètres output et de ne pas spécifier OUTPUT lors de l'exécution de la procédure. Aucune erreur n'est retournée, mais vous ne pouvez pas utiliser la valeur de sortie dans le programme appelant.  
  
### <a name="using-the-cursor-data-type-in-output-parameters"></a>Utilisation du type de données Cursor dans des paramètres OUTPUT  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] Les procédures ne peuvent utiliser le type de données **cursor** que pour les paramètres OUTPUT. Si le type de données **cursor** est spécifié pour un paramètre, les mots clés VARYING et OUTPUT doivent être spécifiés pour ce paramètre dans la définition de la procédure. Un paramètre peut être spécifié comme OUTPUT uniquement, mais si le mot clé VARYING est spécifié dans la déclaration du paramètre, le type de données doit obligatoirement être **cursor** et vous devez également préciser le mot clé OUTPUT.  
  
> [!NOTE]  
>  Le type de données **cursor** ne peut pas être lié à des variables d’application par l’intermédiaire des API de base de données, telles que OLE DB, ODBC, ADO et DB-Library. Les paramètres OUTPUT devant être liés avant qu’une application puisse exécuter une procédure, les procédures qui contiennent des paramètres OUTPUT de type **cursor** ne peuvent pas être appelées à partir des API de base de données. Ces procédures peuvent être appelées à partir de traitements, procédures ou déclencheurs [!INCLUDE[tsql](../../includes/tsql-md.md)] seulement quand la variable OUTPUT de type **cursor** est affectée à une variable [!INCLUDE[tsql](../../includes/tsql-md.md)] locale de type **cursor** .  
  
### <a name="rules-for-cursor-output-parameters"></a>Règles pour les paramètres de sortie de curseur  
 Les règles suivantes régissent les paramètres de sortie de type **cursor** lors de l’exécution de la procédure :  
  
-   Dans le cas d'un curseur avant uniquement, les lignes renvoyées dans le jeu de résultats du curseur sont seulement celles situées au niveau de la position du curseur ou au-delà de celui-ci, à la fin de la procédure, par exemple :  
  
    -   Un curseur ne permettant pas le défilement est ouvert dans une procédure, dans un jeu de résultats de 100 lignes, appelé RS.  
  
    -   La procédure extrait les 5 premières lignes du jeu de résultats RS.  
  
    -   La procédure est renvoyée vers son appelant.  
  
    -   Le jeu de résultats RS renvoyé à l'appelant est constitué des lignes 6 à 100 de RS et le curseur dans l'appelant est placé avant la première ligne de RS.  
  
-   Dans le cas d'un curseur avant uniquement, si le curseur se trouve avant la première ligne lorsque la procédure existe, la totalité du jeu de résultats est renvoyée au traitement d'instructions, à la procédure ou au déclencheur appelant. Lorsqu'elle est renvoyée, la position du curseur est fixée au début de la première ligne.  
  
-   Dans le cas d'un curseur avant uniquement, si le curseur est placé au-delà de la fin de la dernière ligne lorsque la procédure existe, le système renvoie un jeu de résultats vide au traitement d'instructions, à la procédure ou au déclencheur appelant.  
  
    > [!NOTE]  
    >  Un jeu de résultats vide est différent d'une valeur NULL.  
  
-   Dans le cas d'un curseur permettant le défilement, toutes les lignes du jeu de résultats sont renvoyées au traitement d'instructions, à la procédure ou au déclencheur appelant lorsque la procédure existe. Lors du renvoi, la position du curseur est conservée à la position de la dernière extraction exécutée dans la procédure.  
  
-   Pour tous les types de curseur, si le curseur est fermé, une valeur NULL est repassée au traitement d'instructions, à la procédure ou au déclencheur appelant. Cette situation se produit également si un curseur est affecté à un paramètre mais qu'il n'a jamais été ouvert.  
  
    > [!NOTE]  
    >  L'état fermé n'a d'importance qu'au moment du retour. Par exemple, vous pouvez fermer un curseur au cours de l'exécution de la procédure, le rouvrir plus tard dans la procédure et renvoyer le jeu de résultats de ce curseur au traitement d'instructions, à la procédure ou au déclencheur appelant.  
  
### <a name="examples-of-cursor-output-parameters"></a>Exemples de paramètres de sortie de curseur  
 L’exemple ci-dessous crée une procédure avec un paramètre de sortie `@currency_cursor` utilisant le type de données **cursor**. La procédure stockée est ensuite appelée dans un traitement.  
 
 Commencez par créer la procédure qui déclare puis ouvre un curseur dans la table Currency.  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'dbo.uspCurrencyCursor', 'P' ) IS NOT NULL  
    DROP PROCEDURE dbo.uspCurrencyCursor;  
GO  
CREATE PROCEDURE dbo.uspCurrencyCursor   
    @CurrencyCursor CURSOR VARYING OUTPUT  
AS  
    SET NOCOUNT ON;  
    SET @CurrencyCursor = CURSOR  
    FORWARD_ONLY STATIC FOR  
      SELECT CurrencyCode, Name  
      FROM Sales.Currency;  
    OPEN @CurrencyCursor;  
GO  
  
```  
  
 Ensuite, exécutez un traitement d'instructions qui déclare une variable curseur locale, exécute la procédure pour affecter le curseur à la variable locale et extrait les lignes du curseur.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @MyCursor CURSOR;  
EXEC dbo.uspCurrencyCursor @CurrencyCursor = @MyCursor OUTPUT;  
WHILE (@@FETCH_STATUS = 0)  
BEGIN;  
     FETCH NEXT FROM @MyCursor;  
END;  
CLOSE @MyCursor;  
DEALLOCATE @MyCursor;  
GO  
  
```  
  
## <a name="returning-data-using-a-return-code"></a>Renvoi de données au moyen d'un code de retour  
 Une procédure peut retourner une valeur entière appelée « code de retour » pour indiquer l'état d'exécution d'une procédure. Le code de retour d'une procédure se définit au moyen de l'instruction RETURN. Comme dans le cas des paramètres OUTPUT, vous devez enregistrer le code de retour dans une variable lors de l'exécution de la procédure afin de pouvoir utiliser sa valeur dans le programme appelant. Par exemple, la variable d’attribution `@result` , de type de données **int** , sert à stocker le code de retour de la procédure `my_proc`, de sorte que :  
  
```  
DECLARE @result int;  
EXECUTE @result = my_proc;  
```  
  
 Les codes de retour sont couramment utilisés dans les blocs de contrôle de flux des procédures pour définir la valeur du code de retour pour chaque situation d'erreur possible. Vous pouvez utiliser la fonction @@ERROR après une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] pour détecter si une erreur s’est produite pendant l’exécution de l’instruction.  Avant l’introduction de la gestion des erreurs TRY/CATCH/THROW dans TSQL, les codes de retour étaient parfois nécessaires pour déterminer la réussite ou l’échec des procédures stockées.  Les procédures stockées doivent toujours indiquer un échec avec une erreur (générée avec THROW/RAISERROR si nécessaire) et ne pas utiliser un code de retour pour indiquer l’échec.  Il est également recommandé d’éviter d’utiliser le code de retour pour retourner des données d’application.
  
### <a name="examples-of-return-codes"></a>Exemples de codes de retour  
 L'exemple suivant illustre la procédure `usp_GetSalesYTD` de gestion d'erreurs qui définit les valeurs du code de retour pour diverses erreurs. Le tableau suivant montre la valeur entière attribuée par la procédure à chaque erreur possible, et la signification correspondante pour chaque valeur.  
  
|Valeur du code de retour|Signification|  
|-----------------------|-------------|  
|0|Exécution réussie.|  
| 1|La valeur du paramètre nécessaire n'est pas spécifiée.|  
|2|Valeur du paramètre spécifiée non valide.|  
|3|Erreur lors de l'obtention de la valeur des ventes.|  
|4|Valeur des ventes NULL trouvée pour le vendeur.|  
  
```  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.usp_GetSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.usp_GetSalesYTD;  
GO  
CREATE PROCEDURE Sales.usp_GetSalesYTD  
@SalesPerson nvarchar(50) = NULL,  -- NULL default value  
@SalesYTD money = NULL OUTPUT  
AS    
  
-- Validate the @SalesPerson parameter.  
IF @SalesPerson IS NULL  
   BEGIN  
       PRINT 'ERROR: You must specify a last name for the sales person.'  
       RETURN(1)  
   END  
ELSE  
   BEGIN  
   -- Make sure the value is valid.  
   IF (SELECT COUNT(*) FROM HumanResources.vEmployee  
          WHERE LastName = @SalesPerson) = 0  
      RETURN(2)  
   END  
-- Get the sales for the specified name and   
-- assign it to the output parameter.  
SELECT @SalesYTD = SalesYTD   
FROM Sales.SalesPerson AS sp  
JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
WHERE LastName = @SalesPerson;  
-- Check for SQL Server errors.  
IF @@ERROR <> 0   
   BEGIN  
      RETURN(3)  
   END  
ELSE  
   BEGIN  
   -- Check to see if the ytd_sales value is NULL.  
     IF @SalesYTD IS NULL  
       RETURN(4)   
     ELSE  
      -- SUCCESS!!  
        RETURN(0)  
   END  
-- Run the stored procedure without specifying an input value.  
EXEC Sales.usp_GetSalesYTD;  
GO  
-- Run the stored procedure with an input value.  
DECLARE @SalesYTDForSalesPerson money, @ret_code int;  
-- Execute the procedure specifying a last name for the input parameter  
-- and saving the output value in the variable @SalesYTD  
EXECUTE Sales.usp_GetSalesYTD  
    N'Blythe', @SalesYTD = @SalesYTDForSalesPerson OUTPUT;  
PRINT N'Year-to-date sales for this employee is ' +  
    CONVERT(varchar(10), @SalesYTDForSalesPerson);  
  
```  
  
 L'exemple suivant crée un programme chargé de gérer les codes de retour retournés par la procédure `usp_GetSalesYTD` .  
  
```  
-- Declare the variables to receive the output value and return code   
-- of the procedure.  
DECLARE @SalesYTDForSalesPerson money, @ret_code int;  
  
-- Execute the procedure with a title_id value  
-- and save the output value and return code in variables.  
EXECUTE @ret_code = Sales.usp_GetSalesYTD  
    N'Blythe', @SalesYTD = @SalesYTDForSalesPerson OUTPUT;  
--  Check the return codes.  
IF @ret_code = 0  
BEGIN  
   PRINT 'Procedure executed successfully'  
   -- Display the value returned by the procedure.  
   PRINT 'Year-to-date sales for this employee is ' + CONVERT(varchar(10),@SalesYTDForSalesPerson)  
END  
ELSE IF @ret_code = 1  
   PRINT 'ERROR: You must specify a last name for the sales person.'  
ELSE IF @ret_code = 2   
   PRINT 'EERROR: You must enter a valid last name for the sales person.'  
ELSE IF @ret_code = 3  
   PRINT 'ERROR: An error occurred getting sales value.'  
ELSE IF @ret_code = 4  
   PRINT 'ERROR: No sales recorded for this employee.'     
GO  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [PRINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/print-transact-sql.md)   
 [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)   
 [Curseurs](../../relational-databases/cursors.md)   
 [RETURN &#40;Transact-SQL&#41;](../../t-sql/language-elements/return-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  
