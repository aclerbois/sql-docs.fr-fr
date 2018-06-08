---
title: Définir des propriétés de Dimension de Cube | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4401055a15afe73a1f33ee43354f7ee45f887a96
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="define-cube-dimension-properties"></a>Définir des propriétés d'une dimension de cube
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une dimension de cube est une instance d'une dimension de base de données à l'intérieur d'un cube. Une dimension de base de données peut être utilisée dans plusieurs cubes et plusieurs dimensions de cube peuvent être basées sur une seule dimension de base de données. Le tableau suivant décrit les propriétés d'une dimension de cube.  
  
|Propriété| Description|  
|--------------|-----------------|  
|**AllMemberAggregationUsage**|Contrôle la façon dont les agrégations sont conçues par le Concepteur d'agrégation de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Cette propriété peut avoir les valeurs suivantes :<br /><br /> **Full**: chaque agrégation du cube doit inclure le membre Tous.<br /><br /> **None**: aucune agrégation du cube ne peut inclure le membre Tous. Ceci est la valeur par défaut.<br /><br /> **Unrestricted**: le concepteur d’agrégation ne fait l’objet d’aucune restriction.<br /><br /> **Default**: même fonctionnalité que la valeur Unrestricted.|  
|**Description**|Fournit un nom descriptif pour le niveau.|  
|**DimensionID**|Contient l'identificateur unique (ID) de la dimension de base de données.|  
|**HierarchyUniqueNameStyle**|Détermine comment des noms uniques sont générés pour les hiérarchies qui sont contenues dans la dimension de cube. Cette propriété peut avoir les valeurs suivantes :<br /><br /> **IncludeDimensionName**:<br />                    Le nom de la dimension est inclus dans le nom de la hiérarchie. Ceci est la valeur par défaut.<br /><br /> **ExcludeDimensionName**:<br />                    Le nom de la dimension n'est pas inclus dans le nom de la hiérarchie.|  
|**ID**|Contient l'identificateur unique (ID) de la dimension de cube.|  
|**MemberUniqueNameStyle**|Détermine comment des noms uniques sont générés pour les membres des hiérarchies contenues dans la dimension de cube. Cette propriété peut avoir les valeurs suivantes :<br /><br /> **Native**:<br />                      [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] détermine automatiquement les noms uniques des membres. Ceci est la valeur par défaut.<br /><br /> **NamePath**: [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère un nom composé comprenant le nom de chaque niveau et la légende du membre.|  
|**Nom**|Contient le nom convivial de la dimension de cube. Par défaut, le nom d'une dimension de cube est le même que celui de la dimension de base de données, sauf si une autre dimension de cube du même nom est déjà définie.|  
|**Visible**|Détermine si la dimension de cube est visible. La valeur par défaut est **True**.|  
  
## <a name="see-also"></a>Voir aussi  
 [Dimensions & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-analysis-services-multidimensional-data.md)  
  
  
