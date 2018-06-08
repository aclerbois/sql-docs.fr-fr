---
title: Création de calculs de cellules au niveau de requête (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c9cb6f083751b14ad3cd8f2ffaac692ef4e6eb86
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-cell-calculations---query-scoped-cell-calculations"></a>Calculs de cellules MDX - calculs de cellules d’étendue de requête
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Pour décrire les cellules calculées au sein du contexte d’une requête, vous pouvez utiliser le mot clé **WITH** dans la syntaxe MDX (Multidimensional Expressions). La syntaxe du mot clé **WITH** est la suivante :  
  
```  
WITH CELL CALCULATION Cube_Name.CellCalc_Identifier  String_Expression  
```  
  
 La valeur `CellCalc_Identifier` est le nom des cellules calculées. La valeur `String_Expression` contient une liste d’expressions de jeu MDX unidimensionnelles et orthogonales. Chacune de ces expressions d'ensemble doit être convertie en l'une des catégories répertoriées dans le tableau suivant.  
  
|Catégorie|Description|  
|--------------|-----------------|  
|Jeu vide|Expression de jeu MDX qui prend la valeur d'un ensemble vide. Dans ce cas, la portée de la cellule calculée est l'intégralité du cube.|  
|Jeu à un seul membre|Expression de jeu MDX qui prend la valeur d'un seul membre.|  
|Jeu de membres de niveau|Expression de jeu MDX qui prend la valeur des membres d'un même niveau. La fonction MDX *Level_Expression*.**Members** est un exemple d’une telle expression de jeu. Pour inclure des membres calculés, utilisez la fonction MDX *Level_Expression*.**AllMembers**. Pour plus d’informations, consultez [AllMembers &#40;MDX&#41;](../../../mdx/allmembers-mdx.md).|  
|Jeu de descendants|Expression de jeu MDX qui prend la valeur des descendants d'un membre spécifié. La fonction MDX **Descendants**(*Member_Expression*, *Level_Expresion*, *Desc_Flag*) est un exemple d’une telle expression de jeu. Pour plus d’informations, consultez [Descendants &#40;MDX&#41;](../../../mdx/descendants-mdx.md).|  
  
 Si l’argument `String_Expression` ne décrit pas de dimension, la syntaxe MDX suppose que tous ses membres sont inclus pour la construction du sous-cube de calcul. Par conséquent, si l'argument `String_Expression` a pour valeur NULL, la définition de cellules calculées s'applique au cube tout entier.  
  
 L'argument `MDX_Expression` contient une expression MDX qui prend la valeur d'une cellule pour toutes les cellules définies dans l'argument `String_Expression` .  
  
## <a name="additional-considerations"></a>Considérations supplémentaires  
 MDX ne traite la condition de calcul, spécifiée par la propriété **CONDITION** , qu’à une seule reprise. Ce traitement unique apporte des performances supplémentaires lors de l'évaluation de plusieurs définitions de cellules calculées, surtout en cas de cellules qui se chevauchent entre différents tests du cube.  
  
 Le moment auquel ce traitement unique se produit dépend de la portée de création de la définition de cellules calculées :  
  
-   Si elle est créée dans une portée globale et fait partie d'un cube, la syntaxe MDX traite la condition de calcul lors du traitement du cube. Si des cellules sont modifiées dans le cube d'une manière quelconque et que ces cellules font partie du sous-cube de calcul d'une définition de cellules calculées, la condition de calcul risque de ne pas être exacte tant que le cube n'est pas retraité. La modification de cellules peut se produire à partir d'écritures différées, par exemple. Lors du retraitement du cube, la condition de calcul fait elle aussi l'objet d'un nouveau traitement.  
  
-   Si elle est créée avec une portée de session, la syntaxe DMX traite la condition de calcul lorsque l'instruction est émise au cours de la session. Comme dans le cas des définitions de cellules calculées à portée globale, si les cellules sont modifiées, la condition de calcul risque de ne pas être exacte pour la définition de cellules calculées.  
  
-   Si elle est créée avec une portée de requête, la syntaxe MDX traite la condition de calcul lorsque la requête est exécutée. Le problème de la modification des cellules se pose également, même si les délais de latence sont minimes en raison de la faible vitesse de traitement des requêtes MDX.  
  
 Par ailleurs, la syntaxe MDX traite la formule de calcul chaque fois qu'une requête MDX impliquant les cellules comprises dans la définition de cellules calculées est adressée au cube. Ce traitement se produit quelle que soit la portée de création.  
  
## <a name="see-also"></a>Voir aussi  
 [CRÉER une instruction de calcul de cellule & #40 ; MDX & #41 ;](../../../mdx/mdx-data-definition-create-cell-calculation.md)  
  
  
