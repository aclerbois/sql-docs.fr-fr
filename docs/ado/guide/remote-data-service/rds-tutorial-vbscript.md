---
title: Didacticiel RDS (VBScript) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 02/14/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- RDS tutorial [ADO], VBScript
ms.assetid: e2a48c4d-88b1-43ff-a202-9cdec54997d2
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1681297e87731642b7f597e59c6d25ccb833c04d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rds-tutorial-vbscript"></a>Didacticiel RDS (VBScript)
Il s’agit du didacticiel RDS, écrit en Microsoft Visual Basic Scripting Edition. Pour obtenir une description de l’objectif de ce didacticiel, consultez le [didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md).  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Dans ce didacticiel, [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) et [RDS. DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md) sont créés au moment du design, autrement dit, ils sont définis avec des balises d’objet, comme suit : `<OBJECT>...</OBJECT>`. Sinon, ils pu être créés au moment de l’exécution avec le [CreateObject (méthode) (RDS)](../../../ado/reference/rds-api/createobject-method-rds.md) (méthode). Par exemple, le **RDS. DataControl** objet a pu être créé comme suit :  
  
```  
Set DC = Server.CreateObject("RDS.DataControl")  
   <!-- RDS.DataControl -->  
   <OBJECT   
      ID="DC1" CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E33">  
   </OBJECT>  
  
   <!-- RDS.DataSpace -->  
   <OBJECT   
      ID="DS1" WIDTH=1 HEIGHT=1  
      CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36">  
   </OBJECT>  
  
   <SCRIPT LANGUAGE="VBScript">  
  
   Sub RDSTutorial()  
   Dim DF1   
```  
  
## <a name="step-1--specify-a-server-program"></a>Étape 1 : Spécifier un programme de serveur  
 VBScript peut détecter le nom du serveur Web IIS exécute en accédant à la VBScript **Request.ServerVariables** méthode disponible pour les Pages ASP :  
  
```  
"http://<%=Request.ServerVariables("SERVER_NAME")%>"  
```  
  
 Toutefois, pour ce didacticiel, vous devez utiliser le serveur fictif « yourServer ».  
  
> [!NOTE]
>  Faites attention au type de données de **ByRef** arguments. VBScript ne vous permet de spécifier le type de variable, vous devez donc toujours passer un **variante**. Lorsque vous utilisez HTTP, RDS vous autorise à transmettre un type Variant à une méthode qui attend un Variant non-si vous l’appelez avec le **RDS. DataSpace** objet [CreateObject](../../../ado/reference/rds-api/createobject-method-rds.md) (méthode). Lorsque vous utilisez DCOM ou un serveur in-process, vous devez respecter les types de paramètres sur les côtés client et serveur ou que vous recevez une erreur « Incompatibilité de Type ».  
  
```  
Set DF1 = DS1.CreateObject("RDSServer.DataFactory", "http://yourServer")  
```  
  
## <a name="step-2a--invoke-the-server-program-with-rdsdatacontrol"></a>Étape 2 a : appeler le programme serveur avec RDS. DataControl  
 Cet exemple est simplement un commentaire illustrant le comportement par défaut de la **RDS. DataControl** consiste à exécuter la requête spécifiée.  
  
```  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="http://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial2A()  
   Dim RS  
   DC1.Refresh  
   Set RS = DC1.Recordset  
...  
```  
  
## <a name="step-2b--invoke-the-server-program-with-rdsserverdatafactory"></a>Étape 2 b : appeler le programme serveur avec RDSServer.DataFactory  
  
## <a name="step-3--server-obtains-a-recordset"></a>Étape 3 : Le serveur obtient un jeu d’enregistrements  
  
## <a name="step-4--server-returns-the-recordset"></a>Étape 4 : Le serveur retourne le jeu d’enregistrements  
  
```  
Set RS = DF1.Query("DSN=Pubs;", "SELECT * FROM Authors")  
```  
  
## <a name="step-5--datacontrol-is-made-usable-by-visual-controls"></a>Étape 5 : DataControl est rendu utilisable par des contrôles visuels  
  
```  
' Assign the returned recordset to the DataControl.  
  
DC1.SourceRecordset = RS  
```  
  
## <a name="step-6a--changes-are-sent-to-the-server-with-rdsdatacontrol"></a>Étape 6 a : les modifications sont envoyées au serveur avec RDS. DataControl  
 Cet exemple est simplement un commentaire illustrant comment les **RDS. DataControl** effectue des mises à jour.  
  
```  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="http://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial6A()  
Dim RS  
DC1.Refresh  
...  
Set RS = DC1.Recordset  
' Edit the Recordset object...  
' The SERVER and CONNECT properties are already set from Step 2A.  
Set DC1.SourceRecordset = RS  
...  
DC1.SubmitChanges  
```  
  
## <a name="step-6b--changes-are-sent-to-the-server-with-rdsserverdatafactory"></a>Étape 6 b : les modifications sont envoyées au serveur avec RDSServer.DataFactory  
  
```  
DF.SubmitChanges "DSN=Pubs", RS  
  
End Sub  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 **Il s’agit de la fin du didacticiel.**  
  
## <a name="see-also"></a>Voir aussi  
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)   
