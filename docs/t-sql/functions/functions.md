---
title: Quelles sont les fonctions de base de données Microsoft SQL ? | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- built-in functions [SQL Server]
- function [SQL Server] See functions [SQL Server]
- functions [Transact-SQL]
- functions [SQL Server], about functions
- scalar functions
- functions [SQL Server]
ms.assetid: 17186213-5ab5-40b0-b470-b660af1ec44c
caps.latest.revision: 38
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 707cb1e75bff3d28136e53ddeaf17a63396f8078
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708967"
---
# <a name="what-are-the-sql-database-functions"></a>Quelles sont les fonctions de base de données SQL ?
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

Découvrez les catégories de fonctions intégrées que vous pouvez utiliser avec les bases de données SQL. Vous pouvez utiliser les fonctions intégrées ou créer vos propres fonctions définies par l’utilisateur.
  
## <a name="aggregate-functions"></a>Fonctions d'agrégation

Les fonctions d'agrégation effectuent un calcul sur un ensemble de valeurs et retournent une valeur unique. Elles sont autorisées dans la liste SELECT ou dans la clause HAVING d’une instruction SELECT. Vous pouvez utiliser une agrégation en combinaison avec la clause GROUP BY pour calculer l’agrégation sur les catégories de lignes. Utilisez la clause OVER pour calculer l’agrégation sur une plage spécifique de valeurs. La clause OVER ne peut pas suivre les agrégations GROUPING et GROUPING_ID.

Toutes les fonctions d’agrégation sont déterministes, ce qui signifie qu’elles renvoient toujours la même valeur lorsqu’elles s’exécutent sur les mêmes valeurs d’entrée. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).|

## <a name="analytic-functions"></a>Fonctions analytiques
Les fonctions analytiques calculent une valeur d'agrégation basée sur un groupe de lignes. Toutefois, contrairement aux fonctions d’agrégation, les fonctions analytiques peuvent renvoyer plusieurs lignes pour chaque groupe. Vous pouvez utiliser des fonctions analytiques pour calculer des moyennes mobiles, des cumuls, des pourcentages ou des résultats de type « N premiers » dans un groupe.

## <a name="ranking-functions"></a>Fonctions de classement
Les fonctions de classement renvoient une valeur de classement pour chaque ligne d'une partition. Selon la fonction utilisée, certaines lignes peuvent recevoir la même valeur que d'autres lignes. Les fonctions de classement sont non déterministes.

## <a name="rowset-functions"></a>Fonctions d’ensemble de lignes
Les fonctions d’ensemble de lignes renvoient des objets qui peuvent être utilisés comme références de table dans une instruction SQL.

## <a name="scalar-functions"></a>Fonctions scalaires
Effectuent des opérations sur une valeur unique et retournent ensuite une valeur unique. Les fonctions scalaires peuvent être utilisées pour autant qu'une expression soit valide.

### <a name="categories-of-scalar-functions"></a>Catégories de fonctions scalaires
  
|Catégorie de fonctions|Description|  
|-----------------------|-----------------|  
|[Fonctions de configuration](configuration-functions-transact-sql.md)|Retournent des informations concernant la configuration actuelle.|  
|[Fonctions de conversion](conversion-functions-transact-sql.md)|Prennent en charge la conversion de type de données.|  
|[Fonctions de curseur](cursor-functions-transact-sql.md)|Retournent des informations sur les curseurs.|  
|[Types de données et fonctions de date et d’heure](date-and-time-data-types-and-functions-transact-sql.md)|Effectuent des opérations sur des valeurs d'entrée de type date et heure et retournent des valeurs de type date et heure, numérique ou chaîne.|  
|[Fonctions JSON](json-functions-transact-sql.md)|Validez, interrogez et modifiez les données JSON.|  
|[Fonctions logiques](http://msdn.microsoft.com/library/5b2b4546-951b-462d-91d5-e41fc5acd6f9)|Effectuent des opérations logiques.|  
|[Fonctions mathématiques](mathematical-functions-transact-sql.md)|Effectuent des calculs sur la base des valeurs d'entrée fournies comme paramètres aux fonctions et retournent des valeurs numériques.|  
|[Fonctions de métadonnées](metadata-functions-transact-sql.md)|Retournent des informations concernant la base de données et les objets de base de données.|  
|[Fonctions de sécurité](security-functions-transact-sql.md)|Retournent des informations concernant les utilisateurs et les rôles.|  
|[Fonctions de chaîne](string-functions-transact-sql.md)|Effectuent des opérations sur une valeur d’entrée de type chaîne (**char** ou **varchar**) et renvoient une valeur numérique ou de type chaîne.|  
|[Fonctions système](../../relational-databases/system-functions/system-functions-for-transact-sql.md)|Effectuent des opérations et retournent des informations concernant les valeurs, objets et paramètres de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Fonctions statistiques système](system-statistical-functions-transact-sql.md)|Retournent des informations statistiques concernant le système.|  
|[Fonctions texte et image](http://msdn.microsoft.com/library/b9c70488-1bf5-4068-a003-e548ccbc5199)|Effectuent des opérations sur des colonnes ou des valeurs d'entrée de type texte ou image et retournent des informations concernant la valeur.|  
  
## <a name="function-determinism"></a>Déterminisme des fonctions  
 Les fonctions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégrées sont déterministes ou non déterministes. Une fonction déterministe retourne toujours le même résultat chaque fois qu'elle est appelée avec un ensemble de valeurs d'entrée spécifique. Une fonction non déterministe peut retourner des résultats différents chaque fois qu'elle est appelée, même si le même ensemble de valeurs d'entrée spécifique est utilisé. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)  
  
## <a name="function-collation"></a>Classement des fonctions  
 Les fonctions qui acceptent une entrée sous forme de chaîne de caractères et retournent une chaîne de caractères utilisent le classement de la chaîne d'entrée pour la sortie.  
  
 Les fonctions qui acceptent des entrées de type non-caractère et retournent une chaîne de caractères utilisent le classement par défaut de la base de données active pour la sortie.  
  
 Les fonctions qui acceptent plusieurs entrées sous forme de chaîne de caractères et retournent une chaîne de caractères utilisent les règles de priorité des classements pour définir le classement de la chaîne de sortie. Pour plus d’informations, consultez [Priorité de classement &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)   
 [Utilisation de procédures stockées &#40;MDX&#41;](../../mdx/using-stored-procedures-mdx.md)  
  
  
