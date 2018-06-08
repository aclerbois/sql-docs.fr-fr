---
title: Élément NamingTemplate (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: f6b25e36c78d8e03afd0c4a3ebb4ef9a96d2c9f8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="namingtemplate-element-assl"></a>Élément NamingTemplate (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit la façon dont les niveaux sont appelés dans une hiérarchie parent-enfant construite à partir de la [DimensionAttribute](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md) élément parent.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DimensionAttribute>  
   ...  
      <NamingTemplate>...</NamingTemplate>  
   ...  
</DimensionAttribute>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne|  
|Valeur par défaut|Aucune|  
|Cardinalité|0-1: élément facultatif qui peut apparaître une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Élément parent|[DimensionAttribute](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 La valeur de la **NamingTemplate** élément est utilisé uniquement par les attributs parents (en d’autres termes, la valeur de la [utilisation](../../../analysis-services/scripting/properties/usage-element-dimensionattribute-assl.md) élément de la **DimensionAttribute** élément parent a la valeur *Parent*).  
  
 Lorsque vous utilisez un attribut parent pour créer une hiérarchie, les niveaux de la hiérarchie sont déterminés par les relations parent-enfant entre les membres de l'attribut parent. Ainsi, contrairement à d'autres dimensions, les noms de niveaux ne peuvent pas provenir des noms des attributs utilisés pour la hiérarchie.  
  
 Un modèle de nom est utilisé à la place pour générer les noms des niveaux pour les hiérarchies de type parent-enfant. L'élément **NamingTemplate** , défini dans l'attribut parent, contient une expression de chaîne utilisée pour définir les noms des niveaux. Deux moyens permettent de définir un modèle de nom pour un attribut parent. Vous pouvez soit concevoir un modèle d'affectation de nom, soit définir une liste de noms.  
  
 Un modèle d'affectation de nom utilise un astérisque (`*`) en tant que caractère d'espace réservé pour un compteur qui est incrémenté et inséré dans le nom de chaque nouveau niveau ou niveau d'intégration plus important (par exemple, à l'aide de résultats `Level *` dans les noms de niveaux `Level 01`, `Level 02`, `Level 03`, et ainsi de suite, si aucun niveau (Tous) n'est défini). Si un modèle d'affectation de nom ne contient pas de caractère d'espace réservé, il est utilisé d'abord tel quel. Les noms de niveaux qui suivent sont formés par ajout d'un espace et d'un nombre à la fin du modèle (par exemple, en utilisant des résultats `Level` dans les noms de niveaux `Level`, `Level 01`, `Level 02`et ainsi de suite).  
  
 Pour utiliser un ensemble de noms spécifique pour l'affectation des noms, la valeur de l'élément **NamingTemplate** est définie sur une liste de noms de niveaux délimitée par des points-virgules. Chaque nom dans la liste est utilisé pour un nom de niveau suivant. Si le nombre de niveaux dépasse le nombre de noms dans la liste, le dernier nom de la liste est utilisé en guise de modèle pour tous les noms de niveaux supplémentaires ; un espace et un nombre ordinal sont ajoutés au dernier nom comme décrit précédemment. Par exemple, en utilisant des résultats `Division;Group;Unit` dans les noms de niveaux `Division`, `Group`, `Unit`, `Unit 01`, `Unit 02`et ainsi de suite. Ou, par contraste, à l'aide de résultats `Division;Group;Unit *` dans les noms de niveaux `Division`, `Group`, `Unit 03`, `Unit 04`et ainsi de suite.  
  
 Chaque nom dans la liste est traité comme un modèle pour garantir l'unicité des noms de niveaux (par exemple, en utilisant des résultats `Manager;Team Lead;Manager;Team Lead;Worker *` dans les noms de niveaux `Manager`, `Team Lead`, `Manager 01`, `Team Lead 01`, `Worker 05`, `Worker 06`).  
  
 Utilisez deux astérisques (*) pour inclure l’astérisque (\*) dans un nom de niveau dans le cadre d’un modèle d’affectation de noms.  
  
 L’élément qui correspond au parent de **NamingTemplate** dans l’objet d’objets AMO (Analysis Management) est modèle <xref:Microsoft.AnalysisServices.DimensionAttribute>.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément NamingTemplateTranslations &#40;ASSL&#41;](../../../analysis-services/scripting/collections/namingtemplatetranslations-element-assl.md)   
 [Type de données DimensionAttribute &#40;ASSL&#41;](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md)   
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
