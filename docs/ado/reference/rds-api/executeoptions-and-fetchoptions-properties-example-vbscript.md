---
title: ExecuteOptions et FetchOptions, propriétés-exemple (VBScript) | Documents Microsoft
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
- FetchOptions property [ADO], VBScript example
- ExecuteOptions property [ADO]
ms.assetid: 753a4a3d-0fba-40b8-86e7-50b34182ca69
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1e848a71bdf1080cf320e9d9255e5a60827ffabc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="executeoptions-and-fetchoptions-properties-example-vbscript"></a>ExecuteOptions et FetchOptions, propriétés-exemple (VBScript)
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Le code suivant montre comment définir la [ExecuteOptions](../../../ado/reference/rds-api/executeoptions-property-rds.md) et [FetchOptions](../../../ado/reference/rds-api/fetchoptions-property-rds.md) propriétés au moment du design. Si définie, **ExecuteOptions** par défaut est **est adcExecSync**. Ce paramètre indique que lorsque la **RDS. Actualiser** méthode est appelée, il sera exécuté sur le thread appelant actuel, autrement dit, de façon synchrone. Coupez et collez le code suivant dans le bloc-notes ou un autre éditeur de texte et enregistrez-le sous **ExecuteOptionsDesignVBS.asp**.  
  
```  
<!-- BeginExecuteOptionsDesignVBS -->  
<%@ Language=VBScript %>  
<html>  
<head>  
    <meta name="VI60_DefaultClientScript"  content=VBScript>  
    <meta name="GENERATOR" content="Microsoft Visual Studio 6.0">  
    <title>Design-time ExecuteOptions and FetchOptions Properties Example</title>  
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
<h2>Design-time <br> ExecuteOptions and FetchOptions Properties Example</h2>  
  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID=RDS height=1 width=1>  
<PARAM NAME="SQL" VALUE="SELECT FirstName, LastName FROM Employees ORDER BY LastName">  
<PARAM NAME="Connect" VALUE="Provider='sqloledb';Data Source=<%=Request.ServerVariables("SERVER_NAME")%>;Integrated Security='SSPI';Initial Catalog='Northwind'">  
<PARAM NAME="Server" VALUE="http://<%=Request.ServerVariables("SERVER_NAME")%>">  
<PARAM NAME="ExecuteOptions" VALUE="1">  
<PARAM NAME="FetchOptions" VALUE="3">  
</OBJECT>  
  
<TABLE DATASRC=#RDS>  
<TBODY>  
   <TR class="thead2">  
      <TH>First Name</TH>  
      <TH>Last Name</TH>  
   </TR>  
   <TR class="tbody">  
     <TD><SPAN DATAFLD="FirstName"></SPAN></TD>  
     <TD><SPAN DATAFLD="LastName"></SPAN></TD>  
   </TR>  
</TBODY>  
</TABLE>  
  
</body>  
</html>  
<!-- EndExecuteOptionsDesignVBS -->  
```  
  
 L’exemple suivant montre comment définir la **ExecuteOptions** et **FetchOptions** des propriétés au moment de l’exécution dans le code VBScript. Consultez le [Actualiser](../../../ado/reference/rds-api/refresh-method-rds.md) méthode pour obtenir un exemple de ces propriétés. Coupez et collez le code suivant dans le bloc-notes ou un autre éditeur de texte et enregistrez-le sous **ExecuteOptionsRuntimeVBS.asp**.  
  
```  
<!-- BeginExecuteOptionsRuntimeVBS -->  
<%@ Language=VBScript %>  
<html>  
<head>  
    <meta name="VI60_DefaultClientScript"  content=VBScript>  
    <meta name="GENERATOR" content="Microsoft Visual Studio 6.0">  
    <title>Run-time ExecuteOptions and FetchOptions Properties Example</title>  
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
<h2>Run-time <br> ExecuteOptions and FetchOptions Properties Example</h2>  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID=RDS height=1 width=1>  
<PARAM NAME="SQL" VALUE="SELECT FirstName, LastName FROM Employees ORDER BY LastName">  
<PARAM NAME="Connect" VALUE="Provider='sqloledb';Data Source=<%=Request.ServerVariables("SERVER_NAME")%>;Integrated Security='SSPI';Initial Catalog='Northwind'">  
<PARAM NAME="Server" VALUE="http://<%=Request.ServerVariables("SERVER_NAME")%>">  
</OBJECT>  
  
<TABLE DATASRC=#RDS>  
<TBODY>  
   <TR class="thead2">  
      <TH>First Name</TH>  
      <TH>Last Name</TH>  
   </TR>  
   <TR class="tbody">  
     <TD><SPAN DATAFLD="FirstName"></SPAN></TD>  
     <TD><SPAN DATAFLD="LastName"></SPAN></TD>  
   </TR>  
</TBODY>  
</TABLE>  
<Script Language="VBScript">  
Const adcExecSync = 1  
Const adcFetchAsynch = 3  
  
Sub ExecuteHow  
  ' set RDS properties at run-time  
  RDS1.ExecuteOptions = adcExecSync  
  RDS1.FetchOptions = adcFetchAsynch  
  RDS.Refresh  
End Sub  
</Script>  
</body>  
</html>  
<!-- EndExecuteOptionsRuntimeVBS -->  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ExecuteOptions, propriété (RDS)](../../../ado/reference/rds-api/executeoptions-property-rds.md)   
 [FetchOptions, propriété (RDS)](../../../ado/reference/rds-api/fetchoptions-property-rds.md)



