---
title: Nom d’élément (ASSL) | Documents Microsoft
ms.date: 5/8/2018
ms.prod: sql
ms.custom: assl
ms.reviewer: owend
ms.technology: analysis-services
ms.topic: reference
author: minewiskan
ms.author: owend
manager: kfile
ms.openlocfilehash: aa5073bd0728035ff4940b66fd2593eaf13cfd44
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="name-element-assl"></a>Élément Name (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Contient le nom de l'élément parent.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Action> <!-- or one of the elements listed below in the Element Relationships table -->  
   ...  
   <Name>...</Name>  
   ...  
</Action>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Chaîne (jusqu'à 100 caractères)|  
|Valeur par défaut|Varie|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule fois|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Action](../../../analysis-services/scripting/objects/action-element-assl.md), [agrégation](../../../analysis-services/scripting/objects/aggregation-element-assl.md), [AggregationDesign](../../../analysis-services/scripting/objects/aggregationdesign-element-assl.md), [AlgorithmParameter](../../../analysis-services/scripting/objects/algorithmparameter-element-assl.md), [Annotation](../../../analysis-services/scripting/objects/annotation-element-assl.md), [ Assembly](../../../analysis-services/scripting/objects/assembly-element-assl.md), [ClrAssemblyFile](../../../analysis-services/scripting/data-type/clrassemblyfile-data-type-assl.md), [Cube](../../../analysis-services/scripting/objects/cube-element-assl.md), [CubeDimension](../../../analysis-services/scripting/data-type/cubedimension-data-type-assl.md), [CubeHierarchy](../../../analysis-services/scripting/data-type/cubehierarchy-data-type-assl.md), [Base de données](../../../analysis-services/scripting/objects/database-element-assl.md), [DataSource](../../../analysis-services/scripting/objects/datasource-element-assl.md), [DataSourceView](../../../analysis-services/scripting/objects/datasourceview-element-assl.md), [Dimension](../../../analysis-services/scripting/objects/dimension-element-assl.md), [DimensionAttribute](../../../analysis-services/scripting/data-type/dimensionattribute-data-type-assl.md), [Groupe](../../../analysis-services/scripting/objects/group-element-assl.md), [hiérarchie](../../../analysis-services/scripting/objects/hierarchy-element-assl.md), [Kpi](../../../analysis-services/scripting/objects/kpi-element-assl.md), [niveau](../../../analysis-services/scripting/objects/level-element-assl.md), [MdxScript](../../../analysis-services/scripting/objects/mdxscript-element-assl.md), [ Mesure](../../../analysis-services/scripting/objects/measure-element-assl.md), [MeasureGroup](../../../analysis-services/scripting/objects/measuregroup-element-assl.md), [MemberProperty](../../../analysis-services/scripting/objects/attributerelationship-element-assl.md), [MiningModel](../../../analysis-services/scripting/objects/miningmodel-element-assl.md), [MiningModelColumn](../../../analysis-services/scripting/data-type/miningmodelcolumn-data-type-assl.md), [ MiningStructure](../../../analysis-services/scripting/objects/miningstructure-element-assl.md), [MiningStructureColumn](../../../analysis-services/scripting/data-type/miningstructurecolumn-data-type-assl.md), [Partition](../../../analysis-services/scripting/objects/partition-element-assl.md), [autorisation](../../../analysis-services/scripting/data-type/permission-data-type-assl.md), [ Perspective](../../../analysis-services/scripting/objects/perspective-element-assl.md), [PerspectiveCalculation](../../../analysis-services/scripting/data-type/perspectivecalculation-data-type-assl.md), [ReportFormatParameter](../../../analysis-services/scripting/objects/reportformatparameter-element-asl.md), [ReportParameter](../../../analysis-services/scripting/objects/reportparameter-element-assl.md), [ Rôle](../../../analysis-services/scripting/objects/role-element-assl.md), [Server](../../../analysis-services/scripting/objects/server-element-assl.md), [ServerProperty](../../../analysis-services/scripting/objects/serverproperty-element-assl.md), [Trace](../../../analysis-services/scripting/objects/trace-element-assl.md)|  
|Éléments enfants|Aucune|  
  
## <a name="remarks"></a>Notes  
 Chaque élément est utilisé pour définir un objet (une instance de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)], une hiérarchie, un attribut et ainsi de suite) a un **nom** élément en tant que propriété. La valeur d’un **nom** élément présente les restrictions suivantes :  
  
-   La valeur ne peut pas contenir des espaces de début ni de fin. Si les espaces de début ou de fin sont incluses dans la valeur d’un **nom** élément, ces espaces seront supprimés implicitement par [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
-   La valeur ne doit pas contenir de caractères de contrôle. La présence de caractères de contrôle dans le nom est donc fortement déconseillée et peut parfois provoquer des erreurs de validation XML.  
  
     Pour les objets créés à l’aide de la **GetNewName** méthode dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], AMO vérifie et supprime ensuite tous les caractères de contrôle, des espaces de début ou de fin dans le nom. Pour cette raison, à l’aide de **GetNewName** est l’approche recommandée pour la définition des noms d’objets.  
  
     Toutefois, si vous définissez la **nom** propriété directement, de la même validation contrôles ne sont pas effectuées, ce qui peut provoquer des erreurs de validation XML. Une erreur se produit en fonction du caractère de contrôle présent dans le nom.  
  
     Bien que les caractères de contrôle ne doivent jamais être utilisés dans un nom d'objet, Analysis Services ne les empêche pas expressément. Les versions antérieures d'Analysis Services autorisaient parfois des caractères de contrôle dans un nom d'objet. Pour cette raison, SQL Server 2016 Analysis Services et ultérieurement ignore les caractères de contrôle dans un nom d’objet pour éviter de rompre des solutions plus anciennes.  
  
-   Les valeurs réservées suivantes ne peuvent pas être utilisées :  
  
    -   AUX  
  
    -   CLOCK$  
  
    -   COM1 à COM9 (COM1, COM2, COM3, et ainsi de suite)  
  
    -   CON  
  
    -   LPT1 à LPT9 (LPT1, LPT2, LPT3, et ainsi de suite)  
  
    -   NUL  
  
    -   PRN  
  
 Le tableau suivant répertorie les caractères supplémentaires qui ne peut pas être utilisés dans la valeur d’un **nom** élément, en fonction de l’élément parent.  
  
|Élément parent|Caractères non valides|  
|--------------------|------------------------|  
|[Server](../../../analysis-services/scripting/objects/server-element-assl.md)|Le nom doit se conformer aux règles énoncées pour les noms des ordinateurs équipés de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Les adresses IP ne sont pas valides.|  
|[DataSource](../../../analysis-services/scripting/objects/datasource-element-assl.md)|:/\\*&#124;?" [] de (){}<>|  
|[Niveau](../../../analysis-services/scripting/objects/level-element-assl.md), [élément d’attribut](../../../analysis-services/scripting/objects/attribute-element-assl.md)|.,;' `:/\\*&#124;?" & % $! [] de +={}<>|  
|Tous les autres éléments parents|.,;' `:/\\*&#124;?" & % $! %$!+=()[]5D;{}{}<>|  
  
## <a name="see-also"></a>Voir aussi  
 [ID d’élément &#40;ASSL&#41;](../../../analysis-services/scripting/properties/id-element-assl.md)   
 [Propriétés & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/properties/properties-assl.md)  
  
  
