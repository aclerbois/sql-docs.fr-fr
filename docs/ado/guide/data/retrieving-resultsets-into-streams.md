---
title: La récupération des jeux de résultats en flux | Documents Microsoft
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
- streams [ADO], retrieving query results
- query results into stream [ADO]
- retrieving results into stream [ADO]
ms.assetid: 996c1321-c926-4f57-8297-85c8c20de974
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cc99c234d810aef48f4c01bdc83229e55d9c5886
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieving-resultsets-into-streams"></a>La récupération des jeux de résultats dans le flux de données
Au lieu de recevoir les résultats dans traditionnel **Recordset** objet ADO peut récupérer à la place les résultats de la requête dans un flux de données. ADO **flux** objet (ou d’autres objets qui prennent en charge le modèle COM **IStream** interface, telles que ASP **demande** et **réponse** objets ) peut être utilisé pour contenir ces résultats. Une utilisation de cette fonctionnalité consiste à récupérer les résultats au format XML. Avec SQL Server, par exemple, les résultats XML peuvent être retournés de plusieurs façons, par exemple à l’aide de la clause FOR XML avec une requête SQL SELECT ou une requête XPath.  
  
 Pour recevoir les résultats de la requête au format de flux de données plutôt que dans un **Recordset**, vous devez spécifier le **adExecuteStream** constante de **ExecuteOptionEnum** en tant que paramètre de la **Execute** méthode d’un **commande** objet. Si votre fournisseur prend en charge cette fonctionnalité, les résultats sont retournés dans un flux de données lors de l’exécution. Il se peut que vous pouvez être amené à spécifier des propriétés supplémentaires spécifiques au fournisseur avant que le code s’exécute. Par exemple, avec le fournisseur Microsoft OLE DB pour SQL Server, propriétés, telles que **le flux de sortie** dans les **propriétés** collection de la **commande** l’objet doit être spécifié. Pour plus d’informations sur les propriétés dynamiques spécifiques à SQL Server associé à cette fonctionnalité, consultez les propriétés XML-Related dans la documentation en ligne de SQL Server.  
  
## <a name="for-xml-query-example"></a>PAR exemple de requête XML  
 L’exemple suivant est écrit en VBScript, la base de données Northwind :  
  
```  
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<%  Option Explicit      %>  
  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Developer Studio"/>  
<META HTTP-EQUIV="Content-Type" content="text/html"; charset="iso-8859-1">  
<TITLE>FOR XML Query Example</TITLE>  
  
<STYLE>  
   BODY  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
  
   H3  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
</STYLE>  
  
<!-- #include file="adovbs.inc" -->  
<%  
   Response.Write "<H3>Server-side processing</H3>"  
  
   Response.Write "Page Generated @ " & Now() & "<BR/>"  
  
   Dim adoConn  
   Set adoConn = Server.CreateObject("ADODB.Connection")  
  
   Dim sConn  
   sConn = "Provider=SQLOLEDB;Data Source=" & _  
      Request.ServerVariables("SERVER_NAME") & ";" & _  
      Initial Catalog=Northwind;Integrated Security=SSPI;"  
  
   Response.write "Connect String = " & sConn & "<BR/>"  
  
   adoConn.ConnectionString = sConn  
   adoConn.CursorLocation = adUseClient  
  
   adoConn.Open  
  
   Response.write "ADO Version = " & adoConn.Version & "<BR/>"  
   Response.write "adoConn.State = " & adoConn.State & "<BR/>"  
  
   Dim adoCmd  
   Set adoCmd = Server.CreateObject("ADODB.Command")  
   Set adoCmd.ActiveConnection = adoConn  
  
   Dim sQuery  
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT * FROM PRODUCTS WHERE ProductName='Gumbr Gummibrchen' FOR XML AUTO</sql:query></ROOT>"  
  
   Response.write "Query String = " & sQuery & "<BR/>"  
  
   Dim adoStreamQuery  
   Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
   adoStreamQuery.Open  
   adoStreamQuery.WriteText sQuery, adWriteChar  
   adoStreamQuery.Position = 0  
  
   adoCmd.CommandStream = adoStreamQuery  
   adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
  
   Response.write "Pushing XML to client for processing "  & "<BR/>"  
  
   adoCmd.Properties("Output Stream") = Response  
   Response.write "<XML ID='MyDataIsle'>"  
   adoCmd.Execute , , 1024  
   Response.write "</XML>"  
  
%>  
  
<SCRIPT language="VBScript" For="window" Event="onload">  
   Dim xmlDoc  
   Set xmlDoc = MyDataIsle.XMLDocument  
   xmlDoc.resolveExternals=false  
   xmlDoc.async=false  
  
   If xmlDoc.parseError.Reason <> "" then  
      Msgbox "parseError.Reason = " & xmlDoc.parseError.Reason  
   End If  
  
   Dim root, child  
   Set root = xmlDoc.documentElement  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI>" & child.getAttribute("ProductName") & "</LI>"  
   Next  
</SCRIPT>  
  
</HEAD>  
  
<BODY>  
  
   <H3>Client-side processing of XML Document MyDataIsle</H3>  
   <UL id=log>  
   </UL>  
  
</BODY>  
</HTML>  
<!-- EndRecordAndStreamVBS -->  
  
```  
  
 La clause FOR XML fait en sorte que SQL Server pour retourner des données sous la forme d’un document XML.  
  
### <a name="for-xml-syntax"></a>POUR la syntaxe XML  
  
```  
FOR XML [RAW|AUTO|EXPLICIT]  
```  
  
 POUR le XML brut génère des éléments de ligne génériques qui ont des valeurs de colonne en tant qu’attributs. FOR XML AUTO utilise l’heuristique pour générer une arborescence hiérarchique avec des noms d’élément basés sur les noms de table. FOR XML EXPLICIT génère une table universelle avec des relations décrites par les métadonnées.  
  
 Voici un exemple instruction SQL SELECT FOR XML :  
  
```  
SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO  
```  
  
 La commande peut être spécifiée dans une chaîne, comme indiqué précédemment, assigné à **CommandText**, ou sous la forme d’une requête de modèle XML affectée à **CommandStream**. Pour plus d’informations sur les requêtes de modèle XML, consultez [commande flux](../../../ado/guide/data/command-streams.md) ADO ou à l’aide de flux de données pour l’entrée de commande dans la documentation en ligne de SQL Server.  
  
 Une requête de modèle XML, la requête FOR XML s’affiche comme suit :  
  
```  
<sql:query> SELECT * FROM PRODUCTS ORDER BY PRODUCTNAME FOR XML AUTO </sql:query>  
```  
  
 Cet exemple spécifie ASP **réponse** de l’objet pour le **le flux de sortie** propriété :  
  
```  
adoCmd.Properties("Output Stream") = Response  
```  
  
 Ensuite, spécifiez **adExecuteStream** paramètre de **Execute**. Cet exemple encapsule le flux dans les balises XML pour créer un îlot de données XML :  
  
```  
Response.write "<XML ID=MyDataIsle>"  
adoCmd.Execute , , adExecuteStream  
Response.write "</XML>"  
```  
  
### <a name="remarks"></a>Notes  
 À ce stade, XML a été transmis au navigateur client et il est prêt à être affiché. Cela permet à l’aide de code VBScript côté client pour lier le document XML à une instance de DOM et exécuter une boucle dans chaque nœud enfant pour générer une liste de produits au format HTML.
