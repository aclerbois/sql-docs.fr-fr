---
title: À l’aide des fonctions de jeu | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a042092cfcddb985b29e7b0c98612f07c68ac2d0
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34582281"
---
# <a name="using-set-functions"></a>Utilisation de fonctions de jeu
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Une fonction de jeu récupère un jeu à partir d'une dimension, d'une hiérarchie, ou d'un niveau, ou encore en parcourant les emplacements absolus et relatifs des membres contenus dans ces objets et en construisant des jeux de différentes manières.  
  
 Les fonctions de jeu, comme les fonctions de membre et de tuple, sont essentielles à la négociation des structures multidimensionnelles présentes dans Analysis Services. Elles sont également essentielles pour l'obtention de résultats à partir de requêtes MDX (Multidimensional Expressions), car les expressions de jeu définissent les axes d'une requête MDX.  
  
 Une des fonctions de jeu plus courantes est la [membres &#40;définir&#41; &#40;MDX&#41; ](../mdx/members-set-mdx.md) (fonction), qui extrait un jeu contenant tous les membres d’une dimension, une hiérarchie ou un niveau. Ce qui suit est un exemple de son utilisation dans une requête :  
  
 `SELECT`  
  
 `//Returns all of the members on the Measures dimension`  
  
 `[Measures].MEMBERS`  
  
 `ON Columns,`  
  
 `//Returns all of the members on the Calendar Year level of the Calendar Year Hierarchy`  
  
 `//on the Date dimension`  
  
 `[Date].[Calendar Year].[Calendar Year].MEMBERS`  
  
 `ON Rows`  
  
 `FROM [Adventure Works]`  
  
 Une autre fonction fréquemment utilisée est la [Crossjoin &#40;MDX&#41; ](../mdx/crossjoin-mdx.md) (fonction). Elle retourne un jeu de tuples qui représente le produit cartésien des jeux passés dans celui-ci comme paramètres. En termes pratiques, cette fonction vous permet de créer des axes « imbriqués » ou de « tableau croisé dynamique » dans les requêtes :  
  
 `SELECT`  
  
 `//Returns all of the members on the Measures dimension`  
  
 `[Measures].MEMBERS`  
  
 `ON Columns,`  
  
 `//Returns a set containing every combination of all of the members`  
  
 `//on the Calendar Year level of the Calendar Year Hierarchy`  
  
 `//on the Date dimension and all of the members on the Category level`  
  
 `//of the Category hierarchy on the Product dimension`  
  
 `Crossjoin(`  
  
 `[Date].[Calendar Year].[Calendar Year].MEMBERS,`  
  
 `[Product].[Category].[Category].MEMBERS)`  
  
 `ON Rows`  
  
 `FROM [Adventure Works]`  
  
 Le [Descendants &#40;MDX&#41; ](../mdx/descendants-mdx.md) fonction est similaire la **enfants** de fonctionner, mais est plus puissante. Elle retourne les descendants de tout membre à un ou plusieurs niveaux dans une hiérarchie :  
  
 SELECT  
  
 [Measures].[Internet Sales Amount]  
  
 ON Columns,  
  
 //Retourne un jeu qui contient toutes les dates sous l'année civile  
  
 //2004 dans la hiérarchie de calendrier de la dimension Date  
  
 DESCENDANTS(  
  
 [Date]. [Calendar]. [Année civile]. & [2004]  
  
 , [Date].[Calendar].[Date])  
  
 ON Rows  
  
 FROM [Adventure Works]  
  
 Le [commande &#40;MDX&#41; ](../mdx/order-mdx.md) fonction permet de classer le contenu d’un ensemble croissant ou décroissant en fonction d’une expression numérique particulière. La requête suivante retourne les mêmes membres sur les lignes que la requête précédente, mais elle les classe désormais selon la mesure Internet Sales Amount (volume de vente Internet) :  
  
 `SELECT`  
  
 `[Measures].[Internet Sales Amount]`  
  
 `ON Columns,`  
  
 `//Returns a set containing all of the Dates beneath Calendar Year`  
  
 `//2004 in the Calendar hierarchy of the Date dimension`  
  
 `//ordered by Internet Sales Amount`  
  
 `ORDER(`  
  
 `DESCENDANTS(`  
  
 `[Date].[Calendar].[Calendar Year].&[2004]`  
  
 `, [Date].[Calendar].[Date])`  
  
 `, [Measures].[Internet Sales Amount], BDESC)`  
  
 `ON Rows`  
  
 `FROM [Adventure Works]`  
  
 Cette requête illustre également comment le jeu retourné par une fonction de jeu, Descendants, peut être passé comme un paramètre à une autre fonction de jeu, Order.  
  
 Filtrage d’un jeu selon certains critères est très utile lors de l’écriture de requêtes, et à cet effet, vous pouvez utiliser la [filtre &#40;MDX&#41; ](../mdx/filter-mdx.md) de fonction, comme indiqué dans l’exemple suivant :  
  
 `SELECT`  
  
 `[Measures].[Internet Sales Amount]`  
  
 `ON Columns,`  
  
 `//Returns a set containing all of the Dates beneath Calendar Year`  
  
 `//2004 in the Calendar hierarchy of the Date dimension`  
  
 `//where Internet Sales Amount is greater than $70000`  
  
 `FILTER(`  
  
 `DESCENDANTS(`  
  
 `[Date].[Calendar].[Calendar Year].&[2004]`  
  
 `, [Date].[Calendar].[Date])`  
  
 `, [Measures].[Internet Sales Amount]>70000)`  
  
 `ON Rows`  
  
 `FROM [Adventure Works]`  
  
 D'autres fonctions plus sophistiquées existent et vous permettent de filtrer un jeu d'autres manières. Par exemple, la requête suivante illustre le [TopCount &#40;MDX&#41; ](../mdx/topcount-mdx.md) fonction retourne les premiers éléments n dans un jeu :  
  
 `SELECT`  
  
 `[Measures].[Internet Sales Amount]`  
  
 `ON Columns,`  
  
 `//Returns a set containing the top 10 Dates beneath Calendar Year`  
  
 `//2004 in the Calendar hierarchy of the Date dimension by Internet Sales Amount`  
  
 `TOPCOUNT(`  
  
 `DESCENDANTS(`  
  
 `[Date].[Calendar].[Calendar Year].&[2004]`  
  
 `, [Date].[Calendar].[Date])`  
  
 `,10, [Measures].[Internet Sales Amount])`  
  
 `ON Rows`  
  
 `FROM [Adventure Works]`  
  
 Enfin, il est possible d’effectuer un certain nombre d’opérations définies logiques à l’aide de fonctions telles que [Intersect &#40;MDX&#41;](../mdx/intersect-mdx.md), [Union &#40;MDX&#41; ](../mdx/union-mdx.md) et [sauf &#40;MDX&#41; ](../mdx/except-mdx-function.md) fonctions. La requête suivante affiche des exemples de ces deux dernières fonctions :  
  
 `SELECT`  
  
 `//Returns a set containing the Measures Internet Sales Amount, Internet Tax Amount and`  
  
 `//Internet Total Product Cost`  
  
 `UNION(`  
  
 `{[Measures].[Internet Sales Amount], [Measures].[Internet Tax Amount]}`  
  
 `, {[Measures].[Internet Total Product Cost]}`  
  
 `)`  
  
 `ON Columns,`  
  
 `//Returns a set containing all of the Dates beneath Calendar Year`  
  
 `//2004 in the Calendar hierarchy of the Date dimension`  
  
 `//except the January 1st 2004`  
  
 `EXCEPT(`  
  
 `DESCENDANTS(`  
  
 `[Date].[Calendar].[Calendar Year].&[2004]`  
  
 `, [Date].[Calendar].[Date])`  
  
 `,{[Date].[Calendar].[Date].&[915]})`  
  
 `ON Rows`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;syntaxe MDX&#41;](../mdx/functions-mdx-syntax.md)   
 [À l’aide des fonctions membres](../mdx/using-member-functions.md)   
 [Utilisation de fonctions de tuple](../mdx/using-tuple-functions.md)  
  
  
