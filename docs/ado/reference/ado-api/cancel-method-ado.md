---
title: "Cancel (méthode) (ADO) | Documents Microsoft"
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: ado
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: sql
ms.tgt_pltfrm: 
ms.topic: article
apitype: COM
f1_keywords:
- Recordset20::Cancel
- _Record::Cancel
- _Connection::Cancel
- Command25::Cancel
- _Stream::Cancel
helpviewer_keywords:
- Cancel method [ADO]
ms.assetid: e0db4e15-6787-41e2-8f13-9e9b524d620a
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: d9dd6f6c62d9ee20f75dabc8b28831b7376fc00b
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="cancel-method-ado"></a>Cancel (méthode) (ADO)
Annule l’exécution d’un appel de méthode asynchrone en attente.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Cancel  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez le **Annuler** méthode pour terminer l’exécution d’un appel de méthode asynchrone : autrement dit, une méthode est appelée avec le **adAsyncConnect**, **adAsyncExecute**, ou **adAsyncFetch** option.  
  
 Le tableau suivant indique quelle tâche est terminée lorsque vous utilisez la **Annuler** méthode sur un type d’objet particulier.  
  
|Si *objet* est un|Le dernier appel asynchrone à cette méthode se termine.|  
|----------------------|-------------------------------------------------------------|  
|[Command](../../../ado/reference/ado-api/command-object-ado.md)|[Execute](../../../ado/reference/ado-api/execute-method-ado-command.md)|  
|[Connexion](../../../ado/reference/ado-api/connection-object-ado.md)|[Exécutez](../../../ado/reference/ado-api/execute-method-ado-connection.md) ou [ouvert](../../../ado/reference/ado-api/open-method-ado-connection.md)|  
|[Record](../../../ado/reference/ado-api/record-object-ado.md)|[CopyRecord](../../../ado/reference/ado-api/copyrecord-method-ado.md), [DeleteRecord](../../../ado/reference/ado-api/deleterecord-method-ado.md), [MoveRecord](../../../ado/reference/ado-api/moverecord-method-ado.md), or [Open](../../../ado/reference/ado-api/open-method-ado-record.md)|  
|[Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)|[Ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md)|  
|[Stream](../../../ado/reference/ado-api/stream-object-ado.md)|[Ouvrir](../../../ado/reference/ado-api/open-method-ado-stream.md)|  
  
## <a name="applies-to"></a>S'applique à  
  
||||  
|-|-|-|  
|[Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md)|[Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)|[Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)|  
|[Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)|[Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)||  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode Cancel (VB)](../../../ado/reference/ado-api/cancel-method-example-vb.md)   
 [Exemple de méthode Cancel (VBScript)](../../../ado/reference/rds-api/cancel-method-example-vbscript.md)   
 [Exemple de méthode Cancel (VC ++)](../../../ado/reference/ado-api/cancel-method-example-vc.md)   
 [Cancel (méthode) (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)   
 [CancelBatch, méthode (ADO)](../../../ado/reference/ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate, méthode (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [CancelUpdate, méthode (RDS)](../../../ado/reference/rds-api/cancelupdate-method-rds.md)   
 [Execute (méthode) (commande ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)   
 [Execute (méthode) (connexion ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [Open (méthode) (connexion ADO)](../../../ado/reference/ado-api/open-method-ado-connection.md)   
 [Open, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)
