---
title: LinRegIntercept (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6b8368039e1c8cd373ff74d66ff4b3507e5802c1
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579041"
---
# <a name="linregintercept-mdx"></a>LinRegIntercept (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Calcule la régression linéaire d’un jeu et retourne la valeur de l’interception de x dans la ligne de régression y = ax + b.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
LinRegIntercept(Set_Expression, Numeric_Expression_y [ ,Numeric_Expression_x ] )  
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
  
 Le **LinRegIntercept** évalue le jeu spécifié par rapport à la première expression numérique pour obtenir les valeurs de l’axe des ordonnées. La fonction évalue ensuite le jeu spécifié par rapport à la deuxième expression numérique, si spécifiée, pour extraire les valeurs de l'axe des abscisses. Si la deuxième expression numérique n’est pas spécifiée, la fonction utilise le contexte actuel des cellules dans le jeu spécifié en tant que valeurs de l’axe des abscisses. Il est fréquent de ne pas spécifier l'argument de l'axe des abscisses avec la dimension Time.  
  
 Après avoir obtenu l’ensemble de points, le **LinRegIntercept** fonction retourne l’intersection de la ligne de régression (b dans l’équation ci-dessus).  
  
> [!NOTE]  
>  Le **LinRegIntercept** fonction ignore les cellules vides ou les cellules qui contiennent du texte ou des valeurs logiques. Cependant, elle tient compte des cellules dont la valeur est zéro.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne l'intersection d'une droite de régression pour les mesures des ventes par unité et par magasin.  
  
```  
LinRegIntercept(LastPeriods(10),[Measures].[Unit Sales],[Measures].[Store Sales])  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
