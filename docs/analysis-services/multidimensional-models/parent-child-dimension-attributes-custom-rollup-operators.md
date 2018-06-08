---
title: Opérateurs de cumul personnalisés dans les Dimensions Parent-enfant | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 000d6355aee1fc38aa4fdcb97cf02df2a4ef09da
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="parent-child-dimension-attributes---custom-rollup-operators"></a>Attributs de Dimension parent-enfant - opérateurs de cumul personnalisé
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les opérateurs de cumul personnalisé proposent une façon simple de contrôler le mode de cumul des valeurs de membre dans les valeurs parentes à l'intérieur d'une hiérarchie parent-enfant. Dans une dimension dotée d'une relation parent-enfant, vous spécifiez une colonne contenant des opérateurs unaires qui spécifient le cumul de tous les membres non calculés de l'attribut parent. L'opérateur unaire est appliqué aux membres dès que les valeurs des membres parents sont évaluées.  
  
 Les opérateurs unaires sont stockés dans la colonne définie par la propriété **UnaryOperatorColumn** de l'attribut parent et ils sont appliqués à chaque membre de l'attribut. La colonne spécifiée par cette propriété peut résider dans la table de dimension ou dans une table liée à la table de dimension par une clé étrangère de la table de dimension.  
  
 Les opérateurs de cumul personnalisé proposent des fonctionnalités analogues aux formules de membre personnalisées, mais plus simples. Une formule de membre personnalisée utilise les expressions MDX (Multidimensional Expressions) pour déterminer le mode de cumul des membres. De son côté, un opérateur de cumul personnalisé utilise un simple opérateur unaire pour déterminer comment la valeur d'un membre affecte le parent. Les formules de membre personnalisées du niveau précédent d'une dimension remplacent l'opérateur de cumul personnalisé d'un niveau.  
  
## <a name="custom-rollup-precedence"></a>Précédence du cumul personnalisé  
 En termes de précédence, les opérateurs de cumul personnalisé de l'attribut source d'un niveau de la hiérarchie remplacent les formules de membre personnalisées du niveau précédent. Toutefois, les formules de membre personnalisées d'un niveau sont prioritaires sur les opérateurs de cumul personnalisés du niveau suivant.  
  
## <a name="see-also"></a>Voir aussi  
 [Définir des formules de membre personnalisées](../../analysis-services/multidimensional-models/attribute-properties-define-custom-member-formulas.md)   
 [Opérateurs unaires dans les Dimensions Parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension-attributes-unary-operators.md)  
  
  
