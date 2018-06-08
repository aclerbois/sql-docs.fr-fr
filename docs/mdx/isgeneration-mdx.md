---
title: IsGeneration (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 938ed8cbfab24643ceb1294a3831290c43e1bb5c
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578641"
---
# <a name="isgeneration-mdx"></a>IsGeneration (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne une valeur indiquant si un membre spécifié est dans une génération spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
IsGeneration(Member_Expression, Generation_Number)   
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Generation_Number*  
 Expression numérique valide qui précise la génération par rapport à laquelle le membre spécifié est évalué.  
  
## <a name="remarks"></a>Notes  
 Le **IsGeneration** fonction renvoie **true** si le membre spécifié se trouve dans le numéro de génération spécifiée. Sinon, la fonction retourne **false**. Également, si le membre spécifié prend la valeur d’un membre vide, le **IsGeneration** fonction renvoie **false**.  
  
 Pour des besoins d'indexation des générations, les membres feuilles portent l'index de génération 0. Pour déterminer l'index de génération des membres non feuilles, prenez tout d'abord l'index de génération le plus élevé à partir de l'union de tous les membres enfants du membre spécifié, puis ajoutez 1 à cet index. En raison du mode de détermination de l'index de génération des membres non feuilles, un membre non feuille spécifique peut appartenir à plusieurs générations.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne la valeur TRUE si [Date].[Fiscal].CurrentMember appartient à la deuxième génération :  
  
 `WITH MEMBER MEASURES.ISGENERATIONDEMO AS`  
  
 `IsGeneration([Date].[Fiscal].CURRENTMEMBER, 2)`  
  
 `SELECT {MEASURES.ISGENERATIONDEMO} ON 0,`  
  
 `[Date].[Fiscal].MEMBERS ON 1`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
