---
title: Erreurs ADO | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO]
ms.assetid: 9bb84114-a1df-4122-a1b8-ad98dcd85cc3
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a36f5b96ac0c04b6315ba5a135bbab0dbe75df2b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-run-time-errors"></a>Erreurs d’exécution ADO
Erreurs ADO sont signalées à votre programme en tant qu’erreurs d’exécution. Vous pouvez utiliser le mécanisme d’interception des erreurs de votre langage de programmation pour les récupérer et les gérer. Par exemple, en Visual Basic, utilisez la **en cas d’erreur** instruction. Dans Visual C++, cela dépend de la méthode que vous utilisez pour accéder aux bibliothèques ADO. Avec #import, utilisez un **try-catch** bloc. Sinon, les programmeurs C++ doivent extraire explicitement l’objet d’erreur en appelant **GetErrorInfo**. La procédure sub Visual Basic suivante illustre la récupération d’une erreur ADO :

```
' BeginErrorHandlingVB01
Private Sub Form_Load()
' Turn on error handling
On Error GoTo FormLoadError

'Open the database and the recordset for processing.
'
Dim strCnn As String
strCnn = "Provider=sqloledb;" & _
    "Data Source=a-iresmi2000;" & _
    "Initial Catalog=Northwind;Integrated Security=SSPI"

' cnn is a Public Connection Object because
' it was defined WithEvents
Set cnn = New ADODB.Connection
cnn.Open strCnn

' The next line of code intentionally causes
' an error by trying to open a connection
' that has already been opened.
cnn.Open strCnn

' rst is a Public Recordset because it
' was defined WithEvents
Set rst = New ADODB.Recordset
rst.Open "Customers", cnn

Exit Sub

' Error handler
FormLoadError:
    Dim strErr As String
    Select Case Err
        Case adErrObjectOpen
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            strErr = strErr & "Error reported by: " & Err.Source & vbCrLf
            strErr = strErr & "Help File: " & Err.HelpFile & vbCrLf
            strErr = strErr & "Topic ID: " & Err.HelpContext
            MsgBox strErr
            Debug.Print strErr
            Err.Clear
            Resume Next
        ' If some other error occurs that
        ' has nothing to do with ADO, show
        ' the number and description and exit.
        Case Else
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            MsgBox strErr
            Debug.Print strErr
            Unload Me
    End Select
End Sub
' EndErrorHandlingVB01
```

 Cela **Form_Load** procédure événementielle crée intentionnellement une erreur en essayant d’ouvrir le même **connexion** de l’objet à deux reprises. La deuxième fois le **ouvrir** méthode est appelée, le Gestionnaire d’erreurs est activé. Dans ce cas l’erreur est de type **adErrObjectOpen**, de sorte que le Gestionnaire d’erreurs affiche le message suivant avant de reprendre l’exécution du programme :

```
Error #3705: Operation is not allowed when the object is open.
Error reported by: ADODB.Connection
Help File: E:\WINNT\HELP\ADO260.CHM Topic ID: 1003705
```

 Le message d’erreur comprend toutes les informations fournies par Visual Basic **Err** de l’objet à l’exception de la **LastDLLError** valeur, qui ne s’applique pas ici. Le numéro d’erreur vous indique l’erreur qui s’est produite. La description est utile dans les cas dans lesquels vous ne souhaitez pas gérer l’erreur vous-même. Vous pouvez simplement passer le long à l’utilisateur. Bien que vous devez généralement utiliser des messages personnalisés pour votre application, vous ne pouvez pas anticiper toutes les erreurs ; la description fournit un indice sur la cause du problème. Dans l’exemple de code, l’erreur a été signalée par le **connexion** objet. Vous pouvez voir l’ici l’ID programmatique ou le type de l’objet, pas un nom de variable.

> [!NOTE]
>  Visual Basic **Err** objet contient uniquement des informations sur l’erreur la plus récente. ADO **erreurs** collection de la **connexion** objet contient un **erreur** objet pour chaque erreur déclenchée par la dernière opération ADO. Utilisez le **erreurs** collection plutôt que la **Err** objet pour gérer plusieurs erreurs. Pour plus d’informations sur la **erreurs** collection, consultez [erreurs du fournisseur](../../../ado/guide/data/provider-errors.md). Toutefois, s’il n’est pas valide **connexion** objet, le **Err** objet est la seule source pour plus d’informations sur les erreurs ADO.

 Quels types d’opérations sont susceptibles de provoquer des erreurs ADO ? Les erreurs courantes ADO peuvent impliquer une ouverture d’un objet comme un **connexion** ou **Recordset**, tente de mettre à jour des données, ou en appelant une méthode ou propriété n’est pas pris en charge par votre fournisseur.

 Erreurs OLE DB peuvent également être transmis à votre application en tant qu’erreurs d’exécution dans le **erreurs** collection.

 La rubrique suivante fournit plus d’informations sur les erreurs ADO.

-   [Informations de référence sur les erreurs ADO](../../../ado/guide/data/ado-error-reference.md)
