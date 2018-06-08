---
title: CubeDef, objet (ADO MD) | Documents Microsoft
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
- CubeDef
helpviewer_keywords:
- CubeDef object [ADO MD]
ms.assetid: feb2581c-fc41-471c-bb69-29f8a55fda70
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 31e2bf587c19ab8088b0ab702be60fde54247aec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cubedef-object-ado-md"></a>CubeDef, objet (ADO MD)
Représente un cube à partir d’un schéma multidimensionnel, contenant un ensemble de dimensions associées.  
  
## <a name="remarks"></a>Notes  
 Les collections et les propriétés d’un **CubeDef** de l’objet, vous pouvez procédez comme suit :  
  
-   Identifier un **CubeDef** avec la [nom](../../../ado/reference/ado-md-api/name-property-ado-md.md) propriété.  
  
-   Retourne une chaîne qui décrit le cube avec le [Description](../../../ado/reference/ado-md-api/description-property-ado-md.md) propriété.  
  
-   Retourner les dimensions qui composent le cube avec le [Dimensions](../../../ado/reference/ado-md-api/dimensions-collection-ado-md.md) collection.  
  
-   Obtenir des informations supplémentaires sur le **CubeDef** avec ADO standard [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection.  
  
 Le **propriétés** collection contient des propriétés fournies par le fournisseur. Le tableau suivant répertorie les propriétés qui peuvent être disponibles. La liste réelle des propriétés peut varier en fonction de l’implémentation du fournisseur. Consultez la documentation de votre fournisseur pour obtenir une liste plus complète des propriétés disponibles.  
  
|Nom| Description|  
|----------|-----------------|  
|CatalogName|Le nom du catalogue auquel appartient ce cube.|  
|Créé à|Date et heure de création du cube.|  
|CubeGUID|GUID du cube.|  
|CubeName|Nom du cube.|  
|CubeType|Type du cube.|  
|DataUpdatedBy|ID utilisateur de la personne qui effectue la dernière mise à jour de données.|  
| Description|Description explicite du cube.|  
|LastSchemaUpdate|Date et heure de dernière mise à jour de schéma.|  
|SchemaName|Le nom du schéma auquel appartient ce cube.|  
|SchemaUpdatedBy|ID utilisateur de la personne qui effectue la dernière mise à jour de schéma.|  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/cubedef-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CubeDef, exemple (VBScript)](../../../ado/reference/ado-md-api/cubedef-example-vbscript.md)   
 [Objet de catalogue (ADO MD)](../../../ado/reference/ado-md-api/catalog-object-ado-md.md)   
 [Collection de CubeDefs (ADO MD)](../../../ado/reference/ado-md-api/cubedefs-collection-ado-md.md)   
 [Collection de dimensions (ADO MD)](../../../ado/reference/ado-md-api/dimensions-collection-ado-md.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
