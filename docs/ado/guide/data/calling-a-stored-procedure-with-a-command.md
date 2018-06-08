---
title: Appel d’une procédure stockée avec une commande | Documents Microsoft
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
- calling stored procedures [ADO]
- stored procedures [ADO]
- commands [ADO]
ms.assetid: 685f7652-2271-4ede-b552-2eeb8c756b4c
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: dbb9edb3744f1cc2483cfbe4d0d08a06868998d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="calling-a-stored-procedure-with-a-command"></a>Appel d’une procédure stockée avec une commande
Vous pouvez utiliser une commande pour appeler une procédure stockée. L’exemple de code à la fin de cette rubrique fait référence à une procédure stockée dans la base de données Northwind, appelée CustOrdersOrders, qui est défini comme suit.  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 Consultez votre documentation SQL Server pour plus d’informations sur la façon de définir et appeler des procédures stockées.  
  
 Cette procédure stockée est similaire à la commande utilisée dans [paramètres de l’objet commande](../../../ado/guide/data/command-object-parameters.md). Il prend un paramètre Customerid et retourne des informations sur les commandes de ce client. L’exemple de code suivant utilise cette procédure stockée comme source pour ADO **Recordset**.  
  
 À l’aide de la procédure stockée permet d’accéder à une autre fonctionnalité d’ADO : la **paramètres** collection **Actualiser** (méthode). À l’aide de cette méthode, ADO peut automatiquement renseigner toutes les informations sur les paramètres requis par la commande en cours d’exécution. Est une baisse des performances à l’aide de cette technique, car ADO doit interroger la source de données pour obtenir les informations sur les paramètres.  
  
 Autres différences importantes existent entre l’exemple de code suivant et le code dans [paramètres de l’objet commande](../../../ado/guide/data/command-object-parameters.md), où les paramètres ont été entrés manuellement. Tout d’abord, ce code ne définit pas le **Prepared** propriété **True** , car elle est une procédure stockée SQL Server et est précompilé par définition. Ensuite, le **CommandType** propriété de la **commande** objet modifié en **valeur adCmdStoredProc** dans le deuxième exemple pour informer ADO que la commande est une procédure stockée.  
  
 Enfin, dans le deuxième exemple le paramètre doit être auquel index lors de la définition de la valeur, car vous pouvez connaissez pas le nom du paramètre au moment du design. Si vous ne connaissez pas le nom du paramètre, vous pouvez définir le nouveau [NamedParameters](../../../ado/reference/ado-api/namedparameters-property-ado.md) propriété de la **commande** de l’objet à la valeur True et font référence au nom de la propriété. Vous vous demandez peut-être pourquoi la position du premier paramètre est indiqué dans la procédure stockée (@CustomerID) est de 1 au lieu de 0 (`objCmd(1) = "ALFKI"`). Il s’agit, car le paramètre 0 contient une valeur de retour de la procédure stockée SQL Server.  
  
```  
'BeginAutoParamCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objParm1 As New ADODB.Parameter  
    Dim objRs As New ADODB.Recordset  
  
    ' Set CommandText equal to the stored procedure name.  
    objCmd.CommandText = "CustOrdersOrders"  
    objCmd.CommandType = adCmdStoredProc  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Automatically fill in parameter info from stored procedure.  
    objCmd.Parameters.Refresh  
  
    ' Set the param value.  
    objCmd(1) = "ALFKI"  
  
    ' Execute once and display...  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    ' ...then set new param value, re-execute command, and display.  
    objCmd(1) = "CACTU"  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    'clean up  
    objRs.Close  
    objConn.Close  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
    Exit Sub  
  
ErrHandler:  
    'clean up  
    If objRs.State = adStateOpen Then  
        objRs.Close  
    End If  
  
    If objConn.State = adStateOpen Then  
        objConn.Close  
    End If  
  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndAutoParamCmd  
  
'BeginNewConnection  
Private Function GetNewConnection() As ADODB.Connection  
    Dim oCn As New ADODB.Connection  
    Dim sCnStr As String  
  
    sCnStr = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    oCn.Open sCnStr  
  
    If oCn.State = adStateOpen Then  
        Set GetNewConnection = oCn  
    End If  
  
End Function  
'EndNewConnection  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Article de la Base de connaissances 117500](http://go.microsoft.com/fwlink/?LinkId=117500)
