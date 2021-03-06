---
title: "Append (méthode) (vues ADOX) | Documents Microsoft"
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
- Views::raw_Append
- Views::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 6070fd58-3237-4c77-a966-5b39ce5d57e4
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: a5db1507f6574567cc6acd54e9ede053e36f1552
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="append-method-adox-views"></a>Append (méthode) (vues ADOX)
Crée un nouveau [vue](../../../ado/reference/adox-api/view-object-adox.md) de l’objet et l’ajoute à la [vues](../../../ado/reference/adox-api/views-collection-adox.md) collection.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Views.Append Name, Command  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Nom*  
 A **chaîne** valeur qui spécifie le nom de la vue à créer.  
  
 *Command*  
 ADO [commande](../../../ado/reference/ado-api/command-object-ado.md) objet qui représente la vue à créer.  
  
## <a name="remarks"></a>Notes  
 Crée une nouvelle vue de source de données avec le nom et les attributs spécifiés dans le **commande** objet.  
  
 Si le texte de commande par l’utilisateur représente une procédure plutôt qu’une vue, le comportement dépend du fournisseur. **Ajouter** échoue si le fournisseur ne prend pas en charge les commandes persistantes.  
  
> [!NOTE]
>  Lorsque vous utilisez le fournisseur OLE DB pour Microsoft Jet, le **vues** collection **Append** méthode vous permettra de spécifier un **procédure** plutôt qu’un **vue** dans les *commande* paramètre. Le **procédure** sera ajouté à la source de données et est ajoutée à la **vues** collection. Après le **Append**, si le **procédures** et **vues** collections sont actualisées, les **procédure** ne pourra plus être dans le **vues** collection et apparaîtra dans le **procédures** collection.  
  
## <a name="applies-to"></a>S'applique à  
 [Views, collection (ADOX)](../../../ado/reference/adox-api/views-collection-adox.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Views Append, méthode-exemple (VB)](../../../ado/reference/adox-api/views-append-method-example-vb.md)   
 [Append (méthode) (colonnes ADOX)](../../../ado/reference/adox-api/append-method-adox-columns.md)   
 [Append (méthode) (groupes ADOX)](../../../ado/reference/adox-api/append-method-adox-groups.md)   
 [Append (méthode) (Index ADOX)](../../../ado/reference/adox-api/append-method-adox-indexes.md)   
 [Append (méthode) (clés ADOX)](../../../ado/reference/adox-api/append-method-adox-keys.md)   
 [Append (méthode) (procédures ADOX)](../../../ado/reference/adox-api/append-method-adox-procedures.md)   
 [Append (méthode) (Tables ADOX)](../../../ado/reference/adox-api/append-method-adox-tables.md)   
 [Append, méthode (utilisateurs ADOX)](../../../ado/reference/adox-api/append-method-adox-users.md)
