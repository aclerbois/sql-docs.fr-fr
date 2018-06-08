---
title: Objet de colonne (ADOX) | Documents Microsoft
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
- Column
helpviewer_keywords:
- Column object [ADOX]
ms.assetid: 6e772783-1bc8-4ea7-94b2-7d7a52ea5c47
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3d688a64630fbf22ad8efbe987e02d13bfdb07a5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-object-adox"></a>Objet de colonne (ADOX)
Représente une colonne d’une table, un index ou une clé.  
  
## <a name="remarks"></a>Notes  
 Le code suivant crée un nouveau **colonne**:  
  
 `Dim obj As New Column`  
  
 Avec les propriétés et les collections d’un **colonne** de l’objet, vous pouvez :  
  
-   Identifier la colonne avec le [nom de propriété (ADOX)](../../../ado/reference/adox-api/name-property-adox.md) propriété.  
  
-   Spécifiez le type de données de la colonne avec le [Type, propriété (clé) (ADOX)](../../../ado/reference/adox-api/type-property-key-adox.md) propriété.  
  
-   Déterminer si la colonne est de longueur fixe ou si elle peut contenir des valeurs null avec le [propriété des attributs (ADOX)](../../../ado/reference/adox-api/attributes-property-adox.md) propriété.  
  
-   Spécifiez la taille maximale de la colonne avec le [propriété DefinedSize (ADOX)](../../../ado/reference/adox-api/definedsize-property-adox.md) propriété.  
  
-   Pour les valeurs de données numériques, spécifiez l’échelle avec le [propriété NumericScale (ADOX)](../../../ado/reference/adox-api/numericscale-property-adox.md) propriété.  
  
-   Valeur des données numériques, spécifier la précision maximale avec les [propriété précision (ADOX)](../../../ado/reference/adox-api/precision-property-adox.md) propriété.  
  
-   Spécifiez le [objet catalogue (ADOX)](../../../ado/reference/adox-api/catalog-object-adox.md) qui possède la colonne avec le [propriété ParentCatalog (ADOX)](../../../ado/reference/adox-api/parentcatalog-property-adox.md) propriété.  
  
-   Pour les colonnes clés, spécifiez le nom de la colonne associée dans la table en relation avec la [propriété RelatedColumn (ADOX)](../../../ado/reference/adox-api/relatedcolumn-property-adox.md) propriété.  
  
-   Pour les colonnes d’index, spécifiez si l’ordre de tri est croissant ou décroissant avec la [propriété SortOrder (ADOX)](../../../ado/reference/adox-api/sortorder-property-adox.md) propriété.  
  
-   Accéder aux propriétés spécifiques au fournisseur avec le [la Collection de propriétés (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md) collection.  
  
> [!NOTE]
>  Pas toutes les propriétés de **colonne** objets peuvent être pris en charge par votre fournisseur de données. Une erreur se produit si vous avez défini une valeur pour une propriété qui ne prend pas en charge le fournisseur. Pour les nouveaux **colonne** d’objets, l’erreur se produit lorsque l’objet est ajouté à la collection. Pour les objets existants, l’erreur se produit lors de la définition de la propriété.  
>   
>  Lors de la création **colonne** d’objets, l’existence d’une valeur par défaut appropriée pour une propriété facultative ne garantit pas que votre fournisseur prend en charge la propriété. Pour plus d’informations sur les propriétés de votre fournisseur prend en charge, consultez la documentation du fournisseur.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet Column](../../../ado/reference/adox-api/column-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Colonnes et Tables ajouter des méthodes, exemple de propriété Name (VB)](../../../ado/reference/adox-api/columns-and-tables-append-methods-name-property-example-vb.md)   
 [Méthode de fermeture de connexion, exemple de propriété Table Type (VB)](../../../ado/reference/adox-api/connection-close-method-table-type-property-example-vb.md)   
 [Keys Append, méthode, Type de clé, RelatedColumn, RelatedTable et UpdateRule exemple (VB)](../../../ado/reference/adox-api/keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Exemple de Code ADOX : NumericScale et Precision, propriétés-exemple (VB)](../../../ado/reference/adox-api/adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [Exemple de propriété ParentCatalog (VB)](../../../ado/reference/adox-api/parentcatalog-property-example-vb.md)   
 [Exemple de propriété SortOrder (VB)](../../../ado/reference/adox-api/sortorder-property-example-vb.md)   
 [Collection de colonnes (ADOX)](../../../ado/reference/adox-api/columns-collection-adox.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
