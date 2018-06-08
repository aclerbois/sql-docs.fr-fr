---
title: Fonctions (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- DMX
helpviewer_keywords:
- DMX [Analysis Services], functions
- VBA [DMX]
- stored procedures [DMX]
- functions [DMX]
- Excel [DMX]
- Data Mining Extensions [Analysis Services], functions
ms.assetid: 75ab6346-f4a4-4699-90f3-66d35f930ed7
caps.latest.revision: 39
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 50a80c12549555f68a819de433faa39a1628a0c4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-dmx"></a>Fonctions (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Lorsque vous utilisez des Extensions DMX (Data Mining) pour les objets de requête dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], vous pouvez utiliser des fonctions pour retourner des informations plus que simplement les valeurs dans les colonnes dans le modèle d’exploration de données ou le dataset d’entrée. Les requêtes DMX permettent de retourner, par exemple, non seulement la valeur de prévision d'une colonne, mais également la probabilité que la prévision est correcte. Outre les fonctions DMX, vous pouvez également utiliser les fonctions de Microsoft Visual Basic for Applications (VBA), Microsoft Excel et les procédures stockées.  
  
## <a name="dmx-functions"></a>Fonctions DMX  
 Les fonctions DMX permettent d'exécuter les tâches suivantes :  
  
-   Retourner des prévisions  
  
-   Retourner des statistiques sur une prévision, telles que la probabilité et la prise en charge.  
  
-   Filtrer les résultats de la requête.  
  
-   Réorganiser une expression de table.  
  
 Les plupart des fonctions DMX retournent une valeur scalaire, comme la prise en charge d'une prévision, mais certaines retournent un résultat tabulaire. Par exemple, la fonction PredictHistogram retourne une table qui contient la prise en charge et la probabilité pour chaque état de la colonne prédictible spécifiée. Les résultats sont affichés dans une nouvelle colonne tabulaire.  
  
 **Pour plus d’informations :** [fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md), [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)  
  
## <a name="visual-basic-for-applications-vba-and-excel-functions"></a>Fonctions Visual Basic for Applications (VBA) et Excel  
 Outre les fonctions DMX, il est également possible d'appeler un grand nombre de fonctions VBA et Excel à l'aide des instructions DMX. Par exemple, vous pouvez utiliser la fonction lCase pour modifier le mode d’affichage de la colonne Attribute_Name dans le contenu du modèle TM_Decision_Tree. Cela est illustré par l'exemple de code suivant.  
  
```  
SELECT lCase([Attribute_Name])   
FROM [TM_Decision_Tree].CONTENT  
```  
  
 Si la même fonction existe dans VBA et Excel, vous devez faire précéder le nom de fonction dans votre instruction DMX avec **VBA** ou **Excel**. Par exemple, imaginez que vous utilisiez `VBA!Log` ou `Excel!Log`. Si la fonction VBA ou Excel que vous souhaitez utiliser existe également dans le langage DMX ou MDX (Multidimensional Expressions), ou si elle contient le caractère dollar ($), vous devez utiliser des crochets ([]) pour l'isoler. Dans ce cas, l'appel de fonction sera `[VBA!Format]`.  
  
## <a name="stored-procedures"></a>Procédures stockées  
 Vous pouvez utiliser les langages de programmation clr (common langage runtime) pour créer des procédures stockées qui étendent les fonctionnalités de DMX. Par exemple, un modèle d’exploration de données d’arbre de régression retourne des coefficients, tels que A, B et ainsi de suite, qui décrivent l’équation de régression, mais le modèle ne retourne pas de l’équation elle-même, tels que A + Bx = y. Cependant, vous pouvez écrire une procédure stockée qui utilise l'objet de modèle d'exploration de données pour naviguer dans le schéma de contenu, et pour retourner l'équation de régression en tant que résultat. Une instruction DMX peut donc retourner la liste des équations de régression en tant que résultats de requête.  
  
 **Pour plus d’informations :** [gestion des assemblys de modèle multidimensionnel](../analysis-services/multidimensional-models/multidimensional-model-assemblies-management.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence des opérateurs](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Data Mining Extensions &#40;DMX&#41; éléments de syntaxe](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Structure et utilisation des requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Présentation de l’instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
