---
title: Type de données MeasureGroupDimension (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e449579bc1544f0cc26f181dc67dbec5f9e00f2f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="measuregroupdimension-data-type-assl"></a>Type de données MeasureGroupDimension (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit un type de données primitif abstrait représentant la relation entre une dimension et un groupe de mesures.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<MeasureGroupDimension>  
   <CubeDimensionID>...</CubeDimensionID>  
      <Annotations>...</Annotations>  
   <Source>...</Source>  
</MeasureGroupDimension>  
```  
  
## <a name="data-type-characteristics"></a>Caractéristiques du type de données  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Types de données de base|Aucune|  
|Types de données dérivés|[DataMiningMeasureGroupDimension](../../../analysis-services/scripting/data-type/dataminingmeasuregroupdimension-data-type-assl.md), [DegenerateMeasureGroupDimension](../../../analysis-services/scripting/data-type/degeneratemeasuregroupdimension-data-type-assl.md), [ManyToManyMeasureGroupDimension](../../../analysis-services/scripting/data-type/manytomanymeasuregroupdimension-data-type-assl.md), [ReferenceMeasureGroupDimension](../../../analysis-services/scripting/data-type/referencemeasuregroupdimension-data-type-assl.md), [RegularMeasureGroupDimension](../../../analysis-services/scripting/data-type/regularmeasuregroupdimension-data-type-assl.md)|  
  
## <a name="data-type-relationships"></a>Relations du type de données  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|Aucune|  
|Éléments enfants|[Annotations](../../../analysis-services/scripting/collections/annotations-element-assl.md), [CubeDimensionID](../../../analysis-services/scripting/properties/cubedimensionid-element-assl.md), [Source](../../../analysis-services/scripting/properties/source-element-binding-assl.md)|  
|Éléments dérivés|[Dimension](../../../analysis-services/scripting/objects/dimension-element-assl.md) (collection[Dimensions](../../../analysis-services/scripting/collections/dimensions-element-assl.md) de [MeasureGroup](../../../analysis-services/scripting/objects/measuregroup-element-assl.md))|  
  
## <a name="remarks"></a>Notes  
 Chaque type de données **MeasureGroupDimension** est une référence à l'une des dimensions dans le cube. Ces références définissent quelles dimensions du cube s'appliquent au groupe de mesures.  
  
 L'ensemble des attributs fournis détermine la granularité (étendue) avec laquelle les mesures dans le groupe de mesures sont connues. Par exemple, les mesures qui représentent des ventes de produits apparaissent dans le groupe de mesures Sales. Les informations concernant ces mesures sont stockées dans la source de données sous-jacente à une granularité mensuelle, plutôt qu'hebdomadaire ou quotidienne. Dans ce cas, seul l'attribut Month (Mois) serait répertorié pour le type de données **MeasureGroupDimension** décrivant la relation entre une dimension de temps et le groupe de mesures Sales. Dans de rares cas, la granularité peut être définie sous la forme d'un ensemble d'attributs. Par exemple, dans le cas de l'ensemble d'attributs {Day, Week, Month, Year} où Day (Jour) implique Week (Semaine) et Month (Mois) mais Week n'implique pas Month, les mesures contenues dans le groupe de mesures Forecasts (Prévisions) peuvent être connues par mois et par semaine, mais pas par jour.  
  
 Si aucun attribut n'est fourni, le résultat obtenu donnerait l'impression que seul l'attribut de clé de la dimension est répertorié (définition du niveau de granularité le plus bas). Chaque partition d'un groupe de mesures doit posséder la même granularité. L'ensemble des attributs répertoriés ne doit pas apparaître comme un élément redondant par rapport aux relations entre les attributs. Par exemple, si Month implique Year (Année), la granularité est définie sous la forme Month, et non Month et Year.  
  
 Un type de données **MeasureGroupDimension** doit inclure une hiérarchie uniquement s'il inclut des éléments spécifiques à présenter sur celle-ci. (Il n'existe aucun moyen de déterminer quelles hiérarchies s'appliquent à un groupe de mesures particulier). De la même façon, il doit inclure un type de données [MeasureGroupAttribute](../../../analysis-services/scripting/data-type/measuregroupattribute-data-type-assl.md) uniquement s'il dispose d'éléments spécifiques à signaler à ce sujet.  
  
 Chaque hiérarchie doit être un sous-ensemble des hiérarchies incluses dans l'élément [CubeDimension](../../../analysis-services/scripting/data-type/cubedimension-data-type-assl.md). Vous ne pouvez pas sélectionner les niveaux même si certains d'entre eux peuvent être automatiquement désactivés en fonction de la granularité du groupe de mesures.  
  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.MeasureGroupDimension>.  
  
## <a name="see-also"></a>Voir aussi  
 [Analysis Services script des Types de données XML Language & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/data-type/analysis-services-scripting-language-xml-data-types-assl.md)  
  
  
