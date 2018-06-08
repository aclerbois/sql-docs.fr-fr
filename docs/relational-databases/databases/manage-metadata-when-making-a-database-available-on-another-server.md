---
title: Gérer les métadonnées lors de la mise à disposition d’une base de données sur un autre serveur | Microsoft Docs
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- cross-database queries [SQL Server]
- logins [SQL Server], recreating on another server instance
- triggers [SQL Server], DLL
- user-defined error messages [SQL Server]
- permissions [SQL Server], metadata access
- Full-Text Engine [SQL Server]
- metadata [SQL Server], databases available to other instances
- jobs [SQL Server Agent], recreating on another server instance
- failover [SQL Server], managing metadata
- event notifications [SQL Server], metadata
- database mirroring [SQL Server], metadata
- startup options [SQL Server]
- restoring [SQL Server], onto another server instance
- linked servers [SQL Server], metadata
- WMI Provider for Server Events, metadata
- attaching databases [SQL Server]
- log shipping [SQL Server], metadata
- encryption [SQL Server], metadata
- server configuration [SQL Server]
- distributed queries [SQL Server], metadata
- extended stored procedures [SQL Server], metadata
- credentials [SQL Server], metadata
- copying databases
ms.assetid: 5d98cf2a-9fc2-4610-be72-b422b8682681
caps.latest.revision: 84
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a0b300bc3f204af062eac1e151933659216dd921
ms.sourcegitcommit: 38f8824abb6760a9dc6953f10a6c91f97fa48432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="manage-metadata-when-making-a-database-available-on-another-server"></a>Gérer les métadonnées lors de la mise à disposition d’une base de données sur un autre serveur
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cet article est utile dans les cas suivants :  
  
-   Configuration des réplicas de disponibilité d'un groupe de disponibilité [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] .  
  
-   lors de la configuration de la mise en miroir d'une base de données ;  
  
-   lorsque vous vous préparez à intervertir les rôles des serveurs primaire et secondaire dans une configuration de la copie des journaux de transactions ;  
  
-   lors de la restauration d'une base de données sur une autre instance de serveur ;  
  
-   lors de l'attachement d'une copie d'une base de données sur une autre instance de serveur.  
  
 Certaines applications dépendent d'informations, d'entités et/ou d'objets qui n'appartiennent pas au champ d'action d'une base de données mono-utilisateur. Généralement, une application possède des dépendances sur les bases de données **master** et **msdb** ainsi que la base de données utilisateur. Les informations stockées en dehors d'une base de données utilisateur et nécessaires au bon fonctionnement de cette base de données doivent être disponibles sur l'instance du serveur de destination. Par exemple, les connexions pour une application sont stockées en tant que métadonnées dans la base de données **master** , et doivent être recréées sur le serveur de destination. Si un plan de maintenance d’application ou de base de données dépend des travaux de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dont les métadonnées sont stockées dans la base de données **msdb** , vous devez recréer ces travaux sur l’instance du serveur de destination. De la même façon, les métadonnées d’un déclencheur de niveau serveur sont stockées dans la base de données **master**.  
  
 Lorsque vous déplacez la base de données d’une application vers une autre instance de serveur, vous devez recréer toutes les métadonnées des entités et des objets dépendants dans les bases de données **master** et **msdb** sur l’instance du serveur de destination. Par exemple, si une application de base de données utilise des déclencheurs au niveau serveur, il ne suffit pas d'attacher ou de restaurer la base de données sur le nouveau système. La base de données ne fonctionne pas comme prévu, sauf si vous recréez manuellement les métadonnées pour ces déclencheurs dans la base de données **master** .  
  
##  <a name="information_entities_and_objects"></a> Informations, entités, et objets stockés en dehors des bases de données utilisateur  
 La suite de cet article résume les problèmes susceptibles d’affecter une base de données qui est mise à disposition sur une autre instance de serveur. Vous devrez peut-être recréer un ou plusieurs des types d'informations, d'entités ou d'objets répertoriés dans la liste suivante. Pour consulter un résumé, cliquez sur le lien correspondant à l'élément.  
  
-   [Paramètres de configuration du serveur](#server_configuration_settings)  
  
-   [Informations d'identification](#credentials)  
  
-   [Requêtes de bases de données croisées](#cross_database_queries)  
  
-   [Propriété de la base de données](#database_ownership)  
  
-   [Requêtes distribuées/serveurs liés](#distributed_queries_and_linked_servers)  
  
-   [Données chiffrées](#encrypted_data)  
  
-   [Messages d'erreur définis par l'utilisateur](#user_defined_error_messages)  
  
-   [Notifications d'événements et événements WMI (Windows Management Instrumentation) (au niveau du serveur)](#event_notif_and_wmi_events)  
  
-   [Procédures stockées étendues](#extended_stored_procedures)  
  
-   [Moteur d'indexation et de recherche en texte intégral pour les propriétés SQL Server](#ifts_service_properties)  
  
-   [Travaux](#jobs)  
  
-   [Connexions](#logins)  
  
-   [Autorisations](#permissions)  
  
-   [Paramètres de réplication](#replication_settings)  
  
-   [applications Service Broker](#sb_applications)  
  
-   [Procédures de démarrage](#startup_procedures)  
  
-   [Déclencheurs (au niveau du serveur)](#triggers)  
  
##  <a name="server_configuration_settings"></a> Server Configuration Settings  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures installent et démarrent sélectivement les services et les fonctionnalités clés. Ainsi, la surface d'exposition vulnérable aux attaques d'un système est réduite. Dans la configuration par défaut des nouvelles installations, de nombreuses fonctionnalités ne sont pas activées. Si la base de données repose sur une fonctionnalité ou un service qui est désactivé par défaut, cette fonction ou ce service doit être activé sur l'instance du serveur de destination.  
  
 Pour plus d’informations sur ces paramètres et leur activation ou désactivation, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
  
##  <a name="credentials"></a> Informations d'identification  
 Les informations d'identification sont un enregistrement qui contient les informations d'authentification requises pour la connexion à une ressource en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La plupart des informations d'identification sont composées d'un nom de connexion Windows et d'un mot de passe.  
  
 Pour plus d’informations sur cette fonctionnalité, consultez [Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md).  
  
> **REMARQUE : Les comptes proxy de l’Agent**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent des informations d’identification. Pour connaître les informations d'identification d'un compte proxy, utilisez la table système [sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md) .  
  
  
##  <a name="cross_database_queries"></a> Cross-Database Queries  
 Les options de base de données DB_CHAINING et TRUSTWORTHY sont désactivées par défaut. Si elles sont définies sur ON pour la base de données d’origine, vous devrez peut-être les activer sur la base de données de l’instance du serveur de destination. Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 Les opérations d'attachement et de détachement désactivent le chaînage des propriétés des bases de données croisées pour la base de données. Pour plus d’informations sur l’activation du chaînage, consultez [Chaînage des propriétés des bases de données croisées (option de configuration de serveur)](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md).  
  
 Pour plus d’informations, consultez également [Configurer une base de données miroir pour utiliser la propriété Trustworthy &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/set-up-a-mirror-database-to-use-the-trustworthy-property-transact-sql.md).  
  
  
##  <a name="database_ownership"></a> Database Ownership  
 Lorsqu'une base de données est restaurée sur un autre ordinateur, la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l'utilisateur Windows à l'origine de l'opération de restauration devient automatiquement le propriétaire de la nouvelle base de données. Lorsque la base de données est restaurée, l'administrateur du système ou le nouveau propriétaire de la base de données peut modifier la propriété de la base de données.  
  
##  <a name="distributed_queries_and_linked_servers"></a> Requêtes distribuées et serveurs liés  
 Les requêtes distribuées et les serveurs liés sont pris en charge pour les applications OLE DB. Les requêtes distribuées accèdent aux données issues de multiples sources de données hétérogènes sur le même ordinateur ou sur des ordinateurs différents. Une configuration de serveurs liés permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'exécuter des commandes sur les sources de données OLE DB hébergées sur des serveurs distants. Pour plus d’informations sur ces fonctionnalités, consultez [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md).  
  
  
##  <a name="encrypted_data"></a> Encrypted Data  
 Si la base de données que vous mettez à disposition sur une autre instance du serveur contient des données chiffrées et si la clé principale de base de données est protégée par la clé principale du service sur le serveur d'origine, il peut être nécessaire de recréer le chiffrement à clé principale du service. La *clé principale de base de données* est une clé symétrique qui permet de protéger les clés privées des certificats et les clés asymétriques dans une base de données chiffrée. Lors de sa création, la clé principale de base de données est chiffrée à l'aide de l'algorithme Triple DES et d'un mot de passe fourni par l'utilisateur.  
  
 Pour permettre le déchiffrement automatique de la clé principale de base de données sur une instance de serveur, une copie de cette clé est chiffrée à l'aide de la clé principale du service. Cette copie chiffrée est stockée dans la base de données et dans la base **master**. En général, la copie stockée dans **master** est mise à jour sans avertissement chaque fois que la clé principale est modifiée. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente tout d'abord de déchiffrer la clé principale de base de données avec la clé principale de service de l'instance. Si ce déchiffrement échoue, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche dans la banque d'informations d'identification les informations d'identification de clé principale qui possèdent le même GUID de famille que la base de données pour laquelle la clé principale est requise. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente ensuite de déchiffrer la clé principale de base de données avec toutes les informations d'identification correspondantes, jusqu'à ce que le déchiffrement réussisse ou qu'il ne reste plus d'informations d'identification. Une clé principale qui n'est pas chiffrée par la clé principale de service doit être ouverte à l'aide de l'instruction OPEN MASTER KEY et d'un mot de passe.  
  
 Lorsqu'une base de données chiffrée est copiée, restaurée ou attachée à une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une copie de la clé principale de la base de données chiffrée par la clé principale du service n'est pas stockée dans la base **master** sur l'instance du serveur de destination. Sur l'instance du serveur de destination, vous devez ouvrir la clé principale de la base de données. Pour ouvrir la clé principale, exécutez l’instruction suivante : OPEN MASTER KEY DECRYPTION BY PASSWORD **='***mot_de_passe***'**. Nous vous recommandons d'activer alors le déchiffrement automatique de la clé principale de la base de données en exécutant l'instruction suivante : ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY. Cette instruction ALTER MASTER KEY fournit à l'instance du serveur une copie de la clé principale de la base de données qui est chiffrée à l'aide de la clé principale du service. Pour plus d’informations, consultez [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md) et [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md).  
  
 Pour plus d’informations sur l’activation du déchiffrement automatique de la clé principale d’une base de données miroir, consultez [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md).  
  
 Pour plus d'informations, consultez également :  
  
-   [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
-   [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)  
  
-   [Créer des clés symétriques identiques sur deux serveurs](../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
  
##  <a name="user_defined_error_messages"></a> User-defined Error Messages  
 Les messages d’erreur définis par l’utilisateur résident dans l’affichage catalogue [sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md) . Cet affichage catalogue est stocké dans la base de données **master**. Si une application de base de données dépend des messages d’erreur définis par l’utilisateur et que la base de données est mise à disposition sur une autre instance du serveur, utilisez [sp_addmessage](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md) pour ajouter ces messages définis par l’utilisateur sur l’instance du serveur de destination.  

  
##  <a name="event_notif_and_wmi_events"></a> Event Notifications and Windows Management Instrumentation (WMI) Events (at Server Level)  
  
### <a name="server-level-event-notifications"></a>Notifications d'événements au niveau du serveur  
 Les notifications d’événements au niveau du serveur sont stockées dans la base de données **msdb**. Par conséquent, si une application de base de données repose sur une notification d’événement au niveau du serveur, cette notification doit être recréée sur l’instance du serveur de destination. Pour afficher les notifications d’événements sur une instance de serveur, utilisez l’affichage catalogue [sys.server_event_notifications](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md) . Pour plus d'informations, voir [Event Notifications](../../relational-databases/service-broker/event-notifications.md).  
  
 De plus, les notifications d'événements sont remises à l'aide de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Les itinéraires pour les messages entrants ne sont pas inclus dans la base de données qui contient un service. Au lieu de cela, les itinéraires explicites sont stockés dans la base de données **msdb**. Si, pour acheminer les messages entrants jusqu’au service, votre service utilise un itinéraire explicite dans la base de données **msdb** , vous devez recréer cet itinéraire lorsque vous attachez une base de données dans une instance différente.  
  
### <a name="windows-management-instrumentation-wmi-events"></a>Événements WMI (Windows Management Instrumentation)  
 Le fournisseur WMI pour les événements de serveur vous permet d'utiliser WMI (Windows Management Instrumentation) pour surveiller les événements dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutes les applications qui reposent sur des événements au niveau du serveur exposés par le biais du fournisseur WMI (Windows Management Instrumentation) sur lequel repose une base de données doivent être définies sur l'ordinateur de l'instance du serveur de destination. Le fournisseur d'événements WMI crée des notifications d'événements à l'aide d'un service cible défini dans **msdb**.  
  
> **REMARQUE :** Pour plus d’informations, consultez [Fournisseur WMI pour les concepts des événements de serveur](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md).  
  
 **Pour créer une alerte WMI à l'aide de SQL Server Management Studio**  
  
-   [Créer une alerte d'événement WMI](http://msdn.microsoft.com/library/b8c46db6-408b-484e-98f0-a8af3e7ec763)  
  
### <a name="how-event-notifications-work-for-a-mirrored-database"></a>Fonctionnement des notifications d'événements pour une base de données miroir  
 La remise de notifications d'événements entre bases de données qui implique une base de données mise en miroir est distante, par définition, car la base de données mise en miroir peut basculer. [!INCLUDE[ssSB](../../includes/sssb-md.md)] assure une prise en charge spéciale des bases de données mises en miroir, sous la forme d' *itinéraires mis en miroir*. Un itinéraire mis en miroir possède deux adresses : celle de l'instance du serveur principal et celle de l'instance du serveur miroir.  
  
 En configurant des itinéraires mis en miroir, vous rendez le routage [!INCLUDE[ssSB](../../includes/sssb-md.md)] capable de reconnaître la mise en miroir des bases de données. Les itinéraires mis en miroir permettent à [!INCLUDE[ssSB](../../includes/sssb-md.md)] de rediriger de manière transparente les conversations vers l'instance de serveur principal en cours. Par exemple, imaginons un service, Service_A, hébergé par une base de données mise en miroir, Database_A. Supposons que vous ayez besoin qu'un autre service, Service_B, hébergé par Database_B, dialogue avec Service_A. Pour ce faire, Database_B doit posséder un itinéraire mis en miroir pour Service_A. Database_A doit aussi contenir un itinéraire de transport TCP non mis en miroir vers Service_B, qui, contrairement à un itinéraire local, reste valide après un basculement. Ces itinéraires permettent aux accusés de réception de revenir après un basculement. Comme le service de l'expéditeur est toujours nommé de la même manière, l'itinéraire doit spécifier l'instance de broker.  
  
 L'exigence définie pour les itinéraires mis en miroir est valable que le service de la base de données mise en miroir soit le service initiateur ou le service cible :  
  
-   Si le service cible est dans la base de données mise en miroir, le service initiateur doit avoir un itinéraire mis en miroir qui ramène à la cible. Cependant, la cible peut avoir un itinéraire standard qui ramène à l'initiateur.  
  
-   Si le service initiateur est dans la base de données miroir, le service cible doit avoir un itinéraire mis en miroir qui ramène à l'initiateur pour remettre les accusés et les réponses. Cependant, l'initiateur peut avoir un itinéraire standard qui ramène à la cible.  
  
  
##  <a name="extended_stored_procedures"></a> Extended Stored Procedures  
  
> **IMPORTANT !** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez l’ [intégration CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) à la place.  
  
 Les procédures stockées étendues sont programmées à l'aide de l'API de procédure stockée étendue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un membre du rôle serveur fixe **sysadmin** peut enregistrer une procédure stockée étendue avec une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis octroyer à d'autres utilisateurs l'autorisation d'exécuter la procédure. Les procédures stockées étendues ne peuvent être ajoutées qu'à la base de données **master** .  
  
 Les procédures stockées étendues s'exécutent directement dans l'espace d'adressage d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et elles peuvent générer des fuites de mémoire ou d'autres problèmes qui diminuent les performances et la fiabilité du serveur. Il est préférable de stocker les procédures stockées étendues dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distincte de celle qui contient les données de référence et d'utiliser des requêtes distribuées pour accéder à la base de données.  
  
  > [!IMPORTANT]
  > Avant d'ajouter des procédures stockées étendues au serveur et d'accorder à d'autres utilisateurs des autorisations d'exécution (EXECUTE), il est conseillé à l'administrateur système de vérifier minutieusement chaque procédure stockée étendue afin de s'assurer qu'elle ne contient aucun code nuisible ou malveillant.  
  
 Pour plus d’informations, consultez [Autorisations d’objet GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md), [Autorisations d’objet DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md) et [Autorisations d’objet REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md).  
  
  
##  <a name="ifts_service_properties"></a> Full-Text Engine for SQL Server Properties  
 Les propriétés sont définies sur le moteur de texte intégral par [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md). Vérifiez que l'instance du serveur de destination possède les paramètres requis pour ces propriétés. Pour plus d’informations sur ces propriétés, consultez [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md).  
  
 Par ailleurs, si le composant des [analyseurs lexicaux et générateurs de formes dérivées](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) ou le composant des [filtres de recherche en texte intégral](../../relational-databases/search/configure-and-manage-filters-for-search.md) ont des versions différentes sur les instances du serveur d’origine et de destination, il est possible que les requêtes et l’index de texte intégral n’aient pas le même comportement. En outre, le [dictionnaire des synonymes](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md) est stocké dans des fichiers spécifiques à l’instance. Vous devez transférer une copie de ces fichiers à un emplacement équivalent sur l'instance du serveur de destination ou les recréer sur la nouvelle instance.  
  
> **REMARQUE :** Quand vous attachez une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] qui contient des fichiers catalogue de texte intégral à une instance de serveur [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , les fichiers catalogue sont attachés à partir de leur emplacement précédent avec les autres fichiers de base de données, les mêmes que dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations, consultez [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).  
  
 Pour plus d'informations, consultez également :  
  
-   [Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
-   [Mise en miroir de bases de données et catalogues de texte intégral &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)  

  
##  <a name="jobs"></a> Travaux  
 Si la base de données repose sur les travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devrez recréer ces derniers sur l'instance du serveur de destination. Les travaux dépendent de leur environnement. Si vous prévoyez de recréer un travail existant sur l'instance du serveur de destination, cette dernière devra peut-être être modifiée pour correspondre à l'environnement de ce travail sur l'instance du serveur d'origine. Les éléments d'environnement ci-après sont importants :  
  
-   Connexion utilisée par le travail  
  
     Pour créer ou exécuter des travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez d'abord ajouter toutes les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requises par le travail à l'instance du serveur de destination. Pour plus d’informations, consultez [Configurer un utilisateur de manière à créer et à gérer des travaux de l’Agent SQL Server](http://msdn.microsoft.com/library/67897e3e-b7d0-43dd-a2e2-2840ec4dd1ef).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent  
  
     Le compte de démarrage du service définit le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows dans le contexte duquel l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, ainsi que ses autorisations réseau. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute dans le contexte d'un compte d'utilisateur spécifié. Le contexte du service de l'Agent affecte les paramètres du travail et son environnement d'exécution. Le compte doit avoir accès aux ressources, telles que les partages réseau, requises par le travail. Pour plus d’informations sur la façon de sélectionner et de modifier le compte de démarrage du service, consultez [Sélectionner un compte pour le service SQL Server Agent](http://msdn.microsoft.com/library/fe658e32-9e6b-4147-a189-7adc3bd28fe7).  
  
     Le bon fonctionnement du compte de démarrage du service repose sur une configuration correcte du domaine, du système de fichiers et des autorisations de Registre. Qui plus est, un travail peut nécessiter une ressource réseau partagée qui doit être configurée pour le compte du service. Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, qui est associé à une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], possède sa propre ruche de Registre, et ses travaux dépendent généralement d'un ou de plusieurs paramètres de cette ruche de Registre. Pour qu'un travail fonctionne comme prévu, il a besoin de ces paramètres du Registre. Si vous utilisez un script pour recréer un travail dans un autre service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent, il est possible que son Registre ne dispose pas des paramètres corrects pour ce travail. Pour que les travaux recréés fonctionnent correctement sur une instance du serveur de destination, les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent d'origine et de destination doivent posséder les mêmes paramètres de Registre.  
  
    > [!CAUTION]  
    >  La modification des paramètres du Registre sur le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent de destination afin de gérer un travail recréé peut être problématique si les paramètres actuels sont requis par d'autres travaux. En outre, une modification incorrecte du Registre peut sérieusement endommager votre système. Avant que vous apportiez des modifications au Registre, nous vous recommandons de sauvegarder toutes les données importantes qui se trouvent sur l'ordinateur.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Proxys d’Agent  
  
     Un proxy d'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit le contexte de sécurité d'une étape de travail spécifiée. L'exécution d'un travail sur l'instance du serveur de destination implique la recréation manuelle de tous les proxys nécessaires sur cette instance. Pour plus d’informations, consultez [Créer un proxy d’Agent SQL Server](http://msdn.microsoft.com/library/142e0c55-a8b9-4669-be49-b9dc602d5988) et [Résoudre les problèmes liés aux travaux multiserveurs qui utilisent des proxys](http://msdn.microsoft.com/library/fc579bd3-010c-4f72-8b5c-d0cc18a1f280).  
  
 Pour plus d'informations, consultez également :  
  
-   [Implémenter des travaux](http://msdn.microsoft.com/library/69e06724-25c7-4fb3-8a5b-3d4596f21756)  
  
-   [Gestion des connexions et des travaux après un basculement de rôle &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md) (pour la mise en miroir de bases de données)  
  
-   [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md) (quand vous installez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
  
-   [Configurer l’Agent SQL Server](http://msdn.microsoft.com/library/2e361a62-9e92-4fcd-80d7-d6960f127900) (quand vous installez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
  
-   [Implémenter la sécurité de l'Agent SQL Server](http://msdn.microsoft.com/library/d770d35c-c8de-4e00-9a85-7d03f45a0f0d)  
  
 **Pour afficher des travaux existants et leurs propriétés**  
  
-   [Surveiller l'activité des travaux](http://msdn.microsoft.com/library/71cb432b-631d-4b8b-9965-e731b3d8266d)  
  
-   [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)  
  
-   [Afficher des informations sur une étape de travail](http://msdn.microsoft.com/library/e3f06492-dc86-4e06-b186-ea58aff6d591)  
  
-   [dbo.sysjobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
  
 **Pour créer un travail**  
  
-   [Créer un travail](http://msdn.microsoft.com/library/b35af2b6-6594-40d1-9861-4d5dd906048c)  
  
-   [Créer un travail](http://msdn.microsoft.com/library/b35af2b6-6594-40d1-9861-4d5dd906048c)  
  
#### <a name="best-practices-for-using-a-script-to-re-create-a-job"></a>Meilleures pratiques pour utiliser un script afin de recréer un travail  
 Nous vous conseillons de commencer par générer le script d'un travail simple, en recréant le travail sur l'autre service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent et en exécutant le travail pour vérifier qu'il fonctionne comme prévu. Vous pourrait ainsi identifier les incompatibilités éventuelles et essayer de les résoudre. Si un travail faisant l'objet d'un script ne fonctionne pas comme prévu dans son nouvel environnement, nous vous conseillons de créer un travail équivalent qui fonctionne correctement dans cet environnement.  
  

##  <a name="logins"></a> Connexions  
 Une ouverture de session sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide. Cette connexion est utilisée dans le processus d'authentification qui vérifie que le principal peut se connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un utilisateur de base de données pour lequel la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] correspondante n'est pas définie sur une instance serveur, ou l'est de façon incorrecte, ne peut pas se connecter à cette instance. L'utilisateur devient donc un *utilisateur orphelin* de la base de données sur cette instance du serveur. Un utilisateur de base de données peut devenir orphelin lorsqu'une base de données a été restaurée, attachée ou copiée sur une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour générer un script pour une partie ou l'intégralité des objets figurant dans la copie initiale de la base de données, vous pouvez utiliser l'Assistant Générer des scripts, et dans la boîte de dialogue **Sélectionner les options de script** , vous attribuez à l'option **Créer un script des connexions** la valeur **True**.  
  
> **REMARQUE :** Pour plus d’informations sur la configuration des connexions pour une base de données mise en miroir, consultez [Configurer des comptes de connexion pour la mise en miroir de bases de données ou les groupes de disponibilité Always On (SQL Server)](../../database-engine/database-mirroring/set-up-login-accounts-database-mirroring-always-on-availability.md) et [Gestion des connexions et des travaux après un basculement de rôle &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md).  
  
  
##  <a name="permissions"></a> Autorisations  
 Les types d'autorisations suivants peuvent être affectés par la mise à disponibilité d'une base de données sur une autre instance de serveur.  
  
-   Autorisations GRANT, REVOKE ou DENY sur les objets système  
  
-   Autorisations GRANT, REVOKE ou DENY sur une instance de serveur (*autorisations au niveau du serveur*)  
  
### <a name="grant-revoke-and-deny-permissions-on-system-objects"></a>Autorisations GRANT, REVOKE et DENY sur les objets système  
 Les autorisations sur les objets système, tels que les procédures stockées, les procédures stockées étendues, les fonctions et les affichages, sont stockées dans la base de données **master** et doivent être configurées sur l'instance du serveur de destination.  
  
 Pour générer un script pour une partie ou l'intégralité des objets figurant dans la copie initiale de la base de données, vous pouvez utiliser l'Assistant Générer des scripts, et dans la boîte de dialogue **Sélectionner les options de script** , vous attribuez à l'option **Créer un script des autorisations au niveau objet** la valeur **True**.  
  
   > [!IMPORTANT]
   > Si vous générez un script pour les connexions, les mots de passe ne sont pas chiffrés. Si vous disposez de connexions qui utilisent l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez modifier le script sur la destination.  
  
 Les objets système sont consultables dans l’affichage catalogue [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) . Les autorisations sur les objets système sont consultables dans l’affichage catalogue [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) dans la base de données **master**. Pour plus d’informations sur l’interrogation de ces affichages catalogue et l’octroi d’autorisations sur les objets système, consultez [Autorisations d’objet système GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md). Pour plus d’informations, consultez [Autorisations d’objet système REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md) et [Autorisations d’objet système DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-system-object-permissions-transact-sql.md).  
  
### <a name="grant-revoke-and-deny-permissions-on-a-server-instance"></a>Autorisations GRANT, REVOKE et DENY sur une instance de serveur  
 Les autorisations à l'échelle du serveur sont stockées dans la base de données **master** et doivent être configurées sur l'instance du serveur de destination. Pour plus d’informations sur les autorisations serveur d’une instance de serveur, interrogez l’affichage catalogue [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) ; pour plus d’informations sur les principaux de serveur, interrogez l’affichage catalogue [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) et pour plus d’informations sur l’appartenance aux rôles de serveur, interrogez l’affichage catalogue [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Pour plus d’informations, consultez [Autorisations de serveur GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md), [Autorisations de serveur REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md) et [Autorisations de serveur DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md).  
  
#### <a name="server-level-permissions-for-a-certificate-or-asymmetric-key"></a>Autorisations au niveau serveur pour un certificat ou une clé asymétrique  
 Les autorisations au niveau serveur ne peuvent pas être accordées directement à un certificat ou à une clé asymétrique. Les autorisations au niveau serveur sont en fait accordées à une connexion mappée qui est créée exclusivement pour un certificat ou une clé asymétrique spécifique. Par conséquent, chaque certificat ou clé asymétrique qui nécessite des autorisations au niveau serveur doit posséder sa propre *connexion mappée sur un certificat* ou sa propre *connexion mappée sur une clé asymétrique*. Pour octroyer des autorisations au niveau serveur pour un certificat ou une clé asymétrique, accordez les autorisations à sa connexion mappée.  
  
> **REMARQUE :** Une connexion mappée est utilisée uniquement pour l’autorisation du code signé à l’aide de la clé asymétrique ou du certificat correspondant. Les connexions mappées ne peuvent pas être utilisées pour l'authentification.  
  
 La connexion mappée et ses autorisations résident dans la base de données **master**. Si une clé asymétrique ou un certificat réside dans une base de données autre que la base **master**, vous devez les recréer dans la base **master** et les mapper à une connexion. Si vous déplacez, copiez ou restaurez la base de données sur une autre instance de serveur, vous devez recréer son certificat ou sa clé asymétrique dans la base de données **master** de l’instance du serveur de destination, les mapper à une connexion et accorder les autorisations au niveau serveur à la connexion.  
  
 **Pour créer un certificat ou une clé asymétrique**  
  
-   [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
 **Pour mapper un certificat ou une clé asymétrique à une connexion**  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)  
  
 **Pour accorder des autorisations à la connexion mappée**  
  
-   [Autorisations de serveur GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)  
  
 Pour plus d'informations sur les certificats et les clés asymétriques, consultez [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
## <a name="trustworthy-property"></a>Propriété Trustworthy
La propriété de base de données TRUSTWORTHY permet d’indiquer si cette instance de SQL Server fait confiance à la base de données et à son contenu. Quand une base de données est attachée, par défaut et pour des raisons de sécurité, cette option est définie sur OFF, même si elle était définie sur ON sur le serveur d’origine. Pour plus d’informations sur cette propriété, consultez [TRUSTWORTHY, propriété de base de données](../security/trustworthy-database-property.md). Pour savoir comment activer cette option, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  


##  <a name="replication_settings"></a> Replication Settings  
 Si vous restaurez une sauvegarde d'une base de données répliquée sur un autre serveur ou dans une autre base de données, les paramètres de réplication ne peuvent pas être conservés. Dans ce cas, vous devez recréer la totalité des abonnements et des publications une fois les sauvegardes restaurées. Pour faciliter ce processus, créez des scripts pour vos paramètres de réplication actuels, et également pour l'activation et la désactivation de la réplication. Pour recréer facilement vos paramètres de réplication, copiez ces scripts et modifiez les références de nom de serveur afin qu'elles fonctionnent pour l'instance du serveur de destination.  
  
 Pour plus d’informations, consultez [Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md), [ Mise en miroir de bases de données et réplication &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md) et [Copie des journaux de transaction et réplication &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
  
##  <a name="sb_applications"></a> Service Broker Applications  
 De nombreux aspects d'une application [!INCLUDE[ssSB](../../includes/sssb-md.md)] sont déplacés avec la base de données. Cependant, certains aspects doivent être recréés ou reconfigurés dans le nouvel emplacement.  Par défaut et pour des raisons de sécurité, quand une base de données est attachée à partir d’un autre serveur, les options pour *is_broker_enabled*, *is_honor_broker_priority_on* sont définies sur OFF. Pour plus d’informations sur l’activation de ces options, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
  
##  <a name="startup_procedures"></a> Startup Procedures  
 Une procédure de démarrage est une procédure stockée qui est marquée pour l'exécution automatique et qui est exécutée à chaque démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si la base de données dépend de procédures de démarrage, celles-ci doivent être définies sur l'instance du serveur de destination et configurées pour s'exécuter automatiquement au démarrage.  

  
##  <a name="triggers"></a> Triggers (at Server Level)  
 Les déclencheurs DDL lancent des procédures stockées en réponse à différents événements DDL (Data Definition Language). Ces événements correspondent principalement à des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui commencent avec les mots clés CREATE, ALTER et DROP. Certaines procédures stockées système qui effectuent des opérations de type DDL peuvent également activer des déclencheurs DDL.  
  
 Pour plus d'informations sur cette fonctionnalité, consultez [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md).  
  
  
## <a name="see-also"></a> Voir aussi  
 [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)   
 [Copier des bases de données sur d'autres serveurs](../../relational-databases/databases/copy-databases-to-other-servers.md)   
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Basculer vers une base de données secondaire de copie des journaux de transaction &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)   
 [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md)   
 [Dépanner des utilisateurs orphelins &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
  
