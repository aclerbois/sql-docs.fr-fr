---
title: Axe (MDX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: ''
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- AXIS
dev_langs:
- kbMDX
helpviewer_keywords:
- Axis function
ms.assetid: a3a60a1e-e266-4fa1-ae13-bae73544de33
caps.latest.revision: 31
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 2503dcaa13db79acd5cff94c78ad365ee41b0b9b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="axis-mdx"></a>Axis (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne l'ensemble des tuples sur un axe spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Axis(Axis_Number)  
```  
  
## <a name="arguments"></a>Arguments  
 *Axis_Number*  
 Expression numérique valide qui précise le numéro d'axe.  
  
## <a name="remarks"></a>Notes  
 Le **axe** fonction utilise la position de base zéro d’un axe pour retourner le jeu de tuples sur un axe. Par exemple, `Axis(0)` retourne l'axe COLUMNS, `Axis(1)` retourne l'axe ROWS, et ainsi de suite. Le **axe** fonction ne peut pas être utilisée sur l’axe de filtre. Cette fonction peut être utilisée pour faire connaître à des membres calculés le contexte de la requête en cours d'exécution. Par exemple, vous pouvez avoir besoin d'un membre calculé qui fournit la somme des membres sélectionnés seulement sur l'axe des lignes. Elle peut également être utilisée pour faire dépendre la définition d'un axe de la définition d'un autre. Par exemple, en classant le contenu de l'axe des lignes selon la valeur du premier élément sur l'axe des colonnes.  
  
> [!NOTE]  
>  Un axe ne peut référencer qu'un axe antérieur. Par exemple, `Axis(0)` doit se produire après l'évaluation de l'axe COLUMNS, comme sur un axe ROW ou PAGE.  
  
## <a name="examples"></a>Exemples  
 L'exemple de requête suivant montre comment utiliser la fonction Axis :  
  
 `WITH MEMBER MEASURES.AXISDEMO AS`  
  
 `SETTOSTR(AXIS(1))`  
  
 `SELECT MEASURES.AXISDEMO ON 0,`  
  
 `[Date].[Calendar Year].MEMBERS ON 1`  
  
 `FROM [Adventure Works]`  
  
 L'exemple suivant illustre l'utilisation de la fonction Axis au sein d'un membre calculé :  
  
 `WITH MEMBER MEASURES.AXISDEMO AS`  
  
 `SUM(AXIS(1), [Measures].[Internet Sales Amount])`  
  
 `SELECT {[Measures].[Internet Sales Amount],MEASURES.AXISDEMO} ON 0,`  
  
 `{[Date].[Calendar Year].&[2003], [Date].[Calendar Year].&[2004]} ON 1`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
