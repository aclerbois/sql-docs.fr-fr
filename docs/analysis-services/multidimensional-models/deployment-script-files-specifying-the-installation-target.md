---
title: Spécification de la cible d’Installation | Documents Microsoft
ms.custom: ''
ms.date: 03/27/2018
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.service: ''
ms.component: data-mining - "setup-install"
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- input files [Analysis Services]
- installation targets [Analysis Services]
- Analysis Services deployments, installation targets
- Analysis Services Deployment Wizard, installation targets
- deploying [Analysis Services], installation targets
- modifying installation targets
ms.assetid: cb706817-6f63-4771-92c3-b70030bbce3d
caps.latest.revision: ''
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: Inactive
ms.openlocfilehash: c9977badf4e1e8e05cb35ed8828d4cc4ae12cfae
ms.sourcegitcommit: d6881107b51e1afe09c2d8b88b98d075589377de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deployment-script-files---specifying-the-installation-target"></a>Fichiers de Script de déploiement - spécification de la cible d’Installation
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  Le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Assistant Déploiement de lit les informations de cible d’installation à partir de la \< *nom du projet*> .deploymenttargets fichier. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]crée ce fichier lorsque vous générez le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] projet. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] utilise la base de données et le serveur spécifié sur le **déploiement** page de la  *\<nom du projet >* **Pages de propriétés** boîte de dialogue pour créer le \< *nom du projet*> fichier .targets.  
  
## <a name="modifying-the-installation-target-for-deployment"></a>Modification de la cible d'installation pour le déploiement  
 Dans certaines situations, il peut s'avérer nécessaire de déployer un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vers une base de données ou une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] différente de celle spécifiée sur la page **Déploiement** . Par exemple, vous pouvez choisir de déployer le projet vers un serveur où il sera testé avant son déploiement, puis de le déployer vers un serveur de production une fois les tests terminés. Vous pouvez aussi décider de déployer un projet terminé et testé vers plusieurs serveurs de production dans un cluster d'équilibrage de la charge réseau, ou encore vers un serveur de test et un serveur de production.  
  
 Pour déployer un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vers une base de données ou une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] différente, vous pouvez modifier la cible d'installation dans le fichier d'entrée en employant l'une des méthodes décrites dans la procédure suivante.  
  
#### <a name="to-change-the-installation-target-after-the-input-files-have-been-generated"></a>Pour modifier la cible d'installation après la génération des fichiers d'entrée  
  
-   Exécutez l'Assistant Déploiement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode interactif. Sur la page **Cible d'installation** , spécifiez une nouvelle destination pour l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et la base de données.  
  
     —ou—  
  
-   Exécutez l'Assistant Déploiement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'invite de commandes en mode fichier de réponses. Pour plus d'informations sur le mode fichier de réponses, consultez [Running the Analysis Services Deployment Wizard](../../analysis-services/multidimensional-models/running-the-analysis-services-deployment-wizard.md).  
  
     —ou—  
  
-   Modifier la \< *nom du projet*> fichier .deploymenttargets à l’aide de n’importe quel éditeur de texte.  
  
## <a name="see-also"></a>Voir aussi  
 [Spécification des partitions et des Options de déploiement de rôles](../../analysis-services/multidimensional-models/deployment-script-files-partition-and-role-deployment-options.md)   
 [Spécification des paramètres de Configuration pour le déploiement de solutions](../../analysis-services/multidimensional-models/deployment-script-files-solution-deployment-config-settings.md)   
 [Spécification d’Options de traitement](../../analysis-services/multidimensional-models/deployment-script-files-specifying-processing-options.md)  
  
  
