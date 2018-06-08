---
title: StrToSet (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 30a69fa1c80c453aabea282d4e6293e28e244069
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34582181"
---
# <a name="strtoset-mdx"></a>StrToSet (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le jeu spécifié par une chaîne au format MDX (Multidimensional Expressions).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
StrToSet(Set_Specification [,CONSTRAINED] )   
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Specification*  
 Expression de chaîne valide spécifiant, directement ou indirectement, un jeu.  
  
## <a name="remarks"></a>Notes  
 Le **StrToSet** fonction retourne le jeu spécifié dans l’expression de chaîne. Le **StrToSet** fonction est généralement utilisée avec les fonctions définies par l’utilisateur pour retourner un jeu spécifié d’une fonction externe vers une instruction MDX, ou lorsqu’une requête MDX est paramétrable.  
  
-   Lorsque l’indicateur CONSTRAINED est utilisé, le jeu spécifié doit contenir les noms de membres qualifiés ou non qualifié ou un jeu de tuples contenant les noms de membres qualifiés ou comprise entourés les accolades {}. Cet indicateur est employé pour réduire les risques d'attaques par injection au travers de la chaîne spécifiée. Si une chaîne qui ne peut être directement résolue à des noms de membres qualifiés ou non qualifiés est fournie, l'erreur suivante s'affiche : restrictions imposées par l'indicateur CONSTRAINED dans la fonction STRTOSET n'ont pas été respectées. »  
  
-   Si l'indicateur CONSTRAINED n'est pas utilisé, vous pouvez résoudre le jeu spécifié à une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
-   Pour mieux comprendre les différences entre jeux et membres, consultez Utilisation d'expressions de jeu et Utilisation d'expressions de membre.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne le jeu de membres de la hiérarchie d’attribut State-Province à l’aide de la **StrToSet** (fonction). Le jeu spécifié fournit le qu'expression d’ensemble une expression MDX valide.  
  
```  
SELECT StrToSet ('[Geography].[State-Province].Members')  
ON 0  
FROM [Adventure Works]  
  
```  
  
 L'exemple ci-dessous retourne une erreur liée à l'indicateur CONSTRAINED. Tandis que le jeu spécifié fournit une expression d'ensemble MDX valide, l'indicateur CONSTRAINED exige l'usage de noms de membres qualifiés ou non qualifiés dans le jeu spécifié.  
  
```  
SELECT StrToSet ('[Geography].[State-Province].Members', CONSTRAINED)  
ON 0  
FROM [Adventure Works]  
  
```  
  
 L'exemple ci-après retourne la mesure Reseller Sales Amount (montant des ventes du revendeur) pour l'Allemagne et le Canada. Le jeu spécifié fourni dans la chaîne spécifiée contient des noms de membres qualifiés, conformément aux exigences de l'indicateur CONSTRAINED.  
  
```  
SELECT StrToSet ('{[Geography].[Geography].[Country].[Germany],[Geography].[Geography].[Country].[Canada]}', CONSTRAINED)  
ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
