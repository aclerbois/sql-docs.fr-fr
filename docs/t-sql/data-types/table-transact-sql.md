---
title: table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 7/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- table data type [SQL Server]
- table variables [SQL Server]
ms.assetid: 1ef0b60e-a64c-4e97-847b-67930e3973ef
caps.latest.revision: 48
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1751c6c9b6a05cd4337bd376108ef8d7e3d84ea4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-transact-sql"></a>table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Type de données spécial qui peut être utilisé pour stocker un jeu de résultats afin de le traiter ultérieurement. **table** est principalement utilisé pour le stockage temporaire d’un ensemble de lignes retournées comme un jeu de résultats d’une fonction table. Les fonctions et les variables peuvent être déclarées comme étant de type **table**. Les variables de **table** peuvent être utilisées dans les fonctions, les procédures stockées et les lots. Pour déclarer des variables de type **table**, utilisez [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md).
  

**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
table_type_definition ::=   
    TABLE ( { <column_definition> | <table_constraint> } [ ,...n ] )   
  
<column_definition> ::=   
    column_name scalar_data_type   
    [ COLLATE <collation_definition> ]   
    [ [ DEFAULT constant_expression ] | IDENTITY [ ( seed , increment ) ] ]   
    [ ROWGUIDCOL ]   
    [ column_constraint ] [ ...n ]   
  
 <column_constraint> ::=   
    { [ NULL | NOT NULL ]   
    | [ PRIMARY KEY | UNIQUE ]   
    | CHECK ( logical_expression )   
    }   
  
<table_constraint> ::=   
     { { PRIMARY KEY | UNIQUE } ( column_name [ ,...n ] )  
     | CHECK ( logical_expression )   
     }   
```  
  
## <a name="arguments"></a>Arguments  
*table_type_definition*  
Même sous-ensemble d'informations que celui utilisé pour définir une table dans CREATE TABLE. La déclaration de table inclut des définitions de colonnes, des noms, des types de données et des contraintes. Les seuls types de contraintes autorisés sont PRIMARY KEY, UNIQUE KEY et NULL.  
Pour plus d’informations sur la syntaxe, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md), [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md) et [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).
  
*collation_definition*  
Classement de la colonne composé de paramètres régionaux [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows et d’un style de comparaison, de paramètres régionaux Windows et d’une notation binaire, ou d’un classement [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l’argument *collation_definition* n’est pas spécifié, la colonne hérite du classement de la base de données active. Ou, si la colonne est définie comme un type CLR (Common Language Runtime) défini par l'utilisateur, elle hérite du classement du type défini par l'utilisateur.
  
## <a name="remarks"></a>Notes   
Les variables de **table** peuvent être référencées par leur nom dans la clause FROM d’un lot, comme illustré dans l’exemple suivant :
  
```sql
SELECT Employee_ID, Department_ID FROM @MyTableVar;  
```  
  
En dehors d’une clause FROM, les variables de **table** doivent être référencées en utilisant un alias, comme illustré dans l’exemple suivant :
  
```sql
SELECT EmployeeID, DepartmentID   
FROM @MyTableVar m  
JOIN Employee on (m.EmployeeID =Employee.EmployeeID AND  
   m.DepartmentID = Employee.DepartmentID);  
```  
  
Les variables de **table** offrent les avantages suivants pour les requêtes à petite échelle dont les plans ne changent pas et quand les problèmes de recompilation sont primordiaux :
-   Une variable de **table** se comporte comme une variable locale. Elle possède une étendue bien définie. Il s'agit de la fonction, procédure stockée ou traitement dans lequel elle est déclarée.  
     Dans les limites de son étendue, une variable de **table** peut être utilisée comme une table normale. Elle peut s'appliquer partout où une table, ou expression de table, est utilisée dans les instructions SELECT, INSERT, UPDATE et DELETE. Cependant, une variable de **table** ne peut pas être utilisée dans l’instruction suivante :  
  
```sql
SELECT select_list INTO table_variable;
```
  
Les variables de **table** sont automatiquement nettoyées à la fin de la fonction, de la procédure stockée ou du lot dans lequel elles sont définies.
  
-   Les variables de **table** utilisées dans les procédures stockées provoquent moins de recompilations de procédures stockées par rapport aux tables temporaires utilisées quand il n’existe aucun choix basé sur les coûts qui affecte les performances.  
-   La durée de vie d’une transaction impliquant une variable de **table** est simplement égale à celle d’une mise à jour effectuée sur cette variable de **table**. De ce fait, les variables de **table** requièrent moins de ressources de verrouillage et de consignation.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions
Les variables de **table** n’ont pas de statistiques de distribution et ne déclencheront pas de recompilations. Par conséquent, dans de nombreux cas, l'optimiseur va créer un plan de requête en supposant que la variable de table n'a pas de lignes. Pour cette raison, soyez prudent lorsque vous utilisez une variable de table si vous attendez un nombre de lignes supérieur à 100. Les tables Temp peuvent être une meilleure solution dans ce cas. Sinon, pour les requêtes qui joignent la variable de table à d’autres tables, utilisez l’indicateur RECOMPILE, qui contraint l’optimiseur à utiliser la cardinalité correcte pour la variable de table.
  
Les variables de **table** ne sont pas prises en charge dans le modèle de raisonnement basé sur les coûts de l’optimiseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, elles ne doivent pas être utilisées lorsque des tableaux basés sur les coûts sont requis pour obtenir un plan de requête efficace. Les tables temporaires sont préférables lorsque des tableaux basés sur les coûts sont obligatoires. Cela inclut en général les requêtes avec jointures, les décisions de parallélisme et les options de sélection d'index.
  
Les requêtes qui modifient des variables de **table** ne génèrent pas de plans d’exécution parallèles. Les performances peuvent être affectées quand des variables de **table** de grande taille ou des variables de **table** figurant dans des requêtes complexes sont modifiées. Dans ces situations, envisagez d'utiliser des tables temporaires à la place. Pour plus d’informations, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md). Il est toujours possible d’effectuer une mise en parallèle des requêtes qui lisent des variables de **table** sans les modifier.
  
Il est impossible de créer explicitement des index sur des variables de **table** et aucune statistique n’est conservée sur les variables de **table**. Dans ces situations, les performances peuvent s'améliorer en utilisant les tables temporaires à la place, car elles prennent en charge les index et les statistiques. Pour plus d’informations sur les tables temporaires, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).
  
Les contraintes CHECK, les valeurs DEFAULT et les colonnes calculées dans la déclaration de type **table** ne peuvent pas appeler des fonctions définies par l’utilisateur.
  
L’opération d’affectation entre les variables de **table** n’est pas prise en charge.
  
Étant donné que les variables de **table** ont une étendue limitée et qu’elles ne font pas partie de la base de données persistante, elles ne sont pas affectées par les restaurations de transactions.
  
Les variables de table ne peuvent pas être modifiées après la création.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-declaring-a-variable-of-type-table"></a>A. Déclaration d'une variable de type table  
L'exemple suivant crée une variable `table` qui stocke les valeurs définies dans la clause OUTPUT de l'instruction UPDATE. Deux instructions `SELECT` suivent ; elles retournent les valeurs dans `@MyTableVar`, ainsi que les résultats de la mise à jour dans la table `Employee`. Notez que les résultats dans la colonne `INSERTED.ModifiedDate` sont différents des valeurs de la colonne `ModifiedDate` dans la table `Employee`. Ceci s'explique par le fait que le déclencheur `AFTER UPDATE`, qui met à jour la valeur de `ModifiedDate` en fonction de la date actuelle, est défini sur la table `Employee`. Toutefois, les colonnes renvoyées par `OUTPUT` reflètent les données avant l'activation des déclencheurs. Pour plus d’informations, consultez [Clause OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar table(  
    EmpID int NOT NULL,  
    OldVacationHours int,  
    NewVacationHours int,  
    ModifiedDate datetime);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25   
OUTPUT INSERTED.BusinessEntityID,  
       DELETED.VacationHours,  
       INSERTED.VacationHours,  
       INSERTED.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
--Note that ModifiedDate reflects the value generated by an  
--AFTER UPDATE trigger.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
### <a name="b-creating-an-inline-table-valued-function"></a>B. Création d'une fonction table incluse  
L'exemple suivant retourne une fonction table incluse. Pour chaque produit vendu au magasin, il retourne trois colonnes : `ProductID`, `Name` et l’agrégat des totaux annuels par magasin sous `YTD Total`.
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
GO  
```  
  
Pour appeler la fonction, exécutez la requête suivante :
  
```sql
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
## <a name="see-also"></a>Voir aussi
[COLLATE &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/4ba6b7d8-114a-4f4e-bb38-fe5697add4e9)  
[CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)  
[Fonctions définies par l'utilisateur](../../relational-databases/user-defined-functions/user-defined-functions.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)  
[Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)
  
  
