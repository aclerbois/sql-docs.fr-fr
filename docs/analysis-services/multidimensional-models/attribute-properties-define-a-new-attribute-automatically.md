---
title: Définir un nouvel attribut automatiquement | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5368288930526435682723ba8f50d878ff029ce1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="attribute-properties---define-a-new-attribute-automatically"></a>Propriétés d’attribut : permet de définir un nouvel attribut automatiquement
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez créer un nouvel attribut dans une dimension en utilisant la modification par glisser-déplacer dans le Concepteur de dimensions.  
  
### <a name="to-create-a-new-attribute-automatically"></a>Pour créer un nouvel attribut automatiquement  
  
1.  Dans le Concepteur de dimensions, ouvrez la dimension dans laquelle vous souhaitez créer l'attribut.  
  
2.  Sous l’onglet **Structure de dimension** , sélectionnez la colonne du tableau à laquelle vous souhaitez lier l’attribut dans le volet **Vue de source de données** , puis faites-la glisser jusqu’au volet **Attributs** .  
  
     [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] crée l'attribut qui a le même nom que la colonne à laquelle il est lié. Si plusieurs attributs utilisent la même colonne, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ajoute un numéro au nom de l’attribut.  
  
## <a name="see-also"></a>Voir aussi  
 [Dimensions dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/dimensions-in-multidimensional-models.md)   
 [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
  
  
