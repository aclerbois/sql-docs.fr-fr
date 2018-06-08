---
title: DENSE_RANK (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DENSE_RANK_TSQL
- DENSE_RANK
dev_langs:
- TSQL
helpviewer_keywords:
- row ranking [SQL Server]
- DENSE_RANK function
- tied rows [SQL Server]
- ranking rows
ms.assetid: 03871fc6-9592-4016-b0b2-ff543f132b20
caps.latest.revision: 47
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 54121ef549fb76639ec526b3128ffa8abfd7a849
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="denserank-transact-sql"></a>DENSE_RANK (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne le rang des lignes à l'intérieur de la partition d'un jeu de résultats, sans aucun vide dans le classement. Le rang d'une ligne est égal à un plus le nombre de rangs distincts précédant la ligne en question.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
DENSE_RANK ( ) OVER ( [ <partition_by_clause> ] < order_by_clause > )  
```  
  
## <a name="arguments"></a>Arguments  
 \<partition_by_clause>  
 Divise le jeu de résultats généré par la clause [FROM](../../t-sql/queries/from-transact-sql.md) en partitions auxquelles la fonction DENSE_RANK est appliquée. Pour la syntaxe de PARTITION BY, consultez [OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 \<order_by_clause>  
 Détermine l'ordre dans lequel la fonction DENSE_RANK est appliquée aux lignes dans une partition.  
  
## <a name="return-types"></a>Types de retour  
 **bigint**  
  
## <a name="remarks"></a>Notes   
 Si au moins deux lignes sont liées à un rang de la même partition, elles reçoivent le même rang. Par exemple, si les deux meilleurs vendeurs ont la même valeur SalesYTD, leur rang à tous deux est un. Le commercial dont la valeur SalesYTD est immédiatement inférieure reçoit le rang deux. Cela correspond à un rang de plus que le nombre de lignes distinctes précédant cette ligne. Par conséquent, les nombres retournés par la fonction DENSE_RANK ne comportent pas de vides et définissent toujours des rangs consécutifs.  
  
 L'ordre de tri utilisé pour l'ensemble de la requête détermine l'ordre d'apparition des lignes dans un résultat. Cela implique qu'une ligne ayant le rang numéro un n'est pas nécessairement la première ligne de la partition.  
  
 DENSE_RANK n'est pas déterministe. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-ranking-rows-within-a-partition"></a>A. Classement des lignes d'une partition  
 L'exemple suivant classe les produits d'inventaire aux emplacements d'inventaire suivants en fonction de leurs quantités. Le jeu de résultats est partitionné par `LocationID` et classé logiquement par `Quantity`. Notez que les produits 494 et 495 ont la même quantité. Étant liés, ils sont tous deux classés numéro un.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT i.ProductID, p.Name, i.LocationID, i.Quantity  
    ,DENSE_RANK() OVER   
    (PARTITION BY i.LocationID ORDER BY i.Quantity DESC) AS Rank  
FROM Production.ProductInventory AS i   
INNER JOIN Production.Product AS p   
    ON i.ProductID = p.ProductID  
WHERE i.LocationID BETWEEN 3 AND 4  
ORDER BY i.LocationID;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ProductID   Name                               LocationID Quantity Rank  
----------- ---------------------------------- ---------- -------- -----  
494         Paint - Silver                     3          49       1  
495         Paint - Blue                       3          49       1  
493         Paint - Red                        3          41       2  
496         Paint - Yellow                     3          30       3  
492         Paint - Black                      3          17       4  
495         Paint - Blue                       4          35       1  
496         Paint - Yellow                     4          25       2  
493         Paint - Red                        4          24       3  
492         Paint - Black                      4          14       4  
494         Paint - Silver                     4          12       5  
  
(10 row(s) affected)  
  
```  
  
### <a name="b-ranking-all-rows-in-a-result-set"></a>B. Classement de toutes les lignes dans un jeu de résultats  
 L'exemple suivant retourne les dix principaux employés classés en fonction de leur salaire. Étant donné qu'aucune clause PARTITION BY n'a été spécifiée, la fonction DENSE_RANK a été appliquée à toutes les lignes du jeu de résultats.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) BusinessEntityID, Rate,   
       DENSE_RANK() OVER (ORDER BY Rate DESC) AS RankBySalary  
FROM HumanResources.EmployeePayHistory;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID Rate                  RankBySalary  
---------------- --------------------- --------------------  
1                125.50                1  
25               84.1346               2  
273              72.1154               3  
2                63.4615               4  
234              60.0962               5  
263              50.4808               6  
7                50.4808               6  
234              48.5577               7  
285              48.101                8  
274              48.101                8  
```  
  
## <a name="c-four-ranking-functions-used-in-the-same-query"></a>C. Quatre fonctions de classement utilisées dans la même requête  
 Dans l'exemple suivant, les quatre fonctions de classement sont utilisées dans la même requête. Pour consulter des exemples spécifiques à ces fonctions, consultez les rubriques consacrées à chacune d’elles.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS Rank  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS Quartile  
    ,s.SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|FirstName|LastName|Row Number|Rank|Dense Rank|Quartile|SalesYTD|PostalCode|  
|---------------|--------------|----------------|----------|----------------|--------------|--------------|----------------|  
|Michael|Blythe| 1| 1| 1| 1|4557045,0459|98027|  
|Linda|Mitchell|2| 1| 1| 1|5200475,2313|98027|  
|Jillian|Carson|3| 1| 1| 1|3857163,6332|98027|  
|Garrett|Vargas|4| 1| 1| 1|1764938,9859|98027|  
|Tsvi|Reiter|5| 1| 1|2|2811012,7151|98027|  
|Shu|Ito|6|6|2|2|3018725,4858|98055|  
|José|Saraiva|7|6|2|2|3189356,2465|98055|  
|David|Campbell|8|6|2|3|3587378,4257|98055|  
|Tete|Mensa-Annan|9|6|2|3|1931620,1835|98055|  
|Lynn|Tsoflias|10|6|2|3|1758385,926|98055|  
|Rachel|Valdez|11|6|2|4|2241204,0424|98055|  
|Jae|Pak|12|6|2|4|5015682,3752|98055|  
|Ranjit|Varkey Chudukatil|13|6|2|4|3827950,238|98055| 


## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-ranking-rows-within-a-partition"></a>D. Classement des lignes dans une partition  
 L’exemple suivant classe par rang les représentants commerciaux dans chaque secteur de vente en fonction de leurs ventes totales. L'ensemble de lignes est partitionné par `SalesTerritoryGroup` et trié par `SalesAmountQuota`.  
  
```  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryGroup,  
    DENSE_RANK() OVER (PARTITION BY SalesTerritoryGroup ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryGroup != N'NA'  
GROUP BY LastName,SalesTerritoryGroup;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
 LastName          TotalSales     SalesTerritoryGroup  RankResult  
----------------  -------------  -------------------  --------  
Pak               10514000.0000  Europe               1  
Varkey Chudukatil  5557000.0000  Europe               2  
Valdez             2287000.0000  Europe               3  
Carson            12198000.0000  North America        1  
Mitchell          11786000.0000  North America        2  
Blythe            11162000.0000  North America        3  
Reiter             8541000.0000  North America        4  
Ito                7804000.0000  North America        5  
Saraiva            7098000.0000  North America        6  
Vargas             4365000.0000  North America        7  
Campbell           4025000.0000  North America        8  
Ansman-Wolfe       3551000.0000  North America        9  
Mensa-Annan        2753000.0000  North America        10  
Tsoflias           1687000.0000  Pacific              1 
```  

## <a name="see-also"></a> Voir aussi  
 [RANK &#40;Transact-SQL&#41;](../../t-sql/functions/rank-transact-sql.md)   
 [ROW_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/row-number-transact-sql.md)   
 [NTILE &#40;Transact-SQL&#41;](../../t-sql/functions/ntile-transact-sql.md)   
 [Fonctions de classement &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [Fonctions](../../t-sql/functions/functions.md)  
  
  

