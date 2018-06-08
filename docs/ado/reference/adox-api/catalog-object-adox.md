---
title: Catalogue d’objet (ADOX) | Documents Microsoft
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
- Catalog
helpviewer_keywords:
- Catalog object [ADOX]
ms.assetid: bb651639-a488-4e38-b6de-0ed99fa4dd92
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b30a6725a58c96fc414f9ac4c15cc86fc9599329
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="catalog-object-adox"></a>Objet catalogue (ADOX)
Contient des collections ([Tables](../../../ado/reference/adox-api/tables-collection-adox.md), [vues](../../../ado/reference/adox-api/views-collection-adox.md), [utilisateurs](../../../ado/reference/adox-api/users-collection-adox.md), [groupes](../../../ado/reference/adox-api/groups-collection-adox.md), et [procédures](../../../ado/reference/adox-api/procedures-collection-adox.md)) qui décrivent le catalogue de schémas d’une source de données.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez modifier le **catalogue** objet en ajoutant ou supprimant des objets, ou en modifiant des objets existants. Certains fournisseurs peuvent tous les prend pas en charge la **catalogue** des objets ou peuvent prendre en charge uniquement l’affichage des informations de schéma.  
  
 Avec les propriétés et méthodes d’un **catalogue** de l’objet, vous pouvez :  
  
-   Ouvrir le catalogue en définissant le [ActiveConnection](../../../ado/reference/adox-api/activeconnection-property-adox.md) propriété ADO [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet ou une chaîne de connexion valide.  
  
-   Créer un nouveau catalogue avec le [créer](../../../ado/reference/adox-api/create-method-adox.md) (méthode).  
  
-   Identifier les propriétaires des objets dans une **catalogue** avec la [GetObjectOwner](../../../ado/reference/adox-api/getobjectowner-method-adox.md) et [SetObjectOwner](../../../ado/reference/adox-api/setobjectowner-method.md) méthodes.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet Catalog](../../../ado/reference/adox-api/catalog-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété ActiveConnection catalogue (VB)](../../../ado/reference/adox-api/catalog-activeconnection-property-example-vb.md)   
 [Commande et un exemple de propriétés CommandText (VB)](../../../ado/reference/adox-api/command-and-commandtext-properties-example-vb.md)   
 [Méthode de fermeture de connexion, exemple de propriété Table Type (VB)](../../../ado/reference/adox-api/connection-close-method-table-type-property-example-vb.md)   
 [Créer un exemple de méthode (VB)](../../../ado/reference/adox-api/create-method-example-vb.md)   
 [Keys Append, méthode, Type de clé, RelatedColumn, RelatedTable et UpdateRule exemple (VB)](../../../ado/reference/adox-api/keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Collection de paramètres, exemple de commande de propriété (VB)](../../../ado/reference/adox-api/parameters-collection-command-property-example-vb.md)   
 [Exemple de propriété ParentCatalog (VB)](../../../ado/reference/adox-api/parentcatalog-property-example-vb.md)   
 [Procédures ajouter l’exemple de méthode (VB)](../../../ado/reference/adox-api/procedures-append-method-example-vb.md)   
 [Exemple de méthode (VB) de suppression de procédures](../../../ado/reference/adox-api/procedures-delete-method-example-vb.md)   
 [Actualiser les procédures, méthode-exemple (VB)](../../../ado/reference/adox-api/procedures-refresh-method-example-vb.md)   
 [Vues et exemple de Collections de champs (VB)](../../../ado/reference/adox-api/views-and-fields-collections-example-vb.md)   
 [Views Append, méthode-exemple (VB)](../../../ado/reference/adox-api/views-append-method-example-vb.md)   
 [Collection de vues, exemple de propriété CommandText (VB)](../../../ado/reference/adox-api/views-collection-commandtext-property-example-vb.md)   
 [Exemple de méthode (VB) de suppression de vues](../../../ado/reference/adox-api/views-delete-method-example-vb.md)   
 [Actualiser les vues, méthode-exemple (VB)](../../../ado/reference/adox-api/views-refresh-method-example-vb.md)   
 [Collection de groupes (ADOX)](../../../ado/reference/adox-api/groups-collection-adox.md)   
 [Collection de procédures (ADOX)](../../../ado/reference/adox-api/procedures-collection-adox.md)   
 [Collection de tables (ADOX)](../../../ado/reference/adox-api/tables-collection-adox.md)   
 [Collection d’utilisateurs (ADOX)](../../../ado/reference/adox-api/users-collection-adox.md)   
 [Views, collection (ADOX)](../../../ado/reference/adox-api/views-collection-adox.md)
