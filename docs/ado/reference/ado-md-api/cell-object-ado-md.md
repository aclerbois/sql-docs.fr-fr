---
title: Cell, objet (ADO MD) | Documents Microsoft
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
- Cell
helpviewer_keywords:
- Cell object [ADO MD]
ms.assetid: dcc2f044-b785-4a29-9bc5-b673f66eedf9
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 22227f22f42e5848b0826028a855b31b4b82443a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cell-object-ado-md"></a>Objet de cellule (ADO MD)
Représente les données à l’intersection des coordonnées des axes d’un ensemble de cellules.  
  
## <a name="remarks"></a>Notes  
 A **cellule** est retourné par la [élément](../../../ado/reference/ado-md-api/item-property-ado-md-cellset.md) propriété d’un [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md) objet.  
  
 Les collections et les propriétés d’un **cellule** de l’objet, vous pouvez procédez comme suit :  
  
-   Retourner les données dans le **cellule** avec la [valeur](../../../ado/reference/ado-md-api/value-property-ado-md.md) propriété.  
  
-   Renvoyer la chaîne représentant l’affichage mis en forme de la **valeur** propriété avec le [FormattedValue](../../../ado/reference/ado-md-api/formattedvalue-property-ado-md.md) propriété.  
  
-   Retourne la valeur ordinale de la **cellule** au sein de la **ensemble de cellules** avec la [ordinale](../../../ado/reference/ado-md-api/ordinal-property-ado-md-cell.md) propriété.  
  
-   Déterminer la position de la **cellule** au sein de la [CubeDef](../../../ado/reference/ado-md-api/cubedef-object-ado-md.md) avec la [Positions](../../../ado/reference/ado-md-api/positions-collection-ado-md.md) collection.  
  
-   Extraire d’autres informations sur le **cellule** avec ADO standard [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection.  
  
 Le **propriétés** collection contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste réelle des propriétés peut varier en fonction de l’implémentation du fournisseur. Consultez la documentation de votre fournisseur pour obtenir une liste plus complète des propriétés disponibles.  
  
|Nom| Description|  
|----------|-----------------|  
|CouleurFond|Couleur d’arrière-plan utilisée pour afficher la cellule.|  
|FontFlags|Masque de bits détaillant les effets de la police.|  
|FontName|Police utilisée pour afficher la valeur de cellule.|  
|FontSize|Taille de police utilisée pour afficher la valeur de cellule.|  
|CouleurTexte|Couleur de premier plan utilisée pour afficher la cellule.|  
|FormatString|La valeur dans une chaîne mise en forme.|  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/cell-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de l’axe (VBScript)](../../../ado/reference/ado-md-api/axis-example-vbscript.md)   
 [Cellset, objet (ADO MD)](../../../ado/reference/ado-md-api/cellset-object-ado-md.md)   
 [Collection de positions (ADO MD)](../../../ado/reference/ado-md-api/positions-collection-ado-md.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
