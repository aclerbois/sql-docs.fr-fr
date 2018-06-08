---
title: À l’aide d’un objet de connexion | Documents Microsoft
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
- connections [ADO]
ms.assetid: 4b34f971-5699-43e7-9b15-137d334fa66e
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 24dd06d812a1234fd9a7458600e71f77cccdcf63
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-a-connection-object"></a>À l’aide d’un objet de connexion
Avant d’ouvrir un **connexion** de l’objet, vous devez définir certaines informations sur la source de données et le type de connexion. La plupart de ces informations est détenue par le *ConnectionString* paramètre de la [Open (méthode)](../../../ado/reference/ado-api/open-method-ado-connection.md) sur la **connexion** objet, ou par le [ConnectionString propriété](../../../ado/reference/ado-api/connectionstring-property-ado.md) sur la **connexion** objet. Une chaîne de connexion se compose d’une liste de paires de valeur d’argument séparés par des points-virgules, avec les valeurs encadrées par des guillemets simples. Par exemple :  
  
```  
Dim sConn As String  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
```  
  
> [!NOTE]
>  Vous pouvez également spécifier un nom de Source de données (DSN) ODBC ou un fichier UDL (Data Link) dans une chaîne de connexion. Pour plus d’informations sur les sources de données, consultez [gestion de Sources de données](../../../odbc/admin/managing-data-sources.md) de référence du programmeur ODBC. Pour plus d’informations sur l’UDL, consultez [Data Link API Overview](http://msdn.microsoft.com/en-us/95c180ea-bd4f-4dca-b95a-576afd135bbc) dans la référence du programmeur OLE DB.  
  
 En règle générale, vous établissez une connexion en appelant le **Connection.Open** méthode avec une une *chaîne de connexion* comme paramètre. Un exemple est illustré dans l’extrait de code Visual Basic suivant :  
  
```  
Dim oConn As ADODB.Connection  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
' Open a connection.  
Set oConn = New ADODB.Connection  
.Open   
  
' Make a query over the connection.  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
Set oRs = New ADODB.Recordset  
oRs.Open sSQL, , adOpenStatic, adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
' Close the connection.  
oConn.Close  
Set oConn = Nothing  
  
```  
  
 Ici **oRs.Open** prend un **connexion** objet (*oConn*) variable comme valeur de sa *ActiveConnection* paramètre. En outre, le **Connection.CursorLocation** propriété suppose que la valeur par défaut **adUseServer**. Cette option pour créer un contraste le [HelloData](../../../ado/guide/data/hellodata-a-simple-ado-application.md) exemple dans la section précédente. L’instruction suivante entraînerait des erreurs d’exécution.  
  
```  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
```
