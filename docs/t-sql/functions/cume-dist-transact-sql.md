---
title: CUME_DIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CUME_DIST
- CUME_DIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CUME_DIST function
- analytic functions, CUME_DIST
ms.assetid: 491b07f3-9ffd-4cdd-93e5-5abb636fc5ef
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 08d38d1d876ee5b39498e6a28247b20c6cb6cab9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cumedist-transact-sql"></a>CUME_DIST (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-asdw-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-asdw-xxx-md.md)]

Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette fonction calcule la distribution cumulative d'une valeur dans un groupe de valeurs. Autrement dit, `CUME_DIST` calcule la position relative d'une valeur spécifiée dans un groupe de valeurs. En supposant un ordre croissant, le `CUME_DIST` d’une valeur à la ligne *r* correspond au nombre de lignes avec des valeurs inférieures ou égales à la valeur de la ligne *r*, divisé par le nombre de lignes évaluées dans la partition ou le jeu de résultats de la requête. `CUME_DIST` est similaire à la fonction `PERCENT_RANK`.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CUME_DIST( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
  
```  
  
## <a name="arguments"></a>Arguments  
OVER **(** [ *partition_by_clause* ] *order_by_clause*)  

*partition_by_clause* divise le jeu de résultats généré par la clause FROM en partitions auxquelles la fonction est appliquée. Si l’argument *partition_by_clause* n'est pas spécifié, `CUME_DIST` traite toutes les lignes du jeu de résultats de la requête comme un seul groupe. *order_by_clause* détermine l’ordre logique dans lequel l’opération est effectuée. `CUME_DIST` nécessite *order_by_clause*. `CUME_DIST` n’acceptera pas la \<clause ROWS ou RANGE> de la syntaxe OVER. Consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md) pour plus d’informations.
  
## <a name="return-types"></a>Types de retour
**float(53)**
  
## <a name="remarks"></a>Notes   
`CUME_DIST` renvoie une plage de valeurs supérieures à 0 et inférieures ou égales à 1. Les valeurs égales sont toujours évaluées à la même valeur de distribution cumulative. `CUME_DIST` inclut les valeurs NULL par défaut et les traite comme les valeurs les plus basses possibles.
  
`CUME_DIST` n’est pas déterministe. Consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md) pour plus d’informations.
  
## <a name="examples"></a>Exemples  
Cet exemple utilise la fonction `CUME_DIST` pour calculer le percentile de salaire pour chaque employé dans un service donné. `CUME_DIST` retourne une valeur qui représente le pourcentage d'employés dont le salaire est inférieur ou égal à celui de l'employé actuel dans le même service. La fonction `PERCENT_RANK` calcule le rang de pourcentage du salaire de l'employé dans un service. Pour partitionner les lignes du jeu de résultats par service, l’exemple spécifique la valeur *partition_by_clause*. La clause ORDER BY de la clause OVER ordonnance logiquement les lignes dans chaque partition. La clause ORDER BY de l'instruction SELECT détermine l'ordre d'affichage du jeu de résultats.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
  
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[PERCENT_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/percent-rank-transact-sql.md)
  
  
