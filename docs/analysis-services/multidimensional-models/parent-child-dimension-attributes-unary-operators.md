---
title: Opérateurs unaires dans les Dimensions Parent-enfant | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0b7f38bb378650fbd243441086df043295376581
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="parent-child-dimension-attributes---unary-operators"></a>Attributs de Dimension parent-enfant - opérateurs unaires
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans une dimension contenant une relation parent-enfant dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous spécifiez une colonne d’opérateur unaire (ou cumul personnalisé) qui détermine le cumul personnalisé de tous les membres non calculés de l’attribut parent. L'opérateur unaire est appliqué aux membres dès que les valeurs des membres parents sont évaluées. La propriété **UnaryOperatorColumn** définie sur un attribut parent (**Usage**=Parent) spécifie la colonne d’une table dans la vue de source de données qui contient les opérateurs unaires. Les valeurs des opérateurs de cumul personnalisé qui sont stockés dans cette colonne sont appliquées à chaque membre de l'attribut.  
  
 Vous pouvez créer et modifier un calcul nommé sur une table de dimension de la vue de source de données en tant que colonne d'opérateur unaire. L'expression la plus simple, soit « + », retourne le même opérateur pour tous les membres. Mais vous pouvez utiliser n'importe quelle expression dès lors qu'elle retourne un opérateur pour chaque membre.  
  
 Vous pouvez modifier manuellement la définition de la propriété **UnaryOperatorColumn** sur un attribut parent ou utiliser l’amélioration qui permet de définir une agrégation personnalisée dans l’Assistant Business Intelligence pour remplacer l’agrégation par défaut qui est associée aux membres d’une dimension. Pour plus d’informations sur l’utilisation de l’Assistant Business Intelligence pour effectuer cette configuration, consultez [Ajouter une agrégation personnalisée à une dimension](../../analysis-services/multidimensional-models/bi-wizard-add-a-custom-aggregation-to-a-dimension.md).  
  
 La définition par défaut de la propriété **UnaryOperatorColumn** sur un attribut parent est (none), qui désactive les opérateurs de cumul personnalisé. Le tableau suivant répertorie les opérateurs unaires et décrit leur comportement lorsqu'ils sont appliqués à un niveau.  
  
|Opérateurs unaires|Description|  
|--------------------|-----------------|  
|+ (signe plus)|La valeur du membre est ajoutée à la valeur d'agrégation des membres frères placés avant le membre. Ceci est l'opérateur par défaut si aucune colonne d'opérateur unaire n'est définie pour un attribut.|  
|- (signe moins)|La valeur du membre est soustraite de la valeur d'agrégation des membres frères placés avant le membre.|  
|* (astérisque)|La valeur du membre est multipliée par la valeur d'agrégation des membres frères placés avant le membre.|  
|/ (barre oblique)|La valeur du membre est divisée par la valeur d'agrégation des membres frères placés avant le membre.|  
|~ (tilde)|La valeur du membre n'est pas prise en compte.|  
  
 Les valeurs vides et toute valeur non répertoriée dans le tableau sont traitées comme l'opérateur unaire de signe positif (+). Aucun opérateur n'étant prioritaire sur un autre, l'ordre de stockage des membres dans la colonne de l'opérateur unaire détermine l'ordre d'évaluation. Pour modifier l’ordre d’évaluation, créez un nouvel attribut, affectez à sa propriété **Type** la valeur **Sequence**, puis attribuez des numéros de séquence correspondant à l’ordre d’évaluation à sa propriété **Source Column** . Vous devez également classer les membres de l'attribut en fonction de cet attribut. Pour plus d’informations sur l’utilisation de l’Assistant Business Intelligence pour classer les membres d’un attribut, consultez [Définir le classement d’une dimension](../../analysis-services/multidimensional-models/bi-wizard-define-the-ordering-for-a-dimension.md).  
  
 Vous pouvez utiliser la propriété **UnaryOperatorColumn** pour spécifier un calcul nommé qui retourne un opérateur unaire comme caractère littéral pour tous les membres de l’attribut. Il peut suffire de taper un caractère littéral comme `'*'` dans le calcul nommé. L'opérateur par défaut, le signe plus (+), est ainsi remplacé par l'opérateur de multiplication, l'astérisque (*), pour tous les membres de l'attribut. Pour plus d’informations, consultez [Définir des calculs nommés dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-named-calculations-in-a-data-source-view-analysis-services.md).  
  
 Sous l’onglet **Navigateur** du Concepteur de dimensions, vous pouvez afficher les opérateurs unaires en regard de chaque membre d’une hiérarchie. Vous pouvez également modifier les opérateurs unaires lorsque vous utilisez une dimension activée en écriture. Si la dimension n'est pas activée en écriture, vous devez utiliser un outil pour modifier directement la source de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Dimension Attribute Properties Reference](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)   
 [Opérateurs de cumul personnalisés dans les Dimensions Parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension-attributes-custom-rollup-operators.md)   
 [Démarrer l’Assistant Business Intelligence dans le Concepteur de dimensions](../../analysis-services/multidimensional-models/database-dimensions-bi-wizard-in-dimension-designer.md)  
  
  
