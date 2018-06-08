---
title: Tâches d’installation pour Master Data Services | Microsoft Docs
ms.custom: ''
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: bb7aa3e7-8807-42c8-884f-0e41d7a20837
caps.latest.revision: 32
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 7a1c9515612719a749dedd5a796a57c5d25b4d82
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installation-tasks-for-master-data-services"></a>Tâches d’installation pour Master Data Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cet article fournit une vue d’ensemble des tâches d’installation, avec des liens vers des instructions. Pour connaître la procédure d’installation et de configuration de Master Data Services, consultez [Installation et configuration de Master Data Services](../../master-data-services/master-data-services-installation-and-configuration.md). 
  
-   [Tâches de pré-installation](#preinstall): permettent de vérifier la configuration requise avant d’installer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
-   [Opérations d'installation](#install): installez [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] à l'aide du programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d'une invite de commandes.  
  
-   [Tâches de post-installation](#postinstall): ouvrez le [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] pour terminer les opérations consécutives à l’installation. Créez et configurez la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , l'application Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] et les services Web, et déployez un exemple de modèle.  
  
##  <a name="preinstall"></a> Tâches de pré-installation  
  
|Action|Détails|Rubriques connexes|  
|------------|-------------|--------------------|  
|Vérifier la configuration requise pour l'installation|L'ordinateur sur lequel vous exécutez le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir la configuration minimale requise pour :<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> L'application Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] et les services Web.<br /><br /> La base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , si vous hébergez la base de données sur le même ordinateur que l'application Web.<br /><br /> <br /><br /> Vous pouvez séparer l'ordinateur serveur Web et l'ordinateur serveur de base de données en exécutant le programme d'installation uniquement sur l'ordinateur serveur Web et en créant la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] sur un ordinateur distant qui exécute une version et une édition prises en charge de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|[Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)<br /><br /> [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)<br /><br /> [Configuration requise pour l’application web &#40;Master Data Services&#41;](../../master-data-services/install-windows/web-application-requirements-master-data-services.md)<br /><br /> [Configuration requise pour la base de données &#40;Master Data Services&#41;](../../master-data-services/install-windows/database-requirements-master-data-services.md)|  
|Configurer les rôles, les services de rôle et les fonctionnalités requis|Avant d'exécuter le programme d'installation, configurez l'ordinateur avec les rôles, les services de rôle et les fonctionnalités Windows requis.<br /><br /> Remarque : bien que vous puissiez exécuter ultérieurement cette étape dans le flux de travail, il est utile de procéder à cette configuration avant d’exécuter le programme d’installation, afin de pouvoir effectuer les tâches de configuration web immédiatement après.|[Configuration requise pour l’application web &#40;Master Data Services&#41;](../../master-data-services/install-windows/web-application-requirements-master-data-services.md)|  
|Déterminer le prise en charge linguistique|Déterminez la langue que vous souhaitez installer et exécuter dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .|[Déploiements multilingues et globaux &#40;Master Data Services&#41;](../../master-data-services/install-windows/multi-lingual-and-global-deployments-master-data-services.md)|  
  
##  <a name="install"></a> Opérations d'installation  
  
|Action|Détails|Rubriques connexes|  
|------------|-------------|--------------------|  
|Exécuter le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Sur l'ordinateur qui hébergera l'application Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] et les services Web [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , utilisez le programme d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une invite de commandes pour installer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Lorsque vous utilisez le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] est disponible dans la page **Sélection de fonctionnalités** sous **Fonctionnalités partagées**. Lorsque vous utilisez une invite de commandes, [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] est disponible comme paramètre de fonctionnalité. Notez que le processus d'installation par ligne de commande installe [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)], mais ne le configure pas. Vous devez le configurer à l'aide du Gestionnaire de configuration Master Data Services.<br /><br /> Processus d'installation :<br /><br /> Installation des dossiers et des fichiers [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] à l'emplacement que vous spécifiez pour les fonctionnalités partagées et affectation des autorisations à ces objets.<br /><br /> Enregistrement des assemblys [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] dans le Global Assembly Cache (GAC).<br /><br /> Installe [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].|[Installer SQL Server 2016 avec l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)<br /><br /> [Autorisations d’accès aux dossiers et aux fichiers &#40;Master Data Services&#41;](../../master-data-services/folder-and-file-permissions-master-data-services.md)|  
  
##  <a name="postinstall"></a> Tâches de post-installation  
  
|Action|Détails|Rubriques connexes|  
|------------|-------------|--------------------|  
|Ouvrir [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] pour terminer les opérations consécutives à l'installation|Une fois l'installation terminée, ouvrez [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]. [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] effectue les opérations de post-installation suivantes sur l’ordinateur local :<br /><br /> Création d’un groupe Windows, **MDS_ServiceAccounts**, pour contenir les comptes de service [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] pour les pools d’applications.<br /><br /> Sous le chemin d’installation de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , création du dossier MDSTempDir et affectation des autorisations pour **MDS_ServiceAccounts**. Ce dossier est l’endroit où les fichiers de compilation temporaires sont compilés pour l’application web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .<br /><br /> Dans le fichier Web.config [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)], configuration de l’attribut **tempDirectory** de l’élément **\<compilation>** avec le chemin du dossier MDSTempDir.|[Autorisations d’accès aux dossiers et aux fichiers &#40;Master Data Services&#41;](../../master-data-services/folder-and-file-permissions-master-data-services.md)<br /><br /> [Référence de la configuration web &#40;Master Data Services&#41;](../../master-data-services/web-configuration-reference-master-data-services.md)|  
|Créer une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]|Utilisez [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] pour créer une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] pour vos données de référence.|[Créer une base de données Master Data Services](../../master-data-services/install-windows/create-a-master-data-services-database.md)|  
|Créer une application Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]|Utilisez [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] pour créer et configurer une application Web qui hébergera [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].|[Créer une application web Master Data Manager &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md)|  
|Associer une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] à une application Web|Utilisez [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] pour associer votre application Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] à votre base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .|[Associer une base de données Master Data Services et une application Web](../../master-data-services/install-windows/associate-a-master-data-services-database-and-web-application.md)|  
|Configurer la sécurité renforcée d'Internet Explorer|Lorsque vous installez [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] sur un ordinateur Windows Server 2012, vous devrez peut-être configurer la sécurité renforcée d’Internet Explorer pour autoriser le script du site de l’application [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . Sinon, la navigation sur le site de l'application [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] sur le serveur échoue.|[Internet Explorer : configuration de sécurité renforcée](http://go.microsoft.com/fwlink/p/?LinkId=223869)|  
|Installer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]|Les utilisateurs qui travaillent avec des données de référence peuvent installer [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)].|[http://go.microsoft.com/fwlink/?LinkID=398159](http://go.microsoft.com/fwlink/?LinkID=398159)|  
|Activer l'intégration de Data Quality Services (DQS)|Pour les utilisateurs du [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], activez l’intégration avec la fonction DQS, qui permet de faire correspondre des données similaires.|[Activer l'intégration de Data Quality Services avec Master Data Services](../../master-data-services/install-windows/enable-data-quality-services-integration-with-master-data-services.md)|  
|Déployer un exemple de modèle|Les packages d'exemples de modèles sont installés avec Master Data Services et peuvent être déployés à l'aide de l'outil MDSModelDeploy.|[Déploiement des exemples MDS dans SQL Server](~/master-data-services/sql-server-samples-model-deployment-packages-mds.md)|
  
 Si vous rencontrez des problèmes pendant la procédure d'installation ou la configuration initiale, consultez [Résolution des problèmes d'installation et de configuration](http://social.technet.microsoft.com/wiki/contents/articles/troubleshooting-installation-and-configuration-issues-master-data-services.aspx) sur TechNet Wiki.  
  
 Si vous n'avez plus besoin de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] sur un ordinateur, vous pouvez désinstaller [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] et déterminer s'il faut supprimer les éléments qui ne sont pas supprimés par le processus de désinstallation. Pour plus d’informations, consultez [Désinstaller et supprimer Master Data Services](../../sql-server/install/uninstall-and-remove-master-data-services.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Installer SQL Server 2016](../../database-engine/install-windows/install-sql-server.md)  
  
  
