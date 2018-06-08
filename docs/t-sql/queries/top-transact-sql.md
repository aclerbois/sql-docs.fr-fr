---
title: TOP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|queries
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- TOP_TSQL
- TOP
dev_langs:
- TSQL
helpviewer_keywords:
- TOP clause
- first set of query result rows [SQL Server]
- TOP clause, about TOP clause
- queries [SQL Server], results
ms.assetid: da983c0a-06c5-4cf8-a6a4-7f9d66f34f2c
caps.latest.revision: 60
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 26eda1bce8b9f7775b2cada2e9633115020e94fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="top-transact-sql"></a>TOP (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Limite les lignes retournées dans un jeu de résultats de la requête à un nombre spécifié de lignes ou à un pourcentage de lignes dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Quand TOP est utilisé conjointement à la clause ORDER BY, le jeu de résultats est limité au *N* premières lignes ordonnées ; sinon, il retourne les *N* premières lignes dans un ordre aléatoire. Utilisez cette clause pour spécifier le nombre de lignes retournées par une instruction SELECT ou affectées par une instruction INSERT, UPDATE, MERGE ou DELETE.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
[   
    TOP (expression) [PERCENT]  
    [ WITH TIES ]  
]  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
[   
    TOP ( expression )   
    [ WITH TIES ]  
]  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Expression numérique qui définit le nombre de lignes renvoyées. *expression* est converti implicitement en valeur **float** si PERCENT est défini ; sinon, il est converti en **bigint**.  
  
 PERCENT  
 Indique que la requête retourne seulement les premiers *expression* % des lignes du jeu de résultats. Les valeurs fractionnaires sont arrondies à la valeur entière suivante.  
  
 WITH TIES  
 Utilisé lorsque vous voulez retourner deux ou plusieurs lignes liées pour le dernier emplacement dans le jeu de résultats limité. Doit être utilisé avec la clause **ORDER BY**. **WITH TIES** peut entraîner le retour d'un nombre de lignes supérieur à la valeur spécifiée dans *expression*. Par exemple, si *expression* est défini sur 5, mais que 2 lignes supplémentaires correspondent aux valeurs des colonnes **ORDER BY** dans la ligne 5, le jeu de résultats contient 7 lignes.  
  
 TOP...WITH TIES peut être défini uniquement dans les instructions SELECT, et seulement si une clause ORDER BY est spécifiée. L'ordre retourné pour la liaison des enregistrements est arbitraire. ORDER BY n'affecte pas cette règle.  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Dans une instruction SELECT, utilisez toujours une clause ORDER BY avec la clause TOP. Il s'agit de la seule méthode permettant d'indiquer de manière prévisible les lignes qui sont affectées par TOP.  
  
 Utilisez OFFSET et FETCH dans la clause ORDER BY au lieu de la clause TOP pour implémenter une solution de pagination de requête. Une solution de pagination (autrement dit, l'envoi de segments ou de « pages » de données au client) est plus facile à implémenter à l'aide des clauses OFFSET et FETCH. Pour plus d’informations, consultez [Clause ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
 Utilisez TOP (ou OFFSET et FETCH) au lieu de SET ROWCOUNT pour limiter le nombre de lignes retournées. Ces méthodes sont préférables à l'utilisation de SET ROWCOUNT pour les raisons suivantes :  
  
-   Dans le cadre d'une instruction SELECT, l'optimiseur de requête peut considérer la valeur *d’expression* dans les clauses TOP ou FETCH pendant l'optimisation des requêtes. Étant donné que SET ROWCOUNT est utilisé en dehors d'une instruction qui exécute une requête, sa valeur ne peut pas être prise en compte dans un plan de requête.  
  
## <a name="compatibility-support"></a>Prise en charge de la compatibilité  
 Pour des raisons de compatibilité descendante, les parenthèses sont facultatives dans les instructions SELECT. Nous vous recommandons de toujours utiliser des parenthèses pour TOP dans les instructions SELECT pour des raisons de cohérence, l'utilisation des parenthèses étant obligatoire dans les instructions INSERT, UPDATE, MERGE et DELETE.  
  
## <a name="interoperability"></a>Interopérabilité  
 L'expression TOP n'affecte pas les instructions qui peuvent être exécutées en raison d'un déclencheur. Les tables **inserted** et **deleted** dans les déclencheurs retournent seulement les lignes réellement affectées par les instructions INSERT, UPDATE, MERGE ou DELETE. Par exemple, si un INSERT TRIGGER est déclenché comme résultat d'une instruction INSERT qui a utilisé une clause TOP,  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tient compte de la mise à jour de lignes via des vues. La clause TOP pouvant être incluse dans la définition de la vue, des lignes peuvent disparaître de la vue à la suite d'une mise à jour si les lignes ne répondent plus aux conditions de l'expression TOP.  
  
 Quand elle est spécifiée dans l'instruction MERGE, la clause TOP est appliquée *après* la jointure de l'intégralité de la table source et de la table cible, et après la suppression des lignes jointes qui ne sont pas éligibles pour une action d'insertion, de mise à jour ou de suppression. La clause TOP réduit le nombre de lignes jointes à la valeur spécifiée et les actions INSERT, UPDATE ou DELETE sont appliquées aux lignes jointes restantes sans respecter un ordre particulier. Les lignes ne sont donc pas réparties selon un ordre particulier dans le cadre des actions définies dans les clauses WHEN. Par exemple, la spécification de la clause TOP (10) affecte 10 lignes, dont 7 peuvent être mises à jour et 3 insérées, ou alors 1 ligne peut être supprimée, 5 mises à jour et 4 insérées, et ainsi de suite. Étant donné que l'instruction MERGE effectue une analyse complète des tables source et cible, les performances d'E/S peuvent être affectées lorsque la clause TOP est utilisée pour modifier une table volumineuse en créant plusieurs lots. Dans ce scénario, il est important de s'assurer que tous les lots consécutifs ciblent les nouvelles lignes.  
  
 Soyez vigilant lors de la spécification de la clause TOP dans une requête qui contient un opérateur UNION, UNION ALL, EXCEPT ou INTERSECT. Il est possible d'écrire une requête qui retourne des résultats inattendus car l'ordre dans lequel les clauses TOP et ORDER BY sont traitées logiquement n'est pas toujours intuitif lorsque ces opérateurs sont utilisés dans une opération de sélection. Par exemple, d'après le tableau et les données qui suivent, supposez que vous souhaitez renvoyer la voiture rouge la moins chère et la voiture bleue la moins chère. Autrement dit, la berline rouge et le fourgon bleu.  
  
```sql  
CREATE TABLE dbo.Cars(Model varchar(15), Price money, Color varchar(10));  
INSERT dbo.Cars VALUES  
    ('sedan', 10000, 'red'), ('convertible', 15000, 'blue'),   
    ('coupe', 20000, 'red'), ('van', 8000, 'blue');  
```  
  
 Pour obtenir ces résultats, vous pouvez écrire la requête suivante.  
  
```sql  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'red'  
UNION ALL  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'blue'  
ORDER BY Price ASC;  
GO    
```  
  
 Voici l'ensemble des résultats.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 convertible   blue       15000.00
 ```  
  
 Des résultats inattendus sont retournés car la clause TOP est exécutée logiquement avant la clause ORDER BY, qui trie les résultats de l'opérateur (UNION ALL dans ce cas). Par conséquent, la requête précédente retourne toute voiture rouge et toute voiture bleue, puis classe le résultat de cette union d'après le prix. L'exemple suivant affiche la méthode correcte de l'écriture de cette requête pour obtenir le résultat désiré.  
  
```sql  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'red'  
      ORDER BY Price ASC) AS a  
UNION ALL  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'blue'  
      ORDER BY Price ASC) AS b;  
GO    
```  
  
 En utilisant TOP et ORDER BY dans une opération de sous-sélection, vous garantissez que les résultats de la clause ORDER BY sont utilisés par application à la clause TOP et pas au tri du résultat de l'opération UNION.  
  
 Voici l'ensemble des résultats.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 van           blue        8000.00
 ```  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Lorsque TOP est utilisé avec INSERT, UPDATE, MERGE ou DELETE, les lignes référencées ne sont pas réorganisées dans un ordre particulier et la clause ORDER BY ne peut pas être spécifiée directement dans ces instructions. Si vous devez utiliser TOP pour insérer, supprimer ou modifier des lignes dans un ordre chronologique explicite, vous devez utiliser TOP avec une clause ORDER BY spécifiée dans une instruction de sous-sélection. Consultez la section Exemples plus loin dans cette rubrique.  
  
 TOP ne peut pas être utilisé dans des instructions UPDATE et DELETE sur des vues partitionnées.  
  
 TOP ne peut pas être combiné avec OFFSET et FETCH dans la même expression de requête (dans la même étendue de requête). Pour plus d’informations, consultez [Clause ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
|Catégorie|Éléments syntaxiques proposés|  
|--------------|------------------------------|  
|[Syntaxe de base](#BasicSyntax)|TOP • PERCENT|  
|[Y compris les valeurs de lien](#tie)|WITH TIES|  
|[Limitation des lignes affectées par DELETE, INSERT ou UPDATE](#DML)|DELETE • INSERT • UPDATE|  
  
###  <a name="BasicSyntax"></a> Syntaxe de base  
 Les exemples fournis dans cette section présentent les fonctionnalités de base de la clause ORDER BY en utilisant la syntaxe minimale requise.  
  
#### <a name="a-using-top-with-a-constant-value"></a>A. Utilisation de TOP avec une valeur constante  
 Les exemples suivants utilisent une valeur constante pour spécifier le nombre d'employés retournés dans le jeu de résultats de la requête. Dans le premier exemple, les 10 premières lignes non définies sont retournées en l'absence d'utilisation d'une clause ORDER BY. Dans le deuxième exemple, une clause ORDER BY est utilisée pour retourner les 10 premiers employés embauchés récemment.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Select the first 10 random employees.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee;  
GO  
-- Select the first 10 employees hired most recently.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO  
```  
  
#### <a name="b-using-top-with-a-variable"></a>B. Utilisation de TOP avec une variable  
 L'exemple suivant utilise une variable pour spécifier le nombre d'employés retournés dans le jeu de résultats de la requête.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @p AS int = 10;  
SELECT TOP(@p)JobTitle, HireDate, VacationHours  
FROM HumanResources.Employee  
ORDER BY VacationHours DESC;  
GO  
```  
  
#### <a name="c-specifying-a-percentage"></a>C. Spécification d'un pourcentage  
 L'exemple suivant utilise PERCENT pour spécifier le nombre d'employés retournés dans le jeu de résultats de la requête. Il y a 290 employés dans la table `HumanResources.Employee`. Étant donné que 5 pour cent de 290 est une valeur fractionnaire, la valeur est arrondie au nombre entier suivant.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(5)PERCENT JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO    
```  
  
###  <a name="tie"></a> Y compris les valeurs de lien  
  
#### <a name="a-using-with-ties-to-include-rows-that-match-the-values-in-the-last-row"></a>A. Utilisation de WITH TIES pour inclure des lignes qui correspondent aux valeurs dans la dernière ligne  
 L'exemple suivant obtient les `10` % des employés ayant le salaire le plus élevé et les retourne dans l'ordre décroissant en fonction de leur salaire. En définissant `WITH TIES`, vous incluez également dans le jeu de résultats les employés dont le salaire est égal au salaire retourné le plus faible (dernière ligne), même si cette opération entraîne un dépassement du seuil fixé de `10` % des employés.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10)WITH TIES  
pp.FirstName, pp.LastName, e.JobTitle, e.Gender, r.Rate  
FROM Person.Person AS pp   
    INNER JOIN HumanResources.Employee AS e  
        ON pp.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeePayHistory AS r  
        ON r.BusinessEntityID = e.BusinessEntityID  
ORDER BY Rate DESC;  
GO    
```  
  
###  <a name="DML"></a> Limitation des lignes affectées par DELETE, INSERT ou UPDATE  
  
#### <a name="a-using-top-to-limit-the-number-of-rows-deleted"></a>A. Utilisation de TOP pour limiter le nombre de lignes supprimées  
 Quand une clause TOP (*n*) est utilisée avec DELETE, l’opération de suppression est effectuée sur une sélection non définie de *n* lignes. Autrement dit, l’instruction DELETE choisit un nombre (*n*) de lignes qui répondent aux critères définis dans la clause WHERE. L'exemple suivant supprime `20` lignes de la table `PurchaseOrderDetail` dont la date d'échéance est antérieure au 1er juillet 2002.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
 Si vous devez utiliser une clause TOP pour supprimer des lignes dans un ordre chronologique significatif, vous devez associer à cette clause TOP une clause ORDER BY dans une instruction de sous-sélection. La requête suivante supprime les 10 lignes de la table `PurchaseOrderDetail` dont la date d'expiration est la plus proche. Pour garantir que seules 10 lignes sont supprimées, la colonne spécifiée dans l'instruction de sous-sélection (`PurchaseOrderID`) constitue la clé primaire de la table. L'utilisation d'une colonne non-clé dans l'instruction de sous-sélection peut entraîner la suppression de plus de 10 lignes si la colonne spécifiée contient des valeurs dupliquées.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
#### <a name="b-using-top-to-limit-the-number-of-rows-inserted"></a>B. Utilisation de TOP pour limiter le nombre de lignes insérées  
 L'exemple suivant crée la table `EmployeeSales` et insère le nom et les données de ventes de l'année des 5 premiers employés de la table `HumanResources.Employee`. L'instruction INSERT choisit 5 lignes retournées par l'instruction `SELECT` qui répondent aux critères définis dans la clause WHERE.  La clause OUTPUT affiche les lignes insérées dans la table `EmployeeSales`. Notez que la clause ORDER BY dans l'instruction SELECT n'est pas utilisée pour déterminer les 5 premiers employés.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   nvarchar(11) NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  YearlySales  money NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
 Si vous devez utiliser une clause TOP pour insérer des lignes dans un ordre chronologique significatif, vous devez associer à cette clause TOP une clause ORDER BY dans une instruction de sous-sélection, comme illustré dans l'exemple suivant. La clause OUTPUT affiche les lignes insérées dans la table `EmployeeSales`. Notez que les 5 premiers employés sont maintenant insérés selon les résultats de la clause ORDER BY au lieu de lignes non définies.  
  
```sql  
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
#### <a name="c-using-top-to-limit-the-number-of-rows-updated"></a>C. Utilisation de TOP pour limiter le nombre de lignes mises à jour  
 L'exemple suivant utilise la clause TOP pour mettre à jour des lignes dans une table. Quand une clause TOP (*n*) est utilisée avec UPDATE, l’opération de mise à jour est effectuée sur une sélection non définie de lignes. Autrement dit, l’instruction UPDATE choisit un nombre (*n*) de lignes qui répondent aux critères définis dans la clause WHERE. L'exemple suivant retire 10 clients à un vendeur et les attribue à un autre vendeur.  
  
```sql  
USE AdventureWorks2012;  
UPDATE TOP (10) Sales.Store  
SET SalesPersonID = 276  
WHERE SalesPersonID = 275;  
GO  
```  
  
 Si vous devez utiliser la clause TOP pour appliquer des mises à jour en respectant une certaine chronologie, vous devez combiner les clauses TOP et ORDER BY dans une instruction de sous-sélection. L'exemple ci-dessous met à jour les heures de congé des 10 employés dont la date d'embauche est la plus ancienne.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant retourne les 31 premières lignes qui correspondent aux critères de la requête. La clause **ORDER BY** est utilisée pour vérifier que le 31 lignes retournées sont les 31 premières lignes dans l’ordre alphabétique de la colonne `LastName`.  
  
 Utilisation de **TOP** sans spécifier de liens.  
  
```sql  
SELECT TOP (31) FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
 Résultat : 31 lignes sont retournées.  
  
 Utilisation de TOP en spécifiant WITH TIES.  
  
```sql  
SELECT TOP (31) WITH TIES FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
 Résultat : 33 lignes sont retournées, car 3 employés du nom de Brown sont liés aux à la ligne 31.  
  
## <a name="see-also"></a> Voir aussi  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [Clause ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  
  
 
