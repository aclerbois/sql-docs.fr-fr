---
title: Group, objet (ADOX) | Documents Microsoft
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
- Group
helpviewer_keywords:
- group object [ADOX]
ms.assetid: 55ef0ade-68ea-4da5-8aa5-4cd27d1f6d1e
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 89733cc0f4533320e07d701f6645e64dac224fe3
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="group-object-adox"></a>Objet de groupe (ADOX)
Représente un compte de groupe qui dispose des autorisations d’accès au sein d’une base de données sécurisée.  
  
## <a name="remarks"></a>Notes  
 Le [groupes](../../../ado/reference/adox-api/groups-collection-adox.md) collection d’un [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md) représente tous les comptes du catalogue groupe. Le **groupes** collection pour un [utilisateur](../../../ado/reference/adox-api/user-object-adox.md) représente uniquement le groupe auquel appartient l’utilisateur.  
  
 Avec les propriétés, les collections et les méthodes d’un **groupe** de l’objet, vous pouvez :  
  
-   Identifier le groupe avec le [nom](../../../ado/reference/adox-api/name-property-adox.md) propriété.  
  
-   Déterminer si un groupe a lire, écrire ou supprimer des autorisations avec la [GetPermissions](../../../ado/reference/adox-api/getpermissions-method-adox.md) et [SetPermissions](../../../ado/reference/adox-api/setpermissions-method-adox.md) méthodes.  
  
-   Accéder aux comptes d’utilisateur ayant des appartenances du groupe avec la [utilisateurs](../../../ado/reference/adox-api/users-collection-adox.md) collection.  
  
-   Accéder aux propriétés spécifiques au fournisseur avec le [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet Group](../../../ado/reference/adox-api/group-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Collection de groupes (ADOX)](../../../ado/reference/adox-api/groups-collection-adox.md)   
 [Users, collection (ADOX)](../../../ado/reference/adox-api/users-collection-adox.md)
