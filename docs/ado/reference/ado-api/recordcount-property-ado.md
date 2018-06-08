---
title: RecordCount, propriété (ADO) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Recordset15::RecordCount
- Recordset15::GetRecordCount
- Recordset15::get_RecordCount
helpviewer_keywords:
- RecordCount property [ADO]
ms.assetid: 834f0121-394a-44d4-ad7d-999b43a6fe63
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 67416b2282f913e04867b9d4ac23ee0ea0f0c41a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="recordcount-property-ado"></a>RecordCount, propriété (ADO)

Indique le nombre d’enregistrements dans une [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.
  
## <a name="return-value"></a>Valeur retournée

Retourne un **Long** valeur qui indique le nombre d’enregistrements dans la **Recordset**.
  
## <a name="remarks"></a>Notes

Utilisez le **RecordCount** propriété pour déterminer le nombre d’enregistrements se trouvent dans un **Recordset** objet. La propriété renvoie -1 quand ADO ne peut pas déterminer le nombre d’enregistrements ou si le type de curseur ou le fournisseur ne prend pas en charge **RecordCount**. La lecture de la **RecordCount** propriété sur un fermé **Recordset** provoque une erreur.

#### <a name="bookmarks-or-approximate-positioning"></a>Positionnement approximatif ou les signets

Si l’objet Recordset *est* prend en charge les deux signets ou approximative de positionnement, cette propriété retourne le nombre exact d’enregistrements dans le jeu d’enregistrements. Cette propriété retourne le nombre exact indépendamment de si le jeu d’enregistrements a été entièrement remplie.

En revanche, si l’objet Recordset ne *pas* prend en charge les signets ou le positionnement approximatif, l’accès à cette propriété peut être une charge importante pour les ressources. Le drainage se produit parce que tous les enregistrements doivent récupérées et comptées pour renvoyer une valeur RecordCount précise.

- **adBookmark** liées à des signets.
- **adApproxPosition** est lié au positionnement approximatif.

> [!NOTE]
> Dans les versions ADO 2.8 et les versions antérieures, le fournisseur SQLOLEDB extrait tous les enregistrements lorsqu’un curseur côté serveur est utilisé, en dépit du fait qu’elle retourne **True** pour les deux **prend en charge (adApproxPosition)** et **Prend en charge (adBookmark)**.
  
Le type de curseur de la **Recordset** objet selon le nombre d’enregistrements peut être déterminé. Le **RecordCount** propriété retourne -1 pour un curseur avant uniquement ; le nombre réel pour une analyse statique ou curseur de jeu de clés et soit -1 ou le nombre réel pour un curseur dynamique, en fonction de la source de données.
  
## <a name="applies-to"></a>S'applique à

[Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi

[Filter et RecordCount propriétés exemple (VB)](../../../ado/reference/ado-api/filter-and-recordcount-properties-example-vb.md)   
[Filter et RecordCount, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/filter-and-recordcount-properties-example-vc.md)   
[AbsolutePosition, propriété (ADO)](../../../ado/reference/ado-api/absoluteposition-property-ado.md)   
[PageCount, propriété (ADO)](../../../ado/reference/ado-api/pagecount-property-ado.md)
