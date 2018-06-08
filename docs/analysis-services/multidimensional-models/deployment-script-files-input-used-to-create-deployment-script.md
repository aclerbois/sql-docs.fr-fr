---
title: Précisions sur les fichiers d’entrée utilisés pour créer le Script de déploiement | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6b75ec5d7433931a81a0fa6e2c648f85335fbedc
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deployment-script-files---input-used-to-create-deployment-script"></a>Fichiers de Script de déploiement : l’entrée utilisée pour créer le Script de déploiement
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  Lorsque vous générez un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] projet, [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] génère des fichiers pour le projet. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] place ces fichiers dans le dossier de sortie de la [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] projet. Par défaut, les sorties sont envoyées au dossier \Bin. Le tableau suivant répertorie les fichiers XML que crée [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] .  
  
|Fichier| Description|  
|---------------|-----------------|  
|\<*nom du projet*> .asdatabase|Un fichier XMLA pour multidimensionnel ou des projets de modèles tabulaires 1100/1103, ou un fichier JSON pour tabulaire 1200 et les projets de modèle plus élevées. Contient les définitions déclaratives de tous les objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] du projet.|  
|\<*nom du projet*> .deploymenttargets|Contient le nom de l'instance et de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans lesquelles les objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] seront créés.|  
|\<*nom du projet*> .configsettings|Contient des paramètres spécifiques à l'environnement, par exemple des informations sur la connexion à une source de données et les emplacements de stockage des objets. Les paramètres de ce fichier remplacent les paramètres dans le \< *nom du projet*> fichier .asdatabase.|  
|\<*nom du projet*> .deploymentoptions|Contient des options de déploiement qui déterminent, par exemple, si le déploiement est transactionnel et si les objets déployés doivent être traités après le déploiement.|  
  
> [!NOTE]  
>  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ne stocke jamais les mots de passe dans ses fichiers de projet.  
  
## <a name="modifying-the-input-files"></a>Modification des fichiers d'entrée  
 Modification des valeurs dans les fichiers d’entrée, ou les valeurs extraites à partir des fichiers d’entrée, permet de modifier la destination du déploiement, les paramètres de configuration et les options de déploiement sans modifier la totalité \< *nom du projet*> fichier .asdatabase (ou un fichier de script entier si vous générez un script à partir d’un fichier [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données). La possibilité de modifier des fichiers individuels vous permet de créer facilement des scripts de déploiement différents selon les buts poursuivis.  
  
 Les rubriques suivantes expliquent comment modifier des valeurs dans les divers fichiers d'entrée :  
  
-   [Spécification de la cible d'installation](../../analysis-services/multidimensional-models/deployment-script-files-specifying-the-installation-target.md)  
  
-   [Spécification des options de déploiement de partitions et de rôles](../../analysis-services/multidimensional-models/deployment-script-files-partition-and-role-deployment-options.md)  
  
-   [Spécification de paramètres de configuration pour le déploiement de solutions](../../analysis-services/multidimensional-models/deployment-script-files-solution-deployment-config-settings.md)  
  
-   [Spécification d'options de traitement](../../analysis-services/multidimensional-models/deployment-script-files-specifying-processing-options.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution de l'Assistant Déploiement d'Analysis Services](../../analysis-services/multidimensional-models/running-the-analysis-services-deployment-wizard.md)   
 [Comprendre le Script de déploiement d’Analysis Services](../../analysis-services/multidimensional-models/understanding-the-analysis-services-deployment-script.md)  
  
  
