---
title: Intersect (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7acc509891dd1c975ad819bd904e840e369f0d4b
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578521"
---
# <a name="intersect-mdx"></a>Intersect (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne l'intersection de deux ensembles d'entrée, en conservant éventuellement les doublons.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Intersect(Set_Expression1 , Set_Expression2 [ , ALL ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression1*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Set_Expression2*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
## <a name="remarks"></a>Notes  
 Le **Intersect** fonction retourne l’intersection de deux jeux. Par défaut, cette fonction supprime les doublons des deux ensembles avant l'intersection. Les deux ensembles spécifiés doivent avoir le même dimensionnement.  
  
 Le paramètre facultatif **tous les** indicateur conserve les doublons. Si **tous les** est spécifié, le **Intersect** fonction entre en intersection avec les éléments qui comme d’habitude et que l’intersection de chaque doublon dans le premier jeu qui a un doublon correspondant dans le deuxième jeu. Les deux ensembles spécifiés doivent avoir le même dimensionnement.  
  
## <a name="example"></a>Exemple  
 La requête suivante retourne les années 2003 et 2004, les deux membres qui apparaissent dans les deux ensembles spécifiés :  
  
 `SELECT`  
  
 `INTERSECT(`  
  
 `{[Date].[Calendar Year].&[2001], [Date].[Calendar Year].&[2002],[Date].[Calendar Year].&[2003]}`  
  
 `, {[Date].[Calendar Year].&[2002],[Date].[Calendar Year].&[2003], [Date].[Calendar Year].&[2004]})`  
  
 `ON 0`  
  
 `FROM`  
  
 `[Adventure Works]`  
  
 La requête suivante est inopérante, car les deux jeux spécifiés contiennent des membres rattachés à des hiérarchies différentes :   
  
 `SELECT`  
  
 `INTERSECT(`  
  
 `{[Date].[Calendar Year].&[2001]}`  
  
 `, {[Customer].[City].&[Abingdon]&[ENG]})`  
  
 `ON 0`  
  
 `FROM`  
  
 `[Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
