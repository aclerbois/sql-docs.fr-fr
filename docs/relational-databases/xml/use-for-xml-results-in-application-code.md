---
title: Utiliser des résultats FOR XML dans le code de l’application | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, application code usage
- XML [SQL Server], FOR XML clause
- ASP.NET [SQL Server]
- .NET Framework [SQL Server], FOR XML data
- ADO [SQL Server]
- XML data islands [SQL Server]
- data islands [SQL Server]
ms.assetid: 41ae67bd-ece9-49ea-8062-c8d658ab4154
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7a8bbdb0f98dea26d73ec465d6fd9e226329345a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-for-xml-results-in-application-code"></a>Utiliser des résultats FOR XML dans le code de l'application
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  En utilisant des clauses FOR XML avec des requêtes SQL, vous pouvez récupérer et même convertir les résultats de la requête en données XML. Dès lors que les résultats d'une requête FOR XML peuvent être utilisés dans le code de l'application XML, vous pouvez notamment effectuer les opérations suivantes :  
  
-   Interroger des tables SQL pour des instances de valeurs de [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
-   Appliquer la [TYPE Directive in FOR XML Queries](../../relational-databases/xml/type-directive-in-for-xml-queries.md) pour renvoyer les résultats des requêtes contenant des données de type texte ou image au format XML  
  
 Cette rubrique donne des exemples expliquant ces approches.  
  
## <a name="retrieving-for-xml-data-with-ado-and-xml-data-islands"></a>Récupération des données FOR XML avec des îlots de données ADO et XML  
 L’objet ADO **Stream** ou d’autres objets prenant en charge l’interface COM **IStream** , tels que les objets Active Server Pages (ASP) **Request** et **Response** , peuvent servir à contenir les résultats en cas d’utilisation de requêtes FOR XML.  
  
 Le code ASP suivant, par exemple, montre les résultats d'une requête lancée sur la colonne de type **xml** Demographics de la table Sales.Store de la base de données AdventureWorks. La requête recherche plus particulièrement la valeur d'instance de cette colonne pour la ligne où CustomerID est égal à 3.  
  
```  
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<!-- %  Option Explicit      % -->  
<!-- 'Request.ServerVariables("SERVER_NAME") & ";" & _ -->  
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
   sConn = "Provider=SQLOLEDB;Data Source=(local);" & _  
            "Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
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
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO</sql:query></ROOT>"  
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
   Dim root  
   Set root = xmlDoc.documentElement.childNodes.Item(0).childNodes.Item(0).childNodes.Item(0)  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI><B>" & child.nodeName &  ":</B>  " & child.Text  & "</LI>"  
   Next  
   MsgBox xmlDoc.xml  
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
  
 Cet exemple de page ASP contient le code VBScript côté serveur qui utilise ADO pour exécuter la requête FOR XML et renvoyer les résultats XML dans l'îlot de données XML MyDataIsle. Cet îlot de données XML est ensuite renvoyé dans le navigateur en vue d'un traitement supplémentaire côté client. Côté client, le code VBScript supplémentaire sert ensuite à traiter le contenu de l'îlot de données XML. Ce processus a lieu avant l'affichage du contenu sous forme DHTML et avant l'ouverture d'une boîte de message afin de montrer le contenu prétraité de l'îlot de données XML.  
  
#### <a name="to-test-this-example"></a>Pour tester cet exemple  
  
1.  Vérifiez que les services Internet (IIS) sont installés et que l'exemple de base de données AdventureWorks a bien été installé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Cet exemple requiert l'installation de Internet Information Services (IIS) version 5.0 ou ultérieure et l'activation de la prise en charge ASP. De plus, l'exemple de base de données AdventureWorks doit être installé.  
  
2.  Copiez l'exemple de code précédemment fourni et collez-le dans l'éditeur XML ou de texte que vous utilisez. Enregistrez le fichier sous RetrieveResults.asp dans le répertoire racine utilisé pour IIS. Il s'agit généralement de C:Inetpub\wwwroot.  
  
3.  Ouvrez la page ASP dans une fenêtre du navigateur en utilisant l'URL qui suit. Premièrement, remplacez « MyServer » par « localhost » ou par le nom réel du serveur sur lequel résident [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les services Internet (IIS).  
  
    ```  
    http://MyServer/RetrieveResults.asp  
    ```  
  
 La page HTML générée qui en résulte et qui apparaît ressemblera à l'exemple de sortie suivant :  
  
##### <a name="server-side-processing"></a>Traitement côté serveur  
 Page Generated @ 3/11/2006 3:36:02 PM  
  
 Connect String = Provider=SQLOLEDB;Data Source=MyServer;Initial Catalog=AdventureWorks;Integrated Security=SSPI;  
  
 ADO Version = 2.8  
  
 adoConn.State = 1  
  
 Query String = SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO  
  
 Envoi du code XML au client en vue du traitement  
  
##### <a name="client-side-processing-of-xml-document-mydataisle"></a>Traitement côté client du document XML MyDataIsle  
  
-   **AnnualSales:** 1500000  
  
-   **AnnualRevenue:** 150000  
  
-   **BankName:** Primary International  
  
-   **BusinessType:** OS  
  
-   **YearOpened:** 1974  
  
-   **Specialty:** Road  
  
-   **SquareFeet:** 38000  
  
-   **Brands:** 3  
  
-   **Internet:** DSL  
  
-   **NumberEmployees:** 40  
  
 La boîte de message VBScript affichera ensuite le contenu original et non filtré de l'îlot de données XML qui a été renvoyé par les résultats de la requête FOR XML.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Sales.Store>  
    <Demographics>  
      <StoreSurvey xmlns="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/StoreSurvey">  
        <AnnualSales>1500000</AnnualSales>  
        <AnnualRevenue>150000</AnnualRevenue>  
        <BankName>Primary International</BankName>  
        <BusinessType>OS</BusinessType>  
        <YearOpened>1974</YearOpened>  
        <Specialty>Road</Specialty>  
        <SquareFeet>38000</SquareFeet>  
        <Brands>3</Brands>  
        <Internet>DSL</Internet>  
        <NumberEmployees>40</NumberEmployees>  
      </StoreSurvey>  
    </Demographics>  
  </Sales.Store>  
</ROOT>  
```  
  
## <a name="retrieving-for-xml-data-with-aspnet-and-the-net-framework"></a>Récupération des données FOR XML avec ASP.NET et .NET Framework  
 Comme dans l'exemple précédent, le code ASP suivant montre les résultats d'une requête lancée sur une colonne de type **xml** , Demographics, de la table Sales.Store de la base de données AdventureWorks. Comme dans l'exemple précédent, la requête recherche plus particulièrement la valeur d'instance de cette colonne pour la ligne où CustomerID est égal à 3.  
  
 Dans cet exemple, les API gérées Microsoft .NET Framework sont chargées de renvoyer et de rendre les résultats de la requête FOR XML :  
  
1.  **SqlConnection** est utilisé pour ouvrir une connexion à SQL Server en fonction du contenu d'une variable de chaîne de connexion spécifiée strConn.  
  
2.  **SqlDataAdapter** est ensuite utilisé en tant qu'adaptateur de données et utilise la connexion SQL et une chaîne de requête SQL spécifiée pour exécuter la requête FOR XML.  
  
3.  Une fois la requête exécutée, la méthode **SqlDataAdapter.Fill** est appelée et reçoit une instance d'un **DataSet,** MyDataSet, afin de remplir le dataset avec le résultat de la requête FOR XML.  
  
4.  La méthode **DataSet.GetXml** est alors appelée pour retourner les résultats de la requête sous forme de chaîne à afficher dans la page HTML générée par le serveur.  
  
    ```  
    <%@ Page Language="VB" %>  
    <HTML>  
    <HEAD>  
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
    </HEAD>  
    <BODY>  
    <%  
    Dim s as String  
    s = "<H3>Server-side processing</H3>" & _  
        "Page Generated @ " & Now() & "<BR/>"  
  
    Dim SQL As String   
    SQL = "SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO"  
  
    Dim strConn As String   
    strConn = "Server=(local);Database=AdventureWorks;Integrated Security=SSPI;"  
  
    Dim MySqlConn As New System.Data.SqlClient.SqlConnection(strConn)  
    Dim MySqlAdapter As New System.Data.SqlClient.SqlDataAdapter(SQL,MySqlConn)  
    Dim MyDataSet As New System.Data.DataSet  
  
    MySqlConn.Open()  
    s = s & "<P>SqlConnection opened.</P>"   
  
    MySqlAdapter.Fill(MyDataSet)  
    s = s & "<P>" & MyDataSet.GetXml  & "</P>"  
  
    MySqlConn.Close()  
    s = s & "<P>SqlConnection closed.</P>"   
  
    Message.InnerHtml=s  
    %>  
    <SPAN id="Message" runat=server />  
    </BODY>  
    </HTML>  
    ```  
  
#### <a name="to-test-this-example"></a>Pour tester cet exemple  
  
1.  Vérifiez que les services Internet (IIS) sont installés et que l'exemple de base de données AdventureWorks a bien été installé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Cet exemple requiert l'installation de Internet Information Services (IIS) version 5.0 ou ultérieure et l'activation de la prise en charge ASP.NET. De plus, l'exemple de base de données AdventureWorks doit être installé.  
  
2.  Copiez le code précédemment fourni et collez-le dans l'éditeur XML ou de texte que vous utilisez. Enregistrez le fichier sous RetrieveResults.aspx dans le répertoire racine utilisé pour IIS. Il s'agit généralement de C:Inetpub\wwwroot.  
  
3.  Ouvrez la page ASP.NET dans une fenêtre du navigateur en utilisant l'URL qui suit. Premièrement, remplacez « MyServer » par « localhost » ou par le nom réel du serveur sur lequel résident [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les services Internet (IIS).  
  
    ```  
    http://MyServer/RetrieveResults.aspx  
    ```  
  
 La page HTML générée qui en résulte et qui apparaît ressemblera à l'exemple de sortie suivant :  
  
##### <a name="server-side-processing"></a>Traitement côté serveur  
  
```  
Page Generated @ 3/11/2006 3:36:02 PM  
  
SqlConnection opened.  
  
<Sales.Store><Demographics><StoreSurvey xmlns="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/StoreSurvey"><AnnualSales>1500000</AnnualSales><AnnualRevenue>150000</AnnualRevenue><BankName>Primary International</BankName><BusinessType>OS</BusinessType><YearOpened>1974</YearOpened><Specialty>Road</Specialty><SquareFeet>38000</SquareFeet><Brands>3</Brands><Internet>DSL</Internet><NumberEmployees>40</NumberEmployees></StoreSurvey></Demographics></Sales.Store>  
  
SqlConnection closed.  
```  
  
> [!NOTE]  
>  La méthode [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**xml** vous permet de demander que le résultat d’une requête FOR XML soit retourné avec le type de données **xml** plutôt que sous forme de données de type chaîne ou image ; pour cela, il vous suffit de spécifier la [directive TYPE](../../relational-databases/xml/type-directive-in-for-xml-queries.md). L’emploi d’une directive TYPE dans les requêtes FOR XML donne automatiquement accès à des résultats FOR XML très similaires à ceux qui sont présentés dans [Utiliser des données XML dans les applications](../../relational-databases/xml/use-xml-data-in-applications.md).  
  
## <a name="see-also"></a> Voir aussi  
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
