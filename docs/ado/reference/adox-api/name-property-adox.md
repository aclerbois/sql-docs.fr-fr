---
title: Name, propriété (ADOX) | Documents Microsoft
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
- _Table::PutName
- _Table::GetName
- _Key::Name
- _Key::get_Name
- _Column::GetName
- _Index::Name
- _Index::put_Name
- _Column::PutName
- _Key::put_Name
- _Table::put_Name
- _User25::PutName
- _Index::get_Name
- _Column::get_Name
- _Group25::Name
- _Group25::get_Name
- _Column::Name
- _User25::get_Name
- _Table::Name
- _Group25::GetName
- _Index::PutName
- _Column::put_Name
- _Key::GetName
- _Table::get_Name
- _User25::Name
- _User25::put_Name
- _Index::GetName
- _User25::GetName
helpviewer_keywords:
- Name property [ADOX]
ms.assetid: 81b92baf-b6b9-4f4e-9f33-4503795518cd
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0a2ed39fc723fe9d3796e2520c2385b8f4ae3896
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="name-property-adox"></a>Nom, propriété (ADOX)
Indique le nom de l’objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **chaîne** valeur.  
  
## <a name="remarks"></a>Notes  
 Noms n’ont pas à être uniques dans une collection.  
  
 Le **nom** propriété est en lecture/écriture sur [colonne](../../../ado/reference/adox-api/column-object-adox.md), [groupe](../../../ado/reference/adox-api/group-object-adox.md), [clé](../../../ado/reference/adox-api/key-object-adox.md), [Index](../../../ado/reference/adox-api/index-object-adox.md), [Table](../../../ado/reference/adox-api/table-object-adox.md), et [utilisateur](../../../ado/reference/adox-api/user-object-adox.md) objets. Le **nom** propriété est en lecture seule sur [catalogue](../../../ado/reference/adox-api/catalog-object-adox.md), [procédure](../../../ado/reference/adox-api/procedure-object-adox.md), et [vue](../../../ado/reference/adox-api/view-object-adox.md) objets.  
  
 Pour les objets en lecture/écriture (**colonne**, **groupe**, **clé**, **Index**, **Table** et **utilisateur** objets), la valeur par défaut est une chaîne vide (« »).  
  
> [!NOTE]
>  Pour les clés, cette propriété est en lecture seule sur **clé** objets déjà ajoutés à une collection. Pour les tables, cette propriété est en lecture seule pour **Table** objets déjà ajoutés à une collection.  
  
## <a name="applies-to"></a>S'applique à  
  
||||  
|-|-|-|  
|[Column, objet (ADOX)](../../../ado/reference/adox-api/column-object-adox.md)|[Group, objet (ADOX)](../../../ado/reference/adox-api/group-object-adox.md)|[Index, objet (ADOX)](../../../ado/reference/adox-api/index-object-adox.md)|  
|[Key, objet (ADOX)](../../../ado/reference/adox-api/key-object-adox.md)|[Procedure, objet (ADOX)](../../../ado/reference/adox-api/procedure-object-adox.md)|[Property, objet (ADO)](../../../ado/reference/ado-api/property-object-ado.md)|  
|[Table, objet (ADOX)](../../../ado/reference/adox-api/table-object-adox.md)|[User, objet (ADOX)](../../../ado/reference/adox-api/user-object-adox.md)|[View, objet (ADOX)](../../../ado/reference/adox-api/view-object-adox.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Colonnes et Tables ajouter des méthodes, exemple de propriété Name (VB)](../../../ado/reference/adox-api/columns-and-tables-append-methods-name-property-example-vb.md)   
 [Keys Append, méthode, Type de clé, RelatedColumn, RelatedTable et UpdateRule exemple (VB)](../../../ado/reference/adox-api/keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [ParentCatalog, exemple de propriété (VB)](../../../ado/reference/adox-api/parentcatalog-property-example-vb.md)
