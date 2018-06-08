---
title: Append et CreateParameter, méthodes-exemple (JScript) | Documents Microsoft
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
- JScript
helpviewer_keywords:
- CreateParameter method [ADO], JScript example
- Append method [ADO], JScript example
ms.assetid: 37000833-68f4-45f1-b2dd-7f75893d09d9
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d5b7a1e196dd452b5bf6dd407c6dc7e10d237497
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="append-and-createparameter-methods-example-jscript"></a>Append et CreateParameter, méthodes-exemple (JScript)
Cet exemple utilise le [Append](../../../ado/reference/ado-api/append-method-ado.md) et [CreateParameter](../../../ado/reference/ado-api/createparameter-method-ado.md) méthodes pour exécuter une procédure stockée avec un paramètre d’entrée. Coupez et collez le code suivant dans le bloc-notes ou un autre éditeur de texte et enregistrez-le sous **nom AppendJS.asp**.  
  
```  
<!-- BeginAppendJS -->  
<%@LANGUAGE="JScript" %>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<html>  
<head>  
    <title>Append and CreateParameter Methods Example (JScript)</title>  
<style>  
<!--  
body {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
-->  
</style>  
</head>  
  
<body>  
<h1>Append and CreateParameter Methods Example (JScript)</h1>  
<%  
    // verify user-input   
    var iRoyalty = parseInt(Request.Form("RoyaltyValue"));  
    if (iRoyalty > -1)  
    {  
  
        // connection, recordset and command variables  
        var strCnxn = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='pubs';Integrated Security='SSPI';";  
        var Cnxn = Server.CreateObject("ADODB.Connection");  
        var cmdByRoyalty = Server.CreateObject("ADODB.Command");  
        var rsByRoyalty = Server.CreateObject("ADODB.Recordset");  
        var rsAuthor = Server.CreateObject("ADODB.Recordset");  
        // display variables  
        var strMessage;  
  
        try  
        {  
            // open connection and set cursor location  
            Cnxn.Open(strCnxn);  
            Cnxn.CursorLocation = adUseClient;  
  
            // command object initial parameters  
            cmdByRoyalty.CommandText = "byroyalty";  
            cmdByRoyalty.CommandType = adCmdStoredProc;  
  
            // create the new parameter and append to  
            // the Command object's parameters collection  
            var prmByRoyalty = cmdByRoyalty.CreateParameter("percentage", adInteger, adParamInput);  
            cmdByRoyalty.Parameters.Append(prmByRoyalty);  
            prmByRoyalty.Value = iRoyalty;  
  
            cmdByRoyalty.ActiveConnection = Cnxn;  
  
            // execute command  
            rsByRoyalty = cmdByRoyalty.Execute();  
  
            // display results  
            rsAuthor.Open("Authors", Cnxn);  
  
            while (!rsByRoyalty.EOF)  
            {  
                rsAuthor.Filter = "au_id='" + rsByRoyalty.Fields("au_id") + "'";  
  
                // start new line  
                strMessage = "<P>";  
  
                // recordset data  
                strMessage += rsAuthor.Fields("au_fname") + " ";   
                strMessage += rsAuthor.Fields("au_lname") + " ";  
  
                // end the line  
                strMessage += "</P>";  
  
                // show result  
                Response.Write(strMessage);  
  
                // et next record  
                rsByRoyalty.MoveNext;  
            }  
        }  
        catch (e)  
        {  
            Response.Write(e.message);  
        }  
        finally  
        {  
            // clean up  
            if (rsByRoyalty.State == adStateOpen)  
                rsByRoyalty.Close;  
            if (rsAuthor.State == adStateOpen)  
                rsAuthor.Close;  
            if (Cnxn.State == adStateOpen)  
                Cnxn.Close;  
            rsByRoyalty = null;  
            rsAuthor = null;  
            Cnxn = null;  
        }  
    }  
%>  
  
<hr>  
  
<form method="POST" action="AppendJS.asp" id=form1 name=form1>  
  <p align="left">Enter royalty percentage to find (e.g., 40): <input type="text" name="RoyaltyValue" size="5"></p>  
  <p align="left"><input type="submit" value="Submit" name="B1"><input type="reset" value="Reset" name="B2"></p>  
</form>  
  
</body>  
  
</html>  
<!-- EndAppendJS -->  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Append (méthode) (ADO)](../../../ado/reference/ado-api/append-method-ado.md)   
 [CreateParameter, méthode (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)   
 [Objet Field](../../../ado/reference/ado-api/field-object.md)   
 [Collection de champs (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)   
 [Parameter, objet](../../../ado/reference/ado-api/parameter-object.md)
