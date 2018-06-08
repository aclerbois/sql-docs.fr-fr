---
title: EXCEPT et INTERSECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- INTERSECT_TSQL
- EXCEPT_TSQL
- INTERSECT
- EXCEPT
dev_langs:
- TSQL
helpviewer_keywords:
- EXCEPT operator [Transact-SQL]
- queries [SQL Server], comparing
- comparing queries
- INTERSECT operator
ms.assetid: b1019300-171a-4a1a-854f-e1e751de3565
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: c6ff3bafd6d01fb9f4ac591d88104e19e71b2713
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-operators---except-and-intersect-transact-sql"></a>Opérateurs de jeu - EXCEPT et INTERSECT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne des lignes distinctes en comparant les résultats de deux requêtes.  
  
 EXCEPT retourne les lignes distinctes de la requête d'entrée à gauche mais non trouvées par la requête d'entrée à droite.  
  
 INTERSECT retourne des lignes distinctes générées par l’opérateur des requêtes d’entrée à gauche et à droite.  
  
 Voici deux règles essentielles pour combiner les ensembles de résultats de deux requêtes utilisant l'opérande EXCEPT ou l'opérande INTERSECT :  
  
-   Le nombre et l'ordre des colonnes doivent être identiques dans toutes les requêtes.  
  
-   Les types de données doivent être compatibles.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
{ <query_specification> | ( <query_expression> ) }   
{ EXCEPT | INTERSECT }  
{ <query_specification> | ( <query_expression> ) }  
```  
  
## <a name="arguments"></a>Arguments  
 \<*query_specification*> | ( \<*query_expression*> )  
 Spécification ou expression de requête qui retourne les données à comparer avec les données d'une autre spécification ou expression de requête. Les définitions des colonnes faisant partie d'une opération EXCEPT ou INTERSECT ne doivent pas forcément être identiques, mais doivent néanmoins être comparables par le biais d'une conversion implicite. Si les types de données diffèrent, le type utilisé pour effectuer la comparaison et retourner les résultats est déterminé selon les règles de [priorité des types de données](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
 Si les types sont les mêmes mais diffèrent en terme de précision, d'échelle ou de longueur, le résultat se détermine d'après les mêmes règles de combinaison d'expressions. Pour plus d’informations, consultez [Précision, échelle et longueur &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
 La spécification ou l’expression de requête ne peut pas retourner de colonnes de type **xml**, **text**, **ntext**, **image** ou CLR non binaire définis par l’utilisateur car ces types de données ne sont pas comparables.  
  
 EXCEPT  
 Retourne les valeurs distinctes de la requête à gauche de l’opérateur EXCEPT qui ne sont pas également retournées par la requête à droite.  
  
 INTERSECT  
 Retourne toute valeur distincte trouvée par les requêtes à gauche et à droite de l'opérateur INTERSECT.  
  
## <a name="remarks"></a>Notes   
 Si les types de données de colonnes comparables retournés par les requêtes à gauche et à droite de l’opérateur EXCEPT ou INTERSECT sont des types de données caractère et que leur classement diffère, la comparaison nécessaire s’effectue selon les règles de [priorité de classement](../../t-sql/statements/collation-precedence-transact-sql.md). Si cette conversion est impossible, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] retourne une erreur.  
  
 Lors de la comparaison des valeurs de colonnes pour trouver les lignes DISTINCT, deux valeurs NULL sont considérées comme égales.  
  
 Les noms de colonnes du jeu de résultats retournés par EXCEPT ou INTERSECT sont identiques à ceux retournés par la requête se trouvant à gauche de l'opérateur.  
  
 Les noms ou les alias de colonnes placés dans des clauses ORDER BY doivent faire référence aux noms de colonnes retournés par la requête de gauche.  
  
 La propriété de toute colonne acceptant des valeurs NULL, lesquelles font partie du jeu de résultats retourné par EXCEPT ou INTERSECT, revient à la propriété à accepter des valeurs NULL par sa colonne correspondante retournée par la requête se trouvant à gauche de l'opérateur.  
  
 Si EXCEPT ou INTERSECT sont utilisés conjointement avec d'autres opérateurs au sein d'une expression, l'expression finale s'évalue d'après la règle de précédence suivante :  
  
1.  expressions entre parenthèses ;  
  
2.  opérateur INTERSECT ;  
  
3.  EXCEPT et UNION évaluée de gauche à droite d'après leur position dans l'expression.  
  
 Si EXCEPT ou INTERSECT est utilisé pour comparer plus de deux ensembles de requêtes, la conversion de type de données est déterminée en comparant deux requêtes à la fois et en suivant les règles précédemment mentionnées relatives à l'évaluation d'expressions.  
  
 EXCEPT et INTERSECT ne peuvent pas être utilisées dans une définition de vue partitionnée distribuée ou des notifications de requêtes.  
  
 Vous pouvez toujours les utiliser dans les requêtes distribuées à condition de ne les exécuter que sur un serveur local et de ne pas les envoyer à un serveur lié. Il se peut par conséquent que les performances soient affectées par l'utilisation d'EXCEPT et d'INTERSECT dans des requêtes distribuées.  
  
 Les curseurs avant uniquement statiques ou rapides sont entièrement pris en charge dans l'ensemble de résultats s'ils sont utilisés avec une opération EXCEPT ou INTERSECT. Si un curseur piloté par jeu de clés ou un curseur dynamique est utilisé avec une opération EXCEPT ou INTERSECT, le curseur de l'ensemble de résultats de l'opération est converti en curseur statique.  
  
 Si une opération EXCEPT est affichée à l’aide de la fonctionnalité Graphical Showplan de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], elle apparaît sous forme de [Left Anti Semi Join](../../relational-databases/showplan-logical-and-physical-operators-reference.md) alors qu’une opération INTERSECT apparaît sous forme de [Left Semi Join](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent l’utilisation des opérateurs `INTERSECT` et `EXCEPT`. La première requête retourne toutes les valeurs de la table `Production.Product` pour qu'elles soient comparées aux résultats à l'aide de `INTERSECT` et de `EXCEPT`.  
  
```  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product ;  
--Result: 504 Rows  
```  
  
 La requête suivante retourne toute valeur distincte trouvée par les requêtes à gauche et à droite de l'opérateur `INTERSECT`.  
  
```  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
INTERSECT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 238 Rows (products that have work orders)  
```  
  
 La requête suivante retourne toute valeur distincte trouvée par la requête à gauche de l'opérateur `EXCEPT`, mais non trouvée par la requête à droite.  
  
```  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
EXCEPT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 266 Rows (products without work orders)  
```  
  
 La requête suivante retourne toute valeur distincte trouvée par la requête à gauche de l'opérateur `EXCEPT`, mais non trouvée par la requête à droite. Les tables sont inversées par rapport à l'exemple précédent.  
  
```  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.WorkOrder  
EXCEPT  
SELECT ProductID   
FROM Production.Product ;  
--Result: 0 Rows (work orders without products)  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Les exemples suivants illustrent la manière d'utiliser les opérateurs `INTERSECT` et `EXCEPT`. La première requête retourne toutes les valeurs de la table `FactInternetSales` pour qu'elles soient comparées aux résultats à l'aide de `INTERSECT` et de `EXCEPT`.  
  
```  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales;  
--Result: 60398 Rows  
```  
  
 La requête suivante retourne toute valeur distincte trouvée par les requêtes à gauche et à droite de l'opérateur `INTERSECT`.  
  
```  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
INTERSECT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9133 Rows (Sales to customers that are female.)  
```  
  
 La requête suivante retourne toute valeur distincte trouvée par la requête à gauche de l'opérateur `EXCEPT`, mais non trouvée par la requête à droite.  
  
```  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
EXCEPT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9351 Rows (Sales to customers that are not female.)  
```  
  
  

