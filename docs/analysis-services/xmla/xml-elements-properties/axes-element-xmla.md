---
title: Axes, élément (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3a8dfff1c8a551157661bcb1de5700bf51a7f914
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574911"
---
# <a name="axes-element-xmla"></a>Élément Axes (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Contient une collection de [axe](../../../analysis-services/xmla/xml-elements-properties/axis-element-xmla.md) éléments représentant des données de l’axe contenues par un [racine](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md) élément qui utilise le [MDDataSet](../../../analysis-services/xmla/xml-data-types/mddataset-data-type-xmla.md) type de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<root xmlns="urn:schemas-microsoft-com:xml-analysis:mddataset">  
   ...  
   <Axes>  
      <Axis>...</Axis>  
   </Axes>  
   ...  
</root>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|Tout|  
|Valeur par défaut|None|  
|Cardinalité|1-1 : élément requis qui apparaît une fois et une seule.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[racine](../../../analysis-services/xmla/xml-elements-properties/root-element-xmla.md)|  
|Éléments enfants|[Axis](../../../analysis-services/xmla/xml-elements-properties/axis-element-xmla.md)|  
  
## <a name="remarks"></a>Notes  
 Sous le **Axes** élément, le **axe** éléments sont répertoriés dans l’ordre où ils apparaissent dans le jeu de données, en commençant à zéro. Le **AxisFormat** définition de la propriété XMLA détermine comment **axe** mise en forme des éléments. Pour plus d’informations sur la **AxisFormat** propriété, consultez [pris en charge les propriétés XMLA &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties.md).  
  
 Un axe représente un jeu de tuples dans lequel tous les tuples ont la même dimensionnalité. Un jeu peut être représenté de plusieurs façons offrant différents avantages. Par exemple, le jeu de quatre tuples suivant peut être représenté comme une collection de tuples à deux dimensions ou un produit cartésien de deux ensembles unidimensionnels.  
  
|1999|1999|2000|2000|  
|----------|----------|----------|----------|  
|Réel|Budget|Réel|Budget|  
  
 Ce jeu de tuples peut être représenté comme une collection de tuples à deux dimensions :  
  
```  
{ ( 1999, Actual ), ( 1999, Budget ), ( 2000, Actual ), ( 2000, Budget ) }  
```  
  
 Ce jeu peut également être représenté comme un produit cartésien de deux ensembles unidimensionnels :  
  
```  
{ 1999, 2000 } x { Actual, Budget }  
```  
  
 La première représentation, les tuples à deux dimensions, est plus simple à utiliser pour les outils clients. La seconde représentation, un produit cartésien d'ensembles unidimensionnels, utilise moins d'espace et conserve la nature multidimensionnelle du jeu.  
  
 Le tableau suivant répertorie les opérations qui peuvent être utilisées pour définir et caractériser la structure et les membres d'un axe.  
  
|Opération|Description|  
|---------------|-----------------|  
|Membre|La plus petite unité d'un axe représentant le membre d'une hiérarchie de dimension.|  
|Membres|Une collection de **membre** les objets de la même hiérarchie de dimension.|  
|Tuple|Une collection de membres de hiérarchies de dimension différentes.|  
|Tuples|Une collection de **Tuple** objets avec la même dimensionnalité.|  
|Union|Une union d'ensembles.|  
|CrossJoin|Un produit cartésien d'ensembles.|  
  
 Ces opérations traduisent les tuples à deux dimensions et le produit cartésien d'ensembles unidimensionnels de la façon suivante.  
  
## <a name="two-dimensional-tuples"></a>Tuples à deux dimensions  
  
```  
Tuples (  
   Tuple( Member(1999), Member(Actual) ),  
   Tuple( Member(1999), Member(Budget) ),  
   Tuple( Member(2000), Member(Actual) ),  
   Tuple( Member(2000), Member(Budget) )  
```  
  
## <a name="cartesian-product-of-one-dimensional-sets"></a>Produit cartésien d'ensembles unidimensionnels  
  
```  
CrossProduct (  
   Members( Member(1999), Member(2000) ),  
   Members( Member(Actual), Member(Budget) )  
```  
  
 Un client peut utiliser le **AxisFormat** propriété pour demander une représentation spécifique.  
  
## <a name="see-also"></a>Voir aussi
 [Type de données MDDataSet &#40;XMLA&#41;](../../../analysis-services/xmla/xml-data-types/mddataset-data-type-xmla.md)   
 [Propriétés &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-properties/xml-elements-properties.md)  
  
  
