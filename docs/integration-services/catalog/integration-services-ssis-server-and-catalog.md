---
title: Serveur et catalogue Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: service
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- packages [Integration Services], managing
- managing packages [Integration Services]
ms.assetid: 6d667bba-7c25-492a-8f4d-70ebaca28f40
caps.latest.revision: 38
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fa419944aa7a088868a77666f93514fe9c02702d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-server-and-catalog"></a>Serveur et catalogue Integration Services (SSIS)
  Après avoir conçu et testé des packages dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], vous pouvez déployer les projets qui contiennent les packages sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données **SSISDB** . La base de données stocke les objets suivants : packages, projets, paramètres, autorisations, propriétés du serveur et historique opérationnel.  
  
 La base de données **SSISDB** expose les informations d'objet dans des vues publiques que vous pouvez interroger. La base de données fournit également des procédures stockées que vous pouvez appeler pour gérer les objets.  
  
 Avant de pouvoir déployer des projets sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous devez créer le catalogue **SSISDB** .  
  
 Vous trouverez une vue d’ensemble des fonctionnalités du catalogue SSISDB sur la page [Catalogue SSIS](../../integration-services/catalog/ssis-catalog.md).  
  
## <a name="high-availability"></a>Haute disponibilité  
 Tout comme les autres bases de données utilisateur, la base de données **SSISDB** prend en charge la mise en miroir et la réplication de bases de données. Pour plus d’informations sur la mise en miroir et la réplication, consultez la page [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
 Vous pouvez également assurer la haute disponibilité de la base de données SSISDB et de son contenu en utilisant SSIS et les groupes de disponibilité Always On. Pour plus d’informations, consultez [Always On pour le catalogue SSIS (SSISDB](ssis-catalog.md#always-on-for-ssis-catalog-ssisdb). Consultez également ce billet de blog de Matt Masson, [SSIS with Always On](http://go.microsoft.com/fwlink/?LinkId=255873), sur blogs.msdn.com.  
  
##  <a name="ssms"></a> Serveur Integration Services dans SQL Server Management Studio  
 Lorsque vous vous connectez à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui héberge la base de données **SSISDB** , vous affichez les objets suivants dans l'Explorateur d'objets :  
  
-   **Base de données SSISDB**  
  
     La base de données **SSISDB** apparaît sous le nœud **Bases de données** dans l'Explorateur d'objets. Vous pouvez interroger les vues et appeler les procédures stockées qui gèrent le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et les objets stockés sur le serveur.  
  
-   **Catalogues Integration Services**  
  
     Sous le nœud **Catalogues Integration Services** se trouvent des dossiers pour des environnements et des projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Afficher la liste des packages sur le serveur Integration Services](../../integration-services/catalog/view-the-list-of-packages-on-the-integration-services-server.md)  
  
-   [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)  
  
-   [Exécuter des packages Integration Services (SSIS)](../../integration-services/packages/run-integration-services-ssis-packages.md)  
  
## <a name="related-content"></a>Contenu associé  
 Entrée de blog, [SSIS et Always On](http://go.microsoft.com/fwlink/?LinkId=255873), sur blogs.msdn.com.  
  
  
