---
title: Mettre à niveau une instance de cluster de basculement SQL Server (programme d’installation) | Microsoft Docs
ms.custom: ''
ms.date: 01/22/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- upgrading clusters
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], creating clusters
- clusters [SQL Server], creating
- failover clustering [SQL Server], upgrading
ms.assetid: ea8b7d66-e5a1-402f-9928-8f7310e84f5c
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 5067ddd340bfd8493fa0df2bdb9cd15ed5b20d9d
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772175"
---
# <a name="upgrade-a-sql-server-failover-cluster-instance-setup"></a>Mettre à niveau une instance de cluster de basculement SQL Server (programme d'installation)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez mettre à niveau un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers un cluster de basculement [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l’aide de l’interface utilisateur d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou à partir d’une invite de commandes.  
  
 Dans le cas d'une installation locale, vous devez exécuter le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en qualité d'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui dispose des autorisations de lecture sur le partage distant.  
  
 Avant d’effectuer la mise à niveau, consultez [Mise à niveau d’une instance de cluster de basculement SQL Server](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md).  
  
##  <a name="UpgradeSteps"></a> Pour mettre à niveau un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
#### <a name="to-upgrade-a-includessnoversionincludesssnoversion-mdmd-failover-cluster"></a>Pour mettre à niveau un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
1.  Sur le média d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] portant sur l’édition qui correspond à celle que vous mettez à niveau, double-cliquez sur le fichier setup.exe dans le dossier racine. Il se peut que vous deviez installer les composants requis si ceux-ci ne sont pas déjà présents sur l'ordinateur.  
  
2.  Lorsque les composants requis sont installés, l'Assistant Installation démarre le Centre d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour mettre à niveau une instance existante de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], sélectionnez votre instance.  
  
3.  Si les fichiers de support du programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont requis, le programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] les installe. Redémarrez votre ordinateur si vous êtes invité à le faire avant de continuer.  
  
4.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, [!INCLUDE[clickOK](../../../includes/clickok-md.md)].  
  
5.  Dans la page Clé de produit, entrez la clé PID pour l'édition de la nouvelle version qui correspond à l'édition de l'ancienne version du produit. Par exemple, pour mettre à niveau un cluster de basculement Enterprise Edition, vous devez spécifier une clé PID pour [!INCLUDE[ssEnterprise](../../../includes/ssenterprise-md.md)]. Cliquez sur **Suivant** pour continuer. N'oubliez pas que la clé PID que vous utilisez pour une mise à niveau de cluster de basculement doit être cohérente sur tous les nœuds de cluster de basculement dans la même instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
6.  Dans la page Termes du contrat de licence, prenez connaissance du contrat de licence, puis activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Cliquez sur**Suivant**pour continuer. Pour mettre fin au programme d'installation, cliquez sur **Annuler**.  
  
7.  Dans la page Sélectionner une instance, spécifiez l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à mettre à niveau vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Cliquez sur**Suivant**pour continuer.  
  
8.  Dans la page Sélection de composant, les fonctionnalités à mettre à niveau sont présélectionnées. Une description de chaque groupe de composants apparaît dans le volet droit après que vous avez sélectionné le nom de la fonctionnalité. N'oubliez pas que vous ne pouvez pas modifier les fonctionnalités à mettre à niveau, de même que vous ne pouvez pas ajouter de fonctionnalités pendant l'opération de mise à niveau. Pour ajouter des fonctionnalités à une instance mise à niveau de [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] une fois l’opération de mise à niveau terminée, consultez [Ajouter des fonctionnalités à une instance de SQL Server 2016 &#40;programme d’installation&#41;](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-2016-setup.md).  
  
     Les composants requis pour les fonctionnalités sélectionnées sont affichés dans le volet droit. Le programme d'installation de SQL Server installera les composants requis qui ne sont pas déjà installés pendant l'étape d'installation décrite ultérieurement dans cette procédure. Pour gagner du temps, préinstallez ces composants requis sur chaque nœud.  
  
9. Dans la page Configuration de l'instance, les champs sont remplis automatiquement à partir de l'ancienne instance. Vous pouvez spécifier la nouvelle valeur d'InstanceID.  
  
     **ID d’instance** - Par défaut, le nom de l’instance est utilisé comme ID d’instance. Il permet d'identifier les répertoires d'installation et les clés de Registre de votre instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Tel est le cas pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d'instance non défini par défaut, activez la case à cocher **ID d'instance** et entrez une valeur. Si vous remplacez la valeur par défaut, vous devez spécifier le même ID d'instance pour l'instance mise à niveau sur tous les nœuds de cluster de basculement. L'ID d'instance de l'instance mise à niveau doit être identique sur tous les nœuds.  
  
     **Instances et fonctionnalités détectées** - La grille affiche les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui se trouvent sur l’ordinateur où le programme d’installation s’exécute. Cliquez sur**Suivant**pour continuer.  
  
10. La page Espace disque nécessaire calcule l'espace disque nécessaire pour les fonctionnalités que vous spécifiez et compare cet espace à l'espace disque disponible sur l'ordinateur où le programme d'installation s'exécute.  
  
11. Dans la page de mise à niveau de recherche en texte intégral, spécifiez les options de mise à niveau pour les bases de données mises à niveau. Pour plus d’informations, consultez [Options de mise à niveau de recherche en texte intégral](http://msdn.microsoft.com/library/16c9376b-5fbb-4495-a429-06a2493849c9).  
  
12. Dans la page **Création de rapports d'erreurs** , spécifiez les informations que vous souhaitez envoyer à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] afin d'aider à améliorer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les options de création de rapports d'erreurs sont activées par défaut.  
  
13. L'Outil d'analyse de configuration système exécute un autre ensemble de règles pour valider la configuration de votre ordinateur avec les fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous avez spécifiées, avant le début de l'opération de mise à niveau.  
  
14. La page Rapport de mise à niveau de cluster affiche la liste des nœuds de l'instance de cluster de basculement, ainsi que les informations de version de l'instance pour les composants [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de chaque nœud. Elle affiche l'état des scripts de base de données et l'état des scripts de réplication. De plus, elle affiche également des messages d'information sur ce qui doit se produire lorsque vous cliquez sur **Suivant**. Selon le nombre de nœuds de cluster de basculement déjà mis à niveau et le nombre total de nœuds, le programme d’installation affiche une description du comportement du basculement lorsque vous cliquez sur **Suivant**. Il indique également le temps mort inutile potentiel, si vous n'avez pas déjà installé les composants requis.  
  
15. La page Prêt pour la mise à niveau affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Mettre à niveau**. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installe d'abord les composants requis pour les fonctionnalités sélectionnées, puis installe les fonctionnalités.  
  
16. Au cours de la mise à niveau, la page Progression de l'installation fournit des informations d'état pour que vous puissiez contrôler la progression de la mise à niveau sur le nœud actuel au fil de l'exécution du programme d'installation.  
  
17. Une fois la mise à niveau du nœud actuel terminée, la page Rapport de mise à niveau de cluster affiche des informations sur l'état de la mise à niveau pour tous les nœuds de cluster de basculement, les fonctionnalités sur chaque nœud de cluster de basculement, ainsi que leurs informations de version. Vérifiez les informations de version qui sont affichées et poursuivez la mise à niveau des nœuds restants. En cas de basculement des nœuds mis à niveau, ces informations s'affichent également sur la page d'état. Vous pouvez également procéder à une vérification supplémentaire dans l'Administrateur de cluster Windows.  
  
18. Après la mise à niveau, la page Terminé fournit un lien vers le fichier journal résumé de l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**.  
  
19. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
20. Pour terminer la mise à niveau, répétez ces étapes sur tous les autres nœuds du cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="to-upgrade-a-includessnoversionincludesssnoversion-mdmd-multi-subnet-failover-cluster"></a>Pour mettre à niveau un cluster de basculement de sous-réseaux multiples [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
#### <a name="to-upgrade-to-a-includessnoversionincludesssnoversion-mdmd-multi-subnet-failover-cluster-existing-includessnoversionincludesssnoversion-mdmd-cluster-is-a-non-multi-subnet-cluster"></a>Pour mettre à niveau vers un cluster de basculement de sous-réseaux multiples [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (le cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existant est un cluster de sous-réseaux non multiples.)  
  
1.  Suivez la procédure ci-dessus pour mettre à niveau votre cluster vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
2.  Ajoutez un nœud sur un sous-réseau différent à l'aide de l'action d'installation AddNode et confirmez la dépendance de ressource d'adresse IP sur OR dans la page **Configuration du réseau du cluster** . Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
#### <a name="to-upgrade-a-multi-subnet-cluster-currently-using-stretch-v-lan"></a>Pour mettre à niveau un cluster de sous-réseaux multiples utilisant actuellement l'étirement V-LAN  
  
1.  Suivez la procédure ci-dessus pour mettre à niveau votre cluster vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
2.  Modifiez les paramètres réseau pour déplacer le nœud distant vers un autre sous-réseau.  
  
3.  À l'aide de l'outil d'administration de cluster de basculement Windows, ajoutez une nouvelle adresse IP pour le nouveau sous-réseau et définissez la dépendance de ressource d'adresse IP sur OR.  
  
## <a name="next-steps"></a>Next Steps  
 Après avoir effectué la mise à niveau vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], effectuez les tâches suivantes :  
  
-   [Mise à niveau du moteur de base de données](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)  
  
-   [Modifier le mode de compatibilité de base de données et utiliser le magasin des requêtes](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)  
  
-   [Tirer parti des nouveautés de SQL Server 2016](http://msdn.microsoft.com/library/d8879659-8efa-4442-bcbb-91272647ae16)  
  
## <a name="see-also"></a> Voir aussi  
 [Mise à niveau d’une instance de cluster de basculement SQL Server](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)   
 [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Ajouter des fonctionnalités à une instance de SQL Server 2016 &#40;programme d’installation&#41;](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-2016-setup.md)  
  
  
