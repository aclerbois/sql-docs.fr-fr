---
title: Table, objet (ADOX) | Documents Microsoft
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
- Table
helpviewer_keywords:
- Table object [ADOX]
ms.assetid: a6d74000-0828-49ba-850a-63da865f8802
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 33f2838b292817ca788fbd23d2da53752bdb4ce8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-object-adox"></a>Objet table (ADOX)
Représente une table de base de données, y compris les colonnes, les index et les clés.  
  
## <a name="remarks"></a>Notes  
 Le code suivant crée un nouveau **Table**:  
  
```  
Dim obj As New Table  
```  
  
 Avec les propriétés et les collections d’un **Table** de l’objet, vous pouvez :  
  
-   Identifier la table avec la [nom de propriété (ADOX)](../../../ado/reference/adox-api/name-property-adox.md) propriété.  
  
-   Déterminer le type de table avec la [Type, propriété (Table) (ADOX)](../../../ado/reference/adox-api/type-property-table-adox.md) propriété.  
  
-   Accéder aux colonnes de base de données de la table avec la [la Collection de colonnes (ADOX)](../../../ado/reference/adox-api/columns-collection-adox.md) collection.  
  
-   Accéder aux index de la table avec la [index Collection (ADOX)](../../../ado/reference/adox-api/indexes-collection-adox.md).  
  
-   Accéder aux clés de la table avec la [Collection de clés (ADOX)](../../../ado/reference/adox-api/keys-collection-adox.md).  
  
-   Spécifier le catalogue auquel appartient la table avec la [propriété ParentCatalog (ADOX)](../../../ado/reference/adox-api/parentcatalog-property-adox.md) propriété.  
  
-   Retourner des informations de date avec la [propriété DateCreated (ADOX)](../../../ado/reference/adox-api/datecreated-property-adox.md) et [propriété DateModified (ADOX)](../../../ado/reference/adox-api/datemodified-property-adox.md) propriétés.  
  
-   Accéder aux propriétés spécifiques au fournisseur table avec la [la Collection de propriétés (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md) collection.  
  
> [!NOTE]
>  Votre fournisseur de données peut prend pas en charge toutes les propriétés de **Table** objets. Une erreur se produit si vous avez défini une valeur pour une propriété qui ne prend pas en charge le fournisseur. Pour les nouveaux **Table** des objets, l’erreur se produit lorsque l’objet est ajouté à la collection. Pour les objets existants, l’erreur se produit lors de la définition de la propriété.  
>   
>  Lors de la création **Table** des objets, l’existence d’une valeur par défaut appropriée pour une propriété facultative ne garantit pas que votre fournisseur prend en charge la propriété. Pour plus d’informations sur les propriétés de votre fournisseur prend en charge, consultez la documentation du fournisseur.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet Table](../../../ado/reference/adox-api/table-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété ActiveConnection catalogue (VB)](../../../ado/reference/adox-api/catalog-activeconnection-property-example-vb.md)   
 [Colonnes et Tables ajouter des méthodes, exemple de propriété Name (VB)](../../../ado/reference/adox-api/columns-and-tables-append-methods-name-property-example-vb.md)   
 [Méthode de fermeture de connexion, exemple de propriété Table Type (VB)](../../../ado/reference/adox-api/connection-close-method-table-type-property-example-vb.md)   
 [Keys Append, méthode, Type de clé, RelatedColumn, RelatedTable et UpdateRule exemple (VB)](../../../ado/reference/adox-api/keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Exemple de propriété ParentCatalog (VB)](../../../ado/reference/adox-api/parentcatalog-property-example-vb.md)   
 [Collection de colonnes (ADOX)](../../../ado/reference/adox-api/columns-collection-adox.md)   
 [Collection d’index (ADOX)](../../../ado/reference/adox-api/indexes-collection-adox.md)   
 [Collection de clés (ADOX)](../../../ado/reference/adox-api/keys-collection-adox.md)   
 [Collection de propriétés (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [Tables, collection (ADOX)](../../../ado/reference/adox-api/tables-collection-adox.md)
