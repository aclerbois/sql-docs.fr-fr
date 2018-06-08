---
title: Construction jeux nommés dans MDX (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b893dcd86ffcfa68c55057796c431e694903c46a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-named-sets---building-named-sets"></a>MDX des jeux nommés - création de jeux nommés
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Une expression d'ensemble peut être une déclaration assez longue, complexe et donc difficile à assimiler. Ou bien, il est possible que vous l'utilisiez si souvent qu'il devient pénible de la redéfinir à chaque fois. Pour faciliter l’utilisation d’une expression longue, complexe ou fréquemment utilisée, MDX (Multidimensional Expressions) vous permet de traiter une telle expression sous forme de *jeu nommé*.  
  
 Un jeu nommé est essentiellement une expression d'ensemble à laquelle un alias a été attribué. Il peut comprendre les membres ou les fonctions pouvant être normalement intégrées dans un jeu. MDX traitant l'alias du jeu nommé comme une expression d'ensemble, vous pouvez utiliser cet alias où une telle expression est acceptée.  
  
 Vous pouvez définir pour un jeu nommé l'un des contextes suivants :  
  
-   **Étendue de requête** Pour créer un jeu nommé défini en tant que partie d’une requête MDX, et dont l’étendue est donc limitée à la requête, utilisez le mot clé WITH. Vous pouvez ensuite utiliser le jeu nommé au sein d'une instruction MDX SELECT. De la sorte, vous pouvez modifier le jeu nommé créé à l'aide du mot clé WITH sans porter atteinte à l'instruction SELECT.  
  
     Pour plus d’informations sur l’utilisation du mot clé WITH pour la création de jeux nommés, consultez [Création de jeux nommés d’étendue de requête &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-named-sets-creating-query-scoped-named-sets.md).  
  
-   **Étendue de session** Pour créer un jeu nommé dont l’étendue est plus vaste que le contexte de la requête, c’est-à-dire dont l’étendue est la durée de la session MDX, utilisez l’instruction CREATE SET. Un jeu nommé défini à l'aide de l'instruction CREATE SET est disponible pour toutes les requêtes MDX de cette session. L'instruction CREATE SET est appropriée par exemple dans le cas d'une application cliente qui réutilise le même jeu dans différentes requêtes.  
  
     Pour plus d’informations sur l’utilisation de l’instruction CREATE SET pour la création de jeux nommés dans une session, consultez [Création de jeux nommés dans l’étendue de session &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/mdx-named-sets-creating-session-scoped-named-sets.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Instruction SELECT & #40 ; MDX & #41 ;](../../../mdx/mdx-data-manipulation-select.md)   
 [CRÉER une instruction SET & #40 ; MDX & #41 ;](../../../mdx/mdx-data-definition-create-set.md)   
 [Principes de base de requête MDX & #40 ; Analysis Services & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)  
  
  
