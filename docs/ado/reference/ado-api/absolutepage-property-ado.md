---
title: AbsolutePage, propriété (ADO) | Documents Microsoft
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
- Recordset15::AbsolutePage
helpviewer_keywords:
- AbsolutePage property [ADO]
ms.assetid: ddb58a35-ec3a-423c-a504-3c65e62c23d4
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: dd1b4230423661a51102293ae04ff1b773b92c47
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="absolutepage-property-ado"></a>AbsolutePage, propriété (ADO)
Indique la page sur laquelle réside l’enregistrement actif.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Pour le code 32 bits, définit ou retourne un **Long** valeur entre 1 et le nombre de pages dans le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet ([PageCount](../../../ado/reference/ado-api/pagecount-property-ado.md)), ou retourne l’une de la [PositionEnum ](../../../ado/reference/ado-api/positionenum.md) valeurs.  
  
 Pour le code 64 bits, utilisez un type de données qui fournit le stockage d’une valeur 64 bits. Par exemple, vous pouvez utiliser soit **Long** ou une autre valeur peut être de longueur de 64 bits comme DBORDINAL. N’utilisez pas **PositionEnum** valeurs, car ils sont limités à la longueur de 32 bits.  
  
## <a name="remarks"></a>Notes  
 Cette propriété peut être utilisée pour identifier le numéro de page sur laquelle se trouve l’enregistrement actif. Elle utilise le [PageSize](../../../ado/reference/ado-api/pagesize-property-ado.md) propriété pour séparer logiquement le nombre total de lignes de la **Recordset** objet en une série de pages, chacun d'entre eux ayant le nombre d’enregistrements égales à **PageSize** (à l’exception de la dernière page, qui peut comporter moins d’enregistrements). Le fournisseur doit prendre en charge les fonctionnalités requises pour cette propriété doit être disponible.  
  
-   Lors de l’obtention ou la définition de la **AbsolutePage** propriété, ADO utilise le [AbsolutePosition](../../../ado/reference/ado-api/absoluteposition-property-ado.md) propriété et la [PageSize](../../../ado/reference/ado-api/pagesize-property-ado.md) propriété ensemble comme suit :  
  
-   Pour obtenir le **AbsolutePage**, ADO récupère d’abord le **AbsolutePosition**et il divise par la **PageSize**.  
  
-   Pour définir le **AbsolutePage**, ADO déplace le **AbsolutePosition** comme suit : il multiplie le **PageSize** par la nouvelle **AbsolutePage** valeur, puis ajoute 1 à la valeur. Par conséquent, la position actuelle dans le **Recordset** une fois la valeur **AbsolutePage** est le premier enregistrement dans la page.  
  
 Comme le **AbsolutePosition** propriété, **AbsolutePage** basé sur 1 et est égale à 1 lorsque l’enregistrement actif est le premier enregistrement de la **Recordset**. Définissez cette propriété pour déplacer vers le premier enregistrement d’une page particulière. Obtenir le nombre total de pages à partir de la **PageCount** propriété.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AbsolutePage, PageCount et PageSize, propriétés-exemple (VB)](../../../ado/reference/ado-api/absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [AbsolutePage, PageCount et PageSize, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [AbsolutePosition, propriété (ADO)](../../../ado/reference/ado-api/absoluteposition-property-ado.md)   
 [PageCount, propriété (ADO)](../../../ado/reference/ado-api/pagecount-property-ado.md)   
 [PageSize, propriété (ADO)](../../../ado/reference/ado-api/pagesize-property-ado.md)
