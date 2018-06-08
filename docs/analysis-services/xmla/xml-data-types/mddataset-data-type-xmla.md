---
title: Type de données MDDataSet (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 986fade6d9db3d6170d47181ac960d15febdf260
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34573921"
---
# <a name="mddataset-data-type-xmla"></a>Type de données MDDataSet (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Définit un type de données dérivé qui représente les données multidimensionnelles retournées par la [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) (méthode).  
  
 **Namespace** urn : schemas-microsoft-com-analysis : mddataset  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<root xmlns="urn:schemas-microsoft-com:xml-analysis:rowset">  
   <!-- The following elements extend Resultset -->  
   <!-- Optional schema elements -->  
   <OlapInfo>...</OlapInfo>  
   <Axes>...</Axes>  
   <CellData>...</CellData>  
</root>  
```  
  
## <a name="data-type-characteristics"></a>Caractéristiques du type de données  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Types de données de base|[Jeu de résultats](../../../analysis-services/xmla/xml-data-types/resultset-data-type-xmla.md)|  
|Types de données dérivés|None|  
  
## <a name="data-type-relationships"></a>Relations des types de données  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|None|  
|Éléments enfants|[Axes](../../../analysis-services/xmla/xml-elements-properties/axes-element-xmla.md), [CellData](../../../analysis-services/xmla/xml-elements-properties/celldata-element-xmla.md), [OlapInfo](../../../analysis-services/xmla/xml-elements-properties/olapinfo-element-xmla.md)|  
|Éléments dérivés|None|  
  
## <a name="remarks"></a>Notes  
 Le **MDDataSet** type de données fournit les orienté OLAP ensemble de lignes (ou jeu de données) nécessaires pour représenter les données OLAP en XML. Le contenu de cet ensemble de lignes peut varier en fonction des valeurs de la **contenu** et **Format** propriétés fournies dans le [propriétés](../../../analysis-services/xmla/xml-elements-properties/properties-element-xmla.md) collection de la **Execute** (méthode). Pour plus d’informations sur la **contenu** et **Format** propriétés, consultez [pris en charge les propriétés XMLA &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md).  
  
 Pour informations de base à propos des structures de dataset OLE DB pour OLAP, consultez la section « Type de données MDDataSet mappage sur OLE DB » de la spécification XML for Analysis 1.1. Pour obtenir un exemple de langage (XSD XML) définition de schéma XML complète de la **MDDataSet** de type de données, consultez « Appendix D: MDDataSet Example » de la spécification XML for Analysis 1.1.  
  
## <a name="see-also"></a>Voir aussi
 [Types de données XML &#40;XMLA&#41;](../../../analysis-services/xmla/xml-data-types/xml-data-types-xmla.md)  
  
  
