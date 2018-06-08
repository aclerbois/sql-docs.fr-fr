---
title: Flux de données et de persistance | Documents Microsoft
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
- persisted streams [ADO]
- streams [ADO], persistence
ms.assetid: ad5bf52c-fd10-4cfa-bf7d-fcedcaa41eea
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: eb0302de0cc9ac87c55ae0e6c8d44557b517d79d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="streams-and-persistence"></a>Flux de données et de persistance
Le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet [enregistrer](../../../ado/reference/ado-api/save-method.md) méthode magasins, ou *persiste*, un **Recordset** dans un fichier et le [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md)méthode restaure le **Recordset** à partir de ce fichier.  
  
 Avec ADO 2.7 ou version ultérieure, le **enregistrer** et **ouvrir** méthodes peuvent rendre persistante une **Recordset** à un [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet. Cette fonctionnalité est particulièrement utile lorsque vous travaillez avec le Service de données à distance (RDS) et Active Server Pages (ASP).  
  
 Pour plus d’informations sur comment persistance peut être utilisée par lui-même sur les pages ASP, consultez la documentation de ASP actuelle.  
  
 Voici quelques scénarios qui montrent comment **flux** objets et persistance peuvent être utilisés.  
  
## <a name="scenario-1"></a>Scénario 1  
 Ce scénario enregistre simplement un **Recordset** dans un fichier, puis à un **flux**. Il ouvre ensuite le flux persistant dans un autre **Recordset**.  
  
```  
Dim rs1 As ADODB.Recordset  
Dim rs2 As ADODB.Recordset  
Dim stm As ADODB.Stream  
  
Set rs1 = New ADODB.Recordset  
Set rs2 = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
rs1.Open   "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;""", adopenStatic, adLockReadOnly, adCmdText  
rs1.Save "c:\myfolder\mysubfolder\myrs.xml", adPersistXML  
rs1.Save stm, adPersistXML  
rs2.Open stm  
```  
  
## <a name="scenario-2"></a>Scénario 2  
 Ce scénario rend persistant un **Recordset** dans un **flux** au format XML. Il lit ensuite les **flux** dans une chaîne que vous pouvez examiner, manipuler ou afficher.  
  
```  
Dim rs As ADODB.Recordset  
Dim stm As ADODB.Stream  
Dim strRst As String  
  
Set rs = New ADODB.Recordset  
Set stm = New ADODB.Stream  
  
' Open, save, and close the recordset.   
rs.Open "SELECT * FROM Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
rs.Save stm, adPersistXML  
rs.Close  
Set rs = nothing  
  
' Put saved Recordset into a string variable.  
strRst = stm.ReadText(adReadAll)  
  
' Examine, manipulate, or display the XML data.  
...  
```  
  
## <a name="scenario-3"></a>Scénario 3  
 Cet exemple de code montre la persistance de code ASP un **Recordset** au format XML directement à la **réponse** objet :  
  
```  
...  
<%  
response.ContentType = "text/xml"  
  
' Create and open a Recordset.  
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select * from Customers", "Provider=sqloledb;" & _  
        "Data Source=MyServer;Initial Catalog=Northwind;" & _  
        "Integrated Security=SSPI;"""  
  
' Save Recordset directly into output stream.  
rs.Save Response, adPersistXML   
  
' Close Recordset.  
rs.Close  
Set rs = nothing  
%>  
...  
```  
  
## <a name="scenario-4"></a>Scénario 4  
 Dans ce scénario, le code ASP écrit le contenu de la **Recordset** au format ADTG au client. Le [le Service de curseur Microsoft pour OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) peut utiliser ces données pour créer un déconnecté **Recordset**.  
  
 Une nouvelle propriété sur le Bureau à distance [DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md), [URL](../../../ado/reference/rds-api/url-property-rds.md), pointe vers la page .asp qui génère le **Recordset**. Cela signifie une **Recordset** objet peut être obtenu sans les services Bureau à distance à l’aide du côté serveur [DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet ou l’utilisateur de l’écriture d’un objet métier. Cela simplifie considérablement le modèle de programmation des services Bureau à distance.  
  
 Nommé de code côté serveur http://server/directory/recordset.asp:  
  
```  
<%  
Dim rs   
Set rs = Server.CreateObject("ADODB.Recordset")  
rs.Open "select au_fname, au_lname, phone from Authors", ""& _  
        "Provider=sqloledb;Data Source=MyServer;" & _  
        "Initial Catalog=Pubs;Integrated Security=SSPI;"  
response.ContentType = "multipart/mixed"  
rs.Save response, adPersistADTG  
%>  
```  
  
 Code côté client :  
  
```  
<HTML>  
<HEAD>  
<TITLE>RDS Query Page</TITLE>  
</HEAD>  
<body>  
<CENTER>  
<H1>Remote Data Service 2.5</H1>  
<TABLE DATASRC="#DC1">  
   <TR>   
      <TD><SPAN DATAFLD="au_fname"></SPAN></TD>  
      <TD><SPAN DATAFLD="au_lname"></SPAN></TD>  
      <TD><SPAN DATAFLD="phone"></SPAN></TD>  
   </TR>  
</TABLE>  
<BR>  
  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
    ID=DC1 HEIGHT=1 WIDTH = 1>  
    <PARAM NAME="URL" VALUE="http://server/directory/recordset.asp">  
</OBJECT>  
  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 Les développeurs ont également la possibilité d’utiliser un **Recordset** objet sur le client :  
  
```  
...  
function GetRs()   
    {  
    rs = CreateObject("ADODB.Recordset");  
    rs.Open "http://server/directory/recordset.asp"  
    DC1.SourceRecordset = rs;  
    }  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Objet d’enregistrement (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Save, méthode](../../../ado/reference/ado-api/save-method.md)
