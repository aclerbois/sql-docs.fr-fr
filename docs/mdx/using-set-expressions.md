---
title: À l’aide d’Expressions de jeu | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3b384bffc84140b966ea510834054c65986ba292
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581721"
---
# <a name="using-set-expressions"></a>Utilisation d'expressions de jeu
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Un jeu est constitué d'une liste triée de zéro tuple ou plus. Un jeu qui ne contient aucun tuple est appelé jeu vide.  
  
 L'expression complète d'un jeu est constituée de zéro ou de davantage de tuples spécifiés de manière explicite entre accolades :  
  
 {[{ *Tuple_expression* | *cet argument* } [, { *Tuple_expression* | *cet argument* }]...]}  
  
 Les expressions de membre spécifiées dans une expression de jeu sont converties en expressions de tuple à un membre.  
  
## <a name="example"></a>Exemple  
 L'exemple suivant affiche deux expressions d'ensemble utilisées sur les axes des colonnes et des lignes d'une requête :  
  
 `SELECT`  
  
 `{[Measures].[Internet Sales Amount], [Measures].[Internet Tax Amount]} ON COLUMNS,`  
  
 `{([Product].[Product Categories].[Category].&[4], [Date].[Calendar].[Calendar Year].&[2004]),`  
  
 `([Product].[Product Categories].[Category].&[1], [Date].[Calendar].[Calendar Year].&[2003]),`  
  
 `([Product].[Product Categories].[Category].&[3], [Date].[Calendar].[Calendar Year].&[2004])}`  
  
 `ON ROWS`  
  
 `FROM [Adventure Works]`  
  
 Sur l'axe des colonnes, le jeu  
  
 {[Measures].[Internet Sales Amount], [Measures].[Internet Tax Amount]}  
  
 se compose de deux membres de la dimension de mesures. Sur l'axe des lignes, le jeu  
  
 {([Product].[Product Categories].[Category].&[4], [Date].[Calendar].[Calendar Year].&[2004]),  
  
 ([Product].[Product Categories].[Category].&[1], [Date].[Calendar].[Calendar Year].&[2003]),  
  
 ([Product].[Product Categories].[Category].&[3], [Date].[Calendar].[Calendar Year].&[2004])}  
  
 se compose de trois tuples, chacun contient deux références explicites aux membres sur la hiérarchie Product Categories de la dimension Product et la hiérarchie de calendrier de la dimension Date.  
  
 Pour obtenir des exemples de fonctions qui renvoient des jeux, consultez [utilisation des membres, Tuples et jeux &#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/working-with-members-tuples-and-sets-mdx.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;MDX&#41;](../mdx/expressions-mdx.md)  
  
  
