---
title: Mettre à niveau une instance de cluster de basculement SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 10/01/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- upgrading failover clusters
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
ms.assetid: daac41fe-7d0b-4f14-84c2-62952ad8cbfa
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 07fec7f9606d88f9260d2b2e9ae9d0c0e3066e41
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772215"
---
# <a name="upgrade-a-sql-server-failover-cluster-instance"></a>Mettre à niveau une instance de cluster de basculement SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge la mise à niveau d’un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers une nouvelle version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vers un nouveau service pack ou une nouvelle mise à jour cumulative [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], ou durant l’installation d’un nouveau Service Pack ou une nouvelle mise à jour cumulative Windows séparément sur tous les nœuds de cluster de basculement, avec des temps d’arrêt limités à un seul basculement manuel (ou deux basculements manuels en cas de restauration automatique vers l’instance principale d’origine).  
  
 La mise à niveau du système d’exploitation Windows d’un cluster de basculement n’est pas prise en charge par les systèmes d’exploitation antérieurs à [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)]. Pour mettre à niveau un nœud de cluster s’exécutant sur [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)] ou version supérieure, consultez [Exécuter une mise à jour ou mise à niveau propagée](#perform-a-rolling-upgrade-or-update).  
  
 Voici les détails de prise en charge :  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est prise en charge par le biais de l’interface utilisateur et à partir de l’invite de commandes. Vous pouvez exécuter une mise à niveau à partir de l’invite de commandes sur chaque nœud de cluster de basculement. Vous pouvez aussi utiliser l’interface utilisateur du programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour mettre à niveau chaque nœud de cluster.  Pour plus d’informations, consultez [Mettre à niveau une instance de cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance-setup.md) et [Installer SQL Server à partir de l’invite de commandes](../../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md).  
  
-   Les scénarios suivants ne sont pas pris en charge dans le cadre de la mise à niveau de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
    -   Vous ne pouvez pas effectuer une mise à niveau à partir d’une instance autonome de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers un cluster de basculement.  
  
    -   Vous ne pouvez pas ajouter de fonctionnalités à un cluster de basculement. par exemple ajouter le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] à un cluster de basculement existant [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]uniquement.  
  
    -   Vous ne pouvez pas rétrograder un nœud de cluster de basculement à une instance autonome.  
  
    -   La modification de l'édition du cluster de basculement est limitée à certains scénarios. Pour plus d’informations, consultez [Mises à niveau de la version et de l’édition prises en charge](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
-   Pendant la mise à niveau du cluster de basculement, le temps mort est limité au temps de basculement et au délai d'exécution des scripts de mise à niveau. Si vous suivez le processus de mise à niveau propagée du cluster de basculement ci-dessous et respectez tous les prérequis sur tous les nœuds avant de commencer le processus de mise à niveau, le temps mort est minime. Une mise à niveau de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] quand les tables à mémoire optimisée sont en cours d’utilisation prend plus de temps. Pour plus d’informations, consultez [Planifier et tester le plan de mise à niveau du moteur de base de données](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md).  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Avant de commencer, passez en revue les informations importantes suivantes :  
  
-   [Mises à niveau de la version et de l’édition prises en charge](../../../database-engine/install-windows/supported-version-and-edition-upgrades.md) : vérifiez que vous pouvez procéder à une mise à niveau vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à partir de votre version du système d’exploitation Windows et de la version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Par exemple, vous ne pouvez pas effectuer directement la mise à niveau d’une instance de cluster de basculement SQL Server 2005 vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] ou mettre à niveau un cluster de basculement exécutant [!INCLUDE[winxpsvr-md](../../../includes/winxpsvr-md.md)].  
  
-   [Choose a Database Engine Upgrade Method](../../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): sélectionnez la méthode et les étapes de mise à niveau appropriées en fonction des versions et mises à niveau prises en charge ainsi que des autres composants installés dans votre environnement pour mettre à niveau les composants dans le bon ordre.  
  
-   [Planifier et tester le plan de mise à niveau du moteur de base de données](../../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): consultez les notes de version et les problèmes de mise à niveau connus, ainsi que la liste de contrôle préalable à la mise à niveau, puis développez et testez votre plan de mise à niveau.  
  
-   [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) : prenez connaissance de la configuration logicielle requise pour installer [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]. Si des logiciels supplémentaires sont nécessaires, installez-les sur chaque nœud avant de commencer le processus de mise à niveau pour réduire les éventuels temps d’arrêt.  
  
## <a name="perform-a-rolling-upgrade-or-update"></a>Exécuter une mise à jour ou mise à niveau propagée  
 Pour mettre à niveau un cluster du basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)], utilisez le programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour mettre à niveau chaque nœud de cluster de basculement à la fois, en commençant par les nœuds passifs. Lorsque vous mettez à niveau chaque nœud, celui-ci est écarté de la liste des propriétaires possibles du cluster de basculement. En cas de basculement inattendu, les nœuds mis à niveau ne participent pas au basculement tant que la propriété du groupe de ressources de cluster n’est pas transférée vers un nœud mis à niveau par le programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Par défaut, le programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] détermine automatiquement le moment où le basculement doit être effectué vers un nœud mis à niveau. Cela dépend du nombre total de nœuds dans l'instance de cluster de basculement et du nombre de nœuds qui ont déjà été mis à niveau. Lorsque la moitié des nœuds ou plus a déjà été mise à niveau, le programme d’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provoque un basculement vers un nœud mis à niveau lorsque vous effectuez une mise à niveau du nœud suivant. Lors du basculement vers un nœud mis à niveau, le groupe de clusters est déplacé vers un nœud mis à niveau. Tous les nœuds mis à niveau sont placés dans la liste des propriétaires possibles ; par ailleurs, tous les nœuds qui ne sont pas encore mis à niveau sont retirés de la liste des propriétaires possibles. Lorsque vous mettez à niveau chaque nœud restant, celui-ci est ajouté aux propriétaires possibles du cluster de basculement.  
  
 Ce processus entraîne une limitation de la durée du temps mort au temps de basculement et au délai d'exécution des scripts de mise à niveau de la base de données pendant l'ensemble de la mise à niveau du cluster de basculement.  
  
 Pour contrôler le comportement du basculement des nœuds de cluster pendant le processus de mise à niveau, exécutez l'opération de mise à niveau à l'invite de commandes et utilisez le paramètre /FAILOVERCLUSTERROLLOWNERSHIP. Pour plus d’informations, consultez [Installer SQL Server à partir de l’invite de commandes](../../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md).  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Effectuer une mise à niveau de SQL Server à l’aide de l’Assistant Installation &#40;Installation&#41;](../../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Installer SQL Server à partir de l’invite de commandes](../../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)   
 [Mettre à niveau une instance de cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance-setup.md)  
  
  
