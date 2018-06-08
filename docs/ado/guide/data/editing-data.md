---
title: Modification de données | Documents Microsoft
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
- ADO, editing data
- AdUseClient [ADO]
- editing data [ADO]
ms.assetid: ef514f85-c446-4f05-824e-c9313b2ffae1
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 915e96df957b3f81c1b9a6d6731ca942b69e8f1e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="editing-data"></a>Modification des données
Nous avons expliqué comment utiliser ADO pour se connecter à une source de données, d’exécuter une commande, d’obtenir les résultats dans un **Recordset** de l’objet et naviguer dans le **Recordset**. Cette section se concentre sur la prochaine opération ADO fondamentale : modification des données.  
  
 Cette section continue d’utiliser l’exemple **Recordset** introduite dans [examen des données](../../../ado/guide/data/examining-data.md), avec un changement important. Le code suivant permet d’ouvrir le **Recordset**:  
  
```  
'BeginEditIntro  
    Dim strSQL As String  
    Dim objRs1 As ADODB.Recordset  
  
    strSQL = "SELECT * FROM Shippers"  
  
    Set objRs1 = New ADODB.Recordset  
  
    objRs1.Open strSQL, GetNewConnection, adOpenStatic, _  
                adLockBatchOptimistic, adCmdText  
  
    ' Disconnect the Recordset from the Connection object.  
    Set objRs1.ActiveConnection = Nothing  
'EndEditIntro  
```  
  
 Le changement important du code implique de définir la **CursorLocation** propriété de la **connexion** objet égal à **adUseClient** dans le  *GetNewConnection* fonction (présentée dans l’exemple suivant), qui indique l’utilisation d’un curseur client. Pour plus d’informations sur les différences entre les curseurs côté client et côté serveur, consultez [présentation des curseurs et des verrous](../../../ado/guide/data/understanding-cursors-and-locks.md).  
  
 Le **CursorLocation** la propriété **adUseClient** paramètre déplace l’emplacement du curseur de la source de données (dans ce cas, le serveur SQL) vers l’emplacement du code client (la station de travail). Ce paramètre force ADO à invoquer le moteur de curseur Client pour OLE DB sur le client pour créer et gérer le curseur.  
  
 Vous avez pouvez aussi remarqué que le **LockType** paramètre de la **ouvrir** méthode modifiée pour **adLockBatchOptimistic**. Le curseur s’ouvre en mode batch. (Le fournisseur met en cache plusieurs modifications et les écrit dans la source de données sous-jacente uniquement lorsque vous appelez le **UpdateBatch** méthode.) Les modifications qui ont été apportées à la **Recordset** ne sera pas mis à jour dans la base de données jusqu'à ce que le **UpdateBatch** méthode est appelée.  
  
 Enfin, le code dans cette section utilise une version modifiée de la fonction GetNewConnection. Cette version de la fonction renvoie désormais un curseur côté client. La fonction ressemble à ceci :  
  
```  
'BeginNewConnection  
Public Function GetNewConnection() As ADODB.Connection  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim strConnStr As String  
  
    strConnStr = "Provider='SQLOLEDB';Initial Catalog='Northwind';" & _  
                 "Data Source='MySqlServer';Integrated Security='SSPI';"  
  
    objConn.ConnectionString = strConnStr  
    objConn.CursorLocation = adUseClient  
    objConn.Open  
  
    Set GetNewConnection = objConn  
  
    Exit Function  
  
ErrHandler:  
    Set objConn = Nothing  
    Set GetNewConnection = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Function  
'EndNewConnection  
```  
  
 Cette section contient les rubriques suivantes.  
  
-   [Modification d’enregistrements existants](../../../ado/guide/data/editing-existing-records.md)  
  
-   [Ajout d’enregistrements](../../../ado/guide/data/adding-records.md)  
  
-   [Détermination de ce qui est pris en charge](../../../ado/guide/data/determining-what-is-supported.md)  
  
-   [Suppression d’enregistrements avec la méthode Delete](../../../ado/guide/data/deleting-records-using-the-delete-method.md)  
  
-   [Alternatives : utilisation d’instructions SQL](../../../ado/guide/data/alternatives-using-sql-statements.md)
