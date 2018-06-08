---
title: ClosingPeriod (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3d082ec7958ab1b29b5b42108d6f9c5c78fc3470
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577361"
---
# <a name="closingperiod-mdx"></a>ClosingPeriod (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le membre qui correspond au dernier frère parmi les descendants d'un membre spécifié à un niveau spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ClosingPeriod( [ Level_Expression [ ,Member_Expression ] ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
## <a name="remarks"></a>Notes  
 Cette fonction a principalement été conçue pour être exploitée dans une dimension de type Time (dimension de temps) mais peut également être utilisée avec n'importe quelle dimension.  
  
-   Si une expression de niveau est spécifiée, la **ClosingPeriod** fonction utilise la dimension qui contient le niveau spécifié et retourne le dernier frère parmi les descendants du membre par défaut au niveau spécifié.  
  
-   Si une expression de niveau et une expression de membre sont spécifiés, la **ClosingPeriod** fonction retourne le dernier frère parmi les descendants du membre spécifié au niveau spécifié.  
  
-   Si une expression de niveau, ni une expression de membre est spécifiée, le **ClosingPeriod** fonction utilise le niveau par défaut et le membre de la dimension (le cas échéant) dans le cube avec un type de temps.  
  
 Le **ClosingPeriod** fonction est équivalente à l’instruction MDX suivante :  
  
 `Tail(Descendants(Member_Expression, Level_Expression), 1)`.  
  
> [!NOTE]  
>  Le [OpeningPeriod](../mdx/openingperiod-mdx.md) fonction est similaire à la **ClosingPeriod** de fonction, à ceci près que le **OpeningPeriod** fonction retourne le premier frère et non le dernier.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous retourne la valeur de la mesure par défaut pour le membre FY2007 de la dimension Date (doté d'un type sémantique Time). Ce membre est retourné parce que le niveau Fiscal Year (année fiscale) est le premier descendant du niveau [All], la hiérarchie Fiscal est la hiérarchie par défaut parce qu'elle est la première hiérarchie définie par l'utilisateur dans la collection des hiérarchies, et le membre FY2007 est le dernier frère de la hiérarchie à ce niveau.  
  
```  
SELECT ClosingPeriod() ON 0  
FROM [Adventure Works]  
```  
  
 L’exemple suivant retourne la valeur de la mesure par défaut pour le 30 novembre 2006 membre au niveau Date.Date.Date de la hiérarchie d’attribut Date.Date. Ce membre est le dernier frère du descendant du niveau [All] dans la hiérarchie d'attribut Date.Date.  
  
```  
SELECT ClosingPeriod ([Date].[Date].[Date]) ON 0  
FROM [Adventure Works]  
```  
  
 L'exemple ci-après retourne la valeur de la mesure par défaut pour le membre December, 2003 (décembre 2003) qui est le dernier frère du descendant du membre 2003 au niveau Year (année) dans la hiérarchie définie par l'utilisateur Calendar (calendrier).  
  
```  
SELECT ClosingPeriod ([Date].[Calendar].[Month],[Date].[Calendar].[Calendar Year].&[2003]) ON 0  
FROM [Adventure Works]  
```  
  
 L'exemple ci-après retourne la valeur de la mesure par défaut pour le membre June, 2003 (juin 2003) qui est le dernier frère du descendant du membre 2003 au niveau Year (année) dans la hiérarchie définie par l'utilisateur Fiscal.  
  
```  
SELECT ClosingPeriod ([Date].[Fiscal].[Month],[Date].[Fiscal].[Fiscal Year].&[2003]) ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OpeningPeriod &#40;MDX&#41;](../mdx/openingperiod-mdx.md)   
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)   
 [LastSibling &#40;MDX&#41;](../mdx/lastsibling-mdx.md)  
  
  
