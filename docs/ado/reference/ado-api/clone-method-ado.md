---
title: Clone, méthode (ADO) | Documents Microsoft
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
- Recordset20::Clone
- Recordset20::raw_Clone
helpviewer_keywords:
- Clone method [ADO]
ms.assetid: ad49265f-1c05-4271-9bbf-7c00010ac18c
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 33fee6bf55b3175d75879e06949744d0730f6af0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clone-method-ado"></a>Clone, méthode (ADO)
Crée un doublon [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet d’un existant **Recordset** objet. Si vous le souhaitez, spécifie que le clone doit être en lecture seule.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set rstDuplicate = rstOriginal.Clone (LockType)  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un **Recordset** référence d’objet.  
  
#### <a name="parameters"></a>Paramètres  
 *rstDuplicate*  
 Une variable objet qui identifie le doublon **Recordset** objet à créer.  
  
 *rstOriginal*  
 Une variable objet qui identifie le **Recordset** objet à dupliquer.  
  
 *LockType*  
 Ce paramètre est facultatif. A [LockTypeEnum](../../../ado/reference/ado-api/locktypeenum.md) valeur qui spécifie le type de verrou de l’original **Recordset**, ou en lecture seule **Recordset**. Les valeurs valides sont **adLockUnspecified** ou **adLockReadOnly**.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **Clone** dupliqué de la méthode de création de plusieurs **Recordset** des objets, en particulier si vous voulez conserver plusieurs enregistrements en cours dans un ensemble donné d’enregistrements. À l’aide de la **Clone** méthode est plus efficace que la création et l’ouverture d’un nouvel **Recordset** objet qui utilise la même définition que l’original.  
  
 Le [filtre](../../../ado/reference/ado-api/filter-property.md) propriété de l’original **Recordset**, le cas échéant, ne seront pas appliquées sur le clone. Définir le **filtre** propriété du nouveau **Recordset** pour filtrer les résultats. La méthode la plus simple pour copier tout existant **filtre** valeur consiste à affecter directement, comme suit.  
  
```  
rsNew.Filter = rsOriginal.Filter  
```  
  
 L’enregistrement actif d’un clone nouvellement créé est définie sur le premier enregistrement.  
  
 Modifications apportées à une **Recordset** objet sont visibles dans tous ses clones, quel que soit le type de curseur. Toutefois, après l’exécution de [Requery](../../../ado/reference/ado-api/requery-method.md) sur la version d’origine **Recordset**, les clones seront ne plus être synchronisés à l’original.  
  
 Fermeture de l’original **Recordset** ne ferme pas ses copies, ni est fermeture fermer une copie la version d’origine ou les autres copies.  
  
 Vous ne pouvez cloner un **Recordset** objet qui prend en charge des signets. Les valeurs de signet sont interchangeables ; Autrement dit, une référence de signet à partir d’un **Recordset** objet fait référence au même enregistrement dans un de ses clones.  
  
 Certains **Recordset** les événements qui sont déclenchés seront produit également dans toutes les **Recordset** clone. Toutefois, étant donné que l’enregistrement en cours peut différer entre cloné **jeux d’enregistrements**, les événements n’est peut-être pas valides pour le clone. Par exemple, si vous modifiez la valeur d’un champ, un [WillChangeField](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md) événement se produit dans le texte modifié **Recordset** et dans tous les clones. Le *champs* paramètre de la **WillChangeField** l’événement de cloné **Recordset** (où la modification a été apportée pas) fait référence aux champs de l’enregistrement actif du clone, qui peut être un enregistrement différent de celui de l’enregistrement actif d’origine **Recordset** où la modification s’est produite.  
  
 Le tableau suivant fournit une liste complète de tous les **Recordset** événements. Il indique si elles sont valides et déclenchées pour tous les clones recordset générés à l’aide de la **Clone** (méthode).  
  
|Événement|Déclenché dans clones ?|  
|-----------|--------------------------|  
|[EndOfRecordset](../../../ado/reference/ado-api/endofrecordset-event-ado.md)|non|  
|[FetchComplete](../../../ado/reference/ado-api/fetchcomplete-event-ado.md)|non|  
|[FetchProgress](../../../ado/reference/ado-api/fetchprogress-event-ado.md)|non|  
|[FieldChangeComplete](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|Oui|  
|[MoveComplete](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)|non|  
|[RecordChangeComplete](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|Oui|  
|[RecordsetChangeComplete](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|non|  
|[WillChangeField](../../../ado/reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|Oui|  
|[WillChangeRecord](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|Oui|  
|[WillChangeRecordset](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|non|  
|[WillMove](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)|non|  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode Clone (VB)](../../../ado/reference/ado-api/clone-method-example-vb.md)   
 [Clone, méthode-exemple (VBScript)](../../../ado/reference/ado-api/clone-method-example-vbscript.md)   
 [Clone, exemple de méthode (VC++)](../../../ado/reference/ado-api/clone-method-example-vc.md)   
