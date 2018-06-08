---
title: AbsolutePosition, propriété (ADO) | Documents Microsoft
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
- Recordset15::AbsolutePosition
helpviewer_keywords:
- AbsolutePosition property [ADO]
ms.assetid: 79f8ee5e-fc70-46d8-8c29-ebf943c66592
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cc46a659ea191e6bb1437cb16b0e7704cf8e5ba0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="absoluteposition-property-ado"></a>AbsolutePosition, propriété (ADO)
Indique la position ordinale d’une [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) enregistrement en cours de l’objet.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Pour le code 32 bits, définit ou retourne un **Long** valeur entre 1 et le nombre d’enregistrements dans la **Recordset** objet ([RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md)), ou retourne l’une de la [ PositionEnum](../../../ado/reference/ado-api/positionenum.md) valeurs.  
  
 Pour le code 64 bits, utilisez un type de données qui fournit le stockage d’une valeur 64 bits. Par exemple, vous pouvez utiliser soit Long, soit une autre valeur longueur 64 bits comme DBORDINAL. N’utilisez pas **PositionEnum** valeurs, car ils sont limités à la longueur de 32 bits.  
  
## <a name="remarks"></a>Notes  
 Pour définir le **AbsolutePosition** propriété, ADO nécessite que le fournisseur OLE DB que vous utilisez implémenter le [IRowsetLocate:IRowset](https://msdn.microsoft.com/library/windows/desktop/ms721190.aspx) interface.  
  
 L’accès à la **AbsolutePosition** propriété d’un **Recordset** qui a été ouvert avec l’avant uniquement ou curseur dynamique génère l’erreur **adErrFeatureNotAvailable**. Avec d’autres types de curseur, la position correcte sera retournée tant que le fournisseur OLE DB prend en charge la **IRowsetScroll:IRowsetLocate** interface. Si le fournisseur ne prend pas en charge la **IRowsetScroll** interface, la propriété est définie **adPosUnknown**. Consultez la documentation de votre fournisseur pour déterminer si elle prend en charge **IRowsetScroll**.  
  
 Utilisez le **AbsolutePosition** propriété pour accéder à un enregistrement en fonction de sa position ordinale dans la **Recordset** objet, ou pour déterminer la position ordinale de l’enregistrement actif. Le fournisseur doit prendre en charge les fonctionnalités requises pour cette propriété doit être disponible.  
  
 Comme le [AbsolutePage](../../../ado/reference/ado-api/absolutepage-property-ado.md) propriété, **AbsolutePosition** basé sur 1 et est égale à 1 lorsque l’enregistrement actif est le premier enregistrement de la **Recordset**. Vous pouvez obtenir le nombre total d’enregistrements dans la **Recordset** de l’objet à partir de la [RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) propriété.  
  
 Lorsque vous définissez la **AbsolutePosition** propriété, même si elle est à un enregistrement dans le cache actif, ADO recharge le cache avec un nouveau groupe d’enregistrements en commençant par l’enregistrement que vous avez spécifié. Le [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) propriété détermine la taille de ce groupe.  
  
> [!NOTE]
>  Vous ne devez pas utiliser le **AbsolutePosition** propriété en tant que numéro d’enregistrement de substitution. La position d’un enregistrement donné change lorsque vous supprimez un enregistrement précédent. Il n’existe également aucune assurance qu’un enregistrement donné auront le même **AbsolutePosition** si le **Recordset** objet est actualisé ou rouvert. Les signets restent le meilleur moyen de conserver et revenir à une position donnée et constituent la seule manière de se positionner dans tous les types de **Recordset** objets.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [AbsolutePosition et CursorLocation, propriétés-exemple (VB)](../../../ado/reference/ado-api/absoluteposition-and-cursorlocation-properties-example-vb.md)   
 [AbsolutePosition et CursorLocation, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/absoluteposition-and-cursorlocation-properties-example-vc.md)   
 [AbsolutePage, propriété (ADO)](../../../ado/reference/ado-api/absolutepage-property-ado.md)   
 [RecordCount, propriété (ADO)](../../../ado/reference/ado-api/recordcount-property-ado.md)
