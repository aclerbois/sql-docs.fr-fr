---
title: ChildCount, propriété (ADO MD) | Documents Microsoft
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
- ChildCount
- Member::ChildCount
helpviewer_keywords:
- ChildCount property [ADO MD]
ms.assetid: 5463be22-ca50-43ea-9c92-468fc8eda280
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: da861dbc7648edbe93926e8f4df7b560fc4a415f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="childcount-property-ado-md"></a>ChildCount, propriété (ADO MD)
Indique le nombre de membres pour lesquels actuel [membre](../../../ado/reference/ado-md-api/member-object-ado-md.md) objet est le parent dans une hiérarchie.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne un **Long** entier et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **ChildCount** propriété pour retourner une estimation du nombre d’enfants un **membre** a. Les enfants réelles d’un **membre** peuvent être retournées par la [enfants](../../../ado/reference/ado-md-api/children-property-ado-md.md) propriété.  
  
 Pour **membre** des objets d’un [Position](../../../ado/reference/ado-md-api/position-object-ado-md.md) de l’objet, le nombre maximum renvoyé est 65 536. Si le nombre réel d’enfants dépasse 65 536, la valeur retournée sera toujours 65536. Par conséquent, l’application doit interpréter un **ChildCount** de 65 536 comme égale ou supérieure à 65 536 enfants.  
  
 Pour **membre** des objets d’un [niveau](../../../ado/reference/ado-md-api/level-object-ado-md.md) d’objet, utilisez la collection ADO [nombre](../../../ado/reference/ado-api/count-property-ado.md) propriété sur le **enfants** collection afin de déterminer le nombre exact d’enfants. Peut être lente si le nombre d’enfants dans la collection est important de déterminer le nombre exact d’enfants.  
  
## <a name="applies-to"></a>S'applique à  
 [Member, objet (ADO MD)](../../../ado/reference/ado-md-api/member-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Enfants, propriété (ADO MD)](../../../ado/reference/ado-md-api/children-property-ado-md.md)   
 [Count, propriété (ADO)](../../../ado/reference/ado-api/count-property-ado.md)   
 [Members, collection (ADO MD)](../../../ado/reference/ado-md-api/members-collection-ado-md.md)
