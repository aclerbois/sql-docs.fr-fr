---
title: Append (méthode) (utilisateurs ADOX) | Documents Microsoft
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
- Users::raw_Append
- Users::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: b80bc5d5-78ca-4f75-956b-2ac658029cc7
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bf65ab9c0705019dcc56bae4e605b5cb2247d056
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="append-method-adox-users"></a>Append (méthode) (utilisateurs ADOX)
Ajoute un nouveau [utilisateur](../../../ado/reference/adox-api/user-object-adox.md) de l’objet à la [utilisateurs](../../../ado/reference/adox-api/users-collection-adox.md) collection.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Users.Append User[,Password]  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Utilisateur*  
 A **Variant** valeur contenant le **utilisateur** objet à ajouter ou le nom de l’utilisateur à créer et à ajouter.  
  
 *Mot de passe*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient le mot de passe pour l’utilisateur. Le *mot de passe* paramètre correspond à la valeur spécifiée par la [ChangePassword](../../../ado/reference/adox-api/changepassword-method-adox.md) méthode d’un **utilisateur** objet.  
  
## <a name="remarks"></a>Notes  
 Le **utilisateurs** collection d’un [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md) représente tous les utilisateurs du catalogue. Le **utilisateurs** collection pour un [groupe](../../../ado/reference/adox-api/group-object-adox.md) représente uniquement les utilisateurs qui appartiennent à ce groupe.  
  
 Une erreur se produit si le fournisseur ne prend pas en charge la création d’utilisateurs.  
  
> [!NOTE]
>  Avant d’ajouter un **utilisateur** de l’objet à la **utilisateurs** collection d’un **groupe** objet, un **utilisateur** objet avec le même [nom](../../../ado/reference/adox-api/name-property-adox.md) que celui à ajouter doit déjà exister dans le **utilisateurs** collection de la **catalogue**.  
  
## <a name="applies-to"></a>S'applique à  
 [Users, collection (ADOX)](../../../ado/reference/adox-api/users-collection-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisateurs et groupes Append, ChangePassword, méthodes-exemple (VB)](../../../ado/reference/adox-api/groups-and-users-append-changepassword-methods-example-vb.md)   
 [Append (méthode) (colonnes ADOX)](../../../ado/reference/adox-api/append-method-adox-columns.md)   
 [Append (méthode) (groupes ADOX)](../../../ado/reference/adox-api/append-method-adox-groups.md)   
 [Append (méthode) (Index ADOX)](../../../ado/reference/adox-api/append-method-adox-indexes.md)   
 [Append (méthode) (clés ADOX)](../../../ado/reference/adox-api/append-method-adox-keys.md)   
 [Append (méthode) (procédures ADOX)](../../../ado/reference/adox-api/append-method-adox-procedures.md)   
 [Append (méthode) (Tables ADOX)](../../../ado/reference/adox-api/append-method-adox-tables.md)   
 [Append, méthode (vues ADOX)](../../../ado/reference/adox-api/append-method-adox-views.md)
