---
title: RollupChildren (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 18996c8126ceddad73aad65e50099764429b830f
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580701"
---
# <a name="rollupchildren-mdx"></a>RollupChildren (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne une valeur générée par le cumul des valeurs des enfants d'un membre spécifié à l'aide de l'opérateur unaire spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RollupChildren(Member_Expression, Unary_Operator)   
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Unary_Operator*  
 Expression de chaîne valide qui spécifie un opérateur unaire.  
  
## <a name="remarks"></a>Notes  
 Le **RollupChildren** fonction cumule les valeurs des enfants du membre spécifié à l’aide de l’opérateur unaire spécifié.  
  
 Le tableau ci-dessous décrit les opérateurs unaires valides pour cette fonction.  
  
|Opérateur|Résultats|  
|--------------|------------|  
|**+**|total = total + enfant actuel|  
|**-**|total = total - enfant actuel|  
|**\***|total = total * enfant actuel|  
|**/**|total = total / enfant actuel|  
|**%**|total = (total / enfant actuel) * 100|  
|**~**|L’enfant n’est pas utilisé dans le correctif cumulatif. Sa valeur est ignorée.|  
  
 Si l'opérateur dans la propriété de membre ne figure pas dans la liste, une erreur se produit. L'ordre d'évaluation est déterminé par l'ordre des frères, et non par la priorité des opérateurs.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous utilise une propriété de membre appelée « Alternate Rollup Operator » qui contient des valeurs alternatives permettant aux opérateurs unaires de cumuler les enfants de la hiérarchie Net Profit dans la dimension Account de manière alternative. Cette propriété de membre n'existe pas dans le cube Adventure Works mais peut être créée. Cette utilisation de la **RollupChildren** fonction peut être utilisée dans une application de budgétisation pour l’analyse de simulation.  
  
```  
RollupChildren  
   ( [Account].[Net Profit]  
   , [Account].CurrentMember.Properties ('Alternate Rollup Operator') )  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
