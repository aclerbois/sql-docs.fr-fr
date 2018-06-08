---
title: À l’aide d’Expressions de Tuple | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c53a3a80355fd33dc887b6a07991370838e97cf2
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581621"
---
# <a name="using-tuple-expressions"></a>Utilisation d'expressions de tuple
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Un tuple est constitué d'un membre provenant de chaque dimension contenue dans un cube. Par conséquent, un tuple identifie de manière unique une cellule particulière au sein du cube.  
  
> [!NOTE]  
>  Un tuple faisant référence à un ou plusieurs membres qui ne sont pas valides est appelé tuple vide.  
  
 L'expression complète d'un identificateur de tuple est constituée d'un ou plusieurs membres spécifiés explicitement, entourés de parenthèses :  
  
 (*Cet argument* [,*cet argument* ...])  
  
 Un tuple peut être complet, contenir des membres implicites ou contenir un membre unique.  
  
## <a name="tuples-and-implicit-members"></a>Tuples et membres implicites  
 Un tuple spécifiant de manière explicite un membre unique provenant de chaque dimension contenue dans un cube est appelé tuple complet. Cependant, un tuple ne doit pas nécessairement être complet.  
  
 Toute dimension non référencée explicitement dans un tuple est référencée de manière implicite. Le membre de la dimension référencée implicitement dépend de la structure de la dimension et des relations d'attribut définies au sein de cette dernière. S'il y a une référence explicite à une hiérarchie sur la même dimension que la hiérarchie implicitement référencée, et s'il y a une relation directe ou indirecte définie entre la hiérarchie explicitement référencée et la hiérarchie implicitement référencée, le tuple se comporte comme s'il contient le membre sur la hiérarchie implicitement référencée qui existe avec le membre sur la hiérarchie explicitement référencée. Par exemple, si un cube contient une dimension Client avec des attributs Ville et Pays, et s'il existe une relation définie entre ces deux attributs de sorte qu'une Ville a un Pays et un Pays peut contenir plusieurs Villes, l'insertion explicite de la Ville 'Londres' dans votre tuple référence implicitement le Pays 'Royaume-Uni'. Toutefois, si aucune relation d'attribut n'est définie, la relation est dans la direction opposée (par exemple, même si Ville peut avoir une relation avec Pays, vous ne pouvez pas déterminer la Ville d'habitation d'une personne en connaissant seulement le Pays de résidence de cette personne), ou il n'existe aucune relation directe entre les deux attributs définis (il peut y avoir une relation définie de Client à Ville et Client à Pays, mais aucune relation n'est définie entre Ville et Pays), dans ce cas les règles suivantes s'appliquent :  
  
-   Si la hiérarchie référencée implicitement possède un membre par défaut, celui-ci est ajouté au tuple.  
  
-   Si la hiérarchie référencée implicitement ne possède aucun membre par défaut, le **(tous)** membre de la hiérarchie par défaut est utilisé.  
  
-   Si la hiérarchie référencée implicitement ne possède aucun membre par défaut, le premier membre du niveau le plus élevé de la hiérarchie est utilisé.  
  
## <a name="one-member-tuples"></a>Tuples à un membre  
 Si l'expression de tuple possède un membre unique, MDX le convertit en tuple à un membre à des fins d'évaluation de l'expression. En d'autres termes, la fourniture d'une expression de membre `[Measures].[TestMeasure]` plutôt que d'une expression de tuple équivaut d'un point de vue fonctionnel à l'expression de tuple `( [Measures].[TestMeasure] ).`  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;MDX&#41;](../mdx/expressions-mdx.md)   
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
