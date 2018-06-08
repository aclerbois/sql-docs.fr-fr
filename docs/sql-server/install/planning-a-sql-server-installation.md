---
title: Planification d’une installation de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: get-started-article
helpviewer_keywords:
- installing SQL Server, planning
ms.assetid: b1d56f2f-603f-48f2-b902-c715f14a6db9
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0bd9b3fe2a650fcf396f6dde8ba124bca181e14b
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772545"
---
# <a name="planning-a-sql-server-installation"></a>Planification d’une installation de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], suivez ces étapes :  
  
-   Prenez connaissance des conditions requises pour l'installation, des options d'installation, des contrôles de configuration du système et des considérations sur la sécurité pour une installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Exécutez le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour effectuer une installation ou une mise à niveau vers une version ultérieure. Avant d’effectuer une mise à niveau, consultez [Mettre à niveau vers SQL Server](../../database-engine/install-windows/upgrade-sql-server.md).  
  
-   Configurez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide des utilitaires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Indépendamment de la méthode d'installation, vous êtes invité à confirmer l'acceptation des termes de la licence de logiciel en tant que personne physique ou pour le compte d'une entité, sauf si votre utilisation du logiciel est régie par un accord distinct, tel qu'un accord de concession de licence en volume de [!INCLUDE[msCoName](../../includes/msconame-md.md)] ou un accord tiers avec un éditeur de logiciels ou un fabricant OEM.  
  
 Les termes du contrat de licence sont affichés afin que vous puissiez les consulter et les accepter dans l'interface utilisateur du programme d'installation. Les installations sans assistance (avec les paramètres `/Q` ou `/QS`) doivent inclure le paramètre `/IAcceptSQLServerLicenseTerms`. Téléchargez et lisez les termes du contrat de licence séparément dans [Termes du contrat de licence Microsoft SQL Server et autres informations](http://www.microsoft.com/Licensing/product-licensing/sql-server.aspx). Pour les termes du contrat de licence en volume, consultez [Termes de licence et documentation](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=53). Pour les versions antérieures de SQL Server, consultez [Termes du contrat de licence logiciel Microsoft](http://go.microsoft.com/fwlink/?LinkID=148209).  
  
> [!NOTE]  
>  Selon la façon dont vous avez reçu le logiciel (par exemple, via le programme de licence en volume [!INCLUDE[msCoName](../../includes/msconame-md.md)] ), votre utilisation du logiciel peut être soumise à des termes et conditions supplémentaires.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Nouveautés liées à l’installation de SQL Server](../../sql-server/install/what-s-new-in-sql-server-installation.md)  
 Cet article décrit les détails des fonctionnalités nouvelles ou améliorées de l’installation de cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
 Cet article répertorie les configurations matérielle et logicielle minimales requises pour installer et exécuter une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 Cet article traite des recommandations de sécurité qu’il est conseillé de suivre avant d’installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et après avoir installé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
 Cet article décrit la configuration par défaut des services inclus dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ainsi que les options de configuration des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] susceptibles d’être définies durant l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et par la suite.  
  
 [Protocoles réseau et bibliothèques réseau](../../sql-server/install/network-protocols-and-network-libraries.md)  
 Cet article décrit la configuration par défaut des protocoles réseau dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et les options de configuration disponibles.  
  
 [Utiliser plusieurs versions et instances de SQL Server](../../sql-server/install/work-with-multiple-versions-and-instances-of-sql-server.md)  
 Cet article décrit les considérations pour l’installation de plusieurs versions et instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Versions linguistiques locales dans SQL Server](../../sql-server/install/local-language-versions-in-sql-server.md)  
 Cet article décrit les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="related-sections"></a>Sections connexes  
 [Installer SQL Server](../../database-engine/install-windows/install-sql-server.md)  
 Cette section fournit une vue d'ensemble des différentes options d'installation disponibles pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Installer les fonctionnalités Business Intelligence de SQL Server](../../sql-server/install/install-sql-server-business-intelligence-features.md)  
 Cette section de la documentation d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] explique comment installer les fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui font partie de la plateforme Microsoft BI.  
  
 [Mise à niveau vers SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 La section fournit une vue d'ensemble des instances de mise à niveau des versions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] précédentes vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 [Désinstaller SQL Server](../../sql-server/install/uninstall-sql-server.md)  
 Reportez-vous à cette section pour désinstaller complètement une instance existante de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] et préparer le système afin de pouvoir réinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Installation d'un cluster de basculement SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)  
 Cette section de la documentation d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] décrit comment installer et configurer un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a> Voir aussi  
 [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)   
 [Solutions haute disponibilité &#40;SQL Server&#41;](../../sql-server/failover-clusters/high-availability-solutions-sql-server.md)   
 [Avant l'installation du clustering de basculement](../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)   
 [Mettre à niveau SQL Server à l’aide de l’Assistant Installation &#40;Installation&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
