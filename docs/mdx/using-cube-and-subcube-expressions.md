---
title: À l’aide de Cube et les Expressions de sous-cube | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 45ead923cd69361e49b3da43c52aa8941781afe3
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34582271"
---
# <a name="using-cube-and-subcube-expressions"></a>Utilisation d'expressions de cube et de sous-cube
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Les expressions de cube et de sous-cube sont utilisées dans des instructions MDX (Multidimensional Expressions) pour définir, manipuler ou récupérer des données à partir d'un cube ou d'un sous-cube.  
  
## <a name="cube-expressions"></a>Expressions de cube  
 Une expression de cube contient soit un identificateur de cube, soit le mot clé CURRENTCUBE, et ne sont par conséquent que des expressions simples. De nombreuses instructions MDX utilisent le mot clé CURRENTCUBE pour identifier le contexte du cube en cours plutôt que d'exiger un identificateur de cube.  
  
 Un identificateur de cube apparaît comme *Cube_Name* dans les descriptions de notation BNF des instructions MDX.  
  
 Des expressions de cube peuvent apparaître dans plusieurs emplacements. Dans une instruction MDX SELECT, elles spécifient le cube à partir duquel les données doivent être extraites. Dans l'exemple de requête suivant, l'expression [Adventure Works] fait référence au cube de ce nom :  
  
 `SELECT [Measures].[Internet Sales Amount] ON COLUMNS`  
  
 `FROM [Adventure Works]`  
  
 Dans l'instruction CREATE MEMBER, l'expression de cube spécifie sur quel cube le membre calculé que vous créez doit figurer. Dans l'exemple suivant, l'instruction crée une mesure calculée sur la dimension de mesures du cube Adventure Works :  
  
 `CREATE MEMBER [Adventure Works].[Measures].[Test] AS 1`  
  
 Lorsque vous utilisez l'instruction CREATE MEMBER à l'intérieur d'un Script MDX, le nom du cube peut être remplacé par le mot clé CURRENTCUBE, étant donné que le cube où le membre calculé à créer doit être le même cube auquel le Script MDX appartient, comme indiqué dans l'exemple suivant :  
  
 `CREATE MEMBER CURRENTCUBE.[Measures].[Test] AS 1;`  
  
 Cela rend plus facile à copier et coller des définitions de membre calculé à partir d’un cube à l’autre, car le nom du cube est n’est plus codé en dur.  
  
## <a name="subcube-expressions"></a>Expressions de sous-cube  
 Une expression de sous-cube peut contenir un identificateur de sous-cube ou une instruction MDX qui retourne un sous-cube. Si l'expression de sous-cube contient un identificateur de sous-cube, il s'agira d'une expression simple. Si elle contient une instruction MDX qui retourne un sous-cube, il s'agit d'une instruction complexe. L'instruction MDX SELECT, par exemple, retourne un sous-cube et peut être utilisée partout où les expressions de sous-cube sont autorisées, comme indiqué dans l'exemple suivant :  
  
 `SELECT [Measures].MEMBERS ON COLUMNS,`  
  
 `[Date].[Calendar Year].MEMBERS ON ROWS`  
  
 `FROM`  
  
 `(SELECT [Measures].[Internet Sales Amount] ON COLUMNS,`  
  
 `[Date].[Calendar Year].&[2004] ON ROWS`  
  
 `FROM [Adventure Works])`  
  
 Cette utilisation d'une instruction SELECT dans la clause FROM s'appelle aussi une sous-sélection.  
  
 Un autre scénario courant qui utilise des expressions de sous-cube est celui de la création des attributions d'étendues dans un Script MDX. Dans l'exemple suivant, l'instruction SCOPE est utilisée pour limiter une attribution à un sous-cube qui se compose de [Measures].[Internet Sales Amount] :  
  
 `SCOPE([Measures].[Internet Sales Amount]);`  
  
 `This=1;`  
  
 `END SCOPE;`  
  
 Un identificateur de sous-cube apparaît comme *Subcube_Name*. dans les descriptions de notation BNF des instructions MDX.  
  
## <a name="see-also"></a>Voir aussi  
 [La requête MDX de base &#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-query-the-basic-query.md)   
 [Création de sous-cubes dans MDX &#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/building-subcubes-in-mdx-mdx.md)   
 [Instruction CREATE SUBCUBE &#40;MDX&#41;](../mdx/mdx-data-definition-create-subcube.md)   
 [Expressions &#40;MDX&#41;](../mdx/expressions-mdx.md)   
 [Instruction SCOPE &#40;MDX&#41;](../mdx/mdx-scripting-scope.md)  
  
  
