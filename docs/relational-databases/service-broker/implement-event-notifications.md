---
title: Implémenter des notifications d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: service-broker
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- event notifications [SQL Server], target service
- target service [SQL Server]
- event notifications [SQL Server], creating
ms.assetid: 29ac8f68-a28a-4a77-b67b-a8663001308c
caps.latest.revision: 34
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 46be0ae46fff0af1ba926777e042312523500ad0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="implement-event-notifications"></a>Implémenter des notifications d'événements
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Pour implémenter une notification d'événement, vous devez créer un service cible destiné à recevoir les notifications d'événements avant de créer la notification d'événement.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit être configurée pour les notifications d'événements qui envoient des messages à un Service Broker résidant sur un serveur distant. La sécurité du dialogue doit être configurée manuellement conformément au modèle de sécurité totale.  
  
## <a name="creating-the-target-service"></a>Création du service cible  
 Il n'est pas nécessaire que vous créiez un service d'initialisation de [!INCLUDE[ssSB](../../includes/sssb-md.md)], car [!INCLUDE[ssSB](../../includes/sssb-md.md)] inclut le type de message et le contrat de notifications d'événements suivants :  
  
```  
http://schemas.microsoft.com/SQL/Notifications/PostEventNotification  
```  
  
 Le service cible qui reçoit les notifications d'événements doit respecter ce contrat préexistant.  
  
 **Pour créer un service cible**:  
  
1.  Créez une file d'attente pour recevoir les messages.  
  
    > [!NOTE]  
    >  Cette file d'attente reçoit le type de message suivant : `http://schemas.microsoft.com/SQL/Notifications/QueryNotification`.  
  
2.  Créez un service dans la file d'attente qui fasse référence au contrat de notification d'événement.  
  
3.  Créez un itinéraire au niveau du service pour indiquer l'adresse à laquelle [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit expédier les messages pour le service. Si la notification d'événement a pour cible un service de la même base de données, spécifiez `ADDRESS = 'LOCAL'`.  
  
    > [!NOTE]  
    >  [!INCLUDE[ssSB](../../includes/sssb-md.md)] L’acheminement désigne le service appelé à recevoir les messages de notification. Si votre notification d'événement a pour cible un service de serveur distant, il convient de définir des itinéraires à la fois sur le serveur source et sur le serveur cible pour éviter des erreurs de communication bidirectionnelle.  
  
 L'exemple suivant illustre la création d'une file d'attente, d'un service au niveau de cette file d'attente, et d'un itinéraire au niveau du service en vue de gérer les messages relevant du contrat de notification d'événement.  
  
```  
CREATE QUEUE NotifyQueue ;  
GO  
CREATE SERVICE NotifyService  
ON QUEUE NotifyQueue  
(  
[http://schemas.microsoft.com/SQL/Notifications/PostEventNotification]  
);  
GO  
CREATE ROUTE NotifyRoute  
WITH SERVICE_NAME = 'NotifyService',  
ADDRESS = 'LOCAL';  
GO  
```  
  
## <a name="creating-the-event-notification"></a>Création de la notification d'événement  
 Les notifications d'événements sont créées à l'aide de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE EVENT NOTIFICATION et supprimées à l'aide de l'instruction DROP EVENT NOTIFICATION. Pour modifier une notification d'événement, vous devez la supprimer et la recréer.  
  
 L'exemple suivant crée la notification d'événement `CreateDatabaseNotification`. Cette notification envoie un message à propos de tout événement `CREATE_DATABASE` qui se produit sur le serveur au service `NotifyService` précédemment créé.  
  
```  
CREATE EVENT NOTIFICATION CreateDatabaseNotification  
ON SERVER  
FOR CREATE_DATABASE  
TO SERVICE 'NotifyService', '8140a771-3c4b-4479-8ac0-81008ab17984' ;  
```  
  
> [!CAUTION]  
>  Les notifications d'événements reconnaissent les événements CREATE_SCHEMA et les définitions <schema_element> des instructions CREATE SCHEMA comme événements distincts. Par exemple, une notification d'événement est créée sur les deux événements CREATE_SCHEMA et CREATE_TABLE, et vous exécutez le traitement suivant.  
>   
>  `CREATE SCHEMA s`  
>   
>  `CREATE TABLE t1 (col1 int)`  
>   
>  Dans ce cas, la notification d'événement est déclenchée deux fois : une première fois quand se produit l'événement CREATE_SCHEMA, et une deuxième fois quand se produit l'événement CREATE_TABLE. Nous recommandons d'éviter de créer des notifications d'événements sur les événements CREATE_SCHEMA et les textes <schema_element> des définitions CREATE SCHEMA correspondantes, ou de créer une logique dans votre application destinée à éviter la capture des données d'événement non souhaitées.  
  
 **Pour créer une notification d'événement**  
  
-   [CREATE EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-notification-transact-sql.md)  
  
 **Pour supprimer une notification d'événement**  
  
-   [DROP EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-notification-transact-sql.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Obtenir des informations concernant les notifications d'événements](../../relational-databases/service-broker/get-information-about-event-notifications.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
