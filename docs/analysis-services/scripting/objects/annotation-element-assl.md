---
title: Élément d’annotation (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 303ed41b27538d5e21541e2912e892d91fd0f3d0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="annotation-element-assl"></a>Élément Annotation (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient des éléments utilisés pour étendre le schéma ASSL (Analysis Services Scripting Language).  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Annotations>  
   <Annotation>  
      <Name>...</Name>  
      <Visibility>...</Visibility>  
      <Value>...</Value>  
   </Annotation>  
</Annotations >  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Aucune|  
|Valeur par défaut|Aucune|  
|Cardinalité|0-n : élément facultatif pouvant apparaître plusieurs fois.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Annotations](../../../analysis-services/scripting/collections/annotations-element-assl.md)|  
|Éléments enfants|[Nom](../../../analysis-services/scripting/properties/name-element-assl.md), [valeur](../../../analysis-services/scripting/properties/value-element-assl.md), [visibilité](../../../analysis-services/scripting/properties/visibility-element-assl.md)|  
  
## <a name="remarks"></a>Notes  
 Le **Annotation** élément permet d’étendre le schéma ASSL pour tous les objets autres que ceux utilisés uniquement pour la définition d’un type de données complexes. Le **valeur** élément de la **Annotation** élément peut contenir un XML valide à partir de n’importe quel espace de noms XML autre qu’ASSL et soumis aux règles suivantes :  
  
-   Les données XML peuvent contenir uniquement des éléments.  
  
-   Chaque élément doit avoir un nom unique. Il est recommandé que la valeur de la **nom** élément de la **Annotation** l’élément fait référence à l’espace de noms cible.  
  
 Ces règles sont imposées pour autoriser le contenu de la **Annotation** paires d’élément doit être exposée comme un ensemble de nom/valeur via d’autres interfaces, telles que les objets DSO (Decision Support).  
  
 Les commentaires et l’espace blanc dans le **Annotation** élément qui ne sont pas compris dans un élément enfant ne peuvent pas être conservé. De plus, tous les éléments doivent être accessibles en lecture-écriture ; les éléments en lecture seule seront ignorés.  
  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.Annotation>.  
  
## <a name="see-also"></a>Voir aussi  
 [Objets & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/objects/objects-assl.md)  
  
  
