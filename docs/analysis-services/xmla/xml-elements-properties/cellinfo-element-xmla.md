---
title: Élément CellInfo (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3ada531d33baf7007d08ac8a719fca2bf8f1b2e4
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574251"
---
# <a name="cellinfo-element-xmla"></a>Élément CellInfo (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Représente les métadonnées de cellule contenue par le parent [OlapInfo](../../../analysis-services/xmla/xml-elements-properties/olapinfo-element-xmla.md) élément.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<OlapInfo>  
   ...  
   <CellInfo>  
      <!-- One or more cell property definitions -->  
   </CellInfo>  
   ...  
</OlapInfo>  
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
|Éléments parents|[OlapInfo](../../../analysis-services/xmla/xml-elements-properties/olapinfo-element-xmla.md)|  
|Éléments enfants|Une ou plusieurs définitions de propriété de cellule|  
  
## <a name="remarks"></a>Notes  
 Le **CellInfo** élément contient une collection de propriétés de cellule pour les cellules incluses dans le dataset multidimensionnel retourné par une **racine** à l’aide de l’élément le **MDDataSet** type de données. Chaque propriété de cellule dans le **CellInfo** est définie par un élément XML distinct, chacun avec un **nom** attribut et un **type** attribut. Le **nom** attribut de la propriété de cellule correspond au nom d’OLE DB pour la propriété de cellule OLAP représentée par l’élément XML et le **type** attribut représente le type de données XML de la propriété de cellule. Le nom de l’élément XML est utilisé pour identifier la valeur de la propriété de cellule pour les cellules contenues dans le **CellData** élément de la **racine** élément.  
  
 La syntaxe suivante décrit une définition de propriété de cellule :  
  
```  
<CellPropertyDefinition name="string" type"string" />  
```  
  
 Les propriétés disponibles et leurs valeurs peuvent être obtenues via le type de demande DISCOVER_PROPERTIES avec la méthode **Discover** . Il n'existe aucun ordre requis pour les propriétés répertoriées dans l'élément **PropertyList** .  
  
 Un fournisseur peut éventuellement spécifier des valeurs par défaut pour les propriétés de membre ou une cellule individuelles dans le **AxisInfo** ou **CellInfo** section. Les valeurs par défaut peuvent fournir un résultat plus restreint si la propriété affiche toujours, ou presque, la même valeur. Pour indiquer une valeur par défaut pour une propriété, le**par défaut** élément peut éventuellement être spécifié comme un élément enfant de l’un des éléments de définition de propriété de cellule. Par conséquent, l'absence d'une propriété de membre ou de cellule dans le résultat signifie que la valeur indiquée par défaut correspond à la valeur de la propriété de cellule.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment les propriétés de cellule valeur, FORMATTED_VALUE et FORMAT_STRING sont représentées dans le **CellInfo** élément.  
  
```  
<OlapInfo>  
   ...  
      <CellInfo>  
         <Value name="VALUE"></Value>  
         <FmtValue name="FORMATTED_VALUE"></FmtValue>  
         <FormatString name="FORMAT_STRING"></FormatString>  
      </CellInfo>  
</OlapInfo>  
```  
  
## <a name="see-also"></a>Voir aussi
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
