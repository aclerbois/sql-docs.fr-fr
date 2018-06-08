---
title: Configurer la sécurité du dialogue pour les notifications d’événements | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
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
- event notifications [SQL Server], security
ms.assetid: 12afbc84-2d2a-4452-935e-e1c70e8c53c1
caps.latest.revision: 23
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 797aef8ddeab8daaf094556e5ba2ec96754a6ffe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-dialog-security-for-event-notifications"></a>Configurer la sécurité du dialogue pour les notifications d'événements
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit être configurée pour les notifications d'événements qui envoient des messages à un Service Broker résidant sur un serveur distant. La sécurité du dialogue doit être configurée manuellement conformément au modèle de sécurité totale du dialogue [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Le modèle de sécurité totale permet le chiffrement et le déchiffrement des messages échangés avec les serveurs distants. Même si les notifications d'événements sont envoyées dans une direction, les autres messages, comme les erreurs, sont retournés dans la direction opposée.  
  
## <a name="configuring-dialog-security-for-event-notifications"></a>Configuration de la sécurité du dialogue pour les notifications d'événements  
 Les étapes suivantes décrivent le processus à suivre pour implémenter la sécurité du dialogue des notifications d'événements. Ces étapes incluent les actions à exécuter aussi bien sur le serveur source que sur le serveur cible. Le serveur source correspond au serveur sur lequel la notification d'événement est en cours de création. Le serveur cible désigne le serveur qui reçoit le message de notification d'événement. Vous devez effectuer les actions de chaque étape aussi bien sur le serveur source que sur le serveur cible avant de passer à l'étape suivante.  
  
> [!IMPORTANT]  
>  Tous les certificats doivent être créés avec des dates de début et des dates d'expiration valides.  
  
 **Étape 1 : établissez un numéro de port TCP et un nom de service cible.**  
  
 Établissez le port TCP sur lequel le serveur source et le serveur cible recevront les messages. Vous devez également définir le nom du service cible.  
  
 **Étape 2 : configurez le chiffrement et le partage de certificats pour l'authentification de niveau base de données.**  
  
 Exécutez les actions suivantes sur le serveur source et sur le serveur cible.  
  
|Serveur source|Serveur cible|  
|-------------------|-------------------|  
|Choisissez ou créez la base de données qui contiendra la notification d'événement et la clé principale.|Choisissez ou créez la base de données qui contiendra la clé principale.|  
|S'il n'existe pas de clé principale pour la base de données source, [créez-la](../../t-sql/statements/create-master-key-transact-sql.md). Une clé principale est obligatoire sur les bases de données source et cible pour aider à sécuriser leurs certificats respectifs.|S'il n'existe pas de clé principale pour la base de données cible, créez-la.|  
|[Créez une connexion](../../t-sql/statements/create-login-transact-sql.md) et un [utilisateur](../../t-sql/statements/create-user-transact-sql.md) correspondant pour la base de données source.|Créez une connexion et un utilisateur correspondant pour la base de données cible.|  
|[Créez un certificat](../../t-sql/statements/create-certificate-transact-sql.md) dont le propriétaire est l'utilisateur de la base de données source.|Créez un certificat dont le propriétaire est l'utilisateur de la base de données cible.|  
|[Sauvegardez le certificat](../../t-sql/statements/backup-certificate-transact-sql.md) dans un fichier accessible par le serveur cible.|Sauvegardez le certificat dans un fichier accessible par le serveur source.|  
|[Créez un utilisateur](../../t-sql/statements/create-user-transact-sql.md), en spécifiant l'utilisateur de la base de données cible, ainsi que WITHOUT LOGIN. Cet utilisateur sera propriétaire du certificat de la base de données cible créé à partir du fichier de sauvegarde. Il n'est pas nécessaire que l'utilisateur soit mappé à une connexion, parce que le seul objectif de cet utilisateur est d'être propriétaire du certificat de la base de données cible, créé au cours de l'étape 3 suivante.|Créez un utilisateur, en spécifiant l'utilisateur de la base de données source, ainsi que WITHOUT LOGIN. Cet utilisateur sera propriétaire du certificat de la base de données source créé à partir du fichier de sauvegarde. Il n'est pas nécessaire que l'utilisateur soit mappé à une connexion, parce que le seul objectif de cet utilisateur est d'être propriétaire du certificat de la base de données source, créé au cours de l'étape 3 suivante.|  
  
 **Étape 3 : partagez les certificats et attribuez les autorisations pour l'authentification de niveau base de données.**  
  
 Exécutez les actions suivantes sur le serveur source et sur le serveur cible.  
  
|Serveur source|Serveur cible|  
|-------------------|-------------------|  
|[Créez un certificat](../../t-sql/statements/create-certificate-transact-sql.md) à partir du fichier de sauvegarde du certificat cible, en spécifiant comme propriétaire l'utilisateur de la base de données cible.|Créez un certificat à partir du fichier de sauvegarde du certificat source, en spécifiant comme propriétaire l'utilisateur de la base de données source.|  
|[Attribuez l'autorisation](../../t-sql/statements/grant-transact-sql.md) de création de notification d'événement à l'utilisateur de la base de données source. Pour plus d’informations sur cette autorisation, consultez [CREATE EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-notification-transact-sql.md).|Attribuez l'autorisation REFERENCES à l'utilisateur de la base de données cible sur le contrat existant de notifications d'événements [!INCLUDE[ssSB](../../includes/sssb-md.md)] : `http://schemas.microsoft.com/SQL/Notifications/PostEventNotification`.|  
|[Créez une liaison de service distant](../../t-sql/statements/create-remote-service-binding-transact-sql.md) au service cible et spécifiez les informations d'identification de l'utilisateur de la base de données cible. Les liaisons de service distant garantissent que la clé public du certificat dont l'utilisateur de la base de données source est propriétaire authentifie les messages adressés au serveur cible.|[Attribuez](../../t-sql/statements/grant-transact-sql.md) les autorisations CREATE QUEUE, CREATE SERVICE et CREATE SCHEMA à l'utilisateur de la base de données cible.|  
||Si vous n'êtes pas déjà connecté à la base de données comme utilisateur de la base de données cible, connectez-vous maintenant.|  
||[Créez une file d'attente](../../t-sql/statements/create-queue-transact-sql.md) pour recevoir les messages de notification d'événement et [créez un service](../../t-sql/statements/create-service-transact-sql.md) de remise des messages.|  
||[Attribuez l'autorisation SEND](../../t-sql/statements/grant-transact-sql.md) sur le service cible à l'utilisateur de la base de données source.|  
|Fournissez au serveur cible l'identificateur Service Broker de la base de données source. Vous pouvez obtenir cet identificateur en interrogeant la colonne **service_broker_guid** de l’affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) . Pour une notification d’événement de niveau serveur, utilisez l’identificateur Service Broker de **msdb**.|Fournissez au serveur source l'identificateur Service Broker de la base de données cible.|  
  
 **Étape 4 : créez les itinéraires et configurez l'authentification de niveau serveur.**  
  
 Exécutez les actions suivantes sur le serveur source et sur le serveur cible.  
  
|Serveur source|Serveur cible|  
|-------------------|-------------------|  
|[Créez un itinéraire](../../t-sql/statements/create-route-transact-sql.md) vers le service cible et spécifiez l’identificateur Service Broker de la base de données cible et le numéro de port TCP approuvé.|Créez un itinéraire vers le service source et spécifiez l'identificateur Service Broker de la base de données source et le numéro de port TCP approuvé. Pour spécifier le service source, utilisez le service fourni suivant : `http://schemas.microsoft.com/SQL/Notifications/EventNotificationService`.|  
|Basculez vers la base de données **master** pour configurer l’authentification de niveau serveur.|Basculez vers la base de données **master** pour configurer l’authentification de niveau serveur.|  
|S'il n'existe pas de clé principale pour la base de données **master** , [créez-la](../../t-sql/statements/create-master-key-transact-sql.md).|S'il n'existe pas de clé principale pour la base de données **master** , créez-la.|  
|[Créez un certificat](../../t-sql/statements/create-certificate-transact-sql.md) qui authentifie la base de données.|Créez un certificat qui authentifie la base de données.|  
|[Sauvegardez le certificat](../../t-sql/statements/backup-certificate-transact-sql.md) dans un fichier accessible par le serveur cible.|Sauvegardez le certificat dans un fichier accessible par le serveur source.|  
|[Créez un point de terminaison](../../t-sql/statements/create-endpoint-transact-sql.md)et spécifiez le numéro de port TCP approuvé, FOR SERVICE_BROKER (AUTHENTICATION = CERTIFICATE *nom_certificat*), ainsi que le nom du certificat d’authentification.|Créez un point de terminaison et spécifiez le numéro de port TCP approuvé, FOR SERVICE_BROKER (AUTHENTICATION = CERTIFICATE *nom_certificat*), ainsi que le nom du certificat d’authentification.|  
|[Créez une connexion](../../t-sql/statements/create-login-transact-sql.md)et spécifiez celle du serveur cible.|Créez une connexion et spécifiez celle du serveur source.|  
|[Attribuez l'autorisation CONNECT](../../t-sql/statements/grant-transact-sql.md) sur le point de terminaison à la connexion authentificatrice cible.|Attribuez l'autorisation CONNECT sur le point de terminaison à la connexion authentificatrice source.|  
|[Créez un utilisateur](../../t-sql/statements/create-user-transact-sql.md)et spécifiez la connexion authentificatrice cible.|Créez un utilisateur et spécifiez la connexion authentificatrice source.|  
  
 **Étape 5 : partagez les certificats d'authentification de niveau serveur et créez la notification d'événement.**  
  
 Exécutez les actions suivantes sur le serveur source et sur le serveur cible.  
  
|Serveur source|Serveur cible|  
|-------------------|-------------------|  
|[Créez un certificat](../../t-sql/statements/create-certificate-transact-sql.md) à partir du fichier de sauvegarde du certificat cible, en spécifiant comme propriétaire l'utilisateur authentificateur cible.|Créez un certificat à partir du fichier de sauvegarde du certificat source, en spécifiant comme propriétaire l'utilisateur authentificateur source.|  
|Basculez vers la base de données source sur laquelle créer la notification d'événement et, si vous n'êtes pas déjà connecté en tant qu'utilisateur de la base de données source, connectez-vous maintenant.|Basculez vers la base de données cible pour recevoir les messages de notification d'événement.|  
|[Créez la notification d'événement](../../t-sql/statements/create-event-notification-transact-sql.md)et spécifiez le Service Broker, ainsi que l'identificateur de la base de données cible.||  
  
## <a name="see-also"></a> Voir aussi  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Implémenter des notifications d'événements](../../relational-databases/service-broker/implement-event-notifications.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [CREATE EVENT NOTIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-notification-transact-sql.md)  
  
  
