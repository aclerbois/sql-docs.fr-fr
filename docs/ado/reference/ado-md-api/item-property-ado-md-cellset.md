---
title: Item, propriété (ensemble de cellules ADO MD) | Documents Microsoft
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
- Item
- Cellset::Item
helpviewer_keywords:
- Item property [ADO MD]
ms.assetid: 0e93d79b-b12e-4e98-889e-c2dfcca20fd0
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7f22f8195a082ffe1333efe46a270fb1e18057ad
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="item-property-ado-md-cellset"></a>Propriété de l’élément (ensemble de cellules ADO MD)
Récupère une cellule d’un [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md) à l’aide de ses coordonnées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set  
Cell = Cellset.Item ( Positions)  
```  
  
## <a name="parameters"></a>Paramètres  
 *Positions*  
 A **VariantArray** des valeurs qui spécifient une cellule de manière unique. *Positions* peut prendre l’une des opérations suivantes :  
  
-   Un tableau de nombres de position  
  
-   Un tableau des noms de membres  
  
-   La position ordinale  
  
## <a name="remarks"></a>Notes  
 Utilisez le **élément** propriété à retourner un [cellule](../../../ado/reference/ado-md-api/cell-object-ado-md.md) de l’objet dans un [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md) objet. Si le **élément** propriété ne peut pas trouver la cellule correspondant à la *Positions* argument, une erreur se produit.  
  
 Le **élément** propriété est la propriété par défaut pour le **ensemble de cellules** objet. Les formes de syntaxe suivantes sont interchangeables :  
  
```  
  
Cellset.Item ( Positions )Cellset ( Positions )  
```  
  
## <a name="remarks"></a>Notes  
 Le *Positions* argument spécifie la cellule à retourner. Vous pouvez spécifier la cellule par position ordinale ou par la position le long de chaque axe. Lorsque vous spécifiez la cellule par position le long de chaque axe, vous pouvez spécifier la valeur numérique de la position ou les noms des membres pour chaque position.  
  
 La position ordinale est un nombre qui identifie de façon unique une cellule dans le **ensemble de cellules**. Point de vue conceptuel, les cellules sont numérotées dans un **ensemble de cellules** comme si le **ensemble de cellules** ont été un *p*-tableau unidimensionnel, où *p* est le nombre d’axes. Les cellules sont traitées dans l'ordre ligne-champ. Voici la formule pour calculer le nombre ordinal d’une cellule :  
  
 Si les noms de membres sont transmis en tant que chaînes à **élément**, les membres doivent figurer dans l’ordre croissant des identificateurs de l’axe numérique. Sur un axe, les membres doivent être répertoriés en ordre croissant d’imbrication, autrement dit, les membres de la dimension extérieur en premier, suivis par les membres des dimensions. Chaque dimension doit être représentée par une chaîne séparée et la liste des chaînes de membres doit être séparée par des virgules.  
  
> [!NOTE]
>  L’extraction des cellules par nom de membre ne peut pas être pris en charge par votre fournisseur de données. Consultez la documentation de votre fournisseur pour plus d’informations.  
  
## <a name="applies-to"></a>S'applique à  
 [Cellset, objet (ADO MD)](../../../ado/reference/ado-md-api/cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Objet de cellule (ADO MD)](../../../ado/reference/ado-md-api/cell-object-ado-md.md)   
 [Cellset, objet (ADO MD)](../../../ado/reference/ado-md-api/cellset-object-ado-md.md)
