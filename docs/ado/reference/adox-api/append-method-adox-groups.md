---
title: Append (méthode) (groupes ADOX) | Documents Microsoft
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
- Groups::raw_Append
- Groups::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 56b94fc6-7ef0-4e4a-82a3-033b94c46036
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2dc48bab9fa037ab4844bf3e1ab247365bf41070
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="append-method-adox-groups"></a>Append (méthode) (groupes ADOX)
Ajoute un nouveau [groupe](../../../ado/reference/adox-api/group-object-adox.md) de l’objet à la [groupes](../../../ado/reference/adox-api/groups-collection-adox.md) collection.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Groups.Append Group  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Grouper*  
 Le **groupe** objet à ajouter ou le nom du groupe à créer et à ajouter.  
  
## <a name="remarks"></a>Notes  
 Le **groupes** collection d’un [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md) représente tous les comptes de groupe du catalogue. Le **groupes** collection pour un [utilisateur](../../../ado/reference/adox-api/user-object-adox.md) représente uniquement le groupe auquel appartient l’utilisateur.  
  
 Une erreur se produit si le fournisseur ne prend pas en charge la création de groupes.  
  
> [!NOTE]
>  Avant d’ajouter un **groupe** de l’objet à la **groupes** collection d’un **utilisateur** objet, un **groupe** objet avec le même [nom](../../../ado/reference/adox-api/name-property-adox.md) que celui à ajouter doit déjà exister dans le **groupes** collection de la **catalogue**.  
  
## <a name="applies-to"></a>S'applique à  
 [Groups, collection (ADOX)](../../../ado/reference/adox-api/groups-collection-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisateurs et groupes Append, ChangePassword, méthodes-exemple (VB)](../../../ado/reference/adox-api/groups-and-users-append-changepassword-methods-example-vb.md)   
 [Append (méthode) (colonnes ADOX)](../../../ado/reference/adox-api/append-method-adox-columns.md)   
 [Append (méthode) (Index ADOX)](../../../ado/reference/adox-api/append-method-adox-indexes.md)   
 [Append (méthode) (clés ADOX)](../../../ado/reference/adox-api/append-method-adox-keys.md)   
 [Append (méthode) (procédures ADOX)](../../../ado/reference/adox-api/append-method-adox-procedures.md)   
 [Append (méthode) (Tables ADOX)](../../../ado/reference/adox-api/append-method-adox-tables.md)   
 [Append (méthode) (utilisateurs ADOX)](../../../ado/reference/adox-api/append-method-adox-users.md)   
 [Append, méthode (vues ADOX)](../../../ado/reference/adox-api/append-method-adox-views.md)
