---
title: SetPermissions, méthode (ADOX) | Documents Microsoft
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
- User25::SetPermissions
- User25::raw_SetPermissions
- _Group25::SetPermissions
- _Group25::raw_SetPermissions
helpviewer_keywords:
- SetPermissions method [ADOX]
ms.assetid: b7f925d7-b05c-4376-bb49-f8d2c17b8b24
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3178c472bfeb58361ae6d7d889d82d3823b09469
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setpermissions-method-adox"></a>SetPermissions, méthode (ADOX)
Spécifie les autorisations pour un [groupe](../../../ado/reference/adox-api/group-object-adox.md) ou [utilisateur](../../../ado/reference/adox-api/user-object-adox.md) sur un objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
GroupOrUser.SetPermissions Name, ObjectType, Action, Rights [, Inherit] [, ObjectTypeId]  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Nom*  
 A **chaîne** valeur qui spécifie le nom de l’objet pour lequel définir des autorisations.  
  
 *ObjectType*  
 A **Long** valeur qui peut être une de le [ObjectTypeEnum](../../../ado/reference/adox-api/objecttypeenum.md) constantes, qui spécifie le type de l’objet pour lequel obtenir les autorisations.  
  
 *Action*  
 A **Long** valeur qui peut être une de le [ActionEnum](../../../ado/reference/adox-api/actionenum.md) constantes qui spécifie le type d’action à effectuer lors de la définition des autorisations.  
  
 *Droits*  
 A **Long** valeur qui peut être un masque de bits d’un ou plusieurs de la [RightsEnum](../../../ado/reference/adox-api/rightsenum.md) constantes, qui indique les droits à définir.  
  
 *Hériter de*  
 Ce paramètre est facultatif. A **Long** valeur qui peut être une de le [InheritTypeEnum](../../../ado/reference/adox-api/inherittypeenum.md) constantes, qui spécifie la façon dont les objets héritent des autorisations. La valeur par défaut est **adInheritNone**.  
  
 *ObjectTypeId*  
 Ce paramètre est facultatif. A **Variant** valeur qui spécifie le GUID d’un type d’objet de fournisseur qui n’est pas défini par la spécification OLE DB. Ce paramètre est obligatoire si *ObjectType* a la valeur **adPermObjProviderSpecific**; sinon, il n’est pas utilisé.  
  
## <a name="remarks"></a>Notes  
 Une erreur se produit si le fournisseur ne prend pas en charge la définition des droits d’accès pour les groupes ou utilisateurs.  
  
> [!NOTE]
>  Lors de l’appel **SetPermissions**, au paramètre Actions **adAccessRevoke** remplace tous les paramètres de la *droits* paramètre. Ne définissez pas *Actions* à **adAccessRevoke** si vous souhaitez que les droits spécifiés dans le *droits* paramètre prenne effet.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Group, objet (ADOX)](../../../ado/reference/adox-api/group-object-adox.md)|[User, objet (ADOX)](../../../ado/reference/adox-api/user-object-adox.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [GetPermissions et SetPermissions, méthodes-exemple (VB)](../../../ado/reference/adox-api/getpermissions-and-setpermissions-methods-example-vb.md)   
 [GetPermissions, méthode (ADOX)](../../../ado/reference/adox-api/getpermissions-method-adox.md)   
 [Name, propriété (ADOX)](../../../ado/reference/adox-api/name-property-adox.md)
