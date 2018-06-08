---
title: EventReasonEnum | Documents Microsoft
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
- EventReasonEnum
helpviewer_keywords:
- EventReasonEnum enumeration [ADO]
ms.assetid: 7d4a5496-ec2d-4936-b36a-7049a82be4b4
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9b9645e05ce3387ea5948cca07b91ecdc80cd4b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="eventreasonenum"></a>EventReasonEnum
Spécifie la raison qui a provoqué un événement se produise.  
  
|Constante|Valeur| Description|  
|--------------|-----------|-----------------|  
|**adRsnAddNew**|1|Une opération a ajouté un nouvel enregistrement.|  
|**adRsnClose**|9|Une opération a fermé le **Recordset**.|  
|**adRsnDelete**|2|Une opération a supprimé un enregistrement.|  
|**adRsnFirstChange**|11|Une opération effectuée la première modification à un enregistrement.|  
|**adRsnMove**|10|Une opération a déplacé le pointeur d’enregistrement dans le **Recordset**.|  
|**adRsnMoveFirst**|12|Une opération a déplacé le pointeur vers le premier enregistrement dans le **Recordset**.|  
|**adRsnMoveLast**|15|Une opération a déplacé le pointeur vers le dernier enregistrement de la **Recordset**.|  
|**adRsnMoveNext**|13|Une opération a déplacé le pointeur vers l’enregistrement suivant dans la **Recordset**.|  
|**adRsnMovePrevious**|14|Une opération a déplacé le pointeur vers l’enregistrement précédent dans le **Recordset**.|  
|**adRsnRequery**|7|Une opération d’actualisation du [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).|  
|**adRsnResynch**|8|Une opération a resynchronisé les **Recordset** avec la base de données.|  
|**adRsnUndoAddNew**|5|Une opération a annulé l’ajout d’un nouvel enregistrement.|  
|**adRsnUndoDelete**|6|Une opération a annulé la suppression d’un enregistrement.|  
|**adRsnUndoUpdate**|4|Une opération a annulé la mise à jour d’un enregistrement.|  
|**adRsnUpdate**|3|Une opération de mise à jour un enregistrement existant.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC équivalent  
 Package : **com.ms.wfc.data**  
  
|Constante|  
|--------------|  
|AdoEnums.EventReason.ADDNEW|  
|AdoEnums.EventReason.CLOSE|  
|AdoEnums.EventReason.DELETE|  
|AdoEnums.EventReason.FIRSTCHANGE|  
|AdoEnums.EventReason.MOVE|  
|AdoEnums.EventReason.MOVEFIRST|  
|AdoEnums.EventReason.MOVELAST|  
|AdoEnums.EventReason.MOVENEXT|  
|AdoEnums.EventReason.MOVEPREVIOUS|  
|AdoEnums.EventReason.REQUERY|  
|AdoEnums.EventReason.RESYNCH|  
|AdoEnums.EventReason.UNDOADDNEW|  
|AdoEnums.EventReason.UNDODELETE|  
|AdoEnums.EventReason.UNDOUPDATE|  
|AdoEnums.EventReason.UPDATE|  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[WillChangeRecord et RecordChangeComplete, événements (ADO)](../../../ado/reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|[WillChangeRecordset et RecordsetChangeComplete, événements (ADO)](../../../ado/reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|  
|[WillMove et MoveComplete, événements (ADO)](../../../ado/reference/ado-api/willmove-and-movecomplete-events-ado.md)||
