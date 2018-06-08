---
title: Cellset, objet (ADO MD) | Documents Microsoft
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
- Cellset
helpviewer_keywords:
- Cellset object [ADO MD]
ms.assetid: 5e2452c0-cac0-49b2-8099-836c35794d50
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f32af1f621f3470dc0a9175708719965eef5fc71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cellset-object-ado-md"></a>Cellset, objet (ADO MD)
Représente les résultats d’une requête multidimensionnelle. Il est une collection de cellules sélectionnées à partir de cubes ou d’autres ensembles de cellules.  
  
## <a name="remarks"></a>Notes  
 Les données dans un **ensemble de cellules** est récupéré à l’aide d’un accès direct de type tableau. Vous pouvez explorer un membre spécifique pour obtenir des données sur ce membre. Par exemple, le code suivant retourne la légende du premier membre dans la première position sur le premier axe d’un ensemble de cellules nommé `cst`:  
  
```  
cst.Axes(0).Positions(0).Members(0).Caption  
```  
  
## <a name="remarks"></a>Notes  
 Il n’existe pas de notion d’une cellule en cours dans un ensemble de cellules. Au lieu de cela, le [élément](../../../ado/reference/ado-md-api/item-property-ado-md-cellset.md) propriété récupère un spécifique [cellule](../../../ado/reference/ado-md-api/cell-object-ado-md.md) objet à partir de l’ensemble de cellules. Les arguments de la **élément** propriété déterminent la cellule à extraire. Vous pouvez spécifier la valeur ordinale unique d’une cellule. Vous pouvez également récupérer des cellules à l’aide de leur numéro de position sur chaque axe de l’ensemble de cellules. Pour plus d’informations sur l’extraction des cellules, consultez le [élément](../../../ado/reference/ado-md-api/item-property-ado-md-cellset.md) propriété.  
  
 Avec les collections, les méthodes et les propriétés d’un **ensemble de cellules** de l’objet, vous pouvez procédez comme suit :  
  
-   Associer une connexion ouverte avec un **ensemble de cellules** objet en définissant ses [ActiveConnection](../../../ado/reference/ado-md-api/activeconnection-property-ado-md.md) propriété.  
  
-   Exécuter et récupérer les résultats d’une requête multidimensionnelle avec la [ouvrir](../../../ado/reference/ado-md-api/open-method-ado-md.md) (méthode).  
  
-   Récupérer un **cellule** à partir de la **ensemble de cellules** avec la [élément](../../../ado/reference/ado-md-api/item-property-ado-md-cellset.md) propriété.  
  
-   Retourner le [axe](../../../ado/reference/ado-md-api/axis-object-ado-md.md) objets qui définissent le **ensemble de cellules** avec la [Axes](../../../ado/reference/ado-md-api/axes-collection-ado-md.md) collection.  
  
-   Récupérer des informations sur les dimensions utilisées pour filtrer les données dans le **ensemble de cellules** avec la [FilterAxis](../../../ado/reference/ado-md-api/filteraxis-property-ado-md.md) propriété.  
  
-   Renvoyer ou spécifier la requête utilisée pour définir le **ensemble de cellules** avec la [Source](../../../ado/reference/ado-md-api/source-property-ado-md.md) propriété.  
  
-   Retourne l’état actuel de la **ensemble de cellules** (ouvertes, fermées, l’exécution ou connexion) avec le [état](../../../ado/reference/ado-md-api/state-property-ado-md.md) propriété.  
  
-   Fermez toute **ensemble de cellules** avec la [fermer](../../../ado/reference/ado-md-api/close-method-ado-md.md) (méthode).  
  
-   Récupérer des informations spécifiques au fournisseur sur le **ensemble de cellules** avec ADO standard [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-md-api/cellset-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple d’ensemble de cellules (VB)](../../../ado/reference/ado-md-api/cellset-example-vb.md)   
 [Collection d’axes (ADO MD)](../../../ado/reference/ado-md-api/axes-collection-ado-md.md)   
 [Objet de cellule (ADO MD)](../../../ado/reference/ado-md-api/cell-object-ado-md.md)   
 [Objet de connexion (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
