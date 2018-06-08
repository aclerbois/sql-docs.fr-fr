---
title: Laisse (MDX) | Documents Microsoft
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
- LEAVES
dev_langs:
- kbMDX
helpviewer_keywords:
- Leaves function
ms.assetid: 09f908aa-1b2d-4af9-8c8d-c023915241b2
caps.latest.revision: 32
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 2bdbf8b6c116efb305f27c048254c227918d1a26
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="leaves-mdx"></a>Leaves (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne un jeu composé de l'ensemble des attributs (éventuellement limités aux attributs appartenant à une dimension spécifique). Pour chaque attribut x dans le jeu retourné, si x correspond à l'attribut de granularité ou est directement ou indirectement lié à ce dernier, la granularité est définie à l'attribut x sans affecter la tranche. Le **laisse** fonction est conçue pour une utilisation dans une instruction SCOPE ou sur le côté gauche d’une assignation.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Leaves( [ Dimension_expression ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Dimension_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une dimension.  
  
## <a name="remarks"></a>Notes  
 Les membres feuilles sont des tuples formés par la jointure croisée du niveau le plus bas de toutes les hiérarchies d'attribut. Les membres calculés sont exclus.  
  
-   Si un nom de dimension est spécifié, le **laisse** fonction retourne un jeu qui contient les membres feuille de l’attribut clé pour la dimension spécifiée.  
  
-   Si la dimension est associée à plusieurs groupes de mesures, c'est celle de la mesure dans la portée actuelle qui est utilisée.  
  
-   Si aucun nom de dimension n'est spécifié, la fonction retourne un jeu qui contient les membres feuilles du cube tout entier.  
  
    > [!NOTE]  
    >  Si l'expression de dimension se résout à une hiérarchie et si le nom unique de la hiérarchie est identique au nom unique de la dimension (propriété de dimension de cube HierarchyUniqueNameStyle=ExcludeDimensionName et hierarchyname=dimension name), la dimension est alors utilisée.  
  
    > [!IMPORTANT]  
    >  Une erreur est générée si tous les attributs ne présentent pas la même granularité dans les groupes de mesures de l'étendue actuelle.  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
