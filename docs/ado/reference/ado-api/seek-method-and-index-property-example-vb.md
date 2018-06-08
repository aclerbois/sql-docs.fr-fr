---
title: Recherche la méthode et les Index, propriété-Exemple (VB) | Documents Microsoft
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
helpviewer_keywords:
- Seek method [ADO], Visual Basic example
- index property [ADO]
ms.assetid: 337c9eda-9ddf-49ac-94d3-b33114ba6224
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a0fb30723fb55c7e33de4f841eb64b0be5dcafef
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="seek-method-and-index-property-example-vb"></a>Recherche la méthode et les Index, propriété-Exemple (VB)
Cet exemple utilise le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) l’objet [recherche](../../../ado/reference/ado-api/seek-method.md) (méthode) et [Index](../../../ado/reference/ado-api/index-property.md) propriété conjointement avec une donnée ***ID d’employé***, localiser nom de l’employé dans le ***employés*** table de la base de données Nwind.mdb.  
  
```  
'BeginSeekVB  
Public Sub SeekX()  
    On Error GoTo ErrorHandler  
  
    ' To integrate this code replace the data source  
    ' in the connection string  
  
     'recordset and connection variables  
    Dim rstEmployees As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLEmployees As String  
  
    Dim strID As String  
    Dim strPrompt As String  
    strPrompt = "Enter an EmployeeID (e.g., 1 to 9)"  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
                "Data Source='C:\Program Files\Microsoft Office XP\Office10\Samples\northwind.mdb';"  
    Cnxn.Open strCnxn  
  
     ' open recordset server-side for indexing  
    Set rstEmployees = New ADODB.Recordset  
    rstEmployees.CursorLocation = adUseServer  
    strSQLEmployees = "employees"  
    rstEmployees.Open strSQLEmployees, strCnxn, adOpenKeyset, adLockReadOnly, adCmdTableDirect  
  
    ' Does this provider support Seek and Index?  
    If rstEmployees.Supports(adIndex) And rstEmployees.Supports(adSeek) Then  
        rstEmployees.Index = "PrimaryKey"  
        ' Display all the employees  
            rstEmployees.MoveFirst  
            Do While rstEmployees.EOF = False  
                Debug.Print rstEmployees!EmployeeId; ": "; rstEmployees!FirstName; " "; _  
                    rstEmployees!LastName  
                rstEmployees.MoveNext  
            Loop  
  
    ' Prompt the user for an EmployeeID between 1 and 9  
          rstEmployees.MoveFirst  
          Do  
             strID = LCase(Trim(InputBox(strPrompt, "Seek Example")))  
             ' Quit if strID is a zero-length string (CANCEL, null, etc.)  
             If Len(strID) = 0 Then Exit Do  
             If Len(strID) = 1 And strID >= "1" And strID <= "9" Then  
                rstEmployees.Seek Array(strID), adSeekFirstEQ  
                If rstEmployees.EOF Then  
                   Debug.Print "Employee not found.", , "Seek Example"  
                   MsgBox "Employee not found."  
                Else  
                   Debug.Print strID; ": Employee='"; rstEmployees!FirstName; " "; _  
                      rstEmployees!LastName; "'"  
                   MsgBox "EmployeeID is: " + strID + "; Employee Name is: '" + rstEmployees!FirstName + " " + _  
                      rstEmployees!LastName + "'", , "Seek Example"  
                End If  
             Else  
                MsgBox "You entered a wrong EmployeeID, please enter a new ID."  
             End If  
          Loop  
    End If  
  
    ' clean up  
    rstEmployees.Close  
    Cnxn.Close  
    Set rstEmployees = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstEmployees Is Nothing Then  
        If rstEmployees.State = adStateOpen Then rstEmployees.Close  
    End If  
    Set rstEmployees = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndSeekVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Propriété index](../../../ado/reference/ado-api/index-property.md)   
 [Objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Seek, méthode](../../../ado/reference/ado-api/seek-method.md)
