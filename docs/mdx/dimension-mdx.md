---
title: Dimension (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: af73d6f08432a63a207c06d361354d6e45b6a07d
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577931"
---
# <a name="dimension-mdx"></a>Dimension (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la hiérarchie qui contient un membre, un niveau ou une hiérarchie spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Hierarchy syntax  
Hierarchy_Expression.Dimension  
  
Level syntax  
Level_Expression.Dimension  
  
Member syntax  
Member_Expression.Dimension  
  
```  
  
## <a name="arguments"></a>Arguments  
 *Hierarchy_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une hiérarchie.  
  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
### <a name="examples"></a>Exemples  
 L’exemple suivant utilise le **Dimension** fonction, conjointement avec la **nom** pour retourner le nom de la hiérarchie du membre spécifié.  
  
```  
WITH member measures.x as [Product].[Product Model Lines].[Model].&[HL Road Tire].Dimension.Name  
SELECT measures.x on 0  
FROM [Adventure Works]  
```  
  
 L'exemple ci-dessous utilise conjointement la fonction Dimension avec les fonctions Levels et Count pour retourner le nombre de niveaux dans la hiérarchie contenant le membre spécifié.  
  
```  
WITH member measures.x as [Product].[Product Model Lines].[Model].&[HL Road Tire].Dimension.Levels.Count  
SELECT measures.x on 0  
FROM [Adventure Works]  
```  
  
 L’exemple suivant utilise le **Dimension** fonction, conjointement avec la **membres** et **nombre** fonctions, pour retourner le nombre de membres dans la hiérarchie contenant le membre spécifié.  
  
```  
WITH member measures.x as [Product].[Product Model Lines].[Model].&[HL Road Tire].Dimension.Members.Count  
SELECT measures.x on 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Nombre &#40;des niveaux de hiérarchie&#41; &#40;MDX&#41;](../mdx/count-hierarchy-levels-mdx.md)   
 [Nombre &#40;définir&#41; &#40;MDX&#41;](../mdx/count-set-mdx.md)   
 [Niveaux &#40;MDX&#41;](../mdx/levels-mdx.md)   
 [Membres &#40;définir&#41; &#40;MDX&#41;](../mdx/members-set-mdx.md)   
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
