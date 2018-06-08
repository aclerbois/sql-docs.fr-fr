---
title: Connexion à SQL Server | Microsoft Docs
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
- SQL Server, logging in
- services [SQL Server], logging in
- TCP connection string
- connecting to the Database Engine
- logins [SQL Server], about logging in
- named pipe connection string
- log ins [SQL Server]
- shared memory connection string
- logging in [SQL Server]
- logins [SQL Server]
ms.assetid: 77158a9a-d638-4818-90a1-cb2eb57df514
caps.latest.revision: 34
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 769bb9418b3d631648f6f493aeb084b5fea0a619
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="logging-in-to-sql-server"></a>Connexion à SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez vous connecter à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de n’importe quel outil d’administration graphique ou d’une invite de commandes.  
  
 Lorsque vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide d’un outil d’administration graphique tel que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous êtes invité à fournir le nom du serveur, une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un mot de passe, si cela est nécessaire. Si vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant l'authentification Windows, vous n'avez pas besoin de fournir de compte de connexion SQL Server chaque fois que vous accédez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise alors votre compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows pour vous connecter automatiquement. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution dans un mode d’authentification mixte ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows) et que vous choisissez de vous connecter en utilisant l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez spécifier une connexion et un mot de passe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Lorsque c'est possible, utilisez l'authentification Windows.  
  
> [!NOTE]  
>  Si vous avez sélectionné un classement respectant la casse lors de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], votre connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] respecte également la casse.  
  
## <a name="format-for-specifying-the-name-of-sql-server"></a>Format à utiliser pour spécifier le nom de SQL Server  
 Lorsque vous vous connectez à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , vous devez spécifier le nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est l'instance par défaut (une instance sans nom), indiquez alors le nom de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé, ou l'adresse IP de cet ordinateur. Si l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une instance nommée (comme SQLEXPRESS), indiquez le nom de l'ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé, ou l'adresse IP de l'ordinateur, et ajoutez une barre oblique suivie du nom de l'instance.  
  
 Les exemples suivants permettent de se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui s'exécute sur un ordinateur nommé APPHOST. Si une instance nommée est spécifiée, ces exemples utilisent le nom d'instance SQLEXPRESS.  
  
 **Exemples :**  
  
|Type d'instance|Entrée pour le nom du serveur|  
|----------------------|-------------------------------|  
|Connexion à une instance par défaut en utilisant le protocole par défaut. (Il s'agit de l'entrée recommandée pour une instance par défaut.)|APPHOST|  
|Connexion à une instance nommée en utilisant le protocole par défaut. (Il s'agit de l'entrée recommandée pour une instance nommée.)|APPHOST\SQLEXPRESS|  
|Connexion à une instance par défaut sur le même ordinateur en utilisant un point pour indiquer que l'instance s'exécute sur l'ordinateur local.|.|  
|Connexion à une instance nommée sur le même ordinateur en utilisant un point pour indiquer que l'instance s'exécute sur l'ordinateur local.|.\SQLEXPRESS|  
|Connexion à une instance par défaut sur le même ordinateur en utilisant localhost pour indiquer que l'instance s'exécute sur l'ordinateur local.|localhost|  
|Connexion à une instance nommée sur le même ordinateur en utilisant localhost pour indiquer que l'instance s'exécute sur l'ordinateur local.|localhost\SQLEXPRESS|  
|Connexion à une instance par défaut sur le même ordinateur en utilisant (local) pour indiquer que l'instance s'exécute sur l'ordinateur local.|(local)|  
|Connexion à une instance nommée sur le même ordinateur en utilisant (local) pour indiquer que l'instance s'exécute sur l'ordinateur local.|(local)\SQLEXPRESS|  
|Connexion à une instance par défaut sur le même ordinateur en imposant une connexion de mémoire partagée.|lpc:APPHOST|  
|Connexion à une instance nommée sur le même ordinateur en imposant une connexion de mémoire partagée.|lpc:APPHOST\SQLEXPRESS|  
|Connexion à une instance par défaut écoutant l'adresse TCP 192.168.17.28 à l'aide d'une adresse IP.|192.168.17.28|  
|Connexion à une instance nommée écoutant sur l'adresse TCP 192.168.17.28 à l'aide d'une adresse IP.|192.168.17.28\SQLEXPRESS|  
|Connexion à une instance par défaut qui n'écoute pas sur le port TCP par défaut, en spécifiant le port utilisé, dans le cas présent 2828. (Cela n'est pas nécessaire si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] écoute sur le port par défaut (1433).)|APPHOST,2828|  
|Connexion à une instance nommée sur un port TCP désigné, dans le cas présent 2828. (Cela est souvent nécessaire si le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser ne s'exécute pas sur l'ordinateur hôte.)|APPHOST,2828|  
|Connexion à une instance par défaut qui n'écoute pas sur le port TCP par défaut, en spécifiant à la fois l'adresse IP et le port TCP utilisés, dans le cas présent 2828.|192.168.17.28,2828|  
|Connexion à une instance nommée en spécifiant à la fois l'adresse IP et le port TCP utilisés, dans le cas présent 2828.|192.168.17.28,2828|  
|Connexion à une instance par défaut par son nom, en imposant une connexion TCP.|tcp:APPHOST|  
|Connexion à une instance nommée par son nom, en imposant une connexion TCP.|tcp:APPHOST\SQLEXPRESS|  
|Connexion à une instance par défaut en spécifiant un nom de canal nommé.|\\\APPHOST\pipe\unit\app|  
|Connexion à une instance nommée en spécifiant un nom de canal nommé.|\\\APPHOST\pipe\MSSQL$SQLEXPRESS\SQL\query|  
|Connexion à une instance par défaut par son nom, en imposant une connexion par canaux nommés.|np:APPHOST|  
|Connexion à une instance nommée par son nom, en imposant une connexion par canaux nommés.|np:APPHOST\SQLEXPRESS|  
  
## <a name="verifying-your-connection-protocol"></a>Vérification du protocole de connexion  
 En cas de connexion au [!INCLUDE[ssDE](../../includes/ssde-md.md)], la requête suivante renvoie le protocole utilisé pour la connexion actuelle, ainsi que la méthode d'authentification (NTLM ou Kerberos), et indique si la connexion est chiffrée.  
  
```sql  
SELECT net_transport, auth_scheme, encrypt_option   
FROM sys.dm_exec_connections   
WHERE session_id = @@SPID;  
```  
  
## <a name="related-tasks"></a>Related Tasks  
 [Se connecter à une instance de SQL Server &#40;invite de commandes&#41;](../../database-engine/configure-windows/log-in-to-an-instance-of-sql-server-command-prompt.md)  
  
 Les ressources suivantes peuvent vous aider à résoudre un problème de connexion.  
  
-   [Comment faire pour résoudre les problème de connexion au moteur de base de données SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)  
  
-   [Procédure de résolution des problèmes de connectivité SQL](http://blogs.msdn.com/b/sql_protocols/archive/2008/04/30/steps-to-troubleshoot-connectivity-issues.aspx)  
  
## <a name="related-content"></a>Contenu associé  
 [Choisir un mode d’authentification](../../relational-databases/security/choose-an-authentication-mode.md)  
  
 [Utiliser l'utilitaire sqlcmd](../../relational-databases/scripting/sqlcmd-use-the-utility.md)  
  
 [Création d'une connexion](../../t-sql/lesson-2-1-creating-a-login.md)  
  
  
