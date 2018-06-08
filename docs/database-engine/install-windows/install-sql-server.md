---
title: Installer SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: get-started-article
helpviewer_keywords:
- AdventureWorks sample database
- installing SQL Server, preparing to install
- installation [SQL Server]
ms.assetid: 0300e777-d56b-4d10-9c33-c9ebd2489ee5
caps.latest.revision: 59
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 6ea964d0048b410dd5f555759c4a73f547837be9
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771089"
---
# <a name="install-sql-server"></a>Installer SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
 
 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [Installer SQL Server 2014](https://msdn.microsoft.com/library/bb500395(SQL.120).aspx).

 À partir de [!INCLUDE[sssql15](../../includes/sssql15-md.md)], [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] est disponible uniquement en tant qu’application 64 bits. Voici des informations importantes sur la façon d’obtenir SQL Server et de l’installer.

## <a name="installation-details"></a>Détails de l’installation
  
*  **Options**: effectuez l’installation par le biais de l’Assistant Installation, de l’outil sysprep ou d’une invite de commandes
 
*  **Configuration requise**: avant de procéder à l’installation, prenez connaissance de la configuration requise, des vérifications de configuration du système et des considérations sur la sécurité décrites dans [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md) 

* **Processus**: consultez [Installation de SQL Server](../../database-engine/install-windows/installation-for-sql-server-2016.md) pour obtenir des instructions complètes sur le processus d’installation

* **Exemples de bases de données et exemples de code**: 
    * Par défaut, ils ne sont pas installés dans le cadre de l’installation de SQL Server 
    * Pour les installer avec les éditions non-Express de SQL Server, consultez [GitHub](http://github.com/Microsoft/sql-server-samples)
    

## <a name="get-the-installation-media"></a>Obtenir le média d’installation

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="how-to-install-includessnoversionincludesssnoversion-mdmd"></a>Procédure d’installation de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]
 
|Titre|Description|  
|-----------|-----------------|  
|[Installer [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] sur Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)|Prenez connaissance de cet article pour installer [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] sur Windows Server Core.|  
|[Paramètres de l’outil d’analyse de configuration système](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)|Décrit les fonctions de l'Outil d'analyse de configuration système (SCC).|  
|[Installer [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à partir de l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)|Article de procédure pour une installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] standard à l’aide de l’Assistant Installation.|  
|[Installer[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à partir d’une invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)|Article de procédure qui fournit un exemple de syntaxe et de paramètres d’installation pour exécuter une installation sans assistance.|  
|[Installer [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide d’un fichier de configuration](../../database-engine/install-windows/install-sql-server-2016-using-a-configuration-file.md)|Article de procédure qui fournit un exemple de syntaxe et de paramètres d’installation pour exécuter une installation à l’aide d’un fichier de configuration.|  
|[Installer [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)|Article de procédure qui fournit un exemple de syntaxe et de paramètres d’installation pour exécuter une installation à l’aide de SysPrep.|  
|[Ajouter des fonctionnalités à une instance de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] &#40;programme d’installation&#41;](../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-2016-setup.md)|Article de procédure pour mettre à jour les composants d’une instance existante de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Réparer une installation défectueuse de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)|Article de procédure pour réparer une installation endommagée de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Renommer un ordinateur qui héberge une instance autonome de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)|Article de procédure pour mettre à jour les métadonnées système stockées dans sys.servers.|  
|[Installer les mises à jour de maintenance de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/install-sql-server-servicing-updates.md)|Article de procédure portant sur l’installation des mises à jour de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Afficher et lire les fichiers journaux d'installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)|Article de procédure pour rechercher des erreurs dans les fichiers journaux d’installation.|  
|[Valider une installation SQL Server](../../database-engine/install-windows/validate-a-sql-server-installation.md)|Vérifiez l'utilisation du rapport de découverte SQL pour vérifier la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installées sur l'ordinateur.|  
  
  
## <a name="how-to-install-individual-components"></a>Comment installer des composants individuels  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Installer le moteur de base de données SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md)|Explique comment installer et configurer le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].|  
|[Installer la réplication SQL Server](../../database-engine/install-windows/install-sql-server-replication.md)|Explique comment installer et configurer la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Install Distributed Replay - Présentation](../../tools/distributed-replay/install-distributed-replay-overview.md)|Répertorie les articles pour installer la fonctionnalité Distributed Replay.|  
|[Installer les Outils d’administration SQL Server avec SSMS](http://msdn.microsoft.com/library/af68d59a-a04d-4f23-9967-ad4ee2e63381)|Décrit comment installer et configurer les outils d'administration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Installer SQL Server PowerShell](../../database-engine/install-windows/install-sql-server-powershell.md)|Décrit les considérations relatives à l'installation des composants PowerShell [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  

## <a name="how-to-configure-sql-server"></a>Pour configurer SQL Server  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|Cet article fournit une vue d’ensemble de la configuration du pare-feu et décrit comment configurer le Pare-feu Windows.|  
|[Configurer un ordinateur multirésident pour l’accès à SQL Server](../../sql-server/install/configure-a-multi-homed-computer-for-sql-server-access.md)|Cet article explique comment configurer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le Pare-feu Windows avec fonctions avancées de sécurité pour fournir des connexions réseau à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un environnement multirésident.|  
|[Configurer le Pare-feu Windows pour autoriser l’accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md)|Vous pouvez suivre les étapes fournies dans cet article pour configurer les paramètres de port et de pare-feu afin d’autoriser l’accès à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint.|  
  
## <a name="related-sections"></a>Sections connexes  
[Éditions et fonctionnalités prises en charge de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/editions-and-supported-features-for-sql-server-2016.md)  
[Installer les fonctionnalités Business Intelligence de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/install-sql-server-business-intelligence-features.md)  
  [Installation d'un cluster de basculement SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)  
 
  
## <a name="see-also"></a>Voir aussi  

[Planification d'une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)   
 [Mise à niveau vers [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/upgrade-sql-server.md)   
 [Désinstaller [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/uninstall-sql-server.md)   
 [Solutions haute disponibilité &#40;SQL Server&#41;](../../sql-server/failover-clusters/high-availability-solutions-sql-server.md)  
  
  
