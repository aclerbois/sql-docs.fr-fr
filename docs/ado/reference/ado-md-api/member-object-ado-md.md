---
title: Objet de membre (ADO MD) | Documents Microsoft
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
- Member
helpviewer_keywords:
- Member object [ADO MD], members
ms.assetid: 3dedf755-0741-4c3f-8b4e-bff8ff8809c8
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6629f860fb8043387526019ec17c0e1775915a66
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="member-object-ado-md"></a>Objet de membre (ADO MD)
Représente un membre d’un niveau dans un cube, les enfants d’un membre d’un niveau ou un membre d’une position le long d’un axe d’un ensemble de cellules.  
  
## <a name="remarks"></a>Notes  
 Les propriétés d’un **membre** diffèrent selon le contexte dans lequel elle est utilisée. A **membre** d’un [au niveau](../../../ado/reference/ado-md-api/level-object-ado-md.md) dans un [CubeDef](../../../ado/reference/ado-md-api/cubedef-object-ado-md.md) a un [enfants](../../../ado/reference/ado-md-api/children-property-ado-md.md) propriété qui retourne le **membres** du niveau inférieur suivant dans la hiérarchie à partir du **membre**. Pour un **membre** d’un [Position](../../../ado/reference/ado-md-api/position-object-ado-md.md), le **enfants** collection est toujours vide. En outre, le [Type](../../../ado/reference/ado-md-api/type-property-ado-md.md) propriété s’applique uniquement aux **membres** d’un **au niveau**.  
  
 A **membre** de **Position** a deux propriétés qui sont utiles lors de l’affichage du [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md): [DrilledDown](../../../ado/reference/ado-md-api/drilleddown-property-ado-md.md) et [ParentSameAsPrev](../../../ado/reference/ado-md-api/parentsameasprev-property-ado-md.md). Une erreur se produit si ces propriétés sont accessibles sur un **membre** d’un **niveau**.  
  
 Les collections et les propriétés d’un **membre** objet d’un **niveau**, vous pouvez procédez comme suit :  
  
-   Identifier les **membre** avec la [nom](../../../ado/reference/ado-md-api/name-property-ado-md.md) et [UniqueName](../../../ado/reference/ado-md-api/uniquename-property-ado-md.md) propriétés.  
  
-   Retourne une chaîne à utiliser lors de l’affichage du **membre** avec la [légende](../../../ado/reference/ado-md-api/caption-property-ado-md.md) propriété.  
  
-   Retourne une chaîne explicite qui décrit une mesure ou une formule **membre** avec la [Description](../../../ado/reference/ado-md-api/description-property-ado-md.md) propriété.  
  
-   Déterminer la nature de la **membre** avec la [Type](../../../ado/reference/ado-md-api/type-property-ado-md.md) propriété.  
  
-   Obtenir des informations sur la **niveau** de la **membre** avec la [LevelDepth](../../../ado/reference/ado-md-api/leveldepth-property-ado-md.md) et [LevelName](../../../ado/reference/ado-md-api/levelname-property-ado-md.md) propriétés.  
  
-   Obtenir connexes **membres** dans un [hiérarchie](../../../ado/reference/ado-md-api/hierarchy-object-ado-md.md) avec la [Parent](../../../ado/reference/ado-md-api/parent-property-ado-md.md) et [enfants](../../../ado/reference/ado-md-api/children-property-ado-md.md) propriétés.  
  
-   Nombre d’enfants d’un **membre** avec la [ChildCount](../../../ado/reference/ado-md-api/childcount-property-ado-md.md) propriété.  
  
-   Utiliser ADO standard [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection pour obtenir des informations supplémentaires sur le **niveau** objet.  
  
 Avec les collections et les propriétés d’un **membre** d’un **Position** le long d’un [axe](../../../ado/reference/ado-md-api/axis-object-ado-md.md), vous pouvez procédez comme suit :  
  
-   Identifier les **membre** avec la [nom](../../../ado/reference/ado-md-api/name-property-ado-md.md) et [UniqueName](../../../ado/reference/ado-md-api/uniquename-property-ado-md.md) propriétés.  
  
-   Retourne une chaîne à utiliser lors de l’affichage du **membre** avec la [légende](../../../ado/reference/ado-md-api/caption-property-ado-md.md) propriété.  
  
-   Retourne une chaîne explicite qui décrit une mesure ou une formule **membre** avec la [Description](../../../ado/reference/ado-md-api/description-property-ado-md.md) propriété.  
  
-   Obtenir des informations sur la **niveau** de la **membre** avec la [LevelDepth](../../../ado/reference/ado-md-api/leveldepth-property-ado-md.md) et [LevelName](../../../ado/reference/ado-md-api/levelname-property-ado-md.md) propriétés.  
  
-   Nombre d’enfants d’un **membre** avec la [ChildCount](../../../ado/reference/ado-md-api/childcount-property-ado-md.md) propriété.  
  
-   Utilisez le [DrilledDown](../../../ado/reference/ado-md-api/drilleddown-property-ado-md.md) propriété pour déterminer s’il existe au moins un enfant sur le **axe** immédiatement **membre**.  
  
-   Utilisez le [ParentSameAsPrev](../../../ado/reference/ado-md-api/parentsameasprev-property-ado-md.md) propriété pour déterminer si le parent de ce **membre** est le même que le parent de qui précède immédiatement **membre**.  
  
-   Utiliser ADO standard [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection pour obtenir des informations supplémentaires sur le **niveau** objet.  
  
 Le **propriétés** collection contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste réelle des propriétés peut varier en fonction de l’implémentation du fournisseur. Consultez la documentation de votre fournisseur pour obtenir une liste plus complète des propriétés disponibles.  
  
|Nom| Description|  
|----------|-----------------|  
|CatalogName|Le nom du catalogue auquel appartient ce cube.|  
|ChildrenCardinality|Nombre d'enfants de ce membre.|  
|CubeName|Nom du cube.|  
| Description|Description explicite du membre.|  
|DimensionUniqueName|Le nom non ambigu de la [dimension](../../../ado/reference/ado-md-api/dimension-object-ado-md.md).|  
|HierarchyUniqueName|Le nom non ambigu de la hiérarchie.|  
|LevelNumber|La distance entre le niveau et la racine de la hiérarchie.|  
|LevelUniqueName|Le nom non ambigu du niveau.|  
|MemberCaption|Étiquette ou légende associée au membre.|  
|MemberGUID|GUID du membre.|  
|MemberName|Nom du membre.|  
|MemberOrdinal|Le nombre ordinal du membre.|  
|MemberType|Type du membre.|  
|Nom unique de membre|Nom du membre et non équivoque.|  
|ParentCount|Décompte du nombre de parents de ce membre.|  
|ParentLevel|Le numéro de niveau du parent du membre.|  
|ParentUniqueName|Le nom non ambigu du parent du membre.|  
|SchemaName|Le nom du schéma auquel appartient ce cube.|  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/member-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de catalogue (VB)](../../../ado/reference/ado-md-api/catalog-example-vb.md)   
 [Collection de membres (ADO MD)](../../../ado/reference/ado-md-api/members-collection-ado-md.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
