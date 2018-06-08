---
title: AllMembers (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a5720c3e82fdb341635c23d13a9c6bf4346a1cc0
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577171"
---
# <a name="allmembers-mdx"></a>AllMembers (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Évalue une hiérarchie ou une expression de niveau et retourne un jeu qui contient tous les membres de la hiérarchie ou du niveau spécifié, y compris tous les membres calculés figurant dans cette hiérarchie ou ce niveau.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Hierarchy syntax  
Hierarchy_Expression.AllMembers  
  
Level syntax  
Level_Expression.AllMembers  
```  
  
## <a name="arguments"></a>Arguments  
 *Hierarchy_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une hiérarchie.  
  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
## <a name="remarks"></a>Notes  
 Le **AllMembers** fonction retourne un jeu qui contient tous les membres, qui inclut des membres calculés, dans la hiérarchie spécifiée ou d’un niveau. Le **AllMembers** fonction retourne les membres calculés, même si la hiérarchie spécifiée ou un niveau ne contient aucun membre visible.  
  
> [!IMPORTANT]  
>  Lorsqu'une dimension contient uniquement une hiérarchie visible unique, cette hiérarchie peut être désignée soit par le nom de dimension, soit par le nom de la hiérarchie, puisque le nom de dimension dans ce cas est résolu à son unique hiérarchie visible. Par exemple, `Measures.AllMembers` est une expression MDX valide parce qu'elle est résolue à la seule hiérarchie de la dimension de mesures.  
  
> [!NOTE]  
>  Le **AllMembers** fonction est sémantiquement similaire à la [AddCalculatedMembers (MDX)](../mdx/addcalculatedmembers-mdx.md) (fonction).  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne tous les membres dans le [`Date].[Calendar Year]` hiérarchie d’attribut sur l’axe des colonnes, cela inclut les membres calculés, ainsi que l’ensemble de tous les enfants de la `[Product].[Model Name]` hiérarchie sur l’axe des lignes à partir d’attributs le **Adventure Works** cube.  
  
```  
SELECT  
   [Date].[Calendar Year].AllMembers ON COLUMNS,  
   [Product].[Model Name].Children ON ROWS  
FROM  
   [Adventure Works]  
```  
  
 L’exemple suivant retourne tous les membres de la **mesures** dimension sur l’axe des colonnes, cela inclut tous les membres calculés et le jeu de tous les enfants de la `[Product].[Model Name]` hiérarchie sur l’axe des lignes à partir d’attributs le **Adventure Works** cube.  
  
```  
SELECT  
    Measures.AllMembers ON COLUMNS,  
    [Product].[Model Name].Children ON ROWS  
FROM  
    [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [AddCalculatedMembers &#40;MDX&#41;](../mdx/addcalculatedmembers-mdx.md)   
 [Enfants &#40;MDX&#41;](../mdx/children-mdx.md)   
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
