---
title: ExecuteComplete, événement (ADO) | Documents Microsoft
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
- Connection::ExecuteComplete
- ExecuteComplete
helpviewer_keywords:
- ExecuteComplete event [ADO]
ms.assetid: 62470d42-e511-494c-bec4-ad4591734b7b
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ab8c0b33fe31499999cc73d2ebc03ef0d32ace70
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executecomplete-event-ado"></a>ExecuteComplete, événement (ADO)
Le **ExecuteComplete** événement est appelé après l’exécution d’une commande est terminée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ExecuteComplete RecordsAffected, pError, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *RecordsAffected*  
 A **Long** valeur indiquant le nombre d’enregistrements concernés par la commande.  
  
 *pError*  
 Un [erreur](../../../ado/reference/ado-api/error-object.md) objet. Elle décrit l’erreur qui s’est produite si la valeur de **ne** est **contraire**; sinon, elle n’est pas définie.  
  
 *adStatus*  
 Un [il ne](../../../ado/reference/ado-api/eventstatusenum.md) valeur d’état. Lorsque cet événement est appelé, ce paramètre est défini **adStatusOK** si l’opération qui a provoqué l’événement a réussi, ou pour **contraire** si l’opération a échoué.  
  
 Avant le retour de cet événement, définissez ce paramètre sur **adStatusUnwantedEvent** pour éviter toute notification.  
  
 *pCommand*  
 Le [commande](../../../ado/reference/ado-api/command-object-ado.md) objet qui a été exécutée. Contient un **commande** même lors de l’appel de l’objet **Connection.Execute** ou **Recordset.Open** sans créer explicitement un **commande** , dans quels cas la **commande** objet est créé en interne par ADO.  
  
 *pRecordset*  
 A [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet qui est le résultat de la commande exécutée. Cela **Recordset** peut être vide. Vous ne devez jamais détruire cet objet Recordset à partir de ce gestionnaire d’événements. Cela entraîne une Violation d’accès ADO tente d’accéder à un objet qui n’existe plus.  
  
 *pConnection*  
 A [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet. La connexion sur laquelle l’opération a été exécutée.  
  
## <a name="remarks"></a>Notes  
 Un **ExecuteComplete** événement peut se produire en raison du **connexion.** [Exécuter](../../../ado/reference/ado-api/execute-method-ado-connection.md), **commande.** [Exécuter](../../../ado/reference/ado-api/execute-method-ado-command.md), **Recordset.** [Ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md), **Recordset.** [Requery](../../../ado/reference/ado-api/requery-method.md), ou **Recordset.** [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md) méthodes.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC ++)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)
