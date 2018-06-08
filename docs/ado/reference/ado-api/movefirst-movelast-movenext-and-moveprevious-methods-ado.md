---
title: MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO) | Documents Microsoft
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
- Recordset15::MoveLast
- Recordset15::MoveNext
- Recordset15::raw_MoveNext
- Recordset15::raw_MovePrevious
- Recordset15::MoveFirst
- Recordset15::raw_MoveLast
- Recordset15::MovePrevious
- Recordset15::raw_MoveFirst
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- MovePrevious method [ADO]
ms.assetid: a61a01a7-5b33-4150-9126-21dfa63654cb
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b127303e4b74a60e6ef839922bc911c31b360914
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-ado"></a>MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO)
Se déplace vers le premier, au dernier enregistrement suivant ou précédent dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet et en fait l’enregistre actif.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez le **MoveFirst** méthode pour déplacer la position actuelle vers le premier enregistrement dans le **Recordset**.  
  
 Utilisez le **MoveLast** méthode pour déplacer la position actuelle au dernier enregistrement dans le **Recordset**. Le **Recordset** objet doit prendre en charge les signets ou le déplacement du curseur arrière ; sinon, l’appel de méthode génère une erreur.  
  
 Un appel à **MoveFirst** ou **MoveLast** lors de la **Recordset** est vide (à la fois **BOF** et **EOF** ont la valeur True) génère une erreur.  
  
 Utilisez le **MoveNext** méthode pour déplacer l’enregistrement actif positionner un enregistrement vers l’avant (vers le bas de la **Recordset**). Si le dernier enregistrement est l’enregistrement actif et que vous appelez le **MoveNext** (méthode), ADO définit l’enregistrement actif à la position après le dernier enregistrement dans le **Recordset** ([EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) est **True**). Une tentative de déplacement vers l’avant lorsque la **EOF** propriété est déjà **True** génère une erreur.  
  
 Dans ADO 2.5 et versions ultérieur, quand le **Recordset** ont été filtrées ou triées et les données de l’enregistrement actif sont modifiées, l’appel le **MoveNext** méthode déplace les enregistrements de curseur deux avant de l’enregistrement actuel . Il s’agit, car lors de l’enregistrement actif est modifié, l’enregistrement suivant devient l’enregistrement en cours. Appel de **MoveNext** après la modification déplace le curseur d’un enregistrement à partir de l’enregistrement en cours. Cela est différent du comportement dans ADO 2.1 et versions antérieures. Dans les versions antérieures, la modification des données d’un enregistrement en cours dans triées ou filtrées **Recordset** ne modifie pas la position de l’enregistrement actuel, et **MoveNext** déplace le curseur à l’enregistrement suivant immédiatement après l’enregistrement actif.  
  
 Utilisez le **MovePrevious** méthode pour déplacer l’enregistrement actif position d’un enregistrement (vers le haut de la **Recordset**). Le **Recordset** objet doit prendre en charge les signets ou le déplacement du curseur arrière ; sinon, l’appel de méthode génère une erreur. Si le premier enregistrement est l’enregistrement actif et que vous appelez le **MovePrevious** (méthode), ADO définit l’enregistrement actif à la position avant le premier enregistrement dans le **Recordset** ([BOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md)est **True**). Une tentative de déplacement vers l’arrière lorsque la **BOF** propriété est déjà **True** génère une erreur. Si le **Recordset** objet ne prend pas en charge les signets ou le déplacement du curseur arrière, la **MovePrevious** méthode génère une erreur.  
  
 Si le **Recordset** est avant uniquement et que vous souhaitez prendre en charge le défilement arrière et avant, vous pouvez utiliser la [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) propriété pour créer un cache d’enregistrements qui prend en charge le déplacement du curseur vers l’arrière via le [déplacer](../../../ado/reference/ado-api/move-method-ado.md) (méthode). Étant donné que les enregistrements mis en cache sont chargés en mémoire, vous devez éviter la mise en cache plus d’enregistrements qu’il n’est nécessaire. Vous pouvez appeler la **MoveFirst** méthode dans un curseur avant uniquement **Recordset** objet ; cela peut provoquer le fournisseur réexécute la commande qui a généré le **Recordset** objet .  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes-exemple (VB)](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-example-vb.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes-exemple (VBScript)](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-example-vbscript.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-example-vc.md)   
 [Move, méthode (ADO)](../../../ado/reference/ado-api/move-method-ado.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (RDS)](../../../ado/reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [MoveRecord, méthode (ADO)](../../../ado/reference/ado-api/moverecord-method-ado.md)
