---
title: Cubes dans les modèles multidimensionnels | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5962889f38043e675b70558e7561bfc3f63b39ce
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="cubes-in-multidimensional-models"></a>Cubes dans les modèles multidimensionnels
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un cube est une structure multidimensionnelle qui contient des informations à des fins analytiques ; les constituants principaux d'un cube sont des dimensions et des mesures. Les dimensions définissent la structure du cube que vous utilisez pour la découpe, et les mesures fournissent des valeurs numériques agrégées qui intéressent l'utilisateur final. Comme structure logique, un cube permet à une application cliente de récupérer des valeurs, de mesures, comme si elles étaient contenues dans des cellules du cube ; les cellules sont définies pour chaque valeur résumée possible. Une cellule, dans le cube, est définie par l'intersection des membres de dimension et contient les valeurs agrégées des mesures à cette intersection spécifique.  
  
## <a name="benefits-of-using-cubes"></a>Avantages de l'utilisation des cubes  
 Un cube fournit un seul emplacement où toutes les données associées, pour l'analyse, sont stockées.  
  
## <a name="components-of-cubes"></a>Composants des cubes  
 Un cube est composé de :  
  
|Élément|Description|  
|-------------|-----------------|  
|Dimensions|[Dimensions dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/dimensions-in-multidimensional-models.md)|  
|Mesures et groupes de mesures|[Créer des mesures et groupes de mesures dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/create-measures-and-measure-groups-in-multidimensional-models.md)|  
|Partitions|[Partitions dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/partitions-in-multidimensional-models.md)|  
|Perspectives|[Perspectives dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/perspectives-in-multidimensional-models.md)|  
|Hierarchies|[Créer des hiérarchies définies par l’utilisateur](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md)|  
|Actions|[Actions dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/actions-in-multidimensional-models.md)|  
|Indicateurs de performance clés (KPI)|[Indicateurs de Performance clés & #40 ; Indicateurs de performance clés & #41 ; dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/key-performance-indicators-kpis-in-multidimensional-models.md)|  
|calculs|[Calculs dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)|  
|Traductions|[Traductions dans les modèles multidimensionnels &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/translations-in-multidimensional-models-analysis-services.md)|  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Créer un Cube à l’aide de l’Assistant Cube](../../analysis-services/multidimensional-models/create-a-cube-using-the-cube-wizard.md)|Décrit comment utiliser l'Assistant Cube pour définir un cube, des dimensions, des attributs de dimension et des hiérarchies définies par l'utilisateur.|  
|[Créer des mesures et groupes de mesures dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/create-measures-and-measure-groups-in-multidimensional-models.md)|Décrit comment définir des groupes de mesures.|  
|[Calculs dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)|Décrit comment définir et configurer un calcul dans un script MDX.|  
|[Actions dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/actions-in-multidimensional-models.md)|Décrit comment définir et configurer une action.|  
|[Perspectives dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/perspectives-in-multidimensional-models.md)|Décrit comment définir et configurer une perspective.|  
|[Définition de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/defining-stored-procedures.md)|Décrit comment utiliser les procédures stockées.|  
  
  
