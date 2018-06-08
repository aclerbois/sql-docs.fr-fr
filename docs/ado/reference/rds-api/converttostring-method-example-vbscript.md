---
title: Exemple de méthode ConvertToString (VBScript) | Documents Microsoft
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
- ConvertToString method [ADO], VBScript example
ms.assetid: edd0a01c-1a1b-4b91-9966-2529e244abae
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 56a067cd5e58120d2dbcbd21a821810d6d773961
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="converttostring-method-example-vbscript"></a>Exemple de méthode ConvertToString (VBScript)
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 L’exemple suivant montre comment convertir un **Recordset** dans une chaîne codée MIME à l’aide de la **RDSServer.DataFactory ConvertToString** (méthode). Il montre ensuite comment la chaîne peut être convertie en un **Recordset**. Coupez et collez le code suivant dans le bloc-notes ou un autre éditeur de texte et enregistrez-le sous **nom ConvertToString.htm**.  
  
```  
<!-- BeginConvertToStringVBS -->  
<HTML>  
<HEAD><TITLE>ConvertToString Example</TITLE><HEAD>  
<BODY>  
  
<SCRIPT LANGUAGE=VBSCRIPT>  
Sub ConvertToStringX()  
    Dim objRs, objDF, strServer, vString  
    Const adcExecSync = 1  
    Const adcFetchUpFront = 1  
  
    ' Replace value below with your server name to use without ASP.  
    strServer = "http://<%=Request.ServerVariables("SERVER_NAME")%>">  
  
    Set objDF = RDS1.CreateObject("RDSServer.DataFactory", strServer)  
    Set objRs = objDF.Query(txtConnect.Value,txtQueryRecordset.Value)  
  
    ' convert Recordset to MIME encoded string  
    vString = objDF.ConvertToString(objRs)  
  
    ' display MIME string for demo purposes  
    txtRS.value = vString  
  
    ' convert MIME string back to useable ADO Recordset   
    ' using RDS.DataControl  
    RDC1.SQL = vString  
  
    RDC1.ExecuteOptions = adcExecSync  
    RDC1.FetchOptions = adcFetchUpFront  
    RDC1.Refresh  
  
    MsgBox "RecordCount = " & RDC1.Recordset.RecordCount  
End Sub   
</SCRIPT>  
  
Connect String:   
 <INPUT TYPE=Text NAME=txtConnect SIZE=50   
    VALUE="Provider=sqloledb;Initial Catalog=pubs;Integrated Security='SSPI';">   
 <BR>  
  
Query:   
 <INPUT TYPE=Text NAME=txtQueryRecordset SIZE=50   
    VALUE="select * from authors">   
 <BR>  
  
 <INPUT TYPE=Button VALUE="ConvertToString" OnClick="ConvertToStringX()">  
 <BR>  
  
MIME Encoded RS: <BR>  
 <TEXTAREA NAME=txtRS ROWS=15 COLS=50 WRAP=virtual></TEXTAREA>  
  
<!-- RDS.DataSpace  ID RDS1 -->  
 <OBJECT ID="RDS1" WIDTH=1 HEIGHT=1  
     CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36">  
 </OBJECT>  
  
<!-- RDS.DataControl ID RDC1 -->  
 <OBJECT ID="RDC1" WIDTH=1 HEIGHT=1   
     CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33">  
 </OBJECT>  
</BODY>  
</HTML>  
<!-- EndConvertToStringVBS -->  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ConvertToString, méthode (RDS)](../../../ado/reference/rds-api/converttostring-method-rds.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)




