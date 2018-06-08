---
title: Scénario de persistance des objets Recordset XML | Documents Microsoft
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
- XML persistence [ADO], persistence scenario
ms.assetid: 353d569a-043a-4397-9ee6-564c4af8d5f6
caps.latest.revision: 4
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0676644c7429d73d551b65fa56c2203025734b38
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-recordset-persistence-scenario"></a>Scénario de persistance des objets Recordset XML
Dans ce scénario, vous allez créer une application Active Server Pages (ASP) qui enregistre le contenu d’un objet Recordset directement dans l’objet Response ASP.  
  
> [!NOTE]
>  Ce scénario nécessite que votre serveur Internet Information Server 5.0 (IIS) ou ultérieur est installé.  
  
 L’objet Recordset retourné est affiché dans Internet Explorer à l’aide un [objet DataControl (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md).  
  
 Les étapes suivantes sont nécessaires pour créer ce scénario :  
  
-   Configurer l’Application  
  
-   Obtenir les données  
  
-   Envoyer les données  
  
-   Recevoir et afficher les données  
  
## <a name="step-1-set-up-the-application"></a>Étape 1 : Configurer l’Application  
 Créer un répertoire virtuel IIS nommé « XMLPersist » avec des autorisations de script. Créez deux nouveaux fichiers texte dans le dossier vers lequel pointe le répertoire virtuel, un nommé « XMLResponse.asp » l’autre nommé « Default.htm. »  
  
## <a name="step-2-get-the-data"></a>Étape 2 : Obtenir les données  
 Dans cette étape, vous allez écrire le code pour ouvrir un jeu d’enregistrements ADO et de préparer son envoi au client. Ouvrez le fichier XMLResponse.asp avec un éditeur de texte, tel que le bloc-notes et insérez le code suivant.  
  
```  
<%@ language="VBScript" %>  
  
<!-- #include file='adovbs.inc' -->  
  
<%  
  Dim strSQL, strCon  
  Dim adoRec   
  Dim adoCon   
  Dim xmlDoc   
  
  ' You will need to change "MySQLServer" below to the name of the SQL   
  ' server machine to which you want to connect.  
  strCon = "Provider=sqloledb;Data Source=MySQLServer;Initial Catalog=Pubs;Integrated Security=SSPI;"  
  Set adoCon = server.createObject("ADODB.Connection")  
  adoCon.Open strCon  
  
  strSQL = "SELECT Title, Price FROM Titles ORDER BY Price"  
  Set adoRec = Server.CreateObject("ADODB.Recordset")  
  adoRec.Open strSQL, adoCon, adOpenStatic, adLockOptimistic, adCmdText  
```  
  
 Veillez à modifier la valeur de la `Data Source` paramètre dans `strCon` au nom de votre ordinateur Microsoft SQL Server.  
  
 Conserver le fichier ouvert et passez à l’étape suivante.  
  
## <a name="step-3-send-the-data"></a>Étape 3 : Envoyer les données  
 Maintenant que vous avez un jeu d’enregistrements, vous devez l’envoyer au client en l’enregistrant au format XML dans l’objet ASP Response. Ajoutez le code suivant au bas de XMLResponse.asp.  
  
```  
  Response.ContentType = "text/xml"  
  Response.Expires = 0  
  Response.Buffer = False  
  
  Response.Write "<?xml version='1.0'?>" & vbNewLine  
  adoRec.save Response, adPersistXML  
  adoRec.Close  
  Set adoRec=Nothing  
%>  
```  
  
 Notez que l’objet Response ASP est spécifié comme destination pour le jeu d’enregistrements [méthode Save](../../../ado/reference/ado-api/save-method.md). La destination de la méthode Save peut être n’importe quel objet qui prend en charge l’interface IStream, telles que ADO [objet de flux de données (ADO)](../../../ado/reference/ado-api/stream-object-ado.md), ou un nom de fichier qui inclut le chemin d’accès complet à laquelle le jeu d’enregistrements doit être enregistré.  
  
 Enregistrez et fermez XMLResponse.asp avant de passer à l’étape suivante. Copiez également le fichier adovbs.inc à partir du dossier d’installation par défaut ADO bibliothèque dans le même dossier où vous avez enregistré le fichier XMLResponse.asp.  
  
## <a name="step-4-receive-and-display-the-data"></a>Étape 4 : Recevoir et afficher les données  
 Dans cette étape, vous allez créer un fichier HTML avec un [objet DataControl (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet qui pointe vers le fichier XMLResponse.asp pour obtenir l’objet Recordset. Ouvrez le fichier default.htm avec un éditeur de texte, tel que le bloc-notes et ajoutez le code suivant. Remplacez « sqlserver » dans l’URL par le nom de votre serveur.  
  
```  
<HTML>  
<HEAD><TITLE>ADO Recordset Persistence Sample</TITLE></HEAD>  
<BODY>  
  
<TABLE DATASRC="#RDC1" border="1">  
  <TR>  
<TD><SPAN DATAFLD="title"></SPAN></TD>  
<TD><SPAN DATAFLD="price"></SPAN></TD>  
  </TR>  
</TABLE>  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="RDC1">  
   <PARAM NAME="URL" VALUE="XMLResponse.asp">  
</OBJECT>  
  
</BODY>  
</HTML>  
```  
  
 Fermez le fichier default.htm et enregistrez-le dans le même dossier où vous avez enregistré XMLResponse.asp. À l’aide d’Internet Explorer 4.0 ou version ultérieure, ouvrez l’URL http://*sqlserver*/XMLPersist/default.htm et observez les résultats. Les données sont affichées dans un tableau DHTML lié. À présent ouvrir l’URL http:// *sqlserver* /XMLPersist/XMLResponse.asp et observez les résultats. Le code XML s’affiche.  
  
## <a name="see-also"></a>Voir aussi  
 [Save (méthode)](../../../ado/reference/ado-api/save-method.md)   
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
