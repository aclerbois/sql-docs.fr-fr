---
title: Ajouter des fonctionnalités à une instance de SQL Server (programme d’installation) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- feature adding [SQL Server]
- SQL Server, features
- adding features to SQL Server
ms.assetid: 97931fdc-d943-48dd-81b9-ae8b8d2c6dad
caps.latest.revision: 29
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e7cab8cd2057196a0af321b6037fb5d031883188
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771525"
---
# <a name="add-features-to-an-instance-of-sql-server-setup"></a>Ajouter des fonctionnalités à une instance de SQL Server (programme d’installation)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
  
 Cet article fournit une procédure pas à pas pour ajouter des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Certains composants ou services de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont spécifiques à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ils sont également définis comme étant dépendants de l'instance. Ils partagent la même version que l'instance qui les héberge et sont utilisés exclusivement pour cette instance. Vous pouvez ajouter les composants dépendant d'une instance à une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], avec des composants partagés s'ils ne sont pas déjà installés. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
 Pour ajouter des fonctionnalités à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d’une invite de commandes, consultez [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md).  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Avant de continuer, consultez les articles dans [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md).  
  
> [!NOTE]  
>  Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui dispose des autorisations de lecture sur le partage distant.  
  
> [!NOTE]  
>  Lorsque vous ajoutez des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], les paramètres de rapport d'utilisation existants sont appliqués aux fonctionnalités ajoutées récemment. Pour modifier ces paramètres, utilisez l’outil **Rapports d’erreurs et d’utilisation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** du menu **Outils de configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**.  
  
## <a name="procedures"></a>Procédures  
  
#### <a name="to-add-features-to-an-instance-of-includesscurrentincludessscurrent-mdmd"></a>Pour ajouter des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur setup.exe. Si la boîte de dialogue Installation de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] s’affiche, [!INCLUDE[clickOK](../../includes/clickok-md.md)] pour installer les composants requis, puis sur **Annuler** pour quitter l’installation de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  
  
2.  L'Assistant Installation lancera le Centre d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ajouter une nouvelle fonctionnalité à une instance existante de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], cliquez sur **Installation** dans la zone de navigation gauche, puis cliquez sur **Nouvelle installation autonome [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou ajout de fonctionnalités à une installation existante**.  
  
3.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour afficher les détails de vérification, cliquez sur **Afficher les détails**. Pour continuer, [!INCLUDE[clickOK](../../includes/clickok-md.md)].  
  
4.  Dans la page Mises à jour du produit, les dernières mises à jour disponibles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'affichent. Si vous ne souhaitez pas inclure les mises à jour, décochez la case **Inclure les mises à jour du produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Si aucune mise à jour du produit n'est découverte, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'affiche pas cette page et passe automatiquement à la page **Installer les fichiers d'installation** .  
  
5.  Sur la page Installer les fichiers d'installation, le programme d'installation fournit la progression du téléchargement, de l'extraction et de l'installation des fichiers d'installation. Si une mise à jour de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est trouvée, et est spécifiée pour être incluse, cette mise à jour est également installée. Cliquez sur **Installer** pour installer les fichiers de support d’installation.  
  
6.  L'outil d'analyse de configuration système vérifiera l'état système de votre ordinateur avant que le programme d'installation ne se poursuive.  
  
7.  Dans la page Type d’installation, sélectionnez l’option **Ajouter des fonctionnalités à une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]**, puis sélectionnez l’instance à mettre à jour.  
  
8.  Dans la page Sélection de fonctionnalités, sélectionnez les composants que vous voulez installer. Une description de chaque groupe de composants apparaît dans le volet droit après que vous avez sélectionné le nom de la fonctionnalité. Vous pouvez choisir n'importe quelle combinaison de cases à cocher. Pour plus d’informations, consultez [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md). Chaque composant ne peut être installé qu'une seule fois sur une instance donnée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour installer plusieurs composants, vous devez installer une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les composants requis qui n'ont pas déjà été installés lors de l'étape d'installation décrite plus loin dans cette procédure.  
  
     L'outil d'analyse de configuration système vérifiera l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Cliquez sur **Suivant** pour continuer.  
  
9. La page Espace disque nécessaire calcule l'espace disque requis pour les fonctionnalités que vous spécifiez et compare cet espace à l'espace disque disponible sur l'ordinateur où le programme d'installation s'exécute.  
  
10. Le flux de travail du reste de cet article dépend des fonctionnalités que vous avez spécifiées pour votre installation. Il est possible que les pages ne soient pas toutes visibles, en fonction de vos sélections.  
  
11. Dans la page Configuration du serveur — Comptes de service, spécifiez les comptes de connexion des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez sélectionnées.  
  
     Vous pouvez attribuer le même compte de connexion à tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou configurer chaque compte de service individuellement. Vous pouvez également spécifier si les services démarrent automatiquement, sont démarrés manuellement ou sont désactivés. [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous recommande de configurer les comptes de service individuellement afin de fournir des privilèges moindres pour chaque service, sachant que les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disposent des autorisations minimales requises pour effectuer leurs tâches. Pour plus d’informations, consultez [Configuration du serveur - Comptes de service](http://msdn.microsoft.com/library/c283702d-ab20-4bfa-9272-f0c53c31cb9f) et [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     Pour spécifier le même compte de connexion pour tous les comptes de service dans cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], fournissez les informations d'identification dans les champs en bas de page.  
  
     **Remarque relative à la sécurité** : [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
     Lorsque vous avez terminé de spécifier les informations de connexion pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Suivant**.  
  
12. Utilisez l’onglet **Configuration du serveur — Classement** pour spécifier les classements non définis par défaut pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Configuration du serveur - Classement](http://msdn.microsoft.com/library/e3986870-5be4-458b-b671-5ff12a27b022).  
  
13. Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] — Mise en service du compte pour spécifier les éléments suivants :  
  
    -   Mode de sécurité - Sélectionnez l'authentification Windows ou le mode d'authentification mixte pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous sélectionnez Mode d'authentification mixte, vous devez fournir un mot de passe fort pour le compte administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré.  
  
         Lorsque la connexion d'un périphérique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]est établie, le mécanisme de sécurité est le même pour l'authentification Windows et le mode mixte. Pour plus d’informations, consultez [Configuration du moteur de base de données - Configuration du serveur](http://msdn.microsoft.com/library/834b26bc-49de-4033-88d5-6aa7b1609720).  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — Vous devez spécifier au moins un administrateur système pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, cliquez sur **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou sur **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Configuration du moteur de base de données - Configuration du serveur](http://msdn.microsoft.com/library/834b26bc-49de-4033-88d5-6aa7b1609720).  
  
     Lorsque vous avez fini de modifier la liste, cliquez sur **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**.  
  
14. Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] — Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    >  Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Pour plus d’informations, consultez [Configuration du moteur de base de données – Répertoires de données](http://msdn.microsoft.com/library/9b1fa0fc-623b-479a-afc3-4f13bd850487).  
  
15. Utilisez la page Configuration du [!INCLUDE[ssDE](../../includes/ssde-md.md)] — FILESTREAM pour activer FILESTREAM pour votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur FILESTREAM, consultez [Configuration du moteur de base de données - Filestream](http://msdn.microsoft.com/library/641a10a1-ae52-4d26-8f1c-a032a4aeff02). Pour continuer, cliquez sur Suivant.  
  
16. Utilisez la page Configuration d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] - Attribution de privilèges d'accès aux comptes pour spécifier le mode serveur et les utilisateurs ou comptes qui ont les autorisations d'administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Le mode serveur détermine quel sous-systèmes de mémoire et de stockage sont utilisés sur le serveur. Différents types de solution s'exécutent dans différents modes serveur. Si vous envisagez d'exécuter les bases de données multidimensionnelles de cube sur le serveur, choisissez l'option par défaut, le mode serveur multidimensionnel et d'exploration de données. En ce qui concerne les autorisations d'administrateur, vous devez spécifier au moins un administrateur système pour l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour ajouter le compte sous lequel le programme d'installation de SQL Server s'exécute, cliquez sur le bouton **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, cliquez sur **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposeront des privilèges d'administrateur pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations sur le mode serveur et les autorisations d’administrateur, consultez [Configuration Analysis Services – Mise en service de compte](http://msdn.microsoft.com/library/169b1af2-6fe2-467f-8ca4-919f24c620ce).  
  
     Lorsque vous avez fini de modifier la liste, cliquez sur **OK**. Vérifiez la liste d'administrateurs dans la boîte de dialogue de configuration. Une fois la liste complète, cliquez sur **Suivant**.  
  
17. Utilisez la page Configuration du [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] — Répertoires de données pour spécifier les répertoires d'installation non définis par défaut. Pour installer sur les répertoires par défaut, cliquez sur **Suivant**.  
  
    > [!IMPORTANT]  
    >  Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Pour plus d’informations, consultez [Configuration Analysis Services – Répertoires de données](http://msdn.microsoft.com/library/ef732855-b7af-4f40-a619-5573c1c354bb).  
  
18. Utilisez la page Configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour spécifier le type d'installation de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à créer. Pour plus d’informations sur les modes de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Options de configuration Reporting Services &#40;SSRS&#41;](http://msdn.microsoft.com/library/e4561f6c-bc7f-467e-821a-cde8e5cd7391).  
  
19. Utilisez la page Configuration de Distributed Replay Controller pour spécifier les utilisateurs auxquels vous voulez accorder des autorisations administratives sur le service Distributed Replay Controller. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Controller.  
  
     Cliquez sur le bouton **Ajouter l'utilisateur actuel** pour ajouter les utilisateurs auxquels vous voulez accorder des autorisations d'accès au service Distributed Replay Controller. Cliquez sur le bouton **Ajouter** pour ajouter des autorisations d'accès au service Distributed Replay Controller. Cliquez sur le bouton **Supprimer** pour supprimer des autorisations d'accès du service Distributed Replay Controller.  
  
     Pour continuer, cliquez sur **Suivant**.  
  
20. Utilisez la page Configuration de Distributed Replay Client pour spécifier les utilisateurs auxquels vous voulez accorder des autorisations administratives sur le service Distributed Replay Client. Les utilisateurs qui disposent d'autorisations administratives ont un accès illimité au service Distributed Replay Client.  
  
     **Nom du contrôleur** est un paramètre optionnel. La valeur par défaut est \<*blank*>. Entrez le nom du contrôleur avec lequel l'ordinateur client communiquera pour le service Distributed Replay Client. Notez les points suivants :  
  
    -   Si vous avez déjà configuré un contrôleur, entrez son nom lors de la configuration de chaque client.  
  
    -   Si vous n'avez pas encore configuré de contrôleur, vous n'êtes pas obligé de fournir le nom du contrôleur. Toutefois, vous devez entrer manuellement le nom du contrôleur dans le fichier de **configuration du client** .  
  
     Spécifiez le **Répertoire de travail** du service Distributed Replay Client. Le répertoire de travail par défaut est \<*lettre de lecteur*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\WorkingDir\\.  
  
     Spécifiez le **Répertoire des résultats** du service Distributed Replay Client. Le répertoire des résultats par défaut est \<*lettre de lecteur*>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\DReplayClient\ResultDir\\.  
  
     Pour continuer, cliquez sur **Suivant**.  
  
21. Dans la page Création de rapports d'erreurs, spécifiez les informations que vous souhaitez envoyer à [!INCLUDE[msCoName](../../includes/msconame-md.md)] qui aideront à améliorer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les options de création de rapports d'erreurs sont activées par défaut.  
  
22. L'outil d'analyse de configuration système exécutera un autre ensemble de règles pour valider la configuration de votre ordinateur avec les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez spécifiées.  
  
23. La page Prêt pour l'installation affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Dans cette page, le programme d'installation indique si la fonctionnalité de mise à jour du produit est activée ou désactivée et la version de la mise à jour finale. Après avoir cliqué sur Installer, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installera d'abord les composants obligatoires pour les fonctionnalités sélectionnées suivis par l'installation de fonctionnalité.  
  
24. Au cours de l'installation, la page Progression de l'installation fournit des informations sur l'état pour que vous puissiez contrôler la progression de l'installation au fil de l'avancement de l'exécution du programme d'installation.  
  
25. Après l'installation, la page Terminé fournit un lien vers le fichier journal résumé pour l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**.  
  
26. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="next-steps"></a>Next Steps  
 Configurer votre installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Pour réduire la surface d'exposition d'un système, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe et active de façon sélective les services et les fonctionnalités clés. Pour plus d'informations, consultez [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et lire les fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Valider une installation SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)   
 [Réparer une installation défectueuse de SQL Server 2016](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)   
 [Effectuer une mise à niveau de SQL Server à l’aide de l’Assistant Installation &#40;Installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)  
  
  
