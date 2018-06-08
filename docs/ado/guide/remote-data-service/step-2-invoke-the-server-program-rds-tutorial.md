---
title: 'Étape 2 : Appeler le programme de serveur (didacticiel RDS) | Documents Microsoft'
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
- RDS tutorial [ADO], invoking server program
ms.assetid: 5e74c2da-65ee-4de4-8b41-6eac45c3632e
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5e093828f0f1591800a7f8cc7e2b6958e8807c81
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="step-2-invoke-the-server-program-rds-tutorial"></a>Étape 2 : Appeler le programme de serveur (didacticiel RDS)
Quand vous appelez une méthode sur le client *proxy*, le programme sur le serveur qui exécute la méthode. Dans cette étape, vous allez exécuter une requête sur le serveur.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 **Partie A** si vous n’utilisiez pas [RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md) dans ce didacticiel, la façon la plus pratique d’effectuer cette étape serait d’utiliser le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet. Le **RDS. DataControl** combine l’étape précédente de création d’un proxy, cette étape, qui émet une requête.  
  
 Définir le **RDS. DataControl** objet [Server](../../../ado/reference/rds-api/server-property-rds.md) propriété pour identifier où le programme de serveur doit être instancié ; le [Connect](../../../ado/reference/rds-api/connect-property-rds.md) propriété pour spécifier la chaîne de connexion pour accéder à la source de données et le [SQL](../../../ado/reference/rds-api/sql-property.md) propriété pour spécifier le texte de commande de requête. Ensuite, émettez la [Actualiser](../../../ado/reference/rds-api/refresh-method-rds.md) méthode pour forcer le programme du serveur pour se connecter à la source de données, récupérer les lignes spécifiées par la requête et retourner un **Recordset** objet au client.  
  
 Ce didacticiel n’utilise pas le **RDS. DataControl**, mais il s’agit d’aspect si c’était :  
  
```  
Sub RDSTutorial2A()  
   Dim DC as New RDS.DataControl  
   DC.Server = "http://yourServer"  
   DC.Connect = "DSN=Pubs"  
   DC.SQL = "SELECT * FROM Authors"  
   DC.Refresh  
...  
```  
  
 Ni le didacticiel appeler services Bureau à distance avec des objets ADO, mais voici comment se présenterait si tel n’est :  
  
```  
Dim rs as New ADODB.Recordset  
rs.Open "SELECT * FROM Authors","Provider=MS Remote;Data Source=Pubs;" & _  
        "Remote Server=http://yourServer;Remote Provider=SQLOLEDB;"  
```  
  
 **Partie B** la méthode générale de l’exécution de cette étape consiste à appeler la **RDSServer.DataFactory** objet [requête](../../../ado/reference/rds-api/query-method-rds.md) (méthode). Cette méthode prend une chaîne de connexion qui est utilisé pour se connecter à une source de données, et un texte de commande, qui est utilisé pour spécifier les lignes à retourner à partir de la source de données.  
  
 Ce didacticiel utilise le **DataFactory** objet **requête** méthode :  
  
```  
Sub RDSTutorial2B()  
   Dim DS as New RDS.DataSpace  
   Dim DF  
   Dim RS as ADODB.Recordset  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "http://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 3 : Le serveur obtient un jeu d’enregistrements (didacticiel sur les services Bureau à distance)](../../../ado/guide/remote-data-service/step-3-server-obtains-a-recordset-rds-tutorial.md)   
 [Didacticiel RDS (VBScript)](../../../ado/guide/remote-data-service/rds-tutorial-vbscript.md)   
