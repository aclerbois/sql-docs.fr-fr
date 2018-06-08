---
title: Les paramètres Client requis | Documents Microsoft
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
- DataFactory handler in RDS [ADO]
ms.assetid: e776b4e3-fcc4-4bfb-a7e8-5ffae1d83833
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fb864cb5489cc5cd1a82343995ed7a1e73c3a0b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="required-client-settings"></a>Paramètres Client requis
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Spécifiez les paramètres suivants pour utiliser un personnalisé **DataFactory** gestionnaire.  
  
-   Spécifier « fournisseur = MS Remote » dans la [connexion Object (ADO)](../../../ado/reference/ado-api/connection-object-ado.md) objet [fournisseur propriété (ADO)](../../../ado/reference/ado-api/provider-property-ado.md) propriété ou le **connexion** chaîne de connexion de l’objet «**Fournisseur**= « mot clé.  
  
-   Définir le [propriété CursorLocation (ADO)](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété **adUseClient**.  
  
-   Spécifiez le nom du gestionnaire à utiliser dans le [objet DataControl (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md) l’objet **gestionnaire** propriété, ou la [objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md) chaîne de connexion de l’objet » **Gestionnaire**= « mot clé. (Vous ne pouvez pas définir le gestionnaire le **connexion** objet de la chaîne de connexion.)  
  
 Services Bureau à distance fournit un gestionnaire par défaut sur le serveur nommé **MSDFMAP. Gestionnaire**. (Le fichier de personnalisation par défaut est nommé MSDFMAP. INI.)  
  
 **Exemple**  
  
 Supposez que les sections suivantes dans **MSDFMAP. INI** et le nom de source de données, AdvWorks, ont été précédemment définis :  
  
```  
[connect CustomerDataBase]  
Access=ReadWrite  
Connect="DSN=AdvWorks"  
  
[sql CustomerById]  
SQL="SELECT * FROM Customers WHERE CustomerID = ?"  
```  
  
 Les extraits de code suivants sont écrits en Visual Basic :  
  
## <a name="rdsdatacontrol-version"></a>RDS. DataControl Version  
  
```  
Dim dc as New RDS.DataControl  
Set dc.Handler = "MSDFMAP.Handler"  
Set dc.Server = "http://yourServer"  
Set dc.Connect = "Data Source=CustomerDatabase"  
Set dc.SQL = "CustomerById(4)"  
dc.Refresh  
```  
  
## <a name="recordset-version"></a>Version du jeu d’enregistrements  
  
```  
Dim rs as New ADODB.Recordset  
rs.CursorLocation = adUseClient  
```  
  
 Spécifiez soit le [Gestionnaire de propriété (RDS)](../../../ado/reference/rds-api/handler-property-rds.md) propriété ou un mot clé ; le [fournisseur propriété (ADO)](../../../ado/reference/ado-api/provider-property-ado.md) propriété ou un mot clé ; et le *CustomerById* et  *CustomerDatabase* identificateurs. Ouvrez le **Recordset** objet  
  
 rs.Open "CustomerById(4)", "Handler=MSDFMAP.Handler;" & _  
  
```  
"Provider=MS Remote;Data Source=CustomerDatabase;" & _  
"Remote Server=http://yourServer"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Section Connect du fichier de personnalisation](../../../ado/guide/remote-data-service/customization-file-connect-section.md)   
 [Section de personnalisation de fichier SQL](../../../ado/guide/remote-data-service/customization-file-sql-section.md)   
 [Section du fichier de personnalisation UserList](../../../ado/guide/remote-data-service/customization-file-userlist-section.md)   
 [DataFactory, personnalisation](../../../ado/guide/remote-data-service/datafactory-customization.md)   
 [Paramètres Client requis](../../../ado/guide/remote-data-service/required-client-settings.md)   
 [Présentation du fichier de personnalisation](../../../ado/guide/remote-data-service/understanding-the-customization-file.md)   
 [Écriture d’un gestionnaire personnalisé](../../../ado/guide/remote-data-service/writing-your-own-customized-handler.md)






















