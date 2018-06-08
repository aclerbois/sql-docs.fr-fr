---
title: Attribut d’élément (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f32b81a122fe82e2874c763bf68154f03ea75e49
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574861"
---
# <a name="attribute-element-xmla"></a>Élément Attribute (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Définit ou filtre un membre dans un attribut sur lequel un parent [insérer](../../../analysis-services/xmla/xml-elements-commands/insert-element-xmla.md), [mise à jour](../../../analysis-services/xmla/xml-elements-commands/update-element-xmla.md), ou [supprimer](../../../analysis-services/xmla/xml-elements-commands/drop-element-xmla.md) commande effectue.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Attributes>  
   ...  
   <Attribute>  
      <AttributeName>...</AttributeName>  
      <Keys>...</Keys>  
      <!-- The following elements are included only for attributes contained in the Attributes element of a parent Insert or Update command -->  
      <Name>...</Name>  
      <Translations>...</Translations>  
      <CustomRollup>...</CustomRollup>  
      <CustomRollupProperties>...</CustomRollupProperties>  
      <UnaryOperator>...</UnaryOperator>  
      <SkippedLevels>...</SkippedLevels>  
   </Attribute>  
   ...  
</Attributes>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Attributs](../../../analysis-services/xmla/xml-elements-properties/attributes-element-xmla.md)|  
|Éléments enfants|Consultez le tableau ci-dessous.|  
  
|Ancêtre ou parent|Élément enfant|  
|------------------------|-------------------|  
|[DROP](../../../analysis-services/xmla/xml-elements-commands/drop-element-xmla.md), [où](../../../analysis-services/xmla/xml-elements-properties/where-element-xmla.md)|[AttributeName](../../../analysis-services/xmla/xml-elements-properties/attributename-element-xmla.md), [clés](../../../analysis-services/xmla/xml-elements-properties/keys-element-xmla.md)|  
|[Insérer](../../../analysis-services/xmla/xml-elements-commands/insert-element-xmla.md), [mise à jour](../../../analysis-services/xmla/xml-elements-commands/update-element-xmla.md)|[AttributeName](../../../analysis-services/xmla/xml-elements-properties/attributename-element-xmla.md), [CustomRollup](../../../analysis-services/xmla/xml-elements-properties/customrollup-element-xmla.md), [CustomRollupProperties](../../../analysis-services/xmla/xml-elements-properties/customrollupproperties-element-xmla.md), [clés](../../../analysis-services/xmla/xml-elements-properties/keys-element-xmla.md), [nom](../../../analysis-services/xmla/xml-elements-properties/name-element-xmla.md), [ SkippedLevels](../../../analysis-services/xmla/xml-elements-properties/skippedlevels-element-xmla.md), [traductions](../../../analysis-services/xmla/xml-elements-properties/translations-element-xmla.md), [UnaryOperator](../../../analysis-services/xmla/xml-elements-properties/unaryoperator-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 L'élément **Attribute** définit le membre d'attribut qui est inséré, mis à jour ou supprimé par la commande **Insert**, **Update**ou **Drop** , respectivement. Comme ces commandes ne peuvent fonctionner que sur les membres d’un attribut à la fois, le [attributs](../../../analysis-services/xmla/xml-elements-properties/attributes-element-xmla.md) collection de la **insérer**, **mise à jour**, et **supprimer** commandes peuvent contenir un seul **attribut** élément. Toutefois, la collection **Attributes** de l'élément **Where** pour les commandes **Drop** et **Update** peut contenir plusieurs éléments **Attribute** , afin que vous puissiez filtrer les attributs à supprimer ou mettre à jour dans une dimension accessible en écriture.  
  
## <a name="see-also"></a>Voir aussi
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)   
 [Dimensions activées en écriture](../../../analysis-services/multidimensional-models-olap-logical-dimension-objects/write-enabled-dimensions.md)  
  
  
