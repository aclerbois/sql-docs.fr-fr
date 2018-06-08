---
title: Gérer les services du moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, accessing
- Database Engine [SQL Server], services
- managing services [SQL Server], about service management
- services [SQL Server]
- SQL Server Agent service, managing
- SQL Server services, about SQL Server service
- MSSQLServer
- server configuration [SQL Server]
- managing services [SQL Server]
- SQL Server Agent service
- services [SQL Server], managing
- administering SQL Server, services
- SQL Server services
ms.assetid: aa732e43-53ba-4eea-bb9b-089da0766fc1
caps.latest.revision: 43
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b3275cf1deed00bb838144825b8cef243a6aa8e8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-the-database-engine-services"></a>Gérer les services du moteur de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute sur les systèmes d'exploitation en tant que service. Un service est un type d'application qui s'exécute à l'arrière-plan du système. Les services fournissent généralement les fonctions essentielles du système d'exploitation, telles que la gestion Web, l'enregistrement des événements ou la gestion de fichiers. Les services ne peuvent pas s'exécuter sans afficher une interface utilisateur sur le bureau de l'ordinateur. Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent et plusieurs autres composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctionnent en tant que services. Ces services sont généralement démarrés en même temps que le système d'exploitation. Tout dépend de ce qui est spécifié durant l'installation ; certains services ne sont pas démarrés par défaut. Cette section décrit la gestion des différents services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Avant de vous connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez savoir comment démarrer, arrêter, interrompre et reprendre une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une fois connecté, vous pouvez exécuter diverses tâches telles que l'administration du serveur ou l'interrogation d'une base de données.  
  
## <a name="using-the-sql-server-service"></a>Utilisation du service SQL Server  
 Lorsque vous démarrez une instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], vous démarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Après le démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les utilisateurs peuvent établir de nouvelles connexions au serveur. Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être démarré et arrêté de manière identique à un service, localement ou à distance. Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fait référence à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER) s’il s’agit de l’instance par défaut, ou MSSQL$*\<nom_instance>* s’il s’agit d’une instance nommée.  
  
## <a name="using-sql-server-configuration-manager"></a>Utilisation du Gestionnaire de configuration SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous permet d'arrêter, de démarrer ou d'interrompre divers services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas gérer les services [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] .  
  
 Vous pouvez également utiliser le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour afficher les propriétés du service sélectionné. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Le Gestionnaire de configuration est un composant logiciel enfichable MMC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console). Pour plus d’informations sur MMC et le fonctionnement d’un composant logiciel enfichable, consultez l’aide de Windows.  
  
 **Pour accéder au Gestionnaire de configuration SQL Server**  
  
-   Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
 **Pour accéder au Gestionnaire de configuration SQL Server à l’aide de Windows 8**  
  
 Étant donné que le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un composant logiciel enfichable pour le programme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console, et non un programme autonome, le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'apparaît pas en tant qu'application lorsque vous exécutez Windows Server 8.0. Pour ouvrir le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans l’icône **Rechercher** , sous **Applications**, tapez **SQLServerManager12.msc** (pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]), **SQLServerManager11.msc** (pour [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) ou **SQLServerManager10.msc** pour ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]), et appuyez sur **Entrée**.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|||  
|-|-|  
|[Spécifications de sécurité pour la gestion des services](../../database-engine/configure-windows/security-requirements-for-managing-services.md)|[Empêcher le démarrage automatique d’une instance de SQL Server &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-prevent-automatic-startup-of-an-instance.md)|  
|[Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)|[Modifier le compte de démarrage du service pour SQL Server &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-change-the-service-startup-account.md)|  
|[Exécuter SQL Server avec ou sans réseau](../../database-engine/configure-windows/run-sql-server-with-or-without-a-network.md)|[Configurer les options de démarrage du serveur &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)|  
|[Service SQL Server Browser &#40;moteur de base de données et SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)|[Modifier le mot de passe des comptes utilisés par SQL Server &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-change-the-password-of-the-accounts-used.md)|  
|[Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md)|[Configurer des journaux d'erreurs SQL Server](../../database-engine/configure-windows/scm-services-configure-sql-server-error-logs.md)|  
|[Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)|[Modifier le mode d'authentification du serveur](../../database-engine/configure-windows/change-server-authentication-mode.md)|  
|[Démarrer SQL Server en mode mono-utilisateur](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)|[Service SQL Writer](../../database-engine/configure-windows/sql-writer-service.md)|  
|[Démarrage de SQL Server avec une configuration minimale](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)|[Diffuser un message d’arrêt &#40;invite de commandes&#41;](../../database-engine/configure-windows/broadcast-a-shutdown-message-command-prompt.md)|  
|[Se connecter à un autre ordinateur &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-connect-to-another-computer.md)|[Se connecter à une instance de SQL Server &#40;invite de commandes&#41;](../../database-engine/configure-windows/log-in-to-an-instance-of-sql-server-command-prompt.md)|  
|[Définir le démarrage automatique d’une instance de SQL Server &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-set-an-instance-to-start-automatically.md)|[Configurer les autorisations du système de fichiers pour l'accès au moteur de base de données](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md)|  
  
## <a name="related-content"></a>Contenu associé  
 [Configurer l'Agent SQL Server](http://msdn.microsoft.com/library/2e361a62-9e92-4fcd-80d7-d6960f127900)  
  
 [Connexion à SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md)  
  
  
