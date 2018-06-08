---
title: Programmation ADO en JScript | Documents Microsoft
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
- JScript programming in ADO
- ADO, JScript programming
ms.assetid: 62273658-0fe7-4aac-b4d8-f725e6baf043
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ec620ba868a6a72af224b4fc17d0339936d8b48c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="jscript-ado-programming"></a>Programmation ADO en JScript
## <a name="creating-an-ado-project"></a>Création d’un projet ADO  
 Microsoft JScript ne prend pas en charge les bibliothèques de types, vous n’avez pas besoin de la référence ADO dans votre projet. Par conséquent, aucune fonctionnalité associée comme fin de ligne de commande n’est pris en charge. En outre, par défaut, les constantes énumérées ADO ne sont pas définies dans JScript.  
  
 En revanche, ADO comporte deux fichiers contenant les définitions suivantes à utiliser avec JScript :  
  
-   Utilisation de script côté serveur Adojavas.inc, qui est installé dans les dossiers de la bibliothèque ADO.  
  
-   Côté client, utilisez Adcjavas.inc script, lequel est installé dans les dossiers de la bibliothèque ADO.  
  
 Vous pouvez copier et coller les définitions de constante à partir de ces fichiers dans vos pages ASP ou, si vous effectuez le script côté serveur, copiez le fichier Adojavas.inc dans un dossier sur votre site Web et y fait référence à partir de votre page ASP comme suit :  
  
```  
<!--#include File="adojavas.inc"-->  
```  
  
## <a name="creating-ado-objects-in-jscript"></a>Création d’objets ADO en JScript  
 Vous devez utiliser le **CreateObject** l’appel de fonction :  
  
```  
var Rs1;  
Rs1 = Server.CreateObject("ADODB.Recordset");  
```  
  
## <a name="jscript-example"></a>Exemple JScript  
 Le code suivant est un exemple générique de programmation côté serveur en JScript dans un fichier de Page ASP (Active Server) qui ouvre un **Recordset** objet :  
  
```  
<%  @LANGUAGE="JScript" %>  
<!--#include File="adojavas.inc"-->  
<HTML>  
<BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
<%  
var Source = "SELECT * FROM Authors";  
var Connect =  "Provider=sqloledb;Data Source=srv;" +  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
var Rs1 = Server.CreateObject( "ADODB.Recordset.2.5" );  
Rs1.Open(Source,Connect,adOpenForwardOnly);  
Response.Write("Success!");  
%>  
</BODY>  
</HTML>  
```
