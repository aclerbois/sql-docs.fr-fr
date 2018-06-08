---
title: Définition d’un contexte de Cube dans une requête (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2efdfc74bf45f4e8e6b913e651b0be5fa4511034
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="establishing-cube-context-in-a-query-mdx"></a>Définition d'un contexte de cube dans une requête (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Chaque requête MDX s'exécute dans un contexte de cube spécifié. Ce contexte définit les membres qui sont évalués par les expressions incluses dans la requête.  
  
 Dans l'instruction SELECT, la clause FROM détermine le contexte de cube. Ce contexte peut être le cube complet ou seulement un sous-cube de ce cube. Après avoir spécifié le contexte de cube à l'aide de la clause FROM, vous pouvez utiliser d'autres fonctions pour développer ou restreindre ce contexte.  
  
> [!NOTE]  
>  Les instructions SCOPE et CALCULATE vous permettent également de gérer le contexte de cube à l'intérieur d'un script MDX. Pour plus d’informations, consultez [Principes de base des scripts MDX &#40;Analysis Services&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-scripting-fundamentals-analysis-services.md).  
  
## <a name="from-clause-syntax"></a>Syntaxe de la clause FROM  
 La syntaxe suivante décrit la clause FROM :  
  
```  
<SELECT subcube clause> ::=  
   Cube_Identifier |   
   (SELECT [  
      * |   
      ( <SELECT query axis clause> [ , <SELECT query axis clause> ... ] ) ]   
   FROM <SELECT subcube clause> <SELECT slicer axis clause> )  
```  
  
 Dans cette syntaxe, notez que c'est la clause `<SELECT subcube clause>` qui décrit le cube ou le sous-cube sur lequel s'exécute l'instruction SELECT.  
  
 Un exemple simple de clause FROM pourrait s'exécuter sur l'ensemble du cube Adventure Works. Une telle clause FROM aurait le format suivant :  
  
```  
FROM [Adventure Works]  
```  
  
 Pour plus d’informations sur la clause FROM de l’instruction MDX SELECT, consultez [Instruction SELECT &#40;MDX&#41;](../../../mdx/mdx-data-manipulation-select.md).  
  
## <a name="refining-the-context"></a>Affinement du contexte  
 Bien que la clause FROM spécifie le contexte de cube comme dans un cube unique, cela ne doit pas vous empêcher d'utiliser simultanément des données issues de plusieurs cubes.  
  
 Vous pouvez utiliser la fonction [LookupCube](../../../mdx/lookupcube-mdx.md) MDX pour récupérer des données de cubes en dehors du contexte des cubes. De plus, il existe des fonctions telles que [Filter](../../../mdx/filter-mdx.md) qui permettent de restreindre temporairement le contexte durant l’évaluation de la requête.  
  
## <a name="see-also"></a>Voir aussi  
 [Principes de base de requête MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)  
  
  
