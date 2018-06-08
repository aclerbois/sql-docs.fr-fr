---
title: Installation de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.portal.Installation.f1
helpviewer_keywords:
- installing SQL Server, initial installation
- installation [SQL Server]
- initial installation [SQL Server]
ms.assetid: edd75f68-dc62-4479-a596-57ce8ad632e5
caps.latest.revision: 34
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 781bebda84fe0d4e48414fb327e45fe837ed9356
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771535"
---
# <a name="sql-server-installation"></a>Installation de SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L'Assistant Installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une arborescence de fonctionnalités unique pour installer tous les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)]  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  
-   [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]  
-   [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)]  
-   Composants de connectivité  
  
À compter de [!INCLUDE[ss2016](../../includes/sssql15-md.md)], les outils d’administration SQL Server ne sont plus installés à partir de l’arborescence de fonctionnalités principales ; pour plus d’informations, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)  
  
Vous pouvez installer chaque composant individuellement ou sélectionner une combinaison de composants ci-dessus. Pour choisir au mieux les éditions et les composants disponibles dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les fonctionnalités prises en charge par votre version de SQL Server :

- [Éditions et fonctionnalités prises en charge de [!INCLUDE[ss2017](../../includes/sssqlv14-md.md)]](~/sql-server/editions-and-components-of-sql-server-2017.md).  
- [Éditions et fonctionnalités prises en charge de [!INCLUDE[ss2016](../../includes/sssql15-md.md)]](~/sql-server/editions-and-components-of-sql-server-2016.md).  
- [Fonctionnalités prises en charge par les éditions de [!INCLUDE[ss2014](../../includes/sssql14-md.md)]](http://technet.microsoft.com/library/cc645993(v=sql.120).aspx)
  
## <a name="in-this-section"></a>Dans cette section  
Que vous utilisiez l'Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l'invite de commandes pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le processus d'installation implique les étapes suivantes :  
  
[Planification d'une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)  
Décrit comment préparer l'ordinateur pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   Configuration matérielle et logicielle requise.  
-   Vérifier les spécifications de l'Outil d'analyse de configuration système et les problèmes bloquants.  
-   Considérations sur la sécurité.  
  
[Installer SQL Server](../../database-engine/install-windows/install-sql-server.md)  
 Décrit les options d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
[Guide de référence de l'interface utilisateur du programme d'installation de SQL Server](http://msdn.microsoft.com/library/183b5cdd-962e-41ca-8064-ea44f622c77d)  
 Décrit les options d’installation proposées par l’Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
[Mise à niveau vers SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 Décrit les options pour effectuer une mise à niveau vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
[Désinstaller SQL Server](../../sql-server/install/uninstall-sql-server.md)  
 Décrit les procédures pour désinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
[Installation d'un cluster de basculement SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)  
 Cette section de la documentation d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] décrit comment installer et configurer un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
[Installer les fonctionnalités Business Intelligence de SQL Server](../../sql-server/install/install-sql-server-business-intelligence-features.md)  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les fonctions qui font partie de la plateforme BI de Microsoft sont notamment [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], ainsi que plusieurs applications clientes servant à créer ou utiliser des données analytiques. Cette section de la documentation de l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] explique comment installer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
## <a name="more-information"></a>Informations supplémentaires
[Installer des fonctionnalités SQL Server BI avec SharePoint &#40;PowerPivot et Reporting Services&#41;](http://msdn.microsoft.com/library/3166107c-30c2-468e-bb1b-bb42b79b37c3)  
 Cette section explique comment installer les fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un environnement SharePoint. Elle identifie les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles en fonction d'une version et d'une édition spécifiques de SharePoint. Elle inclut aussi des procédures d’installation de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint et Reporting Services en mode SharePoint.  
  
![ssrs_fyi_note](../../analysis-services/instances/install-windows/media/ssrs-fyi-note.png) Installez la nouvelle base de données exemple, [Wide World Importers](https://msdn.microsoft.com/library/mt734199(v=sql.1).aspx). 
  
[Exemples SQL Server et exemples de bases de données supplémentaires](http://sqlserversamples.codeplex.com/)  
 Explique comment installer et configurer les exemples et les exemples de bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Consultez le [Centre de mise à jour SQL Server](https://msdn.microsoft.com/library/ff803383.aspx) pour obtenir des liens et des informations concernant toutes les versions de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]prises en charge.  
  
## <a name="see-also"></a> Voir aussi  
[Nouveautés liées à l'installation de SQL Server](../../sql-server/install/what-s-new-in-sql-server-installation.md)   
[Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  
