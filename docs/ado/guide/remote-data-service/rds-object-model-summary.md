---
title: Résumé du modèle d’objet RDS | Documents Microsoft
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
- RDS objects [ADO], object model summary
- RDS object model [ADO]
ms.assetid: 909f9af7-31db-4eec-ad52-650ce74dac2f
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 01f4af73d416616eeba6d8bcba82c390b09c3b7f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rds-object-model-summary"></a>Résumé du modèle objet RDS
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
|Objet| Description|  
|------------|-----------------|  
|[RDS.DataSpace](../../../ado/reference/rds-api/dataspace-object-rds.md)|Cet objet contient une méthode pour obtenir un serveur proxy. Le proxy peut être la valeur par défaut ou un programme de serveur personnalisé (objet métier). Le programme de serveur peut être appelé sur Internet, un intranet, un réseau local ou être une bibliothèque de liens dynamiques locale.<br /><br /> Le **DataSpace** objet est sécurisé pour le script.|  
|[RDSServer.DataFactory](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)|Cet objet représente le programme de serveur par défaut. Il exécute le comportement mise à jour et de récupération des données du Bureau à distance par défaut.<br /><br /> Le **DataFactory** objet n’est pas sécurisé pour le script.|  
|[RDS.DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md)|Cet objet peut appeler automatiquement le **RDS. DataSpace** et **RDSServer.DataFactory** objets.<br /><br /> Utilisez cet objet pour appeler le comportement de récupération ou de mise à jour de données de services Bureau à distance par défaut.<br /><br /> Cet objet permet également aux contrôles visuels accéder aux retourné **Recordset** objet.<br /><br /> Le **DataControl** objet est sécurisé pour le script.|  
  
## <a name="see-also"></a>Voir aussi  
 [Principes de base des services Bureau à distance](../../../ado/guide/remote-data-service/rds-fundamentals.md)   
 [Scénario des services Bureau à distance](../../../ado/guide/remote-data-service/rds-scenario.md)   
 [Didacticiel RDS](../../../ado/guide/remote-data-service/rds-tutorial.md)   
 [Utilisation et sécurité de RDS](../../../ado/guide/remote-data-service/rds-usage-and-security.md)


