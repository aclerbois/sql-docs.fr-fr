---
title: Prédire (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 238da79ca85c3b0a59d3a043fbd2ca7caecd020f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580981"
---
# <a name="predict-mdx"></a>Predict (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

    
> [!CAUTION]  
>  Cette fonction est en cours de suppression en raison d'incohérences internes.  
>   
>  Passez en revue la section d'exemple pour connaître la solution de contournement en cas d'utilisation d'une expression DMX.  
  
 Retourne une valeur d'expression numérique évaluée sur un modèle d'exploration de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Predict(Mining_Model_Name,String_Expression)   
```  
  
## <a name="arguments"></a>Arguments  
 *Mining_Model_Name*  
 Expression de chaîne valide qui représente le nom d'un modèle d'exploration.  
  
 *String_Expression*  
 Expression de chaîne valide qui prend la valeur d'une expression DMX valide pour le modèle d'exploration de données spécifié.  
  
## <a name="remarks"></a>Notes  
 Le **Predict** fonction évalue l’expression de chaîne spécifiée dans le contexte du modèle d’exploration de données spécifié.  
  
 La syntaxe et les fonctions d'exploration de données sont documentées dans les références MDX (Multidimensional Expressions).  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous cite en prévision le nom du cluster et la distance qui le sépare d'un client particulier à l'aide du modèle d'exploration de données des clusters du client :  
  
```  
WITH MEMBER MEASURES.CLNAME AS   
PREDICT("Customer Clusters", "Cluster()")  
MEMBER MEASURES.CLDISTANCE AS   
PREDICT("Customer Clusters", "ClusterDistance(Cluster())")  
SELECT {MEASURES.CLNAME, MEASURES.CLDISTANCE} ON 0   
FROM [Adventure Works]  
WHERE([Customer].[Customer Geography].[Customer].&[12012])  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
