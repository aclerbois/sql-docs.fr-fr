---
title: 'Instanciation des événements ADO : Visual Basic | Documents Microsoft'
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
dev_langs:
- VB
ms.assetid: dce0a2a3-326f-4aaf-a822-6c5549833afa
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 27487b6eeda322c7e48036fc994ec497f39d98fe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-event-instantiation-visual-basic"></a>Instanciation des événements ADO : Visual Basic
Afin de gérer les événements ADO dans Microsoft® Visual Basic®, vous devez déclarer une variable au niveau du module à l’aide du **WithEvents** (mot clé). La variable peut être déclarée uniquement dans le cadre d’un module de classe et doit être déclarée au niveau du module. Cela n’est pas aussi restrictif qu’il semble, toutefois, étant donné que Visual Basic **formulaire** objets sont également des classes. Le plus simple pour gérer les événements ADO consiste à déclarer une variable à l’aide **WithEvents**. L’exemple suivant gère le **ConnectComplete** événement pour un **connexion** objet :  
  
```  
' BeginEventExampleVB02  
Dim WithEvents connEvent As Connection  
Attribute connEvent.VB_VarHelpID = -1  
  
Private Sub Form_Load()  
Dim strConn As String  
  
   ' Create a new object with event  
   ' handling enabled.  
   strConn = "Provider=sqloledb;" & _  
      "Data Source=MyServer;" & _  
      "Initial Catalog=Northwind;" & _  
      "Integrated Security=SSPI;"  
   Set connEvent = New ADODB.Connection  
   connEvent.Open strConn  
End Sub  
  
Private Sub connEvent_ConnectComplete(ByVal pError As ADODB.Error, _  
    adStatus As ADODB.EventStatusEnum, _  
    ByVal pConnection As ADODB.Connection)  
Dim strMsg As String  
  
   If adStatus = adStatusErrorsOccurred Then  
      Select Case pError.Number  
         Case adErrOperationCancelled  
            ' The operation was cancelled in the  
            ' Will event. Notify the user and exit.  
            strMsg = "I'm sorry you can't connect right now." & vbCrLf  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
         Case Else  
            strMsg = "Error " & Format(pError.Number) & vbCrLf  
            strMsg = strMsg & pError.Description  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
      End Select  
   End If  
   frmWait.btnOK.Enabled = True  
End Sub  
' EndEventExampleVB02  
```  
  
 Le **connexion** objet est déclaré au niveau de la **formulaire** niveau à l’aide de la **WithEvents** (mot clé) pour activer la gestion des événements. Le Gestionnaire d’événements Form_Load crée l’objet en affectant une nouvelle **connexion** objet *connEvent* et ouvre la connexion. Bien entendu, une application réelle est nécessaire d’un traitement supplémentaire dans le Gestionnaire d’événements Form_Load qu’est illustrée ici.
