---
title: LinRegVariance (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 314937dce7f423e2ca57183a9686c0059b2d2970
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579231"
---
# <a name="linregvariance-mdx"></a>LinRegVariance (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Calcule la régression linéaire d’un jeu et retourne la variance associée à la ligne de régression y = ax + b.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
LinRegVariance(Set_Expression, Numeric_Expression_y [ ,Numeric_Expression_x ] ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Numeric_Expression_y*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre représentant les valeurs de l'axe des ordonnées.  
  
 *Numeric_Expression_x*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre représentant les valeurs de l'axe des abscisses.  
  
## <a name="remarks"></a>Notes  
 La régression linéaire qui utilise la méthode des moindres carrés calcule l'équation d'une droite de régression (c'est-à-dire de la meilleure ligne pour une série de points). La ligne de régression a l’équation suivante, où un est de la pente et b l’ordonnée à l’origine :  
  
 y = ax+b  
  
 Le **LinRegVariance** fonction évalue le setagainst spécifié, la première expression numérique pour obtenir les valeurs de l’axe des ordonnées. La fonction évalue ensuite le setagainst spécifié, la deuxième expression, si spécifiée, pour obtenir les valeurs de l’axe des abscisses. Si le deuxième expressionis numérique pas spécifié, la fonction utilise le contexte actuel des cellules dans le jeu spécifié en tant que les valeurs de l’axe des abscisses. Il est fréquent de ne pas spécifier l'argument de l'axe des abscisses avec la dimension Time.  
  
 Après avoir obtenu l’ensemble de points, le **LinRegVariance** fonction retourne la variance statistique qui décrit l’adéquation entre l’équation linéaire et les points.  
  
> [!NOTE]  
>  Le **LinRegVariance** fonction ignore les cellules vides ou les cellules qui contiennent du texte ou des valeurs logiques. Cependant, elle tient compte des cellules dont la valeur est zéro.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne la variance statistique qui décrit l'adéquation entre l'équation linéaire et l'ensemble de points des mesures des ventes par unité et des ventes par magasin.  
  
```  
LinRegVariance(LastPeriods(10),[Measures].[Unit Sales],[Measures].[Store Sales])  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
