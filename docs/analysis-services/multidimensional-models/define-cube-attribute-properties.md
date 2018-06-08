---
title: Définir des propriétés d’attribut de Cube | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f7e2ab2374955710452f1ba1cba91e3a4d8ff8c1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="define-cube-attribute-properties"></a>Définir des propriétés d'attributs de cube
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les propriétés des attributs de cube vous permettent de spécifier des paramètres uniques pour les attributs de dimension des dimensions de cube à partir de la même dimension de base de données. Le tableau suivant décrit les propriétés d'un attribut de cube.  
  
|Propriété| Description|  
|--------------|-----------------|  
|**AggregationUsage**|Indique le mode de création des agrégations d'attribut par l'Assistant Conception d'agrégation. La valeur par défaut est **Default**. Cette propriété peut avoir les valeurs suivantes :<br /><br /> **Par défaut**:<br />                    L'Assistant Conception d'agrégation applique une règle par défaut en fonction du type d'attribut (Full pour les clés, Unrestricted pour les autres types).<br /><br /> **Aucun**:<br />                    Aucune agrégation du cube ne doit inclure cet attribut.<br /><br /> **Illimité**:<br />                    L’Assistant Conception d’agrégation ne fait l’objet d’aucune restriction.<br /><br /> **Complet**:<br />                    Toutes les agrégations de cube doivent inclure cet attribut.|  
|**AttributeHierarchyEnabled**|Indique si la hiérarchie d'attributs est activée sur cette dimension de cube. Cet attribut permet de désactiver les hiérarchies d'attributs sur des cubes ou des rôles de dimension spécifiques. Ce paramètre est sans effet si la hiérarchie d'attributs sous-jacente est désactivée. La valeur par défaut est **True**.|  
|**OptimizedState**|Indique si la hiérarchie d'attributs est optimisée sur cette dimension de cube. Cet attribut permet d'optimiser les hiérarchies d'attributs sur des cubes ou des rôles de dimension spécifiques. Ce paramètre est sans effet si la hiérarchie d'attributs sous-jacente n'est pas optimisée. La valeur par défaut est **FullyOptimized**. Cette propriété peut avoir les valeurs suivantes :<br /><br /> **FullyOptimized**: L’instance construit des index pour la hiérarchie pour augmenter les performances des requêtes. Ceci est la valeur par défaut.<br /><br /> **NotOptimized**:<br />                    L'instance ne construit pas d'index supplémentaire.|  
|**AttributeHierarchyVisible**|Indique si la hiérarchie d'attributs est visible sur cette dimension de cube. Cet attribut permet de rendre visible les hiérarchies d'attributs sur des cubes ou des rôles de dimension spécifiques. Ce paramètre est sans effet si la hiérarchie d'attributs sous-jacente n'est pas visible. La valeur par défaut est **True**.|  
|**AttributeID**|Contient l'identificateur unique (ID) de l'attribut.|  
  
## <a name="see-also"></a>Voir aussi  
 [Définir des propriétés de Dimension de Cube](../../analysis-services/multidimensional-models/define-cube-dimension-properties.md)   
 [Définir des propriétés de hiérarchie de Cube](../../analysis-services/multidimensional-models/define-cube-hierarchy-properties.md)  
  
  
