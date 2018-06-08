---
title: Level, objet (ADO MD) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Level
helpviewer_keywords:
- Level object [ADO MD]
ms.assetid: 37815869-ed30-45fd-9aea-0a986c1b305c
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d3f515c5bc8eaac8e674bcf82bd3a5e47eeefc0f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="level-object-ado-md"></a>Objet de niveau (ADO MD)
Contient un jeu de membres, chacun d'entre eux ayant le même rang dans une hiérarchie.  
  
## <a name="remarks"></a>Notes  
 Les collections et les propriétés d’un **niveau** de l’objet, vous pouvez procédez comme suit :  
  
-   Identifier les **niveau** avec la [nom](../../../ado/reference/ado-md-api/name-property-ado-md.md) et [UniqueName](../../../ado/reference/ado-md-api/uniquename-property-ado-md.md) propriétés.  
  
-   Renvoyer une chaîne à utiliser lors de l’affichage du **niveau** avec la [légende](../../../ado/reference/ado-md-api/caption-property-ado-md.md) propriété.  
  
-   Retourne une chaîne explicite qui décrit le **niveau** avec la [Description](../../../ado/reference/ado-md-api/description-property-ado-md.md) propriété.  
  
-   Retourner le [membre](../../../ado/reference/ado-md-api/member-object-ado-md.md) objets qui composent le **niveau** avec la [membres](../../../ado/reference/ado-md-api/members-collection-ado-md.md) collection.  
  
-   Retourner le nombre de niveaux à partir de la racine de la **niveau** avec la [profondeur](../../../ado/reference/ado-md-api/depth-property-ado-md.md) propriété.  
  
-   Utiliser ADO standard [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection pour obtenir des informations supplémentaires sur le **niveau** objet.  
  
 Le **propriétés** collection contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste réelle des propriétés peut varier en fonction de l’implémentation du fournisseur. Consultez la documentation de votre fournisseur pour obtenir une liste plus complète des propriétés disponibles.  
  
|Nom| Description|  
|----------|-----------------|  
|CatalogName|Le nom du catalogue auquel appartient ce cube.|  
|CubeName|Nom du cube.|  
| Description|Description explicite du niveau.|  
|DimensionUniqueName|Le nom non ambigu de la [dimension](../../../ado/reference/ado-md-api/dimension-object-ado-md.md).|  
|HierarchyUniqueName|Le nom non ambigu de la hiérarchie.|  
|LevelCaption|Étiquette ou légende associée au niveau.|  
|LevelCardinality|Nombre de membres du niveau.|  
|LevelGUID|Le GUID du niveau.|  
|LevelName|Nom du niveau.|  
|LevelNumber|La distance entre le niveau et la racine de la hiérarchie.|  
|LevelType|Le type de niveau.|  
|LevelUniqueName|Le nom non ambigu du niveau.|  
|SchemaName|Le nom du schéma auquel appartient ce cube.|  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/level-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CubeDef, exemple (VBScript)](../../../ado/reference/ado-md-api/cubedef-example-vbscript.md)   
 [Objet de hiérarchie (ADO MD)](../../../ado/reference/ado-md-api/hierarchy-object-ado-md.md)   
 [Collection de niveaux (ADO MD)](../../../ado/reference/ado-md-api/levels-collection-ado-md.md)   
 [Collection de membres (ADO MD)](../../../ado/reference/ado-md-api/members-collection-ado-md.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
