---
title: MeasureGroupMeasures (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cf6992f80d52a98e2b242e17fc1bfff242d9dd21
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580751"
---
# <a name="measuregroupmeasures-mdx"></a>MeasureGroupMeasures (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne un jeu de mesures appartenant au groupe de mesures spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
MEASUREGROUPMEASURES(MeasureGroupName)  
```  
  
## <a name="arguments"></a>Arguments  
 *MeasureGroupName*  
 Expression de chaîne valide contenant le nom du groupe de mesures à partir duquel l'ensemble des mesures doit être extrait.  
  
## <a name="remarks"></a>Notes  
 La chaîne définie doit correspondre exactement au nom du groupe de mesures. L'usage de crochets avec espaces pour les noms des groupes de mesures n'est pas nécessaire.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne toutes les mesures du groupe de mesures Internet Sales (ventes Internet) dans le cube Adventure Works.  
  
```  
SELECT MeasureGroupMeasures('Internet Sales') ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
