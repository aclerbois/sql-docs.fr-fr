---
title: Erreurs du fournisseur | Documents Microsoft
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
- errors collection [ADO]
- provider errors [ADO]
- event-related errors [ADO]
- errors [ADO], provider
- Error object [ADO], provider errors
ms.assetid: cc7d6ff9-2034-45c6-9d61-90b177010054
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 551c3a7e7f90f69601ff84449d60fc79c1375ece
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="provider-errors"></a>Erreurs du fournisseur
En cas d’erreur du fournisseur, une erreur d’exécution de -2147467259 est retournée. Lorsque vous recevez cette erreur, vérifiez le **erreurs** collection actif **connexion** objet, qui contient une ou plusieurs erreurs décrivant ce qui s’est produite.  
  
## <a name="the-ado-errors-collection"></a>La Collection d’erreurs ADO  
 Parce qu’une opération ADO particulière peut produire plusieurs erreurs de fournisseur, ADO expose une collection d’objets d’erreur via le **connexion** objet. Cette collection ne contient aucun objet si une opération se déroule correctement et qu’il contient un ou plusieurs **erreur** objets si quelque chose s’est produite et le fournisseur a déclenché une ou plusieurs erreurs. Examinez chaque objet d’erreur pour déterminer la cause exacte de l’erreur.  
  
 Dès que vous avez terminé toutes les erreurs qui se sont produites, vous pouvez effacer la collection en appelant le **effacer** (méthode). Il est particulièrement important d’effacer explicitement la **erreurs** collection avant d’appeler le **Resync**, **UpdateBatch**, ou **CancelBatch**méthode sur un **Recordset** objet, le **ouvrir** méthode sur un **connexion** de l’objet, ou définir le **filtre** propriété sur un **Recordset** objet. En effaçant la collection explicitement, vous pouvez être certain que n’importe quel **erreur** objets de la collection ne subsiste d’une opération précédente.  
  
 Certaines opérations peuvent générer des avertissements en plus des erreurs. Les avertissements sont également représentés par **erreur** des objets dans le **erreurs** collection. Lorsqu’un fournisseur ajoute un avertissement à la collection, il ne génère pas d’une erreur d’exécution. Vérifiez le **nombre** propriété de la **erreurs** collection pour déterminer si un avertissement a été généré par une opération particulière. Si le nombre est un ou plus, un **erreur** objet a été ajouté à la collection. Dès que vous avez déterminé que le **erreurs** collection contient des erreurs ou des avertissements, vous pouvez effectuer une itération au sein de la collection et récupérer des informations sur chaque **erreur** objet qu’il contient. Le court exemple Visual Basic suivant illustre cela :  
  
```  
' BeginErrorHandlingVB02  
Private Function DeleteCustomer(ByVal CompanyName As String) As Long  
    On Error GoTo DeleteCustomerError  
  
    rst.Find "CompanyName='" & CompanyName & "'"  
DeleteCustomerError:  
Dim objError As ADODB.Error  
Dim strError As String  
  
    If cnn.Errors.Count > 0 Then  
        For Each objError In cnn.Errors  
            strError = strError & "Error #" & objError.Number & _  
                " " & objError.Description & vbCrLf & _  
                "NativeError: " & objError.NativeError & vbCrLf & _  
                "SQLState: " & objError.SQLState & vbCrLf & _  
                "Reported by: " & objError.Source & vbCrLf & _  
                "Help file: " & objError.HelpFile & vbCrLf & _  
                "Help Context ID: " & objError.HelpContext  
        Next  
        MsgBox strError  
    End If  
End Function  
' EndErrorHandlingVB02  
```  
  
 La routine de gestion d’erreur inclut un **pour chaque** boucle qui examine chaque objet de la **erreurs** collection. Dans cet exemple, il accumule un message à afficher. Dans un programme de travail, vous devez écrire du code pour effectuer une tâche appropriée pour chaque erreur, telles que la fermeture de toutes les ouvrir des fichiers et de fermer le programme de façon ordonnée.  
  
## <a name="the-error-object"></a>L’objet d’erreur  
 En examinant un **erreur** vous pouvez déterminer quel message d’erreur s’est produite, et plus important, application ou l’objet a provoqué l’erreur de l’objet. Le **erreur** objet a les propriétés suivantes :  
  
|Nom de la propriété| Description|  
|-------------------|-----------------|  
|**Description**|Description textuelle de l’erreur qui s’est produite.|  
|**HelpContext, HelpFile**|Désigne le fichier d’aide et de la rubrique d’aide qui contient une description de l’erreur qui s’est produite.|  
|**NativeError**|Le numéro d’erreur spécifique au fournisseur.|  
|**Nombre**|Un entier Long qui représente le nombre (répertoriées dans le **ErrorValueEnum**) de l’erreur qui s’est produite.|  
|**Source**|Indique le nom de l’objet ou l’application qui a généré une erreur.|  
|**SQLState**|Un code d’erreur à cinq caractères que le fournisseur renvoie au cours du processus d’une instruction SQL.|  
  
 ADO **erreur** objet est très similaire à Visual Basic standard **Err** objet. Ces propriétés décrivent l’erreur s’est produite. En plus du nombre de l’erreur, vous recevez également deux éléments d’information associés. Le **Native Error** propriété contient un numéro d’erreur spécifique au fournisseur que vous utilisez. Dans l’exemple précédent, le fournisseur est le fournisseur Microsoft OLE DB pour SQL Server, par conséquent, **Native Error** contiendra des erreurs spécifiques à SQL Server. Le **SQLState** propriété possède un code de cinq lettres qui décrit une erreur dans une instruction SQL.  
  
## <a name="event-related-errors"></a>Erreurs liées aux événements  
 Le **erreur** objet est également utilisé lorsque des erreurs liées aux événements se produisent. Vous pouvez déterminer si une erreur s’est produite dans le processus qui a déclenché un événement ADO en vérifiant la **erreur** objet passé comme paramètre d’événement.  
  
 Si l’opération qui provoque un événement se termine avec succès, le *ne* paramètre du Gestionnaire d’événements est défini *adStatusOK*. En revanche, si l’opération qui a déclenché l’événement a échoué, le *ne* paramètre est défini sur *contraire*. Dans ce cas, le *pError* paramètre contiendra un **erreur** objet qui décrit l’erreur.
