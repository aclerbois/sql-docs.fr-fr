---
title: DataSpace et la méthode CreateObject exemple (VBScript) | Documents Microsoft
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
- DataSpace object [RDS], VBScript example
- CreateObject method [ADO], VBScript example
ms.assetid: 12b0e160-5e5c-441f-bed7-ac0bd061e003
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: aea09da62ef5d9f60881c6914bf8ac7181c44b20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dataspace-object-and-createobject-method-example-vbscript"></a>DataSpace et la méthode CreateObject exemple (VBScript)
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 L’exemple suivant montre comment utiliser le [CreateObject](../../../ado/reference/rds-api/createobject-method-rds.md) méthode de la [RDS. DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md) avec l’objet métier par défaut, [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md). Pour tester cet exemple, coupez et collez ce code entre les \<corps > et \</corps > balises dans un code HTML normal de document et nommez-le **DataSpaceVBS.asp**. Le script ASP identifie votre serveur.  
  
```  
<!-- BeginDataSpaceVBS -->  
<html>  
<head>  
<!--use the following META tag instead of adovbs.inc-->  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
    <meta name="VI60_DefaultClientScript"  content=VBScript>  
    <meta name="GENERATOR" content="Microsoft Visual Studio 6.0">  
    <title>DataSpace Object and CreateObject Method Example (VBScript)</title>  
<style>  
<!--  
body {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
.thead {  
   background-color: #008080;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.thead2 {  
   background-color: #800000;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
    }  
-->  
</style>  
</head>  
  
<body>  
<h1>DataSpace Object and CreateObject Method Example (VBScript)</h1>  
  
<H2>RDS API Code Examples</H2>  
<HR>  
<H3>Using Query Method of RDSServer.DataFactory</H3>  
  
<!-- RDS.DataSpace  ID rdsDS-->  
<OBJECT ID="rdsDS" WIDTH=1 HEIGHT=1  
CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36">  
</OBJECT>  
  
<!-- RDS.DataControl with parameters set at run time -->  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=RDS WIDTH=1 HEIGHT=1>  
</OBJECT>  
  
<TABLE DATASRC=#RDS>  
<TBODY>  
  <TR>  
    <TD><SPAN DATAFLD="FirstName"></SPAN></TD>  
    <TD><SPAN DATAFLD="LastName"></SPAN></TD>  
  </TR>  
</TBODY>  
</TABLE>  
  
<HR>  
<INPUT TYPE=BUTTON NAME="Run" VALUE="Run">  
  
<H4>Click Run -  
The <i>CreateObject</i> Method of the RDS.DataSpace Object Creates an instance of the RDSServer.DataFactory.  
The <i>Query</i> Method of the RDSServer.DataFactory is used to bring back a Recordset.</H4>  
  
<Script Language="VBScript">  
  
    Dim rdsDF  
    Dim strServer  
    Dim strCnxn  
    Dim strSQL  
  
    strServer = "http://<%=Request.ServerVariables("SERVER_NAME")%>"  
    strCnxn = "Provider='sqloledb';Data Source=" & _  
            "<%=Request.ServerVariables("SERVER_NAME")%>" & ";" & _  
            "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    strSQL = "Select FirstName, LastName from Employees"  
  
    Sub Run_OnClick()  
  
       Dim rs        
        ' Create Data Factory  
       Set rdsDF = rdsDS.CreateObject("RDSServer.DataFactory", strServer)  
        'Get Recordset    
       Set rs = rdsDF.Query(strCnxn, strSQL)     
       ' Use  RDS.DataControl to bind Recordset to data-aware Table above  
       RDS.SourceRecordset = rs  
  
    End Sub  
</Script>  
  
</body>  
</html>  
<!-- EndDataSpaceVBS -->  
```  
  
 L’exemple suivant montre comment utiliser le **CreateObject** méthode pour créer une instance d’un objet métier personnalisé, VbBusObj.VbBusObjCls. Il utilise également le script pour identifier le nom du serveur Web Active Server Pages.  
  
 Pour voir un exemple complet, ouvrez le sélecteur d’applications exemple. Dans le **couche cliente** colonne, sélectionnez **VBScript dans Internet Explorer**. Dans le **intermédiaire** colonne, sélectionnez **Custom Visual Basic Business Object**.  
  
> [!NOTE]
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = yes** ou **Integrated Security = SSPI** au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.  
  
```  
Sub Window_OnLoad()  
   strServer = "http://<%=Request.ServerVariables("SERVER_NAME")%>"  
   Set BO = ADS1.CreateObject("VbBusObj.VbBusObjCls", strServer)  
   txtConnect.Value = "dsn=Pubs;uid=MyUserID;pwd=MyPassword;"  
   txtGetRecordset.Value = "Select * From authors for Browse"  
End Sub  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CreateObject (méthode) (RDS)](../../../ado/reference/rds-api/createobject-method-rds.md)   
 [DataSpace, objet (RDS)](../../../ado/reference/rds-api/dataspace-object-rds.md)


