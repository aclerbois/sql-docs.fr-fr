---
title: Item, propriété (ADO) | Documents Microsoft
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
- Parameters::GetItem
- Indexes::GetItem
- Parameters::Item
- Tables::Item
- Procedures::Item
- Users::GetItem
- Tables::GetItem
- Procedures::GetItem
- Users::get_Item
- Users::Item
- Views::GetItem
- Groups::Item
- Groups::get_Item
- Columns::Item
- Indexes::Item
- Fields15::GetItem
- Columns::GetItem
- Fields::Item
- Indexes::get_Item
- Columns::get_Item
- Fields15::Item
- Views::get_Item
- Groups::GetItem
- Errors::get_Item
- Fields15::get_Item
- Tables::get_Item
- Views::Item
- Errors::GetItem
- Parameters::get_Item
- Errors::Item
- Procedures::get_Item
helpviewer_keywords:
- Item property [ADO]
ms.assetid: e11484bb-c5c7-42d8-9bb8-21572125d727
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 94826364231c295986d762361d6b946cba7e0b8e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="item-property-ado"></a>Élément propriété (ADO)
Indique un membre spécifique d’une collection, par nom ou un nombre ordinal.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
Set object = collection.Item ( Index )  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne une référence d’objet.  
  
## <a name="parameters"></a>Paramètres  
 *Index*  
 A **Variant** expression qui correspond au nom ou au nombre ordinal d’un objet dans une collection.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **élément** propriété pour retourner un objet spécifique dans une collection. Si **élément** Impossible de trouver un objet dans la collection correspondant à la *Index* argument, une erreur se produit. En outre, certaines collections ne prennent en charge les objets nommés ; pour ces collections, vous devez utiliser des références d’un nombre ordinal.  
  
 Le **élément** propriété est la propriété par défaut pour toutes les collections ; par conséquent, les formes de syntaxe suivantes sont interchangeables :  
  
```  
collection.Item (Index)  
collection (Index)  
```  
  
## <a name="applies-to"></a>S'applique à  
  
||||  
|-|-|-|  
|[Axes, collection (ADO MD)](../../../ado/reference/ado-md-api/axes-collection-ado-md.md)|[Columns, collection (ADOX)](../../../ado/reference/adox-api/columns-collection-adox.md)|[CubeDefs, collection (ADO MD)](../../../ado/reference/ado-md-api/cubedefs-collection-ado-md.md)|  
|[Dimensions, collection (ADO MD)](../../../ado/reference/ado-md-api/dimensions-collection-ado-md.md)|[Errors, collection (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)|[Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)|  
|[Groups, collection (ADOX)](../../../ado/reference/adox-api/groups-collection-adox.md)|[Hierarchies, collection (ADO MD)](../../../ado/reference/ado-md-api/hierarchies-collection-ado-md.md)|[Indexes, collection (ADOX)](../../../ado/reference/adox-api/indexes-collection-adox.md)|  
|[Keys, collection (ADOX)](../../../ado/reference/adox-api/keys-collection-adox.md)|[Levels, collection (ADO MD)](../../../ado/reference/ado-md-api/levels-collection-ado-md.md)|[Members, collection (ADO MD)](../../../ado/reference/ado-md-api/members-collection-ado-md.md)|  
|[Parameters, collection (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md)|[Positions, collection (ADO MD)](../../../ado/reference/ado-md-api/positions-collection-ado-md.md)|[Procedures, collection (ADOX)](../../../ado/reference/adox-api/procedures-collection-adox.md)|  
|[Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)|[Tables, collection (ADOX)](../../../ado/reference/adox-api/tables-collection-adox.md)|[Users, collection (ADOX)](../../../ado/reference/adox-api/users-collection-adox.md)|  
|[Views, collection (ADOX)](../../../ado/reference/adox-api/views-collection-adox.md)|||  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété d’élément (VB)](../../../ado/reference/ado-api/item-property-example-vb.md)   
 [Item, exemple de propriété (VC++)](../../../ado/reference/ado-api/item-property-example-vc.md)   
