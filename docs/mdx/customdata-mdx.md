---
title: CustomData (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5161bb180b6acdf8f6ab6d16d6d0f15bd8f40a39
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577811"
---
# <a name="customdata-mdx"></a>CustomData (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la valeur de la **CustomData** la propriété de chaîne de connexion s’il est défini ; sinon, **null**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CustomData()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 Le **CustomData** fonction peut récupérer le **CustomData** propriété de chaîne de connexion et passer un paramètre à utiliser par les fonctions MDX (Multidimensional Expressions) et les instructions, telles que de configuration [UserName (MDX)](../mdx/username-mdx.md) et [instruction CALL (MDX)](../mdx/mdx-data-manipulation-call.md). Par exemple, cette fonction peut être utilisée dans une expression de la sécurité dynamique pour sélectionner les membres du jeu autorisé/refusé pour la valeur de chaîne dans le **CustomData** propriété de chaîne de connexion.  
  
## <a name="example"></a>Exemple  
 La requête suivante affiche la valeur retournée par la **CustomData** fonction dans une mesure calculée :  
  
```  
WITH MEMBER [Measures].CUSTOMDATADEMO AS CUSTOMDATA()  
SELECT [Measures].CUSTOMDATADEMO ON 0  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
