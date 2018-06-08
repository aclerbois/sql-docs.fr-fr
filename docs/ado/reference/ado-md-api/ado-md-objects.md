---
title: Objets ADO MD | Documents Microsoft
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
helpviewer_keywords:
- ADO MD, objects
- objects [ADO MD]
ms.assetid: 2a32e873-3282-4520-a7ed-89493f1da80e
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3ae297d189b03af51c73a98e6f273601e805b25c
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "32807132"
---
# <a name="ado-md-objects"></a>Objets ADO MD
|||  
|-|-|  
|[Axis](../../../ado/reference/ado-md-api/axis-object-ado-md.md)|Représente une position ou l’axe de filtre d’un ensemble de cellules contenant des membres sélectionnés d’une ou plusieurs dimensions.|  
|[Catalogue](../../../ado/reference/ado-md-api/catalog-object-ado-md.md)|Contient des informations de schéma multidimensionnel (autrement dit, les cubes et sous-jacent dimensions, hiérarchies, niveaux et membres) spécifiques à un fournisseur de données multidimensionnelles (MDP).|  
|[Cellule](../../../ado/reference/ado-md-api/cell-object-ado-md.md)|Représente les données à l’intersection des coordonnées et contenues dans un ensemble de cellules.|  
|[Ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md)|Représente les résultats d’une requête multidimensionnelle. Il est une collection de cellules sélectionnées à partir de cubes ou d’autres ensembles de cellules.|  
|[CubeDef](../../../ado/reference/ado-md-api/cubedef-object-ado-md.md)|Représente un cube à partir d’un schéma multidimensionnel, contenant un ensemble de dimensions associées.|  
|[Dimension](../../../ado/reference/ado-md-api/dimension-object-ado-md.md)|Représente une des dimensions d’un cube multidimensionnel, contenant une ou plusieurs hiérarchies de membres.|  
|[Hierarchy](../../../ado/reference/ado-md-api/hierarchy-object-ado-md.md)|Représente un mode dans lequel les membres d’une dimension peuvent être agrégés ou « remontées ». Une dimension peut être agrégée avec une ou plusieurs hiérarchies.|  
|[Level](../../../ado/reference/ado-md-api/level-object-ado-md.md)|Contient un jeu de membres, chacun d'entre eux ayant le même rang dans une hiérarchie.|  
|[Membre](../../../ado/reference/ado-md-api/member-object-ado-md.md)|Représente un membre d’un niveau dans un cube, les enfants d’un membre d’un niveau ou un membre d’une position le long d’un axe d’un ensemble de cellules.|  
|[Position](../../../ado/reference/ado-md-api/position-object-ado-md.md)|Représente un ensemble d’un ou plusieurs membres de différentes dimensions qui définit un point sur un axe.|  
  
 En outre, le **catalogue** objet est connecté à ADO **connexion** objet, qui est inclus dans la bibliothèque ADO standard :  
  
|Object|Description|  
|------------|-----------------|  
|[Connexion](../../../ado/reference/ado-api/connection-object-ado.md)|Représente une connexion ouverte à une source de données.|  
  
 Les relations entre ces objets sont illustrées dans le [modèle objet ADO MD](../../../ado/reference/ado-md-api/ado-md-object-model.md).  
  
 Nombre d’objets ADO MD peut être contenu dans la collection correspondante. Par exemple, un [CubeDef](../../../ado/reference/ado-md-api/cubedef-object-ado-md.md) objet peut être contenu dans un [CubeDefs](../../../ado/reference/ado-md-api/cubedefs-collection-ado-md.md) collection d’un **catalogue**. Pour plus d’informations, consultez [Collections ADO MD](../../../ado/reference/ado-md-api/ado-md-collections.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des API ADO MD](../../../ado/reference/ado-md-api/ado-md-api-reference.md)   
 [Exemples de Code ADO MD](../../../ado/reference/ado-md-api/ado-md-code-examples.md)   
 [Collections ADO MD](../../../ado/reference/ado-md-api/ado-md-collections.md)   
 [Constantes énumérées ADO MD](../../../ado/reference/ado-md-api/ado-md-enumerated-constants.md)   
 [Méthodes ADO MD](../../../ado/reference/ado-md-api/ado-md-methods.md)   
 [Modèle objet ADO MD](../../../ado/reference/ado-md-api/ado-md-object-model.md)   
 [Propriétés ADO MD](../../../ado/reference/ado-md-api/ado-md-properties.md)
