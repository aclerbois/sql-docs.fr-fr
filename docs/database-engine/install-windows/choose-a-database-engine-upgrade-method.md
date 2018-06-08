---
title: Choisir une méthode de mise à niveau du moteur de base de données | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology:
- server-general
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 5e57a427-2e88-4ef6-b142-4ccad97bcecc
caps.latest.revision: 23
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9504b667d4bf6a3e955ca3c1f048a1238bbbd024
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771445"
---
# <a name="choose-a-database-engine-upgrade-method"></a>Choisir une méthode de mise à niveau du moteur de base de données

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
  
  Si vous planifiez une mise à niveau du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à partir d’une version Release antérieure de SQL Server, pour réduire les interruptions de service et les risques, plusieurs approches sont possibles. Vous pouvez effectuer une mise à niveau sur place, une migration vers une nouvelle installation ou une mise à niveau propagée. Le diagramme suivant vous aidera à choisir l’approche appropriée. Les différentes approches sont également décrites ci-dessous. Pour vous aider avec les points de décision du diagramme, voir aussi [Plan and Test the Database Engine Upgrade Plan](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
 ![Arbre de décision de la méthode de mise à niveau du moteur de base de données](../../database-engine/install-windows/media/database-engine-upgrade-method-decision-tree.png "Arbre de décision de la méthode de mise à niveau du moteur de base de données")  
  
 **Télécharger**  
  
-   Pour télécharger [!INCLUDE[SSnoversion](../../includes/ssnoversion-md.md)], accédez au  **[Centre d’évaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server)**.  
  
-   Vous avez un compte Azure ?  Cliquez **[ici](http://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016)** pour lancer une machine virtuelle avec [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Developer Edition déjà installé.  
  
> [!NOTE]  
>  Vous pouvez également envisager de mettre à niveau la Base de données SQL Azure ou de virtualiser votre environnement SQL Server dans le cadre de votre plan de mise à niveau. Bien que ces sujets ne soient pas abordés dans cet article, voici quelques liens :
>   - [Vue d’ensemble de SQL Server sur des machines virtuelles Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/)
>   - [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/) 
>   - [Sélection d’une option SQL Server dans Azure](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/).  
  
##  <a name="UpgradeInPlace"></a> Mettre à niveau sur place  
 Avec cette approche, le programme d’installation de SQL Server met à niveau l’installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existante en remplaçant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le nouveau [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], puis met à niveau les bases de données système et utilisateur.  La mise à niveau sur place est l’approche la plus simple. Toutefois, elle implique un temps mort, exige plus de temps si un rétablissement s’avère nécessaire, et n’est pas prise en charge dans tous les cas de figure. Pour plus d’informations sur les scénarios de mise à niveau sur place pris en charge et non pris en charge, consultez [Mises à niveau de la version et de l’édition prises en charge](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md).  
  
 Cette approche est souvent utilisée dans les cas suivants :  
  
-   Environnement de développement sans configuration à haute disponibilité.  
  
-   Environnement de production non stratégique pouvant tolérer un temps mort et s’exécutant sur du matériel des logiciels récents. La durée du temps d’arrêt dépend de la taille de votre base de données et de la vitesse de votre sous-système d’E/S. Une mise à niveau de SQL Server 2014 quand les tables optimisées en mémoire sont en cours d’utilisation prend plus de temps. Pour plus d’informations, consultez [Plan and Test the Database Engine Upgrade Plan](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
> [!WARNING]  
>  Lorsque vous exécutez le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est arrêtée, puis redémarrée dans le cadre de l’exécution des vérifications préalables à la mise à niveau.  
  
> [!CAUTION]  
>  Lorsque vous effectuerez une mise à niveau vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] précédente sera remplacée et n'existera plus sur votre ordinateur. Avant d'opérer la mise à niveau, sauvegardez les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les autres objets associés à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] précédente.  
  
 Le diagramme suivant fournit une vue d’ensemble des étapes requises pour une mise à niveau sur place du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 ![Mise à niveau du moteur de base de données - Mise à niveau sur place sans configuration à haute disponibilité](../../database-engine/install-windows/media/database-engine-upgrade-non-ha-in-place-upgrade.png "Mise à niveau du moteur de base de données - Mise à niveau sur place sans configuration à haute disponibilité")  
  
 Pour obtenir des instructions détaillées, consultez [Mettre à niveau SQL Server à l’aide de l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
##  <a name="NewInstallationUpgrade"></a> Migrer vers une nouvelle installation  
 Avec cette approche, vous conservez l’environnement actuel tout en construisant un nouvel environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , souvent sur un nouveau matériel et avec une nouvelle version du système d’exploitation. Après l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le nouvel environnement, vous effectuer un certain nombre d’étapes pour préparer le nouvel environnement afin de pouvoir migrer les bases de données utilisateur de l’environnement existant vers le nouvel environnement, tout en réduisant le temps mort. Ces étapes comprennent la migration des éléments suivants :  
  
-   **Objets système :** certaines applications dépendent d’informations, d’entités ou d’objets qui sont hors de portée d’une base de données à utilisateur unique. Généralement, une application possède des dépendances sur les bases de données master et msdb, ainsi que la base de données utilisateur. Les informations stockées en dehors d'une base de données utilisateur et nécessaires au bon fonctionnement de cette base de données doivent être disponibles sur l'instance du serveur de destination. Par exemple, les connexions pour une application sont stockées en tant que métadonnées dans la base de données master, et doivent être recréées sur le serveur de destination. Si un plan de maintenance d’application ou de base de données dépend des travaux de l’Agent SQL Server dont les métadonnées sont stockées dans la base de données msdb, vous devez recréer ces travaux sur l’instance du serveur de destination. De la même façon, les métadonnées d’un déclencheur de niveau serveur sont stockées dans la base de données master.  
 
   Lorsque vous déplacez la base de données d’une application vers une autre instance de serveur, vous devez recréer toutes les métadonnées des entités et des objets dépendants dans les bases de données master et msdb sur l’instance du serveur de destination. Par exemple, si une application de base de données utilise des déclencheurs au niveau serveur, il ne suffit pas d'attacher ou de restaurer la base de données sur le nouveau système. La base de données ne fonctionne pas comme prévu, sauf si vous recréez manuellement les métadonnées pour ces déclencheurs dans la base de données master. Pour plus d’informations, consultez [Gérer les métadonnées lors de la mise à disposition d’une base de données sur une autre instance de serveur &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
-   **Packages Integration Services SDB stockés dans MSDB :** si vous stockez des packages dans MSDB, vous devez soit écrire ces packages dans un script à l’aide de l’ [dtutil Utility](../../integration-services/dtutil-utility.md) , soit les redéployer sur le nouveau serveur. Avant d’utiliser les packages sur le nouveau serveur, vous devez mettre à niveau les packages vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, voir [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
-   **Clés de chiffrement de Reporting Services :** une part importante de la configuration d’un serveur de rapports est réservée à la création d’une copie de sauvegarde de la clé symétrique utilisée pour le chiffrement d’informations confidentielles. Cet exemplaire de clé sauvegardée est nécessaire dans de nombreuses opérations courantes. Elle vous permet de réutiliser une base de données de serveur de rapports existante dans une nouvelle installation. Pour plus d’informations, consultez [Sauvegarder et restaurer les clés de chiffrement Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md) et [Mettre à niveau et faire migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
  
 Une fois que le nouvel environnement   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possède les mêmes objets système en tant que l’environnement existant, vous migrez les bases de données utilisateur à partir du système existant vers l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’une manière qui réduit les interruptions de service sur le système existant. Vous effectuez la migration de base de données à l’aide d’une sauvegarde et d’une restauration, ou en repointant les LUN si vous êtes dans un environnement SAN. Les étapes des deux méthodes sont exposées dans les schémas ci-dessous.  
  
> [!CAUTION]  
>  La durée du temps d’arrêt dépend de la taille de votre base de données et de la vitesse de votre sous-système d’E/S. Une mise à niveau de SQL Server 2014 quand les tables optimisées en mémoire sont en cours d’utilisation prend plus de temps. Pour plus d’informations, consultez [Planifier et tester le plan de mise à niveau du moteur de base de données](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
 Après la migration des bases de données utilisateur, vous pointez les nouveaux utilisateurs sur la nouvelle instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de diverses méthodes (par exemple, en renommant le serveur, en utilisant une entrée DNS, en modifiant des chaînes de connexion).  La nouvelle approche de l’installation réduit les risques et les temps morts par rapport à une mise à niveau sur place. Elle facilite également les mises à niveau du matériel et du système d’exploitation conjointement avec la mise à niveau vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Si vous avez déjà une solution à haute disponibilité en place ou d’autres environnements d’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], accédez à [Mise à niveau propagée](#RollingUpgrade). Si vous n’avez pas de solution à haute disponibilité en place, vous pouvez envisager de configurer temporairement une [mise en miroir de base de données](http://msdn.microsoft.com/library/ms190941.aspx) pour réduire le temps d’arrêt afin de faciliter cette mise à niveau ou de saisir cette opportunité pour configurer un [groupe de disponibilité Always On](http://msdn.microsoft.com/library/hh510260.aspx) en tant que solution à haute disponibilité permanente.  
  
 Par exemple, vous pouvez utiliser cette approche pour mettre à niveau :  
  
-   Une installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un système d’exploitation non pris en charge.  
  
-   Une installation x86 de SQL Server en tant que [!INCLUDE[ss2016](../../includes/sssql15-md.md)] et ultérieur ne prend pas en charge les installations x86.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers un nouveau matériel ou une nouvelle version du système d’exploitation.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec la consolidation des serveurs.  
  
-   Comme [!INCLUDE[ss2016](../../includes/sssql15-md.md)] et ultérieur, SQL Server 2005 ne prend pas en charge la mise à niveau sur place de SQL Server 2005. Pour plus d’informations, consultez [Effectuez-vous une mise à niveau à partir de SQL Server 2005 ?](../../database-engine/install-windows/are-you-upgrading-from-sql-server-2005.md).  
  
 Les étapes requises pour une nouvelle mise à niveau d’installation varient légèrement selon que vous utilisez un stockage SAN ou un stockage connecté.  
  
-   **Environnement de stockage connecté :** si vous avez un environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisant un stockage connecté, le diagramme suivant et les liens dans le diagramme vous guident dans les étapes requises pour la mise à niveau de la nouvelle installation vers [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
     ![Nouvelle méthode de mise à niveau de l’installation utilisant la sauvegarde et la restauration pour le stockage connecté](../../database-engine/install-windows/media/new-installation-upgrade-method-using-backup-and-restore-for-attached-storage.png "Nouvelle méthode de mise à niveau de l’installation utilisant la sauvegarde et la restauration pour le stockage connecté")  
  
-   **Environnement de stockage SAN :** si vous avez un environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisant un stockage SAN, le diagramme suivant et les liens dans le diagramme vous guident dans les étapes requises pour la mise à niveau de la nouvelle installation vers [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
     ![Nouvelle méthode de mise à niveau de l’installation utilisant le détachement et l’attachement pour le stockage SAN](../../database-engine/install-windows/media/new-installation-upgrade-method-using-detach-and-attach-for-san-storage.png "Nouvelle méthode de mise à niveau de l’installation utilisant le détachement et l’attachement pour le stockage SAN")  
  
##  <a name="RollingUpgrade"></a> Mise à niveau propagée  
 Une mise à niveau propagée est requise dans les environnements de solution SQL Server impliquant plusieurs instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui doivent être mises à niveau dans un certain ordre afin d’optimiser le temps de fonctionnement, de minimiser les risques et de préserver la fonctionnalité. Une mise à niveau propagée est essentiellement la mise à niveau de plusieurs instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un ordre spécifique, soit en effectuant une mise à niveau sur place sur chaque instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]existante, soit en mettant à niveau une nouvelle installation pour faciliter la mise à niveau du matériel et/ou du système d’exploitation dans le cadre du projet de mise à niveau. Il existe plusieurs scénarios dans lesquels vous devez utiliser l’approche de mise à niveau propagée. Ces résultats sont décrits dans les articles suivants :  
  
-   Groupes de disponibilité Always On : pour obtenir des instructions détaillées sur l’exécution d’une mise à niveau propagée dans cet environnement, consultez [Mise à niveau d’instances de réplica d’un groupe de disponibilité Always On](../../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).  
  
-   Instances de cluster de basculement : pour obtenir des instructions détaillées sur l’exécution d’une mise à niveau propagée dans cet environnement, consultez [Mise à niveau d’une instance de cluster de basculement SQL Server](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md).  
  
-   Instances mises en miroir : pour obtenir des instructions détaillées sur l’exécution d’une mise à niveau propagée dans cet environnement, consultez [Mise à niveau des instances en miroir](../../database-engine/database-mirroring/upgrading-mirrored-instances.md).  
  
-   Instances de copie des journaux de transaction : pour obtenir des instructions détaillées sur l’exécution d’une mise à niveau propagée dans cet environnement, consultez [Mise à niveau de la copie des journaux de transaction pour SQL Server &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md).  
  
-   Environnement de réplication : pour obtenir des instructions détaillées pour effectuer une mise à niveau propagée dans cet environnement, consultez [Mettre à niveau des bases de données répliquées](../../database-engine/install-windows/upgrade-replicated-databases.md).
  
-   Environnement avec montée en puissance de SQL Server Reporting Services : pour obtenir des instructions détaillées sur l’exécution d’une mise à niveau propagée dans cet environnement, consultez [Mettre à niveau et faire migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
## <a name="next-steps"></a>Étapes suivantes
 [Planifier et tester le plan de mise à niveau du moteur de base de données](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md)   
 [Mise à niveau du moteur de base de données](../../database-engine/install-windows/complete-the-database-engine-upgrade.md)  
  
  
