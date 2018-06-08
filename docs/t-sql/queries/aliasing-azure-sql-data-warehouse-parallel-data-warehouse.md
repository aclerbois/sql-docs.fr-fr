---
title: Alias (Azure SQL Data Warehouse, Parallel Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.component: t-sql|queries
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 7b3a5c74-05cf-4385-8ee6-6176d003cb8a
caps.latest.revision: 11
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 2a44d6a7e2346de0bcfb1d19f0f67fea6049daaa
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="aliasing-azure-sql-data-warehouse-parallel-data-warehouse"></a>Alias (Azure SQL Data Warehouse, Parallel Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Les alias permettent l’utilisation temporaire d’une chaîne courte facile à mémoriser à la place d’un nom de table ou de colonne dans des requêtes [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] [!INCLUDE[DWsql](../../includes/dwsql-md.md)]. Les alias de table sont souvent utilisés dans les requêtes JOIN, car la syntaxe JOIN requiert des noms d’objet complets lors du référencement de colonnes.  
  
 Les alias doivent être en un seul mot conformément aux règles de nommage des objets. Pour plus d’informations, consultez les règles de nommage des objets dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Les alias ne doivent pas contenir d’espaces vides, ni être entourés de guillemets simples ou doubles.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
object_source [ AS ] alias  
```  
  
## <a name="arguments"></a>Arguments  
 *object_source*  
 Nom de la table ou colonne source.  
  
 AS  
 Préposition d’alias facultative. Si vous utilisez des alias de variable de portée, le mot clé AS n’est pas autorisé.  
  
 *alias*  
 Nom de référence temporaire souhaité pour la table ou la colonne. Vous pouvez utiliser n’importe quel nom d’objet valide. Pour plus d’informations, consultez les règles de nommage des objets dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
## <a name="examples-includesssdwincludessssdw-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant montre une requête contenant plusieurs jointures. Il illustre des alias de table et de colonne.  
  
-   Alias de colonne : dans cet exemple, les colonnes et les expressions référençant des colonnes dans la liste de sélection ont des alias. `SalesTerritoryRegion AS SalesTR` présente un alias de colonne simple. `Sum(SalesAmountQuota) AS TotalSales` montre  
  
-   Alias de table : `dbo.DimSalesTerritory AS st` illustre la création de l’alias `st` pour la table `dbo.DimSalesTerritory`.  
  
```  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryRegion AS SalesTR,  
    RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
  
```  
  
 Le mot clé AS peut être omis, comme ci-dessous, mais il est souvent inclus pour une meilleure lisibilité.  
  
```  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) TotalSales, SalesTerritoryRegion SalesTR,  
RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) RankResult  
FROM dbo.DimEmployee e  
INNER JOIN dbo.FactSalesQuota sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
  
