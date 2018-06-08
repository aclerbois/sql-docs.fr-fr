---
title: Élément MiningStructure (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 537bece3f4222a2ba18bbadf87133876ecfa881d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="miningstructure-element-assl"></a>Élément MiningStructure (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit la structure d'un ensemble de modèles d'exploration de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<MiningStructures>  
   <MiningStructure>  
      <Name>...</Name>  
      <ID>...</ID>  
      <Description>...</<Description>  
      <Source>...</Source>  
      <CreatedTimestamp>...</<CreatedTimestamp>  
      <LastSchemaUpdate>...</LastSchemaUpdate>  
      <LastProcessed>...</LastProcessed>  
      <Translations>...</Translations>  
      <Language>...</Language>  
            <Collation>...</Collation>  
      <ErrorConfiguration>...</ErrorConfiguration>  
      <CacheMode>...</CacheMode>  
            <Columns>...</Columns>  
      <State>...</State>  
      <HoldoutActualSize>...</HoldoutActualSize>  
      <HoldoutMaxCases>...</HoldoutMaxCases>  
      <HoldoutMaxPercent>...</HoldoutMaxPercent>  
      <HoldoutSeed>...</HoldoutSeed>        
            <MiningStructurePermissions>...</<MiningStructurePermissions>  
            <MiningModels>...</MiningModels>  
            <Annotations>...</Annotations>  
   </MiningStructure>  
</MiningStructures>  
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
|Éléments parents|[MiningStructures](../../../analysis-services/scripting/collections/miningstructures-element-assl.md)|  
|Éléments enfants|[Annotations](../../../analysis-services/scripting/collections/annotations-element-assl.md), [CacheMode](../../../analysis-services/scripting/properties/cachemode-element-assl.md), [classement](../../../analysis-services/scripting/properties/collation-element-assl.md), [colonnes](../../../analysis-services/scripting/collections/columns-element-assl.md), [CreatedTimestamp](../../../analysis-services/scripting/properties/createdtimestamp-element-assl.md), [Description ](../../../analysis-services/scripting/properties/description-element-assl.md), [ErrorConfiguration](../../../analysis-services/scripting/objects/errorconfiguration-element-assl.md),<br /><br /> [HoldoutActualSize](../../../analysis-services/scripting/properties/holdoutactualsize-element.md),<br /><br /> [HoldoutMaxCases](../../../analysis-services/scripting/properties/holdoutmaxcases-element.md),<br /><br /> [HoldoutMaxPercent](../../../analysis-services/scripting/properties/holdoutmaxpercent-element.md),<br /><br /> [HoldoutSeed](../../../analysis-services/scripting/properties/holdoutseed-element.md),<br /><br /> [ID](../../../analysis-services/scripting/properties/id-element-assl.md), [langage](../../../analysis-services/scripting/properties/language-element-assl.md), [LastProcessed](../../../analysis-services/scripting/properties/lastprocessed-element-assl.md), [LastSchemaUpdate](../../../analysis-services/scripting/properties/lastschemaupdate-element-assl.md), [MiningModels](../../../analysis-services/scripting/collections/miningmodels-element-assl.md), [ MiningStructurePermissions](../../../analysis-services/scripting/collections/miningstructurepermissions-element-assl.md), [nom](../../../analysis-services/scripting/properties/name-element-assl.md), [Source](../../../analysis-services/scripting/properties/source-element-binding-assl.md), [état](../../../analysis-services/scripting/properties/state-element-assl.md), [traductions](../../../analysis-services/scripting/collections/translations-element-assl.md)|  
  
## <a name="remarks"></a>Notes  
 La structure d'exploration de données définit les colonnes et les liaisons. Après avoir défini une structure d'exploration de données, vous pouvez utiliser cette structure pour définir de nombreux modèles d'exploration de données. La structure d'exploration de données, et chaque modèle d'exploration de données qu'elle contient, peuvent être traités indépendamment.  
  
> [!NOTE]  
>  Les propriétés d’exclusion, **HoldoutMaxCases**, **HoldoutMaxPercent**, **HoldoutSeed**, et **HoldoutActualSize**, ont été introduits dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]. Elles vous permettent de définir une partition sur une structure d'exploration de données qui joue le rôle de jeu de test pour tous les modèles d'exploration de données associés à la structure. [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ne prend pas en charge ces propriétés. Par conséquent, si vous essayez d'utiliser ces propriétés sur une instance de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] retourne une erreur.  
  
## <a name="drillthrough-to-structure-columns"></a>Extraction dans des colonnes de structure  
 Dans [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], un nouvel élément d’autorisation a été ajouté à la [MiningStructurePermissions élément &#40;ASSL&#41; ](../../../analysis-services/scripting/collections/miningstructurepermissions-element-assl.md) collection. Si vous ajoutez **AllowDrillthrough** autorisation à la fois à la [MiningStructurePermissions](../../../analysis-services/scripting/collections/miningstructurepermissions-element-assl.md) et [MiningModelPermission](../../../analysis-services/scripting/objects/miningmodelpermission-element-assl.md) collections, l’extraction est activée à partir du modèle d’exploration de données à la structure, de sorte que les membres d’un rôle qui a **AllowDrillthrough** autorisations sur le modèle peuvent interroger le modèle d’exploration de données et retourner les colonnes de structure qui n’étaient pas incluses dans le modèle.  
  
 Par conséquent, pour protéger des données sensibles ou des informations personnelles, vous devez construire votre vue de source de données de manière à ce qu'elle masque les informations sensibles, et accorder l'autorisation **AllowDrillthrough** sur une structure d'exploration de données uniquement si c'est nécessaire. Pour plus d’informations, consultez [AllowDrillThrough élément &#40;ASSL&#41;](../../../analysis-services/scripting/properties/allowdrillthrough-element-assl.md).  
  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.MiningStructure>.  
  
## <a name="see-also"></a>Voir aussi  
 [Élément MiningModel &#40;ASSL&#41;](../../../analysis-services/scripting/objects/miningmodel-element-assl.md)   
 [Objets &#40;ASSL&#41;](../../../analysis-services/scripting/objects/objects-assl.md)   
 [SÉLECTIONNEZ &AMP;#40;DMX&AMP;#41;](../../../dmx/select-dmx.md)  
  
  
