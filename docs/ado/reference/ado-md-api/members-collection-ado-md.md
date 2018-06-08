---
title: Collection de membres (ADO MD) | Documents Microsoft
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
- Level::Members
- Members
- Position::Members
helpviewer_keywords:
- Members collection [ADO MD]
ms.assetid: 3a647cde-efdc-4394-b1b9-8cbb1b9d689f
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 18a3fac9cff0a41c9d1e7dc820d68ae77c294d4a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="members-collection-ado-md"></a>Collection de membres (ADO MD)
Contient le [membre](../../../ado/reference/ado-md-api/member-object-ado-md.md) objets à partir d’un niveau ou une position le long d’un axe.  
  
## <a name="remarks"></a>Notes  
 A **membres** collection est utilisée pour contenir les types de membres suivants :  
  
-   Les membres qui constituent un niveau dans un cube. Elles se trouvent dans le **membres** collection d’un [niveau](../../../ado/reference/ado-md-api/level-object-ado-md.md) objet. Par exemple, à l’aide de l’exemple à partir de [vue d’ensemble des schémas et données multidimensionnels](../../../ado/guide/multidimensional/overview-of-multidimensional-schemas-and-data.md), les quatre membres du niveau pays sont Canada, aux États-Unis, Royaume-Uni et en Allemagne.  
  
-   Les membres qui sont les enfants d’un membre spécifique au sein d’une hiérarchie. Ces membres sont renvoyés par le [enfants](../../../ado/reference/ado-md-api/children-property-ado-md.md) propriété du parent **membre** objet. Par exemple, en utilisant le même exemple, les deux enfants du membre Canada sont Canada-East et West-Canada.  
  
-   Les membres qui définissent une position spécifique sur un axe d’un [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md). À l’aide de l’ensemble de cellules à partir de [MD](../../../ado/guide/multidimensional/working-with-multidimensional-data.md) par exemple, les deux membres de la première position sur l’axe des x sont Valentine et Seattle. Ces membres sont contenus par le **membres** collection d’un [Position](../../../ado/reference/ado-md-api/position-object-ado-md.md) objet.  
  
 **Membres** est une collection ADO standard. Les propriétés et les méthodes d’une collection, vous pouvez effectuer les tâches suivantes :  
  
-   Obtenir le nombre d’objets dans la collection avec le [nombre](../../../ado/reference/ado-api/count-property-ado.md) propriété.  
  
-   Retourne un objet de la collection avec la valeur par défaut [élément](../../../ado/reference/ado-api/item-property-ado.md) propriété.  
  
-   Mettre à jour les objets dans la collection à partir du fournisseur avec le [Actualiser](../../../ado/reference/ado-api/refresh-method-ado.md) (méthode).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/members-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de l’exemple (VBScript)](../../../ado/reference/ado-md-api/members-example-vbscript.md)   
 [Member, objet (ADO MD)](../../../ado/reference/ado-md-api/member-object-ado-md.md)
