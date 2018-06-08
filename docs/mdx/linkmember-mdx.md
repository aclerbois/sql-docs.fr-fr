---
title: LinkMember (MDX) | Documents Microsoft
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
- LINKMEMBER
dev_langs:
- kbMDX
helpviewer_keywords:
- LinkMember function
ms.assetid: b9106f07-8ea2-4933-aed3-ee9c63acf7ac
caps.latest.revision: 33
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 54fa1459b4ff16aa290f6ff5f4006ebb548dd7da
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="linkmember-mdx"></a>LinkMember (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le membre équivalent à un membre spécifié dans une hiérarchie spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
LinkMember(Member_Expression, Hierarchy_Expression)   
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Hierarchy_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne une hiérarchie.  
  
## <a name="remarks"></a>Notes  
 Le **LinkMember** fonction retourne le membre de la hiérarchie spécifiée qui correspond aux valeurs de clé à chaque niveau du membre spécifié dans une hiérarchie associée. À chaque niveau, les attributs doivent présenter la même cardinalité de clé et le même type de données. Dans les hiérarchies non naturelles, s'il y a plus d'une correspondance pour la valeur de clé d'un attribut, le résultat sera une erreur ou sera indéterminé.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise le **LinkMember** fonction pour retourner la mesure par défaut dans le cube Adventure Works des ascendants du membre du 1er juillet 2002 de la hiérarchie d’attribut Date.Date dans la hiérarchie de calendrier.  
  
```  
SELECT  Hierarchize  
   (Ascendants   
      (LinkMember   
         ([Date].[Date].[July 1, 2002], [Date].[Calendar]  
         )  
       )  
    ) ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Hierarchize & #40 ; MDX & #41 ;](../mdx/hierarchize-mdx.md)   
 [Ascendants &#40;MDX&#41;](../mdx/ascendants-mdx.md)   
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
