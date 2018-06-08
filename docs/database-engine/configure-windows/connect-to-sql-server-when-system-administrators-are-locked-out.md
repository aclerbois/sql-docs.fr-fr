---
title: Se connecter à SQL Server quand les administrateurs système n’y ont plus accès | Microsoft Docs
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
- sa account
- connecting when locked out [SQL Server]
- locked out [SQL Server]
ms.assetid: c0c0082e-b867-480f-a54b-79f2a94ceb67
caps.latest.revision: 15
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 5f2deb6e9dbb33dcc8835884587b683e8c6c0773
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-sql-server-when-system-administrators-are-locked-out"></a>Se connecter à SQL Server lorsque les administrateurs système n'y ont plus accès
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment avoir à nouveau accès à [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en tant qu’administrateur système. Un administrateur système peut perdre l'accès à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'une des raisons suivantes :  
  
-   Toutes les connexions qui sont membres du rôle serveur fixe sysadmin ont été supprimées par erreur.  
  
-   Tous les groupes Windows qui sont membres du rôle serveur fixe sysadmin ont été supprimés par erreur.  
  
-   Les connexions membres du rôle serveur fixe sysadmin sont destinées aux personnes qui ont quitté la société ou qui ne sont pas disponibles.  
  
-   Le compte d'administrateur système (sa) est désactivé ou personne ne connaît le mot de passe.  
  
 L'une des méthodes pour retrouver l'accès consiste à réinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à attacher toutes les bases de données à la nouvelle instance. Cette solution demande beaucoup de temps ; par ailleurs, pour récupérer les connexions, il peut s'avérer nécessaire de restaurer la base de données MASTER à partir d'une sauvegarde. Si la sauvegarde de la base de données MASTER est plus ancienne, il est possible qu'elle ne possède pas toutes les informations. Si la sauvegarde de la base de données MASTER est plus récente, elle possède peut-être les mêmes connexions que l'instance précédente ; par conséquent, les administrateurs sont toujours bloqués.  
  
## <a name="resolution"></a>Résolution  
 Démarrez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur à l’aide de l’option **-m** ou **-f** . Tout membre du groupe Administrateurs local de l'ordinateur peut se connecter ensuite à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme membre du rôle serveur fixe sysadmin.  
  
> [!NOTE]  
>  Lorsque vous démarrez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur, arrêtez au préalable le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Sinon, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut se connecter en premier et vous empêcher de vous connecter en tant que second utilisateur.  
  
 Quand vous utilisez l’option **-m** avec **sqlcmd** ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], vous pouvez limiter les connexions à une application cliente spécifiée. Par exemple, **-m"sqlcmd"** limite les connexions à une connexion unique, laquelle doit s’identifier en tant que programme client **sqlcmd** . Utilisez cette option lorsque vous démarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur et qu'une application cliente inconnue utilise la seule connexion disponible. Pour vous connecter par le biais de l’éditeur de requête dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], utilisez **-m"Microsoft SQL Server Management Studio - Query"**.  
  
> [!IMPORTANT]  
>  N'utilisez pas cette option comme fonctionnalité de sécurité. L'application cliente fournit le nom d'application cliente et peut fournir un nom erroné dans la chaîne de connexion.  
  
 Pour obtenir des instructions pas à pas sur le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode mono-utilisateur, consultez [Configurer les options de démarrage du serveur &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  
  
## <a name="step-by-step-instructions"></a>Instructions détaillées  
 Les instructions suivantes décrivent le processus de connexion à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] s'exécutant sous Windows 8 ou une version ultérieure. De légers ajustements sont requis pour les versions antérieures de SQL Server ou Windows. Pour exécuter ces instructions, vous devez être connecté à Windows en tant que membre du groupe Administrateurs locaux et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] doit être installé sur l'ordinateur.  
  
1.  À partir de la page Démarrer, démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Dans le menu **Affichage** , sélectionnez **Serveurs inscrits**. (Si votre serveur n’est pas déjà inscrit, cliquez avec le bouton droit sur **Groupes de serveurs locaux**, pointez sur **Tâches**, puis cliquez sur **Inscrire les serveurs locaux**.)  
  
2.  Dans la zone Serveurs inscrits, cliquez avec le bouton droit sur votre serveur, puis cliquez sur **Gestionnaire de configuration SQL Server**. Cette opération doit demander une autorisation d'exécution en tant qu'administrateur, puis ouvrir le programme Gestionnaire de configuration.  
  
3.  Fermez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
4.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le volet gauche, sélectionnez **Services SQL Server**. Dans le volet droit, recherchez votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. (L’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut **(MSSQLSERVER)** après le nom de l’ordinateur. Les instances nommées sont affichées en majuscules et portent le même nom que dans la zone Serveurs inscrits.) Cliquez avec le bouton droit sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis cliquez sur **Propriétés**.  
  
5.  Sous l’onglet **Paramètres de démarrage**, dans la zone **Spécifiez un paramètre de démarrage**, tapez `-m`, puis cliquez sur **Ajouter**. (Il s'agit d'un trait d'union suivi d'un m minuscule)  
  
    > [!NOTE]  
    >  Certaines versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'ont pas d'onglet **Paramètres de démarrage** . Dans ce cas, sous l’onglet **Avancé** , double-cliquez sur **Paramètres de démarrage**. Les paramètres s'ouvrent dans une fenêtre très petite. Veillez à ne pas modifier les paramètres existants. Tout en bas, ajoutez un nouveau paramètre `;-m` , puis cliquez sur **OK**. (Il s'agit d'un point-virgule, suivi d'un trait d'union et d'un m minuscule.)  
  
6.  Cliquez sur **OK**puis, après le message de redémarrage, cliquez avec le bouton droit sur le nom de votre serveur et cliquez sur **Redémarrer**.  
  
7.  Après le redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , votre serveur passe en mode mono-utilisateur. Vérifiez que l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas en cours d’exécution. S'il est démarré, il utilise votre unique connexion.  
  
8.  Sur l'écran de démarrage de Windows 8, cliquez avec le bouton droit sur l'icône de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Au bas de l'écran, sélectionnez **Exécuter en tant qu'administrateur**. (Cette opération transfère vos informations d'identification d'administrateur à SSMS.)  
  
    > [!NOTE]  
    >  Pour les versions antérieures de Windows, l’option **Exécuter en tant qu’administrateur** apparaît comme sous-menu.  
  
     Dans certaines configurations, SSMS essaie d'établir plusieurs connexions. Les connexions multiples échouent car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en mode mono-utilisateur. Vous pouvez sélectionner l'une des opérations suivantes. Procédez de l'une des manières suivantes :  
  
    1.  Connectez-vous avec l'Explorateur d'objets en utilisant l'authentification Windows (qui contient vos informations d'identification d'administrateur). Développez **Sécurité**, **Connexions**, puis double-cliquez sur votre propre connexion. Sur la page **Rôles serveur** , sélectionnez **sysadmin**, puis cliquez sur **OK**.  
  
    2.  Au lieu de vous connecter avec l'Explorateur d'objets, connectez-vous avec une fenêtre de requête en utilisant l'authentification Windows (qui contient vos informations d'identification d'administrateur). (Vous pouvez vous connecter de cette manière uniquement si vous ne vous êtes pas connecté avec l'Explorateur d'objets.) Exécutez le code (semblable au suivant) permettant d'ajouter une connexion d'authentification Windows qui est membre du rôle serveur fixe **sysadmin** . L’exemple suivant ajoute un utilisateur de domaine nommé `CONTOSO\PatK`.  
  
        ```  
        CREATE LOGIN [CONTOSO\PatK] FROM WINDOWS;  
        ALTER SERVER ROLE sysadmin ADD MEMBER [CONTOSO\PatK];  
        ```  
  
    3.  Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute en mode d'authentification mixte, connectez-vous avec une fenêtre de requête en utilisant l'authentification Windows (qui inclut vos informations d'identification d'administrateur). Exécutez le code (semblable au suivant) permettant de créer une connexion d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est membre du rôle serveur fixe **sysadmin** .  
  
        ```  
        CREATE LOGIN TempLogin WITH PASSWORD = '************';  
        ALTER SERVER ROLE sysadmin ADD MEMBER TempLogin;  
        ```  
  
        > [!WARNING]  
        >  Remplacez ************ par un mot de passe fort.  
  
    4.  Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute en mode d’authentification mixte et que vous souhaitez réinitialiser le mot de passe du compte **sa** , connectez-vous avec une fenêtre de requête en utilisant l’authentification Windows (qui inclut vos informations d’identification d’administrateur). Modifiez le mot de passe du compte **sa** en respectant la syntaxe suivante.  
  
        ```  
        ALTER LOGIN sa WITH PASSWORD = '************';  
        ```  
  
        > [!WARNING]  
        >  Remplacez ************ par un mot de passe fort.  
  
9. Les étapes suivantes rebasculent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en mode multi-utilisateur. Fermez SSMS.  
  
10. Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le volet gauche, sélectionnez **Services SQL Server**. Dans le volet droit, cliquez avec le bouton droit sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis cliquez sur **Propriétés**.  
  
11. Sous l’onglet **Paramètres de démarrage** , dans la zone **Paramètres existants** , sélectionnez `-m` , puis cliquez sur **Supprimer**.  
  
    > [!NOTE]  
    >  Certaines versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'ont pas d'onglet **Paramètres de démarrage** . Dans ce cas, sous l’onglet **Avancé** , double-cliquez sur **Paramètres de démarrage**. Les paramètres s'ouvrent dans une fenêtre très petite. Supprimez `;-m` que vous avez ajouté précédemment, puis cliquez sur **OK**.  
  
12. Cliquez avec le bouton droit sur le nom de votre serveur, puis cliquez sur **Redémarrer**.  
  
 Vous devez désormais être en mesure de vous connecter normalement avec l'un des comptes qui est maintenant membre du rôle serveur fixe **sysadmin** .  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer SQL Server en mode mono-utilisateur](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)   
 [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md)  
  
  
