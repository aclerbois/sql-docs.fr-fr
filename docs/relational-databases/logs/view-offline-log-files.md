---
title: Afficher les fichiers journaux hors connexion | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: logs
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Log File Viewer, viewing offline logs
- offline log files
ms.assetid: 9223e474-f224-4907-a4f2-081e11db58f5
caps.latest.revision: 18
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 26b2085e6947b33b77779b1eb4785e3854db3e21
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-offline-log-files"></a>Afficher les fichiers journaux hors connexion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Depuis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], vous pouvez consulter les fichiers journaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'une instance locale ou distante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque l'instance cible est hors connexion ou ne peut pas démarrer.  
  
 Vous pouvez accéder aux fichiers journaux hors connexion à partir des serveurs inscrits, ou par programmation via des requêtes WMI et WQL (WMI Query Language).  
  
> [!NOTE]  
>  Vous pouvez également utiliser ces méthodes pour vous connecter à une instance qui est en ligne, mais à laquelle, pour une raison quelconque, vous ne pouvez pas vous connecter via une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="before-you-begin"></a>Avant de commencer  
 Pour vous connecter aux fichiers journaux hors connexion, une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être installée sur l'ordinateur que vous utilisez pour consulter les fichiers journaux hors connexion, ainsi que sur l'ordinateur où se trouvent les fichiers journaux en question. Si une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installée sur les deux ordinateurs, vous pouvez afficher les fichiers hors connexion pour les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et pour les instances qui exécutent des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur ces ordinateurs.  
  
 Si vous utilisez des serveurs inscrits, l'instance à laquelle vous voulez vous connecter doit être enregistrée sous **Groupes de serveurs locaux** ou **Serveurs de gestion centralisée** (l'instance peut être enregistrée seule ou être membre d'un groupe de serveurs). Pour plus d'informations sur l'ajout d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour des serveurs inscrits, consultez les rubriques suivantes :  
  
-   [Créer ou modifier un groupe de serveurs &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/create-or-edit-a-server-group-sql-server-management-studio.md)  
  
-   [Inscrire un serveur connecté &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/register-a-connected-server-sql-server-management-studio.md)  
  
-   [Créer un serveur d’administration centralisée et un groupe de serveurs &#40;SQL Server Management Studio&#41;](../../tools/sql-server-management-studio/create-a-central-management-server-and-server-group.md)  
  
 Pour plus d'informations sur l'affichage des fichiers journaux hors connexion par programmation via des requêtes WMI et WQL, consultez les rubriques suivantes :  
  
-   [Classe SqlErrorLogEvent](../../relational-databases/wmi-provider-configuration-classes/sqlerrorlogevent-class.md) (cette rubrique indique comment récupérer des valeurs pour les événements enregistrés dans un fichier journal spécifié).  
  
-   [Classe SqlErrorLogFile](../../relational-databases/wmi-provider-configuration-classes/sqlerrorlogfile-class.md) (cette rubrique indique comment récupérer des informations concernant tous les fichiers journaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
##  <a name="BeforeYouBegin"></a> Autorisations  
 Pour vous connecter à un fichier journal hors connexion, vous devez disposer des autorisations suivantes sur les ordinateurs local et distant :  
  
-   Accès en lecture à l’espace de noms WMI **Root\Microsoft\SqlServer\ComputerManagement12** . Par défaut, tout le monde dispose de l'accès en lecture via l'autorisation Activer le compte. Pour plus d'informations, consultez la procédure « Pour vérifier les autorisations WMI », plus loin dans cette section.  
  
-   Autorisation en lecture pour le dossier qui contient les fichiers journaux des erreurs. Par défaut, ces fichiers journaux se trouvent à l’emplacement suivant (où \<*Lecteur>* représente le lecteur sur lequel vous avez installé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et où \<*NomInstance*> est le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) :  
  
     **\<Lecteur>:\Program Files\Microsoft SQL Server\MSSQL13.\<NomInstance>\MSSQL\Log**  
  
 Pour vérifier les paramètres de sécurité de l'espace de noms WMI, utilisez le composant logiciel enfichable Contrôle WMI.  
  
#### <a name="to-verify-wmi-permissions"></a>Pour vérifier les autorisations WMI  
  
1.  Ouvrez le composant logiciel enfichable Contrôle WMI. Pour ce faire, effectuez l'une des procédures suivantes, en fonction de votre système d'exploitation :  
  
    -   Cliquez sur **Démarrer**, tapez **wmimgmt.msc** dans la zone **Rechercher** , puis appuyez sur Entrée.  
  
    -   Cliquez sur **Démarrer**, puis sur **Exécuter**, tapez **wmimgmt.msc**et appuyez sur Entrée.  
  
2.  Par défaut, le composant logiciel enfichable Contrôle WMI gère l'ordinateur local.  
  
     Si vous souhaitez vous connecter à un ordinateur distant, suivez ces étapes :  
  
    1.  Cliquez avec le bouton droit sur **Contrôle WMI (local)**, puis cliquez sur **Se connecter à un autre ordinateur**.  
  
    2.  Dans la boîte de dialogue **Changer d'ordinateur géré** , cliquez sur **Un autre ordinateur**.  
  
    3.  Entrez le nom de l'ordinateur distant, puis cliquez sur **OK**.  
  
3.  Cliquez avec le bouton droit sur **Contrôle WMI (local)** ou **Contrôle WMI (***NomOrdinateurDistant***)**, puis cliquez sur **Propriétés**.  
  
4.  Dans la boîte de dialogue **Propriétés du Contrôle WMI** , cliquez sur l'onglet **Sécurité** .  
  
5.  Dans l'arborescence d'espace de noms, localisez l'espace de noms suivant, puis cliquez dessus :  
  
     **Root\Microsoft\SqlServer\ComputerManagement10**  
  
6.  Cliquez sur **Sécurité**.  
  
7.  Assurez-vous que le compte qui sera utilisé a l'autorisation **Activer le compte** . Cette autorisation autorise l'accès en lecture aux objets WMI.  
  
### <a name="view-log-files"></a>Afficher les fichiers journaux  
 La procédure suivante indique comment afficher les fichiers journaux hors connexion via des serveurs inscrits. Cette procédure suppose que la condition suivante est remplie :  
  
 L'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous voulez vous connecter est déjà enregistrée auprès des serveurs inscrits.  
  
##### <a name="to-view-log-files-for-instances-that-are-offline"></a>Pour afficher des fichiers journaux d'instances hors connexion  
  
1.  Si vous souhaitez afficher les fichiers journaux hors connexion sur une instance locale, assurez-vous que vous démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] avec des autorisations élevées. Pour ce faire, lorsque vous démarrez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], cliquez avec le bouton droit sur **SQL Server Management Studio**, puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
2.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans le menu **Affichage** , cliquez sur **Serveurs inscrits**.  
  
3.  Dans l'arborescence de la console, trouvez l'instance sur laquelle vous voulez afficher les fichiers hors connexion.  
  
4.  Procédez de l'une des manières suivantes :  
  
    -   Si l’instance est sous **Groupes de serveurs locaux**, développez **Groupes de serveurs locaux**, développez le groupe de serveurs (si l’instance est un membre d’un groupe), cliquez avec le bouton droit sur l’instance, puis cliquez sur **Afficher le journal SQL Server**.  
  
    -   Si l’instance est le serveur de gestion centralisée lui-même, développez **Serveurs de gestion centralisée**, cliquez avec le bouton droit sur l’instance, pointez sur **Actions du serveur de gestion centralisée**, puis cliquez sur **Afficher le journal SQL Server**.  
  
    -   Si l’instance est sous **Serveurs de gestion centralisée**, développez **Serveurs de gestion centralisée**, développez le serveur de gestion centralisée, cliquez avec le bouton droit sur l’instance (ou développez un groupe de serveurs et cliquez avec le bouton droit sur l’instance), puis cliquez sur **Afficher le journal SQL Server**.  
  
5.  Si vous vous connectez à une instance locale, la connexion est établie à l'aide des informations d'identification de l'utilisateur actuel.  
  
     Si vous vous connectez à une instance distante, dans la boîte de dialogue **Visionneuse du fichier journal - Se connecter en tant que** , effectuez l’une ou l’autre des procédures suivantes :  
  
    -   Pour vous connecter en tant qu'utilisateur actuel, assurez-vous que la case à cocher **Se connecter en tant qu'autre utilisateur** est désactivée, puis cliquez sur **OK**.  
  
    -   Pour vous connecter en tant qu'autre utilisateur, activez la case à cocher **Se connecter en tant qu'autre utilisateur** , puis cliquez sur **Définir l'utilisateur**. Lorsqu’un message vous y invite, entrez les informations d’identification de l’utilisateur (avec le nom d’utilisateur au format *nom-domaine*\\*nom-utilisateur*), cliquez sur **OK**, puis cliquez de nouveau sur **OK** pour vous connecter.  
  
    > [!NOTE]  
    >  Si les fichiers journaux mettent trop de temps à se charger, vous pouvez cliquer sur **Arrêt** dans la barre d’outils de la Visionneuse du fichier journal.  
  
## <a name="see-also"></a> Voir aussi  
 [Visionneuse du fichier journal](../../relational-databases/logs/log-file-viewer.md)  
  
  
