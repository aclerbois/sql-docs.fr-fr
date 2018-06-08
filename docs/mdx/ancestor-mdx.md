---
title: Ancêtre (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5e30774aa9173bfcb7851096a25363c81d7abe9f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577071"
---
# <a name="ancestor-mdx"></a>Ancestor (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Cette fonction retourne l'ancêtre d'un membre spécifié à un niveau ou une distance spécifique du membre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Level syntax  
Ancestor(Member_Expression, Level_Expression)  
  
Numeric syntax  
Ancestor(Member_Expression, Distance)  
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
 *distance*  
 Expression numérique valide qui spécifie la distance depuis le membre spécifié.  
  
## <a name="remarks"></a>Notes  
 Avec la **ancêtre** (fonction), vous fournissez la fonction avec une expression de membre MDX, puis une expression MDX d’un niveau qui est un ancêtre du membre ou une expression numérique qui représente le nombre de niveaux au-dessus de ce membre. Avec ces informations, le **ancêtres** fonction retourne le membre ancêtre à ce niveau.  
  
> [!NOTE]  
>  Pour retourner un jeu qui contient le membre ancêtre, plutôt que juste le membre ancêtre, utilisez le [ancêtres &#40;MDX&#41; ](../mdx/ancestors-mdx.md) (fonction).  
  
 Si une expression de niveau est spécifiée, la **ancêtre** fonction retourne l’ancêtre d’un membre spécifié au niveau spécifié. Si le membre spécifié n'apparaît pas dans la même hiérarchie en tant que niveau spécifié, la fonction retourne une erreur.  
  
 Si vous spécifiez une distance, le **ancêtre** fonction retourne l’ancêtre du membre spécifié qui est le nombre d’étapes spécifiés plus haut dans la hiérarchie spécifiée par l’expression de membre. Vous pouvez spécifier un membre en tant que membre d'une hiérarchie d'attribut, d'une hiérarchie définie par l'utilisateur ou, dans certains cas, d'une hiérarchie parent-enfant. Un nombre 1 retourne le parent d'un membre ; un nombre 2 retourne le grand-parent d'un membre (le cas échéant). Un nombre 0 retourne le membre lui-même.  
  
> [!NOTE]  
>  Utilisez cette forme de la **ancêtre** fonction pour les cas dans lequel le niveau du parent est inconnu et ne peut pas être nommé.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise une expression de niveau et retourne la mesure Internet Sales Amount (volume de vente Internet) pour chaque State-Province (état-Province) en Australie ; il dévoile également le pourcentage de volume de vente Internet total pour l'Australie.  
  
```  
WITH MEMBER Measures.x AS [Measures].[Internet Sales Amount] /   
   (  
   [Measures].[Internet Sales Amount],    
      Ancestor   
         (  
         [Customer].[Customer Geography].CurrentMember,  
            [Customer].[Customer Geography].[Country]  
         )  
   ), FORMAT_STRING = '0%'  
SELECT {[Measures].[Internet Sales Amount], Measures.x} ON 0,  
{  
   Descendants   
      (  
        [Customer].[Customer Geography].[Country].&[Australia],  
           [Customer].[Customer Geography].[State-Province], SELF   
      )  
} ON 1  
FROM [Adventure Works]  
```  
  
 L'exemple suivant utilise une expression numérique et retourne la mesure Internet Sales Amount (volume de vente Internet) pour chaque State-Province (état-Province) en Australie ; il dévoile également le pourcentage de volume de vente Internet total pour tous les pays.  
  
```  
WITH MEMBER Measures.x AS [Measures].[Internet Sales Amount] /   
   (  
      [Measures].[Internet Sales Amount],  
         Ancestor   
            ([Customer].[Customer Geography].CurrentMember, 2)  
   ), FORMAT_STRING = '0%'  
SELECT {[Measures].[Internet Sales Amount], Measures.x} ON 0,  
{  
   Descendants   
      (  
         [Customer].[Customer Geography].[Country].&[Australia],  
            [Customer].[Customer Geography].[State-Province], SELF   
      )  
} ON 1  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
