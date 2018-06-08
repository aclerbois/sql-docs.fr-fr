---
title: Afficher l’objet (ADOX) | Documents Microsoft
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
- View
helpviewer_keywords:
- View object [ADOX]
ms.assetid: 653421ce-7b94-43d0-9bc6-4900f8f2af45
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a55d77b7bb7f79ee5871445d5169a4dfd2de5f5d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-object-adox"></a>Objet de vue (ADOX)
Représente un ensemble filtré d’enregistrements ou une table virtuelle. Lorsqu’il est utilisé conjointement avec ADO [commande](../../../ado/reference/ado-api/command-object-ado.md) objet, le **vue** objet peut être utilisé pour l’ajout, la suppression ou la modification des vues.  
  
## <a name="remarks"></a>Notes  
 Une vue est une table virtuelle, créée à partir d’autres tables de base de données ou des vues. Le **vue** objet vous permet de créer une vue sans avoir à connaître ou utiliser la syntaxe du fournisseur « CREATE VIEW ».  
  
 Avec les propriétés d’un **vue** de l’objet, vous pouvez :  
  
-   Identifier la vue avec le [nom](../../../ado/reference/adox-api/name-property-adox.md) propriété.  
  
-   Spécifiez le ADO **commande** objet qui peut être utilisé pour ajouter, supprimer ou modifier les vues avec la [commande](../../../ado/reference/adox-api/command-property-adox.md) propriété.  
  
-   Retourner des informations de date avec la [DateCreated](../../../ado/reference/adox-api/datecreated-property-adox.md) et [DateModified](../../../ado/reference/adox-api/datemodified-property-adox.md) propriétés.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet View](../../../ado/reference/adox-api/view-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vues et exemple de Collections de champs (VB)](../../../ado/reference/adox-api/views-and-fields-collections-example-vb.md)   
 [Views Append, méthode-exemple (VB)](../../../ado/reference/adox-api/views-append-method-example-vb.md)   
 [Collection de vues, exemple de propriété CommandText (VB)](../../../ado/reference/adox-api/views-collection-commandtext-property-example-vb.md)   
 [Exemple de méthode (VB) de suppression de vues](../../../ado/reference/adox-api/views-delete-method-example-vb.md)   
 [Views, collection (ADOX)](../../../ado/reference/adox-api/views-collection-adox.md)
