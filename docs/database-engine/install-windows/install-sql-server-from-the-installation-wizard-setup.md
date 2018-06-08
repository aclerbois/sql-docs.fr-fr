---
title: Installer SQL Server 2016 avec l’Assistant Installation (programme d’installation) | Microsoft Docs
ms.custom: ''
ms.date: 09/06/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- installing SQL Server, steps
- Setup [SQL Server], steps
- SQL Server, installing
ms.assetid: 6ad23de1-2bab-4933-9122-c09f5565028d
caps.latest.revision: 91
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 977041d3925ed11fc6098f1617c95c263391171f
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771495"
---
# <a name="install-sql-server-from-the-installation-wizard-setup"></a>Installer SQL Server à partir de l’Assistant Installation (programme d’installation)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
 
Cet article explique comment installer SQL Server avec l’Assistant Installation. Il s’applique à [!INCLUDE[SQLServer2016](../../includes/sssql15-md.md)] et [!INCLUDE[SQLServer2017](../../includes/sssqlv14-md.md)]. Pour consulter le contenu relatif aux versions précédentes de SQL Server, consultez [Installer SQL Server 2014 à partir de l’Assistant Installation (programme d’installation)](http://msdn.microsoft.com/library/ms143219(SQL.120).aspx).

Cet article fournit une procédure d’installation pas à pas d’une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec l’Assistant Installation du programme d’installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une seule arborescence de fonctionnalités pour l'installation de tous les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ce qui vous évite d'installer individuellement les composants. Pour plus d’informations sur l’installation individuelle des composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Installer SQL Server](../../database-engine/install-windows/install-sql-server.md#how-to-install-individual-components).  

 Ces articles supplémentaires décrivent d’autres façons d’installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  

-   [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)  
  
-   [Installer SQL Server à l’aide d’un fichier de configuration](../../database-engine/install-windows/install-sql-server-2016-using-a-configuration-file.md)  
  
-   [Installer SQL Server à l’aide de SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)  
  
-   [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
-   [Mettre à niveau SQL Server à l’aide de l’Assistant Installation &#40;Installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md) 

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Avant d’installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les articles dans [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).  
  
> [!NOTE]  
> Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.  
 
 ###  <a name="bkmk_ga_instalpatch"></a> Installer le correctif obligatoire 

Microsoft a identifié un problème avec la version spécifique des fichiers binaires Microsoft VC++ 2013 Runtime qui sont installés en tant que composants requis par SQL Server. Si cette mise à jour des fichiers binaires du runtime VC n’est pas installée, SQL Server risque de rencontrer des problèmes de stabilité dans certains scénarios. Avant d’installer SQL Server, suivez les instructions données dans [Notes de publication de SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch) pour voir si votre ordinateur nécessite un correctif pour les fichiers binaires du runtime VC.  
  
## <a name="to-install-includessnoversionincludesssnoversion-mdmd"></a>Pour installer [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe.  
  
2.  L'Assistant Installation exécute le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour créer une nouvelle installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cliquez sur **Installation** dans la zone de navigation gauche, puis cliquez sur **Nouvelle installation autonome [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou ajout de fonctionnalités à une installation existante**.  

3.  Dans la page Clé du produit, sélectionnez selon le cas l'option correspondant à l'installation d'une édition gratuite de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d'une version de production du produit avec clé PID. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
     Pour continuer, cliquez sur **Suivant**.  

4.  Dans la page Termes du contrat de licence, prenez connaissance du contrat de licence et, si vous en acceptez les termes, activez la case à cocher **J'accepte les termes du contrat de licence.** , puis cliquez sur **Suivant**. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
5.  Dans la fenêtre Règles globales, la procédure d'installation passera automatiquement à la fenêtre Mises à jour du produit s'il n'existe aucune erreur de règle.  
  
6.  La page [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update s'affiche si la case à cocher [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update n'est pas activée dans Panneau de configuration\Tous les éléments du Panneau de configuration\Windows Update\Modifier les paramètres. Une coche sur la page [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update permet de modifier les paramètres de l'ordinateur de façon à inclure les dernières mises à jour lorsque vous recherchez les mises à jour de Windows.  

7.  Dans la page Mises à jour du produit, les dernières mises à jour disponibles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'affichent. Si aucune mise à jour du produit n'est découverte, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'affiche pas cette page et passe automatiquement à la page **Installer les fichiers d'installation** .  

8.  Sur la page Installer les fichiers d'installation, le programme d'installation fournit la progression du téléchargement, de l'extraction et de l'installation des fichiers d'installation. Si une mise à jour de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est trouvée, et est spécifiée pour être incluse, cette mise à jour est également installée. Si aucune mise à jour n’est trouvée, le programme d’installation progresse automatiquement. 
  
9. Dans les **Règles d’installation**, le programme d’installation de SQL Server vérifie si des problèmes affectent l’exécution de l’installation. En cas d’échec, cliquez sur la colonne **État** pour plus d’informations. Sinon, cliquez sur **Suivant**. 

10. Pour **Type d’installation**, indiquez si vous souhaitez effectuer une nouvelle installation ou ajouter des fonctionnalités à une installation existante. Cliquez sur **Suivant**. 
  
11. Dans la page Sélection de fonctionnalités, sélectionnez les composants que vous voulez installer. Par exemple, pour installer une nouvelle instance du moteur de base de données SQL Server, cochez la case **Services Moteur de base de données**.

    Une description de chaque groupe de composants apparaît dans le volet **Description du composant** une fois que vous avez sélectionné le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher. Pour plus d’informations, consultez [Éditions et composants de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) et [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-supported-features-for-sql-server-2016.md).
  
     La configuration requise pour les fonctionnalités sélectionnées est affichée dans le volet **Configuration requise pour les composants sélectionnés** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure.  
  
     Vous pouvez également spécifier un répertoire personnalisé pour les composants partagés en utilisant le champ situé au bas de la page Sélection de fonctionnalités. Pour modifier le chemin d'installation des composants partagés, mettez à jour le chemin d'accès dans le champ situé en bas de la boîte de dialogue ou cliquez sur le bouton **Parcourir** afin d'accéder à un répertoire d'installation. Le chemin d'installation par défaut est [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)].  
  
     Le chemin d'accès spécifié pour les composants partagés doit être un chemin d'accès absolu. Le dossier ne doit pas être compressé ni chiffré. Les lecteurs mappés ne sont pas pris en charge.  
  
     SQL Server utilise deux répertoires pour les fonctionnalités partagées :
  
     * Répertoire des fonctionnalités partagées  
  
     * Répertoire des fonctionnalités partagées (x86)  
  
     Le chemin d'accès spécifié pour chacune des options ci-dessus doit être différent.  
  
12. La fenêtre Règles de fonctionnalité avancera automatiquement si toutes les règles passent.  
  
13. Dans la page Configuration de l'instance, spécifiez s'il faut installer une instance par défaut ou une instance nommée. Pour plus d'informations, consultez [Instance Configuration](../../sql-server/install/instance-configuration.md#instance-configuration).  
  
     **ID d'instance** — Par défaut, le nom de l'instance est utilisé comme ID d'instance. Il permet d'identifier les répertoires d'installation et les clés de Registre de votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tel est le cas pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d’instance non défini par défaut, spécifiez une autre valeur dans la zone de texte **ID d’instance** .  
  
    > [!NOTE]  
    >  Les instances autonomes classiques de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], qu’il s’agisse d’instances par défaut ou d’instances nommées, n’utilisent pas de valeur non définie par défaut pour l’ **ID d’instance**.  
  
     Tous les Service Packs et mises à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'appliqueront à chaque composant d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     **Instances installées** — La grille affiche les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui se trouvent sur l'ordinateur où le programme d'installation s'exécute. Si une instance par défaut est déjà installée sur l'ordinateur, vous devez installer une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)].  
  
     Le flux de travail du reste de l’installation dépend des fonctionnalités que vous avez spécifiées pour votre installation. Il est possible que les pages ne soient pas toutes visibles, en fonction de vos sélections.  
  
14. Utilisez la page Configuration du serveur — Comptes de service pour spécifier les comptes de connexion des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez choisi d'installer.  
  
     Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Vous pouvez également spécifier si les services démarrent automatiquement, sont démarrés manuellement ou sont désactivés. [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous recommande de configurer les comptes de service individuellement afin d'octroyer le moins de privilèges à chaque service, sachant que les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disposent des autorisations minimales requises pour effectuer leurs tâches. Pour plus d’informations, consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Pour spécifier le même compte d'ouverture de session pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page.  
  
    > [!IMPORTANT]  
    > [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
    
    > [!NOTE]
    > À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], cochez la case *Accorder le privilège Effectuer une tâche de maintenance en volume au service Moteur de base de données SQL Server* pour permettre au compte de service [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] d’utiliser [l’initialisation instantanée de fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md).
  
     Utilisez la page Configuration du serveur — Classement pour spécifier les classements non définis par défaut pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
15. Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] – Configuration du serveur pour spécifier les éléments suivants :  
  
    -   Mode de sécurité — Sélectionnez l'authentification Windows ou le mode d'authentification mixte pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous sélectionnez Mode d'authentification mixte, vous devez fournir un mot de passe fort pour le compte administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré.  
  
         Lorsque la connexion d'un périphérique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]est établie, le mécanisme de sécurité est le même pour l'authentification Windows et le mode mixte. Pour plus d’informations, consultez [Configuration du moteur de base de données - Configuration du serveur](../../sql-server/install/instance-configuration.md#database-engine-configuration---server-configuration).  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — Vous devez spécifier au moins un administrateur système pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, cliquez sur **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou sur **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] – Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    > Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Pour plus d’informations, consultez [Configuration du moteur de base de données – Répertoires de données](../../sql-server/install/instance-configuration.md#database-engine-configuration---data-directories).  
  
     Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] - FILESTREAM pour activer FILESTREAM pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Configuration du moteur de base de données - Filestream](../../sql-server/install/instance-configuration.md#database-engine-configuration---filestream).  
  
     Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] - TempDB pour configurer la taille des fichiers, le nombre de fichiers, les répertoires d’installation autres que par défaut et les paramètres de croissance des fichiers pour TempDB. Pour plus d’informations, consultez [Configuration du moteur de base de données - TempDB](../../sql-server/install/instance-configuration.md#database-engine-configuration---tempdb).  
  
16. Utilisez la page Configuration d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] - Attribution de privilèges d'accès aux comptes pour spécifier le mode serveur et les utilisateurs ou comptes qui ont les autorisations d'administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Le mode serveur détermine quel sous-systèmes de mémoire et de stockage sont utilisés sur le serveur. Différents types de solution s'exécutent dans différents modes serveur. Si vous envisagez d'exécuter les bases de données multidimensionnelles de cube sur le serveur, choisissez l'option par défaut, le mode serveur multidimensionnel et d'exploration de données. En ce qui concerne les autorisations d'administrateur, vous devez spécifier au moins un administrateur système pour l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de SQL Server s'exécute, cliquez sur le bouton **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations sur le mode serveur et les autorisations d’administrateur, consultez [Configuration Analysis Services – Mise en service de compte](../../sql-server/install/instance-configuration.md#analysis-services-configuration---account-provisioning).  

   Lorsque vous avez fini de modifier la liste, cliquez sur **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**.
   
   Utilisez la page Configuration du [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] — Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
   
   > [!IMPORTANT]  
   > Lors de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si vous spécifiez le même chemin d’accès de répertoire pour INSTANCEDIR et SQLUSERDBDIR, SQL Server Agent et la recherche en texte intégral ne démarrent pas en raison d’autorisations manquantes.  
   >  
   > Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
   
   Pour plus d’informations, consultez [Configuration Analysis Services – Répertoires de données](../../sql-server/install/instance-configuration.md#analysis-services-configuration---data-directories).  
   
17. Utilisez la page Configuration de Distributed Replay Controller pour spécifier les utilisateurs auxquels vous voulez accorder des autorisations administratives sur le service Distributed Replay Controller. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Controller.  
  
     Cliquez sur le bouton **Ajouter l'utilisateur actuel** pour ajouter les utilisateurs auxquels vous voulez accorder des autorisations d'accès au service Distributed Replay Controller. Cliquez sur le bouton **Ajouter** pour ajouter des autorisations d'accès au service Distributed Replay Controller. Cliquez sur le bouton **Supprimer** pour supprimer des autorisations d'accès du service Distributed Replay Controller.  
  
     Pour continuer, cliquez sur **Suivant**.  
  
18. Utilisez la page Configuration de Distributed Replay Client pour spécifier les utilisateurs auxquels vous voulez accorder des autorisations administratives sur le service Distributed Replay Client. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Client.  
  
     **Nom du contrôleur** est un paramètre optionnel. La valeur par défaut est \<*blank*>. Entrez le nom du contrôleur avec lequel l'ordinateur client communiquera pour le service Distributed Replay Client. Notez les points suivants :  
  
    -   Si vous avez déjà configuré un contrôleur, entrez son nom lors de la configuration de chaque client.  
  
    -   Si vous n'avez pas encore configuré de contrôleur, vous n'êtes pas obligé de fournir le nom du contrôleur. Toutefois, vous devez entrer manuellement le nom du contrôleur dans le fichier de **configuration du client** .  
  
     Spécifiez le **Répertoire de travail** du service Distributed Replay Client. Le répertoire de travail par défaut est \<*lettre de lecteur*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     Spécifiez le **Répertoire des résultats** du service Distributed Replay Client. Le répertoire des résultats par défaut est \<*lettre de lecteur*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     Pour continuer, cliquez sur **Suivant**.  
  
19. La page Prêt pour l'installation affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Dans cette page, le programme d'installation indique si la fonctionnalité de mise à jour du produit est activée ou désactivée et la version de la mise à jour finale.  
  
     Pour continuer, cliquez sur **Installer**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités.  
  
20. Au cours de l'installation, la page Progression de l'installation fournit des informations d'état de sorte que vous puissiez contrôler la progression de l'installation au fil de l'exécution du programme d'installation.  
  
21. Après l'installation, la page Terminé fournit un lien vers le fichier journal résumé pour l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**.  
  
22. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d'informations, consultez [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="next-steps"></a>Étapes suivantes  
 Configurez votre nouvelle installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour réduire la surface d'exposition d'un système, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe et active de façon sélective les services et fonctionnalités clés. Pour plus d'informations, consultez [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Valider une installation SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)   
 [Réparer une installation défectueuse de SQL Server 2016](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)   
 [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Effectuer une mise à niveau vers SQL Server 2016 à l’aide de l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Installer SQL Server 2016 à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)  
  
  
