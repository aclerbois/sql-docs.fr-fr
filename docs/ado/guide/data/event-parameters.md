---
title: Paramètres d’événement | Documents Microsoft
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
helpviewer_keywords:
- Error parameter [ADO]
- Object parameter [ADO]
- Status parameter [ADO]
- events [ADO], parameters
- Reason parameter [ADO]
- event parameters [ADO]
ms.assetid: bd5c5afa-d301-4899-acda-40f98a6afa4d
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9946b424f5ed885ad432610c7c053dddc6332954
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="event-parameters"></a>Paramètres d’événement
Chaque gestionnaire d’événements a un paramètre d’état qui contrôle le Gestionnaire d’événements. Pour les événements terminées, ce paramètre est également utilisé pour indiquer la réussite ou l’échec de l’opération qui a généré l’événement. Événements plus complètes ont également un paramètre d’erreur pour fournir des informations sur toute erreur qui se sont produites et un ou plusieurs paramètres de l’objet qui font référence à des objets ADO utilisés pour effectuer l’opération. Par exemple, le [ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md) événement inclut les paramètres de l’objet pour le **commande**, **Recordset**, et **connexion** objets associé à l’événement. Dans l’exemple suivant de Microsoft® Visual Basic®, vous pouvez voir les pCommand, Connection et pConnection objets qui représentent les **commande**, **Recordset**, et **deconnexion** les objets qui sont utilisés par le **Execute** (méthode).  
  
```  
Private Sub connEvent_ExecuteComplete(ByVal RecordsAffected As Long, _  
     ByVal pError As ADODB.Error, _  
     adStatus As ADODB.EventStatusEnum, _  
     ByVal pCommand As ADODB.Command, _  
     ByVal pRecordset As ADODB.Recordset, _  
     ByVal pConnection As ADODB.Connection)  
```  
  
 À l’exception de la **erreur** de l’objet, les mêmes paramètres sont transmis aux événements Will. Ce permet d’étudier des objets qui seront utilisées dans l’opération en attente et de déterminent si l’opération doit être autorisée à la fin.  
  
 Certains gestionnaires d’événements ont un *raison* paramètre, qui fournit des informations supplémentaires sur la raison pour laquelle l’événement s’est produite. Par exemple, le **WillMove** et **MoveComplete** événements peuvent se produire en raison de l’une des méthodes de navigation (**MoveNext**, **MovePrevious**, et ainsi de suite) ou à la suite d’une actualisation.  
  
## <a name="status-parameter"></a>Paramètre d’état  
 Lorsque la routine de gestionnaire d’événements est appelée, le *état* est affectée à une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**adStatusOK**|Passé à va et événements Complete. Cette valeur signifie que l’opération qui a provoqué l’événement s’est terminée correctement.|  
|**adStatusErrorsOccurred**|Transmis uniquement aux événements Complete. Cette valeur signifie que l’opération qui a provoqué l’événement a échoué ou qu’un événement Will a annulé l’opération. Vérifiez le *erreur* paramètre pour plus de détails.|  
|**adStatusCantDeny**|Passé sera uniquement aux événements. Cette valeur signifie que l’opération ne peut pas être annulée par l’événement Will. Elle doit être effectuée.|  
  
 Si vous déterminez, dans l’événement Will, que l’opération doit continuer, laissez le *état* paramètre inchangé. Tant que le paramètre d’état entrant n’a pas été défini **adStatusCantDeny**, toutefois, vous pouvez annuler l’opération en attente en modifiant *état* à **adStatusCancel**. Dans ce cas, l’événement Complete associé à l’opération a son *état* paramètre la valeur **contraire**. Le **erreur** objet passé à l’événement Complete contiendra la valeur **adErrOperationCancelled**.  
  
 Si vous ne voulez plus traiter un événement, vous pouvez définir *état* à **adStatusUnwantedEvent** et votre application n’est plus reçoivent une notification de cet événement. Toutefois, n’oubliez pas que certains événements peuvent être déclenchés pour plusieurs raisons. Dans ce cas, vous devez spécifier **adStatusUnwantedEvent** pour chaque raison possible. Par exemple, pour arrêter de recevoir des notifications en attente **RecordChange** des événements, vous devez définir le *état* paramètre **adStatusUnwantedEvent** pour  **adRsnAddNew**, **adRsnDelete**, **adRsnUpdate**, **adRsnUndoUpdate**, **adRsnUndoAddNew**, **adRsnUndoDelete**, et **adRsnFirstChange** qu’ils se produisent.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**adStatusUnwantedEvent**|Demande que ce gestionnaire d’événements ne recevoir aucune autre notification.|  
|**adStatusCancel**|Demander l’annulation de l’opération qui doit se produire.|  
  
## <a name="error-parameter"></a>Paramètre d’erreur  
 Le *erreur* paramètre est une référence à ADO [erreur](../../../ado/reference/ado-api/error-object.md) objet. Lorsque le *état* paramètre est défini sur **contraire**, le **erreur** objet contient des détails sur la raison pour laquelle l’opération a échoué. Si l’événement est associé à un événement Complete a annulé l’opération en définissant le *état* paramètre **adStatusCancel**, l’objet d’erreur est toujours définie  **adErrOperationCancelled**.  
  
## <a name="object-parameter"></a>Paramètre de l’objet  
 Chaque événement reçoit un ou plusieurs objets représentant les objets impliqués dans l’opération. Par exemple, le **ExecuteComplete** événement reçoit un **commande** objet, un **Recordset** objet et un **connexion** objet.  
  
## <a name="reason-parameter"></a>Paramètre Reason  
 Le *raison* paramètre, *adReason*, fournit des informations supplémentaires sur la raison pour laquelle l’événement s’est produite. Les événements avec un *adReason* paramètre peut être appelé plusieurs fois, même pour la même opération, pour une raison différente chaque fois. Par exemple, le **WillChangeRecord** Gestionnaire d’événements est appelé pour les opérations qui sont sur le point d’effectuer ou d’annuler l’insertion, la suppression ou la modification d’un enregistrement. Si vous voulez traiter un événement uniquement lorsque cela se produit pour une raison particulière, vous pouvez utiliser la *adReason* paramètre pour filtrer les occurrences qui vous n'intéressent pas. Par exemple, si vous souhaitez traiter les événements de modification de l’enregistrement uniquement lorsqu’ils se produisent car un enregistrement a été ajouté, vous pouvez utiliser ce qui suit.  
  
```  
' BeginEventExampleVB01  
Private Sub rsTest_WillChangeRecord(ByVal adReason As ADODB.EventReasonEnum, ByVal cRecords As Long, adStatus As ADODB.EventStatusEnum, ByVal pRecordset As ADODB.Recordset)  
   If adReason = adRsnAddNew Then  
       ' Process event  
       '...  
   Else  
       ' Cancel event notification for all  
       ' other possible adReason values.  
       adStatus = adStatusUnwantedEvent  
   End If  
End Sub  
' EndEventExampleVB01  
```  
  
 Dans ce cas, la notification peut se produire potentiellement pour chacune des autres raisons. Toutefois, il se produit une seule fois pour chaque motif. Une fois la notification s’est produit une fois pour chaque raison, vous recevrez notification uniquement pour l’ajout d’un nouvel enregistrement.  
  
 En revanche, vous devez définir *ne* à **adStatusUnwantedEvent** qu’une seule fois pour demander qu’un gestionnaire d’événements sans un **adReason** événement réception stop de paramètre notifications.  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du Gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)   
 [Instanciation des événements ADO par langage](../../../ado/guide/data/ado-event-instantiation-by-language.md)   
 [Fonctionnement conjoint des gestionnaires d’événements](../../../ado/guide/data/how-event-handlers-work-together.md)   
 [Types d’événements](../../../ado/guide/data/types-of-events.md)
