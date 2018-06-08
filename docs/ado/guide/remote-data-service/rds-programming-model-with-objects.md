---
title: Modèle de programmation des services Bureau à distance avec des objets | Documents Microsoft
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
- RDS programming model [ADO]
- RDS objects [ADO]
ms.assetid: 07ce0ef0-72f1-48f4-823d-1b65d28c0926
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 32cd0b412e06f591eee69581e1e7ea8f871cab7f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rds-programming-model-with-objects"></a>Modèle de programmation des services Bureau à distance avec des objets
L’objectif des services Bureau à distance consiste à accéder à et de mettre à jour les sources de données via un intermédiaire tel que IIS. Le modèle de programmation spécifie la séquence d’activités nécessaires pour atteindre cet objectif. Le modèle objet spécifie les objets dont les méthodes et propriétés affectent le modèle de programmation.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Services Bureau à distance fournit la possibilité d’exécuter la séquence d’actions suivante :  
  
-   Spécifiez le programme à appeler sur le serveur et obtenir un moyen (proxy) pour faire référence à celui-ci à partir du client ([RDS. DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md)).  
  
-   Appeler le programme serveur. Passer des paramètres pour le programme de serveur qui identifie la source de données et la commande d’émettre (proxy ou [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md)).  
  
-   Le programme serveur obtient un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet à partir de la source de données, généralement à l’aide d’ADO. Si vous le souhaitez, le **Recordset** objet est traité sur le serveur ([RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)).  
  
-   Le programme serveur retourne le dernier **Recordset** objet à l’application cliente (proxy).  
  
-   Sur le client, le **Recordset** objet est placé dans une forme facilement utilisable par des contrôles visuels (contrôle visuel et **RDS. DataControl**).  
  
-   Modifications apportées à la **Recordset** objet sont envoyés sur le serveur et permet de mettre à jour la source de données (**RDS. DataControl** ou **RDSServer.DataFactory**).  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du modèle objet RDS](../../../ado/guide/remote-data-service/rds-object-model-summary.md)   
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)   
 [DataFactory, objet (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)   
 [DataSpace, objet (RDS)](../../../ado/reference/rds-api/dataspace-object-rds.md)   
 [Scénario des services Bureau à distance](../../../ado/guide/remote-data-service/rds-scenario.md)   
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)   
 [Objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)   
 [Utilisation et sécurité de RDS](../../../ado/guide/remote-data-service/rds-usage-and-security.md)


