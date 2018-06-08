---
title: Serveurs distants | Microsoft Docs
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
- server management [SQL Server], remote servers
- remote servers [SQL Server], configuring
- remote servers [SQL Server]
- servers [SQL Server], remote
- remote access option
ms.assetid: abf0fa24-f199-4273-9a1a-e8787ac9bee1
caps.latest.revision: 30
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 868cd0d8d68103afe5f6e50ae1519c11c52ff0f3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="remote-servers"></a>Serveurs distants
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les serveurs distants sont pris en charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] uniquement pour des raisons de compatibilité descendante. Il est préférable d'utiliser plutôt des serveurs liés dans les nouvelles applications. Pour plus d’informations, consultez [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md).  
  
 Une configuration de serveur distant permet à un client connecté à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’exécuter une procédure stockée sur une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans qu’il soit nécessaire d’établir une connexion distincte. Le serveur auquel le client est connecté accepte donc la demande du client et l'envoie au serveur distant, pour le compte du client. Le serveur distant traite la demande et renvoie les résultats au serveur d'origine, qui à son tour transmet les résultats au client. Lors de l'élaboration d'une configuration de serveur distant, la mise en œuvre de la sécurité doit être étudiée avec soin.  
  
 Si vous voulez élaborer une configuration de serveur afin d'exécuter des procédures stockées sur un autre serveur mais que vous ne disposez pas de configurations de serveur distant existantes, utilisez des serveurs liés en remplacement des serveurs distants. Les procédures stockées et les requêtes distribuées sont autorisées pour les serveurs liés ; en revanche, seules les procédures stockées sont autorisées pour les serveurs distants.  
  
## <a name="remote-server-details"></a>Informations détaillées sur les serveurs distants  
 Les serveurs distants sont installés par paires. Pour constituer une paire de serveurs distants, configurez les deux serveurs afin qu'ils se reconnaissent mutuellement en tant que serveurs distants.  
  
 La plupart du temps, vous n'avez pas à modifier les options de configuration des serveurs distants. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Détermine les valeurs par défaut sur les ordinateurs locaux et distants afin de permettre les connexions de serveur distant.  
  
 Pour que l’accès aux serveurs distants fonctionne, l’option de configuration **remote access** doit avoir la valeur 1 tant sur les ordinateurs locaux que sur les ordinateurs distants. (valeur par défaut).  **remote access** contrôle les connexions à partir des serveurs distants. Vous pouvez réinitialiser cette option de configuration à l’aide de la procédure stockée [!INCLUDE[tsql](../../includes/tsql-md.md)] **sp_configure** ou de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour définir cette option dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], utilisez l’option **Autoriser les accès distants à ce serveur** dans la page Connexions de la boîte de dialogue **Propriétés du serveur**. Pour accéder à la page **Connexions de la boîte de dialogue Propriétés du serveur** , dans l’Explorateur d’objets, cliquez avec le bouton droit sur le nom du serveur, puis cliquez sur **Propriétés**. Dans la page **Propriétés du serveur** , cliquez sur la page **Connexions** .  
  
 À partir du serveur local, vous pouvez désactiver une configuration de serveur distant pour empêcher les utilisateurs situés sur le serveur distant d'accéder à ce serveur local auquel il est couplé.  
  
## <a name="security-for-remote-servers"></a>Sécurité des serveurs distants  
 Pour permettre les appels de procédures à distance (RPC) sur un serveur distant, vous devez définir des mappages de connexion sur le serveur distant et éventuellement sur le serveur local exécutant une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par défaut, les appels de procédure distante sont désactivés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette configuration accroît la sécurité de votre serveur en réduisant sa zone de surface attaquable. Vous devez activer cette fonctionnalité avant de pouvoir l'utiliser. Pour plus d’informations, consultez [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
### <a name="setting-up-the-remote-server"></a>Configuration du serveur distant  
 Les mappages de connexion à distance doivent être configurés sur le serveur distant. Ils permettent au serveur distant d'établir le mappage entre la connexion d'accès entrant RPC d'un serveur donné et la connexion d'accès locale. Les mappages de connexion à distance peuvent être définis à l’aide de la procédure stockée **sp_addremotelogin** sur le serveur distant.  
  
> [!NOTE]  
>  **ne prend pas en charge l’option** trusted  **pour** sp_remoteoption [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="setting-up-the-local-server"></a>Configuration du serveur local  
 Vous ne devez pas définir de mappage de connexion sur le serveur local pour les connexions locales authentifiées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la connexion locale et le mot de passe pour se connecter au serveur distant. En revanche, pour les connexions authentifiés par Windows, vous devez définir un mappage de connexion locale sur un serveur local qui définit la connexion et le mot de passe à utiliser par une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cas de connexion RPC sur un serveur distant.  
  
 Pour les connexions créées par l’authentification Windows, vous devez créer un mappage vers un nom de connexion et un mot de passe à l’aide de la procédure stockée **sp_addlinkedservlogin** . Ce nom de connexion et ce mot de passe doivent correspondre à la connexion d’accès et au mot de passe entrants attendus par le serveur distant et créés à l’aide de **sp_addremotelogin**.  
  
> [!NOTE]  
>  Lorsque c'est possible, utilisez l'authentification Windows.  
  
### <a name="remote-server-security-example"></a>Exemple de sécurité de serveur distant  
 Prenons les installations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivantes : **serverSend** et **serverReceive**. **serverReceive** est configuré pour un mappage entre une connexion d’accès entrant du serveur **serverSend**, appelée **Sales_Mary**, et une connexion authentifiée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du serveur **serverReceive**, appelée **Alice**. Une autre connexion d’accès entrant de **serverSend**, appelée **Joe** est mappée à une connexion authentifiée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du serveur **serverReceive**** appelée **Joe**.  
  
 L’exemple de code Transact-SQL suivant illustre la configuration du serveur `serverSend` en vue de l’exécution de RPC sur le serveur `serverReceive`.  
  
```  
--Create remote server entry for RPCs   
--from serverSend in serverReceive.  
EXEC sp_addserver 'serverSend';  
GO  
  
--Create remote login mapping for login 'Sales_Mary' from serverSend  
--to Alice.  
EXEC sp_addremotelogin 'serverSend', 'Alice', 'Sales_Mary';  
GO  
--Create remote login mapping for login Joe from serverReceive   
--to same login.  
--Assumes same password for Joe in both servers.  
EXEC sp_addremotelogin 'serverSend', 'Joe', 'Joe';  
GO  
```  
  
 Sur `serverSend`, un mappage de connexion locale est créé pour une connexion authentifiée Windows `Sales\Mary` à une connexion `Sales_Mary`. Aucun mappage local n'est nécessaire pour la connexion `Joe`puisque le même nom de connexion et le même mot de passe sont utilisés par défaut et que le serveur `serverReceive` possède un mappage pour la connexion `Joe`.  
  
```  
--Create a remote server entry for RPCs from serverReceive.  
EXEC sp_addserver 'serverReceive';  
GO  
--Create a local login mapping for the Windows authenticated login.  
--Sales\Mary to Sales_Mary. The password should match the  
--password for the login Sales_Mary in serverReceive.  
EXEC sp_addlinkedsrvlogin 'serverReceive', false, 'Sales\Mary',  
   'Sales_Mary', '430[fj%dk';  
GO  
```  
  
## <a name="viewing-local-or-remote-server-properties"></a>Affichage des propriétés des serveurs locaux ou distants  
 Vous pouvez utiliser la procédure stockée étendue **xp_msver** pour consulter les attributs des serveurs locaux ou distants. Ces attributs comprennent le numéro de version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le nombre de processeurs de l'ordinateur et leur type, ainsi que la version du système d'exploitation. À partir du serveur local, vous pouvez afficher les bases de données, les fichiers, les connexions et les outils d'un serveur distant. Pour plus d’informations, consultez [xp_msver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-msver-transact-sql.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)  
  
## <a name="related-content"></a>Contenu associé  
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
 [Configurer l'option de configuration de serveur remote access](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)  
  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)  
  
  
