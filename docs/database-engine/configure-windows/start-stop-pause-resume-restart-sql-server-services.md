---
title: Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 02/26/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, start and stop services
- stopping SQL Server Agent
- parameters [SQL Server], startup options
- SQL Server, startup options
- Database Engine [SQL Server], starting and stopping services
- pausing SQL Server
- PowerShell [SQL Server], starting and stopping services
- single-user mode [SQL Server], starting in
- SQL Server Management Studio [SQL Server], starting or stopping services
- stopping SQL Server Browser service
- starting SQL Server Agent
- default instances [SQL Server], starting and stopping
- SQL Server Agent, starting and stopping
- command prompt [SQL Server], starting and stopping SQL Server services
- continuing SQL Server
- starting SQL Server Database Engine
- net stop commands [SQL Server]
- command prompt [SQL Server], SQL Browser service
- Configuration Manager, start and stop services
- resuming SQL Server
- startup options [SQL Server]
- named instances [SQL Server], starting and stopping
- net start commands [SQL Server]
- SQL Server, starting and stopping
- stopping SQL Server
- starting SQL Server Browser service
- SQL Server Database Engine setting startup options
- administering SQL Server, starting and stopping services
- Management Studio [SQL Server], starting or stopping services
ms.assetid: 32660a02-e5a1-411a-9e57-7066ca459df6
caps.latest.revision: 20
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2e0ca8a8db9121cb224f84ef38e4f03b0f8239f4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="start-stop-pause-resume-restart-sql-server-services"></a>Démarrer, arrêter, suspendre, reprendre, redémarrer les services SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
 > Pour le contenu relatif aux versions antérieures de SQL Server, consultez [Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](https://msdn.microsoft.com/en-US/library/hh403394(SQL.120).aspx).

  Cette rubrique explique comment démarrer, arrêter, interrompre, reprendre ou redémarrer le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser à l’aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , de  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], des commandes **net** à partir d’une invite de commandes, de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou de PowerShell.  
  
-   **Avant de commencer :**  
  
    -   [Quels sont ces services ?](#Services)  
  
    -   [Informations supplémentaires](#MoreInformation)  
  
    -   [Sécurité](#Security)  
  
-   **Instructions pour utiliser :**  
  
    -   [Gestionnaire de configuration SQL Server](#SSCMProcedure)  
  
    -   [SQL Server Management Studio](#SSMSProcedure)  
  
    -   [commandes net à partir d'une fenêtre d'invite de commandes](#CommandPrompt)  
  
    -   [Transact-SQL](#TsqlProcedure)  
  
    -   [PowerShell](#PowerShellProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Services"></a> Qu'est-ce que le service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent et le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser ?  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont des programmes exécutables qui s'exécutent en tant que service Windows. Les programmes qui s'exécutent en tant que service Windows peuvent continuer à fonctionner sans afficher d'activité sur l'écran de l'ordinateur.  
  
 **[!INCLUDE[ssDE](../../includes/ssde-md.md)] service**  
 Processus exécutable qui est le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut être l'instance par défaut (avec une limite d'une par ordinateur), ou peut être l'une des nombreuses instances nommées du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Utilisez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour déterminer quelles instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont installées sur l'ordinateur. L’instance par défaut (si vous l’installez) est répertoriée sous **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)**. Les instances nommées (si vous les installez) sont répertoriées sous **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (<nom_instance>)**. Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express est installé en tant que **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQLEXPRESS)**.  
  
 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Agent**  
 Service Windows qui exécute des tâches administratives planifiées, appelées travaux et alertes. Pour plus d’informations, consultez [SQL Server Agent](http://msdn.microsoft.com/library/8d1dc600-aabb-416f-b3af-fbc9fccfd0ec). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent n'est pas disponible dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser**  
 Service Windows qui écoute les demandes entrantes des ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et fournit aux clients des informations sur les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installées sur l'ordinateur. Une seule instance du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est utilisée pour toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installées sur l'ordinateur.  
  
###  <a name="MoreInformation"></a> Informations supplémentaires  
  
-   La suspension du service [!INCLUDE[ssDE](../../includes/ssde-md.md)] empêche les nouveaux utilisateurs de se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)], mais les utilisateurs qui sont déjà connectés peuvent continuer à travailler jusqu'à ce que leurs connexions soient interrompues. Utilisez suspendre lorsque vous souhaitez attendre que les utilisateurs aient terminé leur travail avant d'arrêter le service. Cela leur permet d'effectuer les transactions en cours. Reprendre permet au [!INCLUDE[ssDE](../../includes/ssde-md.md)] d'accepter à nouveau de nouvelles connexions. Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent ne peut pas être suspendu ou repris.  
  
-   Le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] indiquent l'état actuel des services à l'aide des icônes suivantes.  
  
     **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gestionnaire de configuration**  
  
    -   Une flèche verte sur l'icône située à côté du nom du service indique que le service a démarré.  
  
    -   Un carré rouge sur l'icône située à côté du nom du service indique que le service s'est arrêté.  
  
    -   Deux lignes bleues verticales sur l'icône située à côté du nom du service indique que le service est suspendu.  
  
    -   Lors du redémarrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)], un carré rouge indique que le service s'est arrêté, puis une flèche verte indique que le service a démarré.  
  
     **[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]**  
  
    -   Une flèche blanche sur l'icône de cercle vert située à côté du nom du service indique que le service a démarré.  
  
    -   Un carré blanc sur l'icône de cercle rouge située à côté du nom du service indique que le service s'est arrêté.  
  
    -   Deux lignes blanches verticales sur une icône de cercle bleue située à côté du nom du service indiquent que le service est suspendu.  
  
-   Lorsque vous utilisez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], seules les options possibles sont disponibles. Par exemple, si le service a déjà démarré, **Démarrer** n'est pas disponible.  
  
-   Lors de l'exécution sur un cluster, le service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] est mieux géré à l'aide de l'Administrateur de cluster.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Par défaut, seuls les membres du groupe des administrateurs locaux peuvent démarrer, arrêter, interrompre, reprendre ou redémarrer un service. Pour accorder aux non-administrateurs la capacité de gérer des services, consultez [Comment accorder aux utilisateurs des droits de gestion des services dans Windows Server 2003](http://support.microsoft.com/kb/325349). (le processus est semblable sur d'autres versions de Windows).  
  
 L’arrêt du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l’aide de la commande [!INCLUDE[tsql](../../includes/tsql-md.md)]**SHUTDOWN** requiert l’appartenance aux rôles serveur fixes **sysadmin** ou **serveradmin** , et n’est pas transférable.  
  
##  <a name="SSCMProcedure"></a> Utilisation du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
#### <a name="starting-includessnoversionincludesssnoversion-mdmd-configuration-manager"></a>Démarrage du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
     Étant donné que le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un composant logiciel enfichable pour le programme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console et non pas un programme autonome, le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’apparaît pas en tant qu’application dans les versions plus récentes de Windows. Voici les chemins d’accès aux quatre dernières versions lorsque Windows est installé sur le lecteur C.  
  
    |||  
    |-|-|  
    |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|  
    |[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|C:\Windows\SysWOW64\SQLServerManager12.msc|  
    |[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|C:\Windows\SysWOW64\SQLServerManager11.msc|  
    |[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|C:\Windows\SysWOW64\SQLServerManager10.msc|  
  
#### <a name="to-start-stop-pause-resume-or-restart-the-an-instance-of-the-includessdenoversionincludesssdenoversion-mdmd"></a>Pour démarrer, arrêter, interrompre, reprendre, ou redémarrer une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]  
  
1.  Démarrez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en suivant les instructions ci-dessus.  
  
2.  Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.  
  
3.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le volet gauche, cliquez sur **Services SQL Server**.  
  
4.  Dans le volet de résultats, cliquez avec le bouton droit sur **SQL Server (MSSQLServer)** ou sur une instance nommée, puis cliquez sur **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre**ou **Redémarrer**.  
  
5.  Cliquez sur **OK** pour fermer le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Pour démarrer une instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] avec les options de démarrage, consultez [Configurer les options de démarrage du serveur &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  
  
#### <a name="to-start-stop-pause-resume-or-restart-the-includessnoversionincludesssnoversion-mdmd-browser-or-an-instance-of-the-includessnoversionincludesssnoversion-mdmd-agent"></a>Pour démarrer, arrêter, interrompre, reprendre, ou redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser ou une instance de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Démarrez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en suivant les instructions ci-dessus.  
  
2.  Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.  
  
3.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le volet gauche, cliquez sur **Services SQL Server**.  
  
4.  Dans le volet de résultats, cliquez avec le bouton droit sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser**, **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent (MSSQLServer)** ou **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent (<nom_instance>)** pour une instance nommée, puis cliquez sur **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre** ou **Redémarrer**.  
  
5.  Cliquez sur **OK** pour fermer le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  L’agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être suspendu.  
  
##  <a name="SSMSProcedure"></a> Utilisation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio  
  
#### <a name="to-start-stop-pause-resume-or-restart-the-an-instance-of-the-includessdenoversionincludesssdenoversion-mdmd"></a>Pour démarrer, arrêter, interrompre, reprendre, ou redémarrer une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]  
  
1.  Dans l’Explorateur d’objets, connectez-vous à l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], cliquez avec le bouton droit sur l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à démarrer, puis cliquez **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre**ou **Redémarrer**.  
  
     Ou, dans Serveurs inscrits, cliquez avec le bouton droit sur l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à démarrer, pointez sur **Contrôle du service**, puis cliquez sur **Démarrer**, **Arrêter**, **Suspendre**, **Reprendre**ou **Redémarrer**.  
  
2.  Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.  
  
3.  Lorsque vous y êtes invité, et si vous souhaitez exécuter l'action, cliquez sur **Oui**.  
  
#### <a name="to-start-stop-or-restart-the-an-instance-of-the-includessnoversionincludesssnoversion-mdmd-agent"></a>Pour démarrer, arrêter ou redémarrer une instance de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Dans l’Explorateur d’objets, connectez-vous à l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], cliquez avec le bouton droit sur **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent**, puis cliquez sur **Démarrer**, **Arrêter**ou **Redémarrer**.  
  
2.  Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Oui**.  
  
3.  Lorsque vous y êtes invité, et si vous souhaitez exécuter l'action, cliquez sur **Oui**.  
  
##  <a name="CommandPrompt"></a> À partir de la fenêtre d'invite de commandes en utilisant les commandes net  
 Les services [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être démarrés, arrêtés ou suspendus à l’aide des commandes [!INCLUDE[msCoName](../../includes/msconame-md.md)] **net** Windows.  
  
###  <a name="dbDefault"></a> Pour démarrer l'instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)]  
  
-   À partir d'une invite de commandes, entrez l'une des commandes suivantes :  
  
     **net start "SQL Server (MSSQLSERVER)"**  
  
     -ou-  
  
     **net start MSSQLSERVER**  
  
###  <a name="dbNamed"></a> Pour démarrer une instance nommée du [!INCLUDE[ssDE](../../includes/ssde-md.md)]  
  
-   À partir d'une invite de commandes, entrez l'une des commandes suivantes. Remplacez *\<nom_instance>* par le nom de l’instance à gérer.  
  
     **net start "SQL Server (** *nom_instance* **)"**  
  
     -ou-  
  
     **net start MSSQL$** *nom_instance*  
  
###  <a name="dbStartup"></a> Pour démarrer le [!INCLUDE[ssDE](../../includes/ssde-md.md)] avec des options de démarrage  
  
-   Ajoutez les options de démarrage à la fin de l’instruction **"SQL Server (MSSQLSERVER)"** , en les séparant par un espace. Lors d’un démarrage avec l’instruction **net start**, les options de démarrage utilisent une barre oblique (/) au lieu d’un tiret (-).  
  
     **net start "SQL Server (MSSQLSERVER)" /f /m**  
  
     -ou-  
  
     **net start MSSQLSERVER /f /m**  
  
    > [!NOTE]  
    >  Pour plus d’informations sur les options de démarrage, consultez [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
###  <a name="agDefault"></a> Pour démarrer l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   À partir d'une invite de commandes, entrez l'une des commandes suivantes :  
  
     **net start "SQL Server Agent (MSSQLSERVER)"**  
  
     -ou-  
  
     **net start SQLSERVERAGENT**  
  
###  <a name="agNamed"></a> Pour démarrer l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   À partir d'une invite de commandes, entrez l'une des commandes suivantes. Remplacez *nom_instance* par le nom de l’instance à gérer.  
  
     **net start "SQL Server Agent (** *nom_instance* **)"**  
  
     -ou-  
  
     **net start SQLAgent$** *nom_instance*  
  
 Pour obtenir des informations sur la façon d’exécuter l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode détaillé à des fins de résolution des problèmes, consultez [Application sqlagent90](../../tools/sqlagent90-application.md).  
  
###  <a name="Browser"></a> Pour démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser  
  
-   À partir d'une invite de commandes, entrez l'une des commandes suivantes :  
  
     **net start "SQL Server Browser"**  
  
     -ou-  
  
     **net start SQLBrowser**  
  
###  <a name="pauseStop"></a> Pour suspendre ou arrêter des services à partir de la fenêtre d'invite de commandes  
  
-   Pour suspendre ou arrêter des services, modifiez les commandes des façons suivantes.  
  
    -   Pour suspendre un service, remplacez **net start** par **net pause**.  
  
    -   Pour arrêter un service, remplacez **net start** par **net stop**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut être arrêté à l’aide de l’instruction **SHUTDOWN** .  
  
#### <a name="to-stop-the-includessdeincludesssde-mdmd-using-includetsqlincludestsql-mdmd"></a>Pour arrêter le [!INCLUDE[ssDE](../../includes/ssde-md.md)] à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
-   Pour attendre la fin des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et des procédures stockées en cours d'exécution, puis arrêter le [!INCLUDE[ssDE](../../includes/ssde-md.md)], exécutez l'instruction suivante.  
  
    ```sql  
    SHUTDOWN;   
    ```  
  
-   Pour arrêter le [!INCLUDE[ssDE](../../includes/ssde-md.md)] immédiatement, exécutez l'instruction suivante.  
  
    ```sql  
    SHUTDOWN WITH NOWAIT;   
    ```  
  
 Pour plus d’informations sur l’instruction **SHUTDOWN**, consultez [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md).  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
  
#### <a name="to-start-and-stop-includessdeincludesssde-mdmd-services"></a>Pour démarrer et arrêter des services du [!INCLUDE[ssDE](../../includes/ssde-md.md)]  
  
1.  Dans une fenêtre d'invite de commandes, démarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell en exécutant la commande suivante.  
  
    ```ms-dos  
    sqlps  
    ```  
  
2.  À l'invite de commandes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell, exécutez la commande suivante. Remplacez `computername` par le nom de votre ordinateur.  
  
    ```powershell  
    # Get a reference to the ManagedComputer class.  
    CD SQLSERVER:\SQL\computername  
    $Wmi = (get-item .).ManagedComputer  
  
    ```  
  
3.  Identifiez le service que vous souhaitez arrêter ou démarrer. Choisissez l'une des lignes suivantes. Remplacez `instancename` par le nom de l'instance nommée.  
  
    -   Pour obtenir une référence à l'instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
        ```powershell  
        $DfltInstance = $Wmi.Services['MSSQLSERVER']  
        ```  
  
    -   Pour obtenir une référence à une instance nommée du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
        ```powershell  
        $DfltInstance = $Wmi.Services['MSSQL$instancename']  
        ```  
  
    -   Pour obtenir une référence au service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur l'instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
        ```powershell  
        $DfltInstance = $Wmi.Services['SQLSERVERAGENT']  
        ```  
  
    -   Pour obtenir une référence au service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur une instance nommée du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
        ```powershell  
        $DfltInstance = $Wmi.Services['SQLAGENT$instancename']  
        ```  
  
    -   Pour obtenir une référence au service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser.  
  
        ```powershell  
        $DfltInstance = $Wmi.Services['SQLBROWSER']  
        ```  
  
4.  Terminez l'exemple pour démarrer, puis arrêter le service sélectionné.  
  
    ```powershell  
    # Display the state of the service.  
    $DfltInstance  
    # Start the service.  
    $DfltInstance.Start();  
    # Wait until the service has time to start.  
    # Refresh the cache.  
    $DfltInstance.Refresh();   
    # Display the state of the service.  
    $DfltInstance  
    # Stop the service.  
    $DfltInstance.Stop();  
    # Wait until the service has time to stop.  
    # Refresh the cache.  
    $DfltInstance.Refresh();   
    # Display the state of the service.  
    $DfltInstance  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d'ensemble de la documentation d'installation de SQL Server](http://msdn.microsoft.com/library/2620439a-f9d3-4b3c-9968-48f60b4bb9a5)   
 [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md)   
 [Démarrer SQL Server avec une configuration minimale](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)   
 [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)  
  
  

