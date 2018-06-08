---
title: LinRegR2 (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ba43920ccaf246b47a160e5e6b9b210955289fdd
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579071"
---
# <a name="linregr2-mdx"></a>LinRegR2 (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Calcule la régression linéaire d’un jeu et retourne le coefficient de détermination, R<sup>2</sup>.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
LinRegR2(Set_Expression, Numeric_Expression_y [ ,Numeric_Expression_x ] )  
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
  
 Le **LinRegR2** fonction évalue le setagainst spécifié la première expressionto numérique obtenir les valeurs de l’axe des ordonnées. La fonction évalue ensuite le jeu spécifié par rapport à la deuxième expression numérique, si spécifiée, pour extraire les valeurs de l'axe des abscisses. Si le deuxième expressionis numérique pas spécifié, la fonction utilise le contexte actuel des cellules dans le jeu spécifié en tant que les valeurs de l’axe des abscisses. Ne pas spécifier le x-axisargument est souvent utilisé avec la dimension de temps.  
  
 Après avoir obtenu l’ensemble de points, le **LinRegR2** fonction retourne le R statistique<sup>2</sup> qui décrit l’adéquation entre l’équation linéaire et les points.  
  
> [!NOTE]  
>  Le **LinRegR2** fonction ignore les cellules vides ou les cellules qui contiennent du texte ou des valeurs logiques. Cependant, elle tient compte des cellules dont la valeur est zéro.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant retourne le R statistique<sup>2</sup> qui décrit le degré d’ajustement de l’équation de régression linéaire pour les points de ventes par unité et les mesures de ventes du magasin.  
  
```  
LinRegR2(LastPeriods(10), [Measures].[Unit Sales],[Measures].[Store Sales])  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
