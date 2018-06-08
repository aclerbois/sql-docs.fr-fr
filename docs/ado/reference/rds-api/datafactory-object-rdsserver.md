---
title: DataFactory, objet (RDSServer) | Documents Microsoft
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
apitype: COM
helpviewer_keywords:
- DataFactory object [ADO]
ms.assetid: e75240c2-b749-471e-b6ea-98cae232efbe
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 35918fe661c3148cd3b2962f69975ea9925a6b05
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datafactory-object-rdsserver"></a>DataFactory, objet (RDSServer)
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Cet objet de métier côté serveur par défaut implémente des méthodes qui fournissent l’accès en lecture/écriture aux sources de données spécifiées pour les applications côté client.  
  
 Le **RDSServer.DataFactory** objet est conçu comme un objet Automation côté serveur qui reçoit les demandes du client. Dans une implémentation Internet, il réside sur un serveur Web et est instancié par le composant ADISAPI. Le **RDSServer.DataFactory** objet fournit en lecture et accès en écriture aux données spécifiées sources, mais ne contient pas d’une logique de règles de validation ou de gestion.  
  
 Si vous utilisez une méthode qui est disponible à la fois dans le **RDSServer.DataFactory** et [RDS. DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) objets, le Service de données distant utilise le **RDS. DataControl** version par défaut. La valeur par défaut suppose un scénario de programmation de base, où le **RDSServer.DataFactory** sert d’un objet métier côté serveur générique.  
  
 Si vous souhaitez que votre application Web gère le traitement côté serveur de tâches spécifiques, vous pouvez remplacer le **RDSServer.DataFactory** avec un objet métier personnalisé.  
  
 Vous pouvez créer des objets métier côté serveur qui appellent le **RDSServer.DataFactory** méthodes, telles que [requête](../../../ado/reference/rds-api/query-method-rds.md) et [CreateRecordset](../../../ado/reference/rds-api/createrecordset-method-rds.md). Cela est utile si vous souhaitez ajouter des fonctionnalités à vos objets métier, mais tirer parti des technologies de Service de données distant existants.  
  
 Le **DataFactory** objet n’est pas sécurisé pour les scripts qui s’exécutent sur le côté client.  
  
 L’ID de classe pour le **RDSServer.DataFactory** objet est D 9381D8F5-0288-11. 0-9501-00AA00B911A5.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet DataFactory (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [DataFactory (exemple d’objet), Query (exemple de méthode) et CreateObject (exemple de méthode) (VBScript)](../../../ado/reference/rds-api/datafactory-object-query-method-and-createobject-method-example-vbscript.md)


