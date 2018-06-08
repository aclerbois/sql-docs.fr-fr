---
title: 'Étape 6 : Les modifications sont envoyées au serveur (didacticiel RDS) | Documents Microsoft'
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
- RDS tutorial [ADO], changes sent to server
ms.assetid: b1e927d6-7d50-4978-9eef-045043cdce7a
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0537d8d05553b4e50861bda664d2cdc53489cb82
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-6-changes-are-sent-to-the-server-rds-tutorial"></a>Étape 6 : Les modifications sont envoyées au serveur (didacticiel RDS)
Si le **Recordset** objet est modifié, toutes les modifications (autrement dit, les lignes qui sont ajoutées, modifiées ou supprimées) peuvent être envoyées sur le serveur.  
  
> [!NOTE]
>  Le comportement par défaut des services Bureau à distance peut être appelé implicitement avec les objets ADO et le fournisseur Microsoft OLE DB d’accès à distance. Les requêtes peuvent renvoyer **Recordset**s et modifié **Recordset**s peut mettre à jour la source de données. Ce didacticiel n’appelle pas les services Bureau à distance avec des objets ADO, mais il s’agit d’aspect si c’était :  
  
```  
Dim rs as New ADODB.Recordset  
rs. "SELECT * FROM Authors","=MS Remote;=Pubs;" & _  
=http://yourServer;=SQLOLEDB;"  
...              ' Edit the Recordset.  
rs.   ' The equivalent of   
...  
```  
  
 **Partie A** supposer que dans cet exemple que vous n’avez utilisé le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) et un **Recordset** objet est associé à la **RDS. DataControl**. Le [SubmitChanges](../../../ado/reference/rds-api/submitchanges-method-rds.md) méthode met à jour la source de données avec les modifications apportées à la **Recordset** si l’objet le [Server](../../../ado/reference/rds-api/server-property-rds.md) et [Connect](../../../ado/reference/rds-api/connect-property-rds.md) propriétés sont toujours définies.  
  
```  
Sub RDSTutorial6A()  
Dim DC as New RDS.DataControl  
Dim RS as ADODB.Recordset  
DC. = "http://yourServer"  
DC. = "DSN=Pubs"  
DC. = "SELECT * FROM Authors"  
DC.  
...  
Set RS = DC.  
   ' Edit the Recordset.  
...  
DC.  
...  
```  
  
 **Partie B** vous a également mettre à jour le serveur avec le [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) objet, en spécifiant une connexion et un **Recordset** objet.  
  
```  
Sub RDSTutorial6B()  
Dim DS As New RDS.DataSpace  
Dim RS As ADODB.Recordset  
Dim DC As New RDS.DataControl  
Dim DF As Object  
Dim blnStatus As Boolean  
Set DF = DS.("RDSServer.DataFactory", "http://yourServer")  
Set RS = DF. ("DSN=Pubs", "SELECT * FROM Authors")  
DC. = RS    ' Visual controls can now bind to DC.  
    ' Edit the Recordset.  
blnStatus = DF."DSN=Pubs", RS  
End Sub  
```  
  
 **Il s’agit de la fin du didacticiel.**  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="see-also"></a>Voir aussi  
 [Fournisseur d’accès à distance Microsoft OLE DB (fournisseur de services ADO)](../../../ado/guide/appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md)   
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)   
 [Didacticiel RDS (VBScript)](../../../ado/guide/remote-data-service/rds-tutorial-vbscript.md)   
