---
title: Utilisation du fournisseur WMI pour les événements serveur | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: wmi
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- event notifications [WMI]
- permissions [WMI]
- connection strings [WMI]
- security [WMI]
- Service Broker [WMI]
- WMI Provider for Server Events, connection strings
- WMI Provider for Server Events, Service Broker
- notifications [WMI]
- WMI Provider for Server Events, security
ms.assetid: cd974b3b-2309-4a20-b9be-7cfc93fc4389
caps.latest.revision: 33
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 98f1dfb796dbd30eb6e4d7f4e20d9bf37854d902
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="working-with-the-wmi-provider-for-server-events"></a>Utilisation du fournisseur WMI pour les événements de serveur
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique fournit des indications que vous devez prendre en compte avant de programmer à l'aide du fournisseur WMI pour les événements de serveur.  
  
## <a name="enabling-service-broker"></a>Activation de Service Broker  
 Le fournisseur WMI pour les événements de serveur fonctionne en traduisant les requêtes WQL pour les événements en notifications d'événements dans la base de données que vous ciblez. Il peut être utile de comprendre comment les notifications d'événements fonctionnent lorsque vous programmez en fonction du fournisseur. Pour plus d’informations, consultez [Fournisseur WMI pour les concepts des événements de serveur](http://technet.microsoft.com/library/ms180560.aspx).  
  
 En particulier, comme les notifications d'événements créées par le fournisseur WMI utilisent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour envoyer des messages sur les événements de serveur, ce service doit être activé partout où les événements sont générés. Si votre programme interroge des événements sur une instance de serveur, le [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans msdb de cette instance doit être activé, car ceci est l'emplacement du service [!INCLUDE[ssSB](../../includes/sssb-md.md)] cible (nommé SQL/Notifications/ProcessWMIEventProviderNotification/v1.0) qui est créé par le fournisseur. Si votre programme interroge des événements dans une base de données ou sur un objet de base de données particulier, le [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans cette base de données cible doit être activé. Si le [!INCLUDE[ssSB](../../includes/sssb-md.md)] correspondant n'est pas activé après le déploiement de votre application, tous les événements générés par la notification d'événements sous-jacente sont envoyés à la file d'attente du service utilisé par la notification d'événements, mais ils ne sont pas retournés à votre application de gestion WMI tant que le [!INCLUDE[ssSB](../../includes/sssb-md.md)] n'est pas activé.  
  
 La requête suivante détermine quelles instances Service Broker sont activées sur une instance de serveur, ainsi que le GUID des instances Service Broker :  
  
```  
SELECT name, is_broker_enabled, service_broker_guid FROM sys.databases;  
```  
  
 Le GUID du Service Broker de msdb présente un intérêt particulier car il s'agit de l'emplacement du service cible du fournisseur.  
  
 Pour activer [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans une base de données, utilisez l’option ENABLE_BROKER SET de la [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) instruction.  
  
## <a name="specifying-a-connection-string"></a>Spécification d'une chaîne de connexion  
 Les applications dirigent le fournisseur WMI pour les événements de serveur vers une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en se connectant à un espace de noms WMI défini par le fournisseur. Le service WMI de Windows mappe cet espace de noms à la DLL de fournisseur, Sqlwep.dll, puis la charge en mémoire. Chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a son propre espace de noms WMI, qui utilise par défaut : \\ \\.\\ *racine*\Microsoft\SqlServer\ServerEvents\\*nom_instance*. *nom_instance* les valeurs par défaut à MSSQLSERVER dans une installation par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions-and-server-authentication"></a>Autorisations et authentification serveur  
 Pour accéder au fournisseur WMI pour les événements de serveur, le client d'où provient une application de gestion WMI doit correspondre à une connexion authentifiée Windows ou à un groupe dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiée dans la chaîne de connexion de l'application.  
  
## <a name="permissions-and-event-notification-scope"></a>Autorisations et étendue des notifications d'événements  
 Le fournisseur WMI pour les événements de serveur traduit des requêtes WQL en notifications d'événements dans la base de données cible. Pour cette raison, l'application appelante doit avoir non seulement les autorisations minimales requises pour accéder au fournisseur, mais aussi les autorisations correctes dans la base de données pour créer les notifications d'événements requises. Les autorisations sont les suivantes :  
  
-   Pour créer une notification d'événements dont l'étendue correspond à la base de données, au minimum, l'autorisation CREATE DATABASE DDL EVENT NOTIFICATION est requise sur la base de données actuelle.  
  
-   Pour créer une notification d'événements sur une instruction DDL dont l'étendue correspond au serveur, au minimum, l'autorisation CREATE DDL EVENT NOTIFICATION est requise sur le serveur.  
  
-   Pour créer une notification d'événements sur un événement de trace, au minimum, l'autorisation CREATE TRACE EVENT NOTIFICATION est requise sur le serveur.  
  
-   Pour créer une notification d'événements dont l'étendue correspond à une file d'attente, au minimum, l'autorisation ALTER est requise sur la file d'attente.  
  
 Pour plus d’informations sur la façon dont l’étendue requêtes WQL, consultez [à l’aide de WQL avec le fournisseur WMI pour les événements serveur](http://technet.microsoft.com/library/ms180524\(v=sql.105\).aspx).  
  
 Pour illustrer l'étendue, considérez une application de fournisseur WMI qui inclut la requête WQL suivante :  
  
```  
SELECT * FROM ALTER_TABLE  
WHERE DatabaseName = "AdventureWorks2012"   
    AND SchemaName = "Person"  
    AND ObjectName = "Person"  
    AND ObjectType = "TABLE";  
```  
  
 Le fournisseur WMI traduit cette requête en une notification d'événements créée dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Cela signifie que l'appelant doit avoir les autorisations requises pour créer une telle notification d'événements, notamment l'autorisation CREATE DATABASE DDL EVENT NOTIFICATION dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
 Si une requête WQL spécifie une notification d'événements dont l'étendue correspond au niveau serveur, par exemple en émettant la requête SELECT * FROM ALTER_TABLE, l'application appelante doit avoir l'autorisation CREATE DDL EVENT NOTIFICATION au niveau du serveur. Notez que les notifications d'événements dont l'étendue est le serveur sont stockées dans la base de données master. Vous pouvez utiliser la [sys.server_event_notifications](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md) vue pour consulter leurs métadonnées de catalogue.  
  
> [!NOTE]  
>  L'étendue de la notification d'événements qui est créée par le fournisseur WMI (serveur, base de données ou objet) dépend finalement du résultat du processus de vérification des autorisations qui est utilisé par le fournisseur WMI. Cela est affecté par le jeu d'autorisations de l'utilisateur qui appelle le fournisseur et sur la vérification de la base de données interrogée.  
>   
>  Dans l'exemple précédent, le fournisseur commence par essayer de créer une notification d'événements dont l'étendue est la base de données (`ON DATABASE`). Si le fournisseur vérifie que la base de données existe et que l'appelant possède les autorisations requises pour créer une notification d'événements dessus, l'inscription réussit. Si l'inscription ne réussit pas, le fournisseur essaie de créer une notification d'événements sur le serveur (`ON SERVER`). En supposant que cette tentative réussit, tous les événements `ALTER_TABLE` qui se produisent sur le serveur sont envoyés du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au processus du service WMI. Toutefois, le fournisseur élimine par filtrage tous les événements qui ne s'appliquent pas à la base de données `AdventureWorks`. Bien que ce processus augmente potentiellement la quantité de trafic réseau nécessaire pour l'étendue de l'événement, il vous apporte également la souplesse d'enregistrer des requêtes WQL sur les bases de données avant qu'elles soient créées, puis de recevoir les données d'événement après la création de la base de données et le démarrage de l'activité DDL sur cette dernière.  
  
## <a name="permissions-and-message-verification"></a>Autorisations et vérification de message  
 Le fournisseur WMI n'envoie pas de messages pour les notifications d'événements si les deux conditions suivantes sont vraies :  
  
-   L'utilisateur qui a créé la notification d'événements par le biais du fournisseur WMI n'existe plus dans la base de données ou ne possède plus l'autorisation requise pour créer une notification d'événements semblable.  
  
-   Les notifications d'événements sont créées sur les événements suivants :  
  
    -   DROP_LOGIN  
  
    -   ALTER_LOGIN  
  
    -   DROP_USER  
  
    -   ALTER_USER  
  
    -   ADD_ROLE_MEMBER  
  
    -   DROP_ROLE_MEMBER  
  
    -   ADD_SERVER_ROLE_MEMBER  
  
    -   DROP_SERVER_ROLE_MEMBER  
  
    -   DENY ou REVOKE (S'applique uniquement aux autorisations ALTER DATABASE, ALTER ANY DATABASE EVENT NOTIFICATION, CREATE DATABASE DDL EVENT NOTIFICATION, CONTROL SERVER, ALTER ANY EVENT NOTIFICATION, CREATE DDL EVENT NOTIFICATION ou CREATE TRACE EVENT NOTIFICATION.)  
  
## <a name="working-with-event-data-on-the-client-side"></a>Utilisation de données d'événements côté client  
 Lorsque le fournisseur WMI pour les événements de serveur crée la notification d’événements requise dans la base de données cible, la notification d’événements envoie les données d’événement au service cible dans msdb, nommé **SQL/Notifications/ProcessWMIEventProviderNotification/v1.0**. Le service cible place l’événement dans une file d’attente dans **msdb** qui est nommé **WMIEventProviderNotificationQueue**. (Le service et la file d’attente sont créés de façon dynamique par le fournisseur lors de sa première connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].) Le fournisseur lit ensuite les données d'événement XML de cette file d'attente et les convertit au format MOF avant de les retourner à l'application cliente. Les données MOF sont composées des propriétés de l'événement demandé par la requête WQL comme une définition de classe CIM (Common Information Model). Chaque propriété a un type CIM correspondant. Par exemple, le `SPID` propriété est retournée en tant que type CIM **Sint32**. Les types CIM pour chaque propriété sont répertoriés sous chaque classe d’événements dans [fournisseur WMI pour les Classes d’événements de serveur et les propriétés](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-classes-and-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fournisseur WMI pour les concepts des événements de serveur](http://technet.microsoft.com/library/ms180560.aspx)  
  
  
