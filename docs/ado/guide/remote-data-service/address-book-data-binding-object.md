---
title: Objet de liaison de données de carnet d’adresses | Documents Microsoft
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
- RDS scenarios [ADO], data-binding object
- address book application scenario [ADO], data-binding object
ms.assetid: 080c1925-d453-4b89-92ac-c93591490518
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8efa72c893f0b2ddd07c834a07976babfbb46233
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="address-book-data-binding-object"></a>Objet de liaison de données du carnet d’adresses
L’application de carnet d’adresses utilise le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objet pour lier des données à partir de la base de données SQL Server à un objet visuel (dans ce cas, un tableau DHTML) dans la page de l’application client HTML. La logique de programmation pilotée par événements VBScript utilise le [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) à :  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
-   La base de données, envoyer des mises à jour à la base de données et actualiser la grille de données.  
  
-   Autoriser les utilisateurs à accéder au premier, ensuite, précédente, ou le dernier enregistrement dans la grille de données.  
  
 Le code suivant définit le **RDS. DataControl** composant :  
  
```  
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=DC1 Width=1 Height=1>  
   <PARAM NAME="SERVER" VALUE="http://<%=Request.ServerVariables("SERVER_NAME")%>">  
   <PARAM NAME="CONNECT" VALUE="Provider=sqloledb;  
Initial Catalog=AddrBookDb;Integrated Security=SSPI;">  
</OBJECT>  
```  
  
 La balise OBJECT définit le **RDS. DataControl** composant dans le programme. La balise comprend deux types de paramètres :  
  
-   Ceux associés à la balise d’objet générique.  
  
-   Spécifiques à la **RDS. DataControl** objet.  
  
## <a name="generic-object-tag-parameters"></a>Paramètres de la balise objet générique  
 Le tableau suivant décrit les paramètres associés à la balise d’objet.  
  
|Paramètre| Description|  
|---------------|-----------------|  
|***ID DE CLASSE***|Un nombre unique de 128 bits qui identifie le type d’objet incorporé dans le système. Cet identificateur est conservé dans le Registre système de l’ordinateur local. (Pour les ID de classe de la **RDS. DataControl** d’objets, consultez [RDS. Objet DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md).)|  
|***ID***|Définit un identificateur de document à l’échelle pour l’objet incorporé est utilisé pour l’identifier dans le code.|  
  
## <a name="rdsdatacontrol-tag-parameters"></a>RDS. Paramètres de la balise de DataControl  
 Le tableau suivant décrit les paramètres spécifiques à la **RDS. DataControl** objet. (Pour une liste complète de la **RDS. DataControl** de l’objet et savoir quand les implémenter, consultez [RDS. Objet DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md).)  
  
|Paramètre| Description|  
|---------------|-----------------|  
|[SERVEUR](../../../ado/reference/rds-api/server-property-rds.md)|Si vous utilisez HTTP, la valeur est le nom de l’ordinateur serveur précédé par `http://`.|  
|[CONNECT](../../../ado/reference/rds-api/connect-property-rds.md)|Fournit les informations de connexion nécessaires pour la **RDS. DataControl** pour se connecter à SQL Server.|  
|[SQL](../../../ado/reference/rds-api/sql-property.md)|Définit ou retourne la chaîne de requête utilisée pour récupérer le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Boutons de commande de l’application Carnet d’adresses](../../../ado/guide/remote-data-service/address-book-command-buttons.md)


