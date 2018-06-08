---
title: Élément Axis (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 50d2694db85b1391602c6f8d9d9307da4e79b65b
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574421"
---
# <a name="axis-element-xmla"></a>Élément Axis (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Contient un jeu de tuples utilisé pour représenter un axe unique dans un jeu de données multidimensionnel contenue par un [Axes](../../../analysis-services/xmla/xml-elements-properties/axes-element-xmla.md) élément qui utilise le [MDDataSet](../../../analysis-services/xmla/xml-data-types/mddataset-data-type-xmla.md) type de données retourné par la [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) (méthode).  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Axes>  
   ...  
   <Axis> <!-- when AxisFormat XMLA property is set to ClusterFormat -->  
      <CrossProduct>...</CrossProduct>  
   </Axis>  
   <Axis> <!-- when AxisFormat XMLA property is set to TupleFormat or CustomFormat -->  
      <Tuples>...</Tuples>  
   </Axis>  
   ...  
</Axes>  
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
|Éléments parents|[Axes](../../../analysis-services/xmla/xml-elements-properties/axes-element-xmla.md)|  
|Éléments enfants|[CrossProduct](../../../analysis-services/xmla/xml-elements-properties/crossproduct-element-xmla.md) ou [Tuples](../../../analysis-services/xmla/xml-elements-properties/tuples-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 Le contenu de la **axe** élément varie selon la valeur de la **AxisFormat** propriété XMLA utilisée par le **Execute** (méthode).  
  
## <a name="tupleformat"></a>TupleFormat  
 Quand une application cliente définit la propriété **AxisFormat** avec la valeur *TupleFormat*, un axe est représenté comme un ensemble de tuples. Chaque **axe** élément contient un **Tuples** élément qui représente le jeu de tuples sur cet axe. Chaque tuple est représenté à l’aide d’un élément **Tuple** qui contient des éléments **Member** de chaque hiérarchie sur l’axe.  
  
## <a name="clusterformat"></a>ClusterFormat  
 Lorsqu’une application cliente définit le **AxisFormat** propriété *ClusterFormat*, les membres de chaque axe sont divisés en clusters dans lequel chaque cluster représente un produit croisé entre des ensembles ordonnés de membres de chaque hiérarchie. Chaque **axe** élément se compose d’un ou plusieurs **CrossProduct** éléments. Chaque **CrossProduct** élément contient un **membres** , élément pour chaque hiérarchie sur l’axe.  
  
## <a name="customformat"></a>CustomFormat  
 Lorsqu’une application cliente définit le **AxisFormat** propriété *CustomFormat*, la valeur est traitée identique à la *TupleFormat* valeur par une instance d’Analysis Services.  
  
## <a name="examples"></a>Exemples  
  
### <a name="description"></a>Description  
 L’exemple suivant illustre la structure de la **axe** éléments lorsqu’un client spécifie *TupleFormat* ou *CustomFormat* pour le **AxisFormat** propriété XMLA les membres suivants de l’axe :  
  
|||||  
|-|-|-|-|  
|Hiérarchie **Time**|1999|1999|2000|  
|Hiérarchie **Category**|Réel|Budget|Budget|  
  
### <a name="code"></a>Code  
  
```  
<Axes>  
   <Axis name="Axis0">  
      <Tuples>  
         <Tuple>  
            <Member Hierarchy="Time">  
               <UName>[Time].[1999]</UName>  
               ...  
            </Member>  
            <Member Hierarchy="Category">  
               <UName>[Scenario].[Actual]</UName>  
               ...  
            </Member>  
         </Tuple>  
         <Tuple>  
            <Member Hierarchy="Time">  
               <UName>[Time].[1999]</UName>  
               ...  
            </Member>  
            <Member Hierarchy="Category">  
               <UName>[Scenario].[Budget]</UName>  
               ...  
            </Member>  
         </Tuple>  
         <Tuple>  
            <Member Hierarchy="Time">  
               <UName>[Time].[2000]</UName>  
               ...  
            </Member>  
            <Member Hierarchy="Category">  
               <UName>[Scenario].[Budget]</UName>  
               ...  
            </Member>  
         </Tuple>  
      </Tuples>  
   </Axis>  
   ...  
</Axes>  
```  
  
### <a name="description"></a>Description  
 L’exemple suivant illustre la structure de la **axe** éléments lorsqu’un client spécifie *ClusterFormat* pour le **AxisFormat** propriété XMLA les membres suivants de l’axe :  
  
||||||  
|-|-|-|-|-|  
|Hiérarchie **Time**|1999|1999|2000|2001|  
|Hiérarchie **Category**|Réel|Budget|Budget|Budget|  
|Clusters|Cluster 1|Cluster 1|Cluster 1|Cluster 2|  
  
### <a name="code"></a>Code  
  
```  
<Axes>  
   <Axis name="Axis0">  
      <CrossProduct Size = "4">  
         <Members Hierarchy="Time">  
            <Member>  
               <UName>[Time].[1999]</UName>  
               ...  
            </Member>  
            <Member>  
               <UName>[Time].[2000]</UName>  
               ...  
            </Member>  
         </Members>  
         <Members Hierarchy="Category">  
            <Member>  
               <UName>[Scenario].[Actual]</UName>  
               ...  
            </Member>  
            <Member>  
               <UName>[Scenario].[Budget]</UName>  
               ...  
            </Member>  
         </Members>  
      </CrossProduct>  
      <CrossProduct Size = "1">  
         <Members Hierarchy="Time">  
            <Member>  
               <UName>[Time].[2001]</UName>  
               ...  
            </Member>  
         </Members>  
         <Members Hierarchy="Category">  
            <Member>  
               <UName>[Scenario].[Budget]</UName>  
               ...  
            </Member>  
         </Members>  
      </CrossProduct>  
   </Axis>  
   ...  
</Axes>  
```  
  
## <a name="see-also"></a>Voir aussi
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
