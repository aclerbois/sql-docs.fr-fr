---
title: Élément Row (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 84cf252303832ed157981103ffaede9718949e16
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576211"
---
# <a name="row-element-xmla"></a>Élément row (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Contient une ligne unique de données pour un [racine](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md) élément qui contient les données sous forme de tableau retournées par une [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) ou [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) appel de méthode.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<root xmlns="urn:schemas-microsoft-com:xml-analysis:rowset">  
   <row>  
      <!-- One or more column elements -->  
   </row>  
</root>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|0-n : élément facultatif pouvant apparaître plusieurs fois.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[racine](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md) (à l’aide de la [ensemble de lignes](../../../analysis-services/xmla/xml-data-types/rowset-data-type-xmla.md) type de données)|  
|Éléments enfants|Un ou plusieurs éléments de colonne.|  
  
## <a name="remarks"></a>Notes  
 Chaque ligne retournée par un élément **root** contenant des données sous forme de tableau a un élément **row** correspondant. Chaque colonne dans l'élément **root** est représentée par un élément XML distinct. La valeur de la colonne pour l'élément **row** correspond aux données figurant dans l'élément XML, et le nom de la colonne correspond au nom de l'élément XML.  
  
 Il existe deux moyens d'exprimer une valeur NULL pour une colonne au sein d'une ligne :  
  
-   Un élément de colonne manquant implique que la colonne est NULL.  
  
-   L'élément de colonne peut faire appel à l'attribut `xsi:nil='true'` pour indiquer qu'il possède une valeur NULL.  
  
 Par exemple, si une ligne comporte une seule colonne appelée Store_Name, dont la valeur est NULL, elle peut être représentée des deux façons suivantes :  
  
```  
<row>  
</row>  
```  
  
 - ou -  
  
```  
<row>  
   <Store_name xsi:nil='true'/>  
</row>  
```  
  
 Si un élément de colonne contient une erreur, un élément **Error** fournit des informations à propos de cette erreur, comme le décrit l'exemple suivant :  
  
```  
<row>   <Store_name>  
      <Error xmlns="urn:schemas-microsoft-com:xml-analysis:exception">  
         <ErrorCode>3238658054</ErrorCode>  
         <Description>The object [X] was not found in the cube when [X] was parsed.</Description>  
      </Error>  
   </Store_name>  
</row>  
```  
  
 Pour plus d’informations sur la colonne d’affectation de noms et des informations de schéma pour les données tabulaires, consultez [Type de données d’ensemble de lignes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-data-types/rowset-data-type-xmla.md).  
  
## <a name="see-also"></a>Voir aussi
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
