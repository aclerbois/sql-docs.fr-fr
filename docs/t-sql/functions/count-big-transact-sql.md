---
title: COUNT_BIG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- COUNT_BIG_TSQL
- COUNT_BIG
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT_BIG function
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT_BIG function
ms.assetid: f2e3601f-487e-4917-bb01-47b1047908cd
caps.latest.revision: 44
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a76766659a771d1ab88d0fa33a9ef62f986a8a06
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="countbig--sql"></a>COUNT_BIG (-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Cette fonction retourne le nombre d’éléments figurant dans un groupe. `COUNT_BIG` est similaire à la fonction [COUNT](../../t-sql/functions/count-transact-sql.md). Ces fonctions diffèrent uniquement par les types de données des valeurs qu’elles retournent. `COUNT_BIG` retourne toujours une valeur de type de données **bigint**. `COUNT` retourne toujours une valeur de type de données **int**.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
-- Syntax for SQL Server and Azure SQL Database  
  
COUNT_BIG ( { [ ALL | DISTINCT ] expression } | * )  
   [ OVER ( [ partition_by_clause ] [ order_by_clause ] ) ]  
```  
  
```sql
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  

-- Aggregation Function Syntax  
COUNT_BIG ( { [ [ ALL | DISTINCT ] expression ] | * } )  
  
-- Analytic Function Syntax  
COUNT_BIG ( { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
## <a name="arguments"></a>Arguments  
ALL  
Applique la fonction d'agrégation à toutes les valeurs. La valeur ALL est utilisée par défaut.
  
DISTINCT  
Précise que la fonction `COUNT_BIG` doit renvoyer le nombre de valeurs non nulles uniques.
  
*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type. Notez que `COUNT_BIG` ne prend pas en charge les fonctions d’agrégation ou sous-requêtes dans une expression.
  
*\**  
Spécifie que `COUNT_BIG` doit compter toutes les lignes pour déterminer le nombre total de lignes de la table à retourner. `COUNT_BIG(*)` ne sélectionne aucun paramètre et ne prend pas en charge l’utilisation de DISTINCT. `COUNT_BIG(*)` ne nécessite pas de paramètre *expression* puisque, par définition, elle n’utilise les données d’aucune colonne en particulier. `COUNT_BIG(*)` retourne le nombre de lignes de la table spécifiée et conserve les doublons. Il compte chaque ligne séparément, y compris les lignes qui contiennent des valeurs NULL.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] **)**  
*partition_by_clause* divise le jeu de résultats généré par la clause `FROM` en partitions auxquelles la fonction `COUNT_BIG` est appliquée. S'il n'est pas spécifié, la fonction gère toutes les lignes du jeu de résultats de la requête en un seul groupe. *order_by_clause* détermine l’ordre logique de l’opération. Consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md) pour plus d’informations.
  
## <a name="return-types"></a>Types de retour
**bigint**
  
## <a name="remarks"></a>Notes   
COUNT_BIG(\*) renvoie le nombre d'éléments figurant dans un groupe, y compris les valeurs NULL et les doublons.
  
COUNT_BIG(ALL *expression*) évalue l’argument *expression* pour chaque ligne d’un groupe et retourne le nombre de valeurs non-NULL.
  
COUNT_BIG (DISTINCT *expression*) évalue l’argument *expression* pour chaque ligne d’un groupe et retourne le nombre de valeurs non-NULL uniques.
  
COUNT_BIG est une fonction déterministe lorsqu'elle est utilisée ***sans*** les clauses OVER et ORDER BY. Elle n'est pas déterministe lorsqu’elle est utilisée ***avec*** les clauses OVER et ORDER BY. Consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md) pour plus d’informations.
  
## <a name="examples"></a>Exemples  
Pour obtenir des exemples, consultez [COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md).
  
## <a name="see-also"></a>Voir aussi
[Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md)  
[int, bigint, smallint et tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
[OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
