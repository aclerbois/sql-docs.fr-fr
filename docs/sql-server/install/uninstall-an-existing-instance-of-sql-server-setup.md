---
title: Désinstaller une instance existante de SQL Server (programme d’installation) | Microsoft Docs
ms.custom: ''
ms.date: 01/27/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- removing instances of SQL Server
- uninstalling instances of SQL Server
- removing SQL Server
- instances of SQL Server, uninstalling
- uninstalling SQL Server
ms.assetid: 3c64b29d-61d7-4b86-961c-0de62261c6a1
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 5bea9587b92e1452891e24475cc33a8baeec437b
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772635"
---
# <a name="uninstall-an-existing-instance-of-sql-server-setup"></a>Désinstaller une instance existante de SQL Server (programme d'installation)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

 > Pour obtenir du contenu relatif à des versions précédentes de SQL Server, consultez [Désinstaller une instance existante de SQL Server (programme d’installation)](https://msdn.microsoft.com/en-US/library/ms143412(SQL.120).aspx).

  Cet article explique comment désinstaller une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En suivant les étapes de cet article, vous préparez également le système afin de pouvoir réinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
>**IMPORTANT !** Pour désinstaller une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez être administrateur local et disposer des autorisations requises pour vous connecter en tant que service.  
  
> **REMARQUE :** Pour désinstaller un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez la fonctionnalité Supprimer un nœud proposée par le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour supprimer chaque nœud individuellement. Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  
  
 Tenez compte des scénarios importants indiqués ci-après avant de désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   Avant de supprimer les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'un ordinateur disposant de la quantité minimale de mémoire physique requise, vous devez vous assurer que la taille du fichier d'échange est suffisante. La taille du fichier d'échange doit être égale à deux fois la quantité de mémoire physique. Une mémoire virtuelle insuffisante peut entraîner la suppression incomplète de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Si vous avez plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser est automatiquement désinstallé lorsque la dernière instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] est elle-même désinstallée.  
  
     Si vous souhaitez désinstaller tous les composants de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vous devez désinstaller manuellement le composant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser en utilisant **Programmes et fonctionnalités** dans le **Panneau de configuration**.  
  
1.  Quand vous désinstallez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les fichiers de données tempdb qui ont été ajoutés pendant le processus d’installation sont supprimés. Les fichiers avec le modèle de nom tempdb_mssql_*.ndf sont supprimés s’ils existent dans le répertoire de base de données système.  
  
### <a name="before-you-uninstall"></a>Avant la désinstallation  
  
1.  **Sauvegardez vos données.** Bien que ce ne soit pas une étape obligatoire, vous pouvez enregistrer des bases de données dans leur état actuel. Peut-être souhaitez-vous également enregistrer les modifications qui ont été apportées aux bases de données système. Dans l'une de ces situations, veillez à sauvegarder les données avant de désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous avez aussi la possibilité d'enregistrer une copie de tous les fichiers journaux et de données dans un dossier autre que le dossier MSSQL. Le dossier MSSQL est supprimé durant la désinstallation.  
  
     Les fichiers que vous devez enregistrer incluent les fichiers de base de données suivants :  
  
    -   Master.mdf  
  
    -   mastlog.ldf  
  
    -   Model.mdf  
  
    -   Modellog.ldf  
  
    -   Msdbdata.mdf  
  
    -   Msdblog.ldf  
  
    -   Mssqlsystemresource.mdf  
  
    -   Mssqlsustemresource.ldf  
  
    -   Tempdb.mdf  
  
    -   Templog.ldf  
  
    -   ReportServer[$InstanceName] : correspond à la base de données par défaut [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
    -   ReportServer[$InstanceName]TempDB : correspond à la base de données temporaire par défaut [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
2.  **Supprimez les groupes de sécurité locaux.** Avant de désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], supprimez les groupes de sécurité locaux des composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  **Arrêtez tous**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **les services.** Nous vous recommandons d'arrêter tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant de désinstaller les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La désinstallation peut échouer s'il existe des connexions actives.  
  
4.  **Utiliser un compte bénéficiant des autorisations appropriées** Connectez-vous au serveur à l'aide du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à l'aide d'un compte doté d'autorisations équivalentes. Par exemple, vous pouvez vous connecter au serveur à l'aide d'un compte qui est membre du groupe Administrateurs local.  
  
### <a name="to-uninstall-an-instance-of-includessnoversionincludesssnoversion-mdmd"></a>To Uninstall an Instance of [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
1.  Pour commencer le processus de désinstallation, cliquez sur **Panneau de configuration** , puis accédez à **Programmes et fonctionnalités**.  
  
2.  Cliquez avec le bouton droit sur **SQL Server 2016** , puis sélectionnez **Désinstaller**. Cliquez ensuite sur **Supprimer**. Ainsi, vous démarrez l'Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Les règles de support du programme d'installation s'exécutent pour vérifier la configuration de votre ordinateur. Pour continuer, cliquez sur **Suivant**.  
  
3.  Dans la page Sélectionner une instance, utilisez la zone de liste déroulante pour spécifier une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à supprimer ou l'option pour supprimer uniquement les fonctionnalités partagées et outils d'administration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour continuer, cliquez sur **Suivant**.  
  
4.  Dans la page Sélectionner les composants, spécifiez les fonctionnalités à supprimer de l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Les règles de suppression s'exécutent pour vérifier que l'opération peut s'effectuer correctement.  
  
5.  Dans la page **Prêt pour la suppression** , consultez la liste des composants et des fonctionnalités à désinstaller. Cliquez sur **Supprimer** pour commencer la désinstallation  
  
6.  Immédiatement après avoir désinstallé la dernière instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les autres programmes associés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont toujours visibles dans la liste des programmes de **Programmes et fonctionnalités**. Toutefois, si vous fermez **Programmes et fonctionnalités**, la prochaine fois que vous ouvrirez **Programmes et fonctionnalités**, la liste des programmes sera actualisée afin de ne montrer que ceux qui sont toujours installés.  
  
### <a name="if-the-uninstallation-fails"></a>En cas d'échec de la désinstallation  
  
1.  Si le processus de désinstallation ne se termine pas correctement, essayez de résoudre le problème à l'origine de cet échec. Les articles suivants peuvent vous aider à comprendre la cause de l'échec de la désinstallation :  
  
    -   [Procédure pour identifier les problèmes d'installation de SQL Server 2008 dans les fichiers journaux d'installation](http://support.microsoft.com/kb/955396/en-us)  
  
    -   [Afficher et lire les fichiers journaux d'installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
2.  Si vous ne parvenez pas à remédier à la cause de l'échec de désinstallation, vous pouvez contacter le support technique de [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Dans certains cas, comme la suppression involontaire de fichiers importants, la réinstallation du système d'exploitation peut être requise avant de réinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'ordinateur.  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et lire les fichiers journaux d'installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
