---
title: IN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- IN_TSQL
- IN
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], matching
- NOT IN keyword
- 8623 (Database Engine error)
- matching values in subquery or list [SQL Server]
- IN keyword
- 8632 (Database Engine error)
ms.assetid: 4419de73-96b1-4dfe-8500-f4507915db04
caps.latest.revision: 37
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f63b55304f883ebac069e36d7f91a5f2f372184c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="in-transact-sql"></a>IN (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Détermine si une valeur donnée correspond à la valeur d'une liste ou d'une sous-requête.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
test_expression [ NOT ] IN   
    ( subquery | expression [ ,...n ]  
    )   
```  
  
## <a name="arguments"></a>Arguments  
 *test_expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide.  
  
 *subquery*  
 Sous-requête avec un jeu de résultats d'une colonne. Cette colonne doit avoir le même type de données que *test_expression*.  
  
 *expression*[ **,**... *n* ]  
 Liste d'expressions utilisée pour vérifier une correspondance. Toutes les expressions doivent avoir le même type de données que *test_expression*.  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="result-value"></a>Valeur des résultats  
 Si la valeur de *test_expression* est égale à une valeur retournée par la *sous-requête* ou à l’une des *expressions* de la liste séparée par des virgules, le résultat est TRUE. Dans le cas contraire, ce résultat est FALSE.  
  
 L’utilisation de NOT IN inverse la valeur de la *sous-requête* ou l’*expression*.  
  
> [!CAUTION]  
>  Toute valeur Null retournée par la *sous-requête* ou l’*expression* comparée à *test_expression* en utilisant IN ou NOT IN retourne UNKNOWN. L'utilisation de valeurs Null avec IN ou NOT IN peut produire des résultats inattendus.  
  
## <a name="remarks"></a>Notes   
 Incluant explicitement un très grand nombre de valeurs (plusieurs milliers de valeurs séparées par des virgules) entre parenthèses, une clause IN peut consommer des ressources et retourner des erreurs 8623 ou 8632. Pour contourner ce problème, stockez les éléments dans la liste IN dans une table et utilisez une sous-requête SELECT dans une clause IN.  
  
 Error 8623 :  
  
 `The query processor ran out of internal resources and could not produce a query plan. This is a rare event and only expected for extremely complex queries or queries that reference a very large number of tables or partitions. Please simplify the query. If you believe you have received this message in error, contact Customer Support Services for more information.`  
  
 Erreur 8632 :  
  
 `Internal error: An expression services limit has been reached. Please look for potentially complex expressions in your query, and try to simplify them.`  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-comparing-or-and-in"></a>A. Comparaison des opérateurs OR et IN  
 L'exemple suivant sélectionne une liste des noms d'employés qui sont ingénieurs concepteurs, concepteurs d'outils ou assistants marketing.  
  
```  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle = 'Design Engineer'   
   OR e.JobTitle = 'Tool Designer'   
   OR e.JobTitle = 'Marketing Assistant';  
GO  
```  
  
 Vous obtenez toutefois les mêmes résultats en utilisant IN.  
  
```  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle IN ('Design Engineer', 'Tool Designer', 'Marketing Assistant');  
GO  
```  
  
 Voici le jeu de résultats obtenu pour l'une ou l'autre des requêtes.  
  
```  
FirstName   LastName      Title  
---------   ---------   ---------------------  
Sharon      Salavaria   Design Engineer                                     
Gail        Erickson    Design Engineer                                     
Jossef      Goldberg    Design Engineer                                     
Janice      Galvin      Tool Designer                                       
Thierry     D'Hers      Tool Designer                                       
Wanida      Benshoof    Marketing Assistant                                 
Kevin       Brown       Marketing Assistant                                 
Mary        Dempsey     Marketing Assistant                                 
  
(8 row(s) affected)  
```  
  
### <a name="b-using-in-with-a-subquery"></a>B. Utilisation de l'opérateur IN avec une sous-requête  
 L'exemple suivant recherche tous les ID des vendeurs dans la table `SalesPerson` pour les employés dont le quota de ventes annuel est supérieur à 250 000 $. Il sélectionne ensuite, dans la table `Employee`, les noms de tous les employés dont l'`EmployeeID` correspond aux résultats issus de la sous-requête `SELECT`.  
  
```  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
FirstName   LastName                                             
---------   --------   
Tsvi         Reiter                                              
Michael      Blythe                                              
Tete         Mensa-Annan                                         
  
(3 row(s) affected)  
```  
  
### <a name="c-using-not-in-with-a-subquery"></a>C. Utilisation de l'opérateur NOT IN avec une sous-requête  
 L'exemple suivant recherche les vendeurs qui n'ont pas de quota supérieur à 250 000 $. `NOT IN` identifie les vendeurs qui ne correspondent pas aux éléments de la liste de valeurs.  
  
```  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID NOT IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-in-and-not-in"></a>D. Utilisation des clauses IN et NOT IN  
 L’exemple suivant recherche toutes les entrées dans la table `FactInternetSales` qui correspondent à des valeurs `SalesReasonKey` dans la table `DimSalesReason`.  
  
```  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
IN (SELECT SalesReasonKey FROM DimSalesReason);   
```  
  
 L’exemple suivant recherche toutes les entrées dans la table `FactInternetSalesReason` qui ne correspondent pas à des valeurs `SalesReasonKey` dans la table `DimSalesReason`.  
  
```  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
NOT IN (SELECT SalesReasonKey FROM DimSalesReason);  
```  
  
### <a name="e-using-in-with-an-expression-list"></a>E. Utilisation de la clause IN avec une liste d’expressions  
 L’exemple suivant recherche tous les ID des vendeurs figurant dans la table `DimEmployee` dont le prénom est `Mike` ou `Michael`.  
  
```  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName  
FROM DimEmployee  
WHERE FirstName IN ('Mike', 'Michael');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)   
 [SOME &#124; ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md)  
  
  



