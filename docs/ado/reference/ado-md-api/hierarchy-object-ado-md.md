---
title: "Objet de hiérarchie (ADO MD) | Documents Microsoft"
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: ado
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: sql
ms.tgt_pltfrm: 
ms.topic: article
apitype: COM
f1_keywords:
- Hierarchy
helpviewer_keywords:
- Hierarchy object [ADO MD]
ms.assetid: 034af340-ac79-494e-ba5e-2b57da1cb9de
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 355560aecba3e18317aa91ed1a09dcc9ed344f5f
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="hierarchy-object-ado-md"></a>Objet de hiérarchie (ADO MD)
Représente un mode dans lequel les membres d’un [dimension](../../../ado/reference/ado-md-api/dimension-object-ado-md.md) peuvent être agrégées ou « remontées ». Une dimension peut être agrégée avec une ou plusieurs hiérarchies.  
  
## <a name="remarks"></a>Notes  
 Les collections et les propriétés d’un **hiérarchie** de l’objet, vous pouvez procédez comme suit :  
  
-   Identifier les **hiérarchie** avec la [nom](../../../ado/reference/ado-md-api/name-property-ado-md.md) et [UniqueName](../../../ado/reference/ado-md-api/uniquename-property-ado-md.md) propriétés.  
  
-   Retourne une chaîne explicite qui décrit le **hiérarchie** avec la [Description](../../../ado/reference/ado-md-api/description-property-ado-md.md) propriété.  
  
-   Retourner le [niveau](../../../ado/reference/ado-md-api/level-object-ado-md.md) objets qui composent le **hiérarchie** avec la [niveaux](../../../ado/reference/ado-md-api/levels-collection-ado-md.md) collection.  
  
-   Utiliser ADO standard [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection pour obtenir des informations supplémentaires sur le **hiérarchie** objet.  
  
 Le **propriétés** collection contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste réelle des propriétés peut varier en fonction de l’implémentation du fournisseur. Consultez la documentation de votre fournisseur pour obtenir une liste plus complète des propriétés disponibles.  
  
|Nom| Description|  
|----------|-----------------|  
|AllMember|Le membre au niveau plus élevé de la hiérarchie.|  
|CatalogName|Le nom du catalogue auquel appartient ce cube.|  
|CubeName|Nom du cube.|  
|DefaultMember|Le nom unique du membre par défaut pour cette hiérarchie.|  
| Description|Description explicite de la hiérarchie.|  
|DimensionType|Le type de dimension à laquelle appartient cette hiérarchie.|  
|DimensionUniqueName|Le nom non ambigu de la dimension.|  
|HierarchyCaption|Étiquette ou légende associée à la hiérarchie.|  
|HierarchyCardinality|Nombre de membres de la hiérarchie.|  
|HierarchyGUID|Le GUID de la hiérarchie.|  
|HierarchyName|Nom de la hiérarchie.|  
|HierarchyUniqueName|Le nom non ambigu de la hiérarchie.|  
|SchemaName|Le nom du schéma auquel appartient ce cube.|  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/hierarchy-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CubeDef, exemple (VBScript)](../../../ado/reference/ado-md-api/cubedef-example-vbscript.md)   
 [Objet de dimension (ADO MD)](../../../ado/reference/ado-md-api/dimension-object-ado-md.md)   
 [Collection de hiérarchies (ADO MD)](../../../ado/reference/ado-md-api/hierarchies-collection-ado-md.md)   
 [Collection de niveaux (ADO MD)](../../../ado/reference/ado-md-api/levels-collection-ado-md.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
