---
title: CursorOptionEnum | Documents Microsoft
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
- CursorOptionEnum
helpviewer_keywords:
- CursorOptionEnum enumeration [ADO]
ms.assetid: 4e10cda7-ce81-4466-94c2-844d38191cf1
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 51b30e8f2ccba9dd33979f0fb3cb9cb3b6d270c1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cursoroptionenum"></a>CursorOptionEnum
Spécifie que les fonctionnalités du [prend en charge](../../../ado/reference/ado-api/supports-method.md) méthode doit effectuer un test.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adAddNew**|0x1000400|Prend en charge la [AddNew](../../../ado/reference/ado-api/addnew-method-ado.md) méthode pour ajouter de nouveaux enregistrements.|  
|**adApproxPosition**|0x4000|Prend en charge la [AbsolutePosition](../../../ado/reference/ado-api/absoluteposition-property-ado.md) et [AbsolutePage](../../../ado/reference/ado-api/absolutepage-property-ado.md) propriétés.|  
|**adBookmark**|0x2000|Prend en charge la [signet](../../../ado/reference/ado-api/bookmark-property-ado.md) propriété pour accéder à des enregistrements spécifiques.|  
|**adDelete**|0x1000800|Prend en charge la [supprimer](../../../ado/reference/ado-api/delete-method-ado-recordset.md) méthode pour supprimer des enregistrements.|  
|**adFind**|0x80000|Prend en charge la [trouver](../../../ado/reference/ado-api/find-method-ado.md) méthode de localisation d’une ligne dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).|  
|**adHoldRecords**|0x100|Recherche d’autres enregistrements ou modifie la position suivante sans valider toutes les modifications en attente.|  
|**adIndex**|0x100000|Prend en charge la [Index](../../../ado/reference/ado-api/index-property.md) propriété pour nommer un index.|  
|**adMovePrevious**|0x200|Prend en charge la [MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md) et [MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md) méthodes, et [déplacer](../../../ado/reference/ado-api/move-method-ado.md) ou [GetRows](../../../ado/reference/ado-api/getrows-method-ado.md) méthodes de l’enregistrement actuel vers l’arrière de position sans nécessiter de signets.|  
|**adNotify**|0x40000|Indique que le fournisseur de données sous-jacent prend en charge les notifications (qui détermine si **Recordset** sont pris en charge).|  
|**adResync**|0x20000|Prend en charge la [Resync](../../../ado/reference/ado-api/resync-method.md) pour mettre à jour le curseur avec les données qui sont visibles dans la base de données sous-jacente.|  
|**adSeek**|0x200000|Prend en charge la [recherche](../../../ado/reference/ado-api/seek-method.md) méthode de localisation d’une ligne dans un **Recordset**.|  
|**adUpdate**|0x1008000|Prend en charge la [mise à jour](../../../ado/reference/ado-api/update-method.md) méthode permettant de modifier les données existantes.|  
|**adUpdateBatch**|0x10000|Prend en charge la mise à jour par lot ([UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) et [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) méthodes) pour transmettre des groupes de modifications au fournisseur.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.CursorOption.ADDNEW|  
|AdoEnums.CursorOption.APPROXPOSITION|  
|AdoEnums.CursorOption.BOOKMARK|  
|AdoEnums.CursorOption.DELETE|  
|AdoEnums.CursorOption.FIND|  
|AdoEnums.CursorOption.HOLDRECORDS|  
|AdoEnums.CursorOption.INDEX|  
|AdoEnums.CursorOption.MOVEPREVIOUS|  
|AdoEnums.CursorOption.NOTIFY|  
|AdoEnums.CursorOption.RESYNC|  
|AdoEnums.CursorOption.SEEK|  
|AdoEnums.CursorOption.UPDATE|  
|AdoEnums.CursorOption.UPDATEBATCH|  
  
## <a name="applies-to"></a>S'applique à  
 [Supports, méthode](../../../ado/reference/ado-api/supports-method.md)
