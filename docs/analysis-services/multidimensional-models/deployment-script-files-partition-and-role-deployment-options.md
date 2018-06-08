---
title: Spécification des partitions et des Options de déploiement de rôle | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 64a8eeff0447b6a62e7e1f8e21bf48ae3a6b99a3
ms.sourcegitcommit: b3bb41424249de198f22d9c6d40df4996f083aa6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="deployment-script-files---partition-and-role-deployment-options"></a>Fichiers de Script de déploiement - Partition et les Options de déploiement de rôle
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  Le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Assistant Déploiement de lit les options de déploiement de rôles et de partitions à partir de la \< *nom du projet*> .deploymentoptions fichier. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]crée ce fichier lorsque vous générez le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] projet. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] utilise les options de déploiement de rôles et de partitions actuelles projet lorsque le \< *nom du projet*> .deploymentoptions est créé. Pour plus d'informations sur les paramètres de configuration, consultez [Précisions sur les fichiers d'entrée utilisés pour créer le script de déploiement](../../analysis-services/multidimensional-models/deployment-script-files-input-used-to-create-deployment-script.md).  
  
## <a name="reviewing-the-partition-and-role-deployment-options"></a>Examen des options de déploiement de partitions et de rôles  
 Les options de déploiement dans le \< *nom du projet*> .deploymentoptions sont les suivantes :  
  
 **Options de déploiement de partitions**  
 Le \< *nom du projet*> .deploymentoptions Spécifie si les partitions existantes de la base de données de destination sont conservées ou remplacée (option par défaut). Si les partitions existantes sont conservées, seules les nouvelles partitions sont déployées, et les partitions et les conceptions d'agrégation sur tous les groupes de mesures restent inchangées.  
  
> [!NOTE]  
>  Si le groupe de mesures dans lequel existe la partition est supprimé, la partition est supprimée automatiquement.  
  
 **Options de déploiement de rôles**  
 Le \< *nom du projet*> .deploymentoptions spécifie une des options de déploiement de rôle suivantes :  
  
-   Les rôles et les membres de rôle existant dans la base de données de destination sont conservés, et seuls les nouveaux rôles et membres de rôle sont déployés.  
  
-   Tous les rôles et les membres de rôle existant dans la base de données de destination sont remplacés par les rôles et les membres de rôle déployés.  
  
-   Les rôles et les membres de rôle existant dans la base de données de destination sont conservés, et aucun nouveau rôle n'est déployé.  
  
-   **Remarque** Lorsque des membres et des rôles existants sont conservés, les autorisations associées à ces rôles sont réinitialisées à Aucune autorisation. Les autorisations de sécurité sont contenues dans les objets qu'elles sécurisent, et non dans les rôles de sécurité auxquels elles sont associées. Pour plus d'informations sur la manière de gérer ce comportement à l'aide de l'Assistant Déploiement de Analysis Services, consultez l'article relatif à la conservation de rôles et de membres dans la Base de connaissances Microsoft.  
  
## <a name="modifying-the-partition-and-role-deployment-options"></a>Modification des options de déploiement de partitions et de rôles  
 Il se peut que vous deviez déployer les [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de projet à l’aide des options de partition et le rôle différentes de celles stockées dans le \< *nom du projet*> .deploymentoptions fichier. Par exemple, vous pouvez choisir de conserver des partitions existantes, les rôles et les membres du rôle, au lieu de remplacer toutes les partitions existantes, les rôles et les membres comme indiqué dans le \< *nom du projet*> .deploymentoptions fichier.  
  
 Pour modifier le déploiement des partitions et des rôles dans un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] projet, vous ne pouvez pas modifier les paramètres de partitions et de rôles au sein du projet, car le  *\<nom du projet >* **Pages de propriétés** boîte de dialogue de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] n’affiche pas ces options. Si vous souhaitez modifier les options de déploiement de rôles et les partitions, vous devez modifier ces informations dans le \< *nom du projet*> fichier .deploymentoptions lui-même. La procédure suivante décrit comment modifier les options de déploiement de rôles et de partitions dans le \< *nom du projet*> .deploymentoptions fichier.  
  
#### <a name="to-change-the-deployment-of-partitions-or-roles-after-the-input-files-have-been-generated"></a>Pour modifier le déploiement de partitions ou de rôles après la génération des fichiers d'entrée  
  
-   Exécutez l'Assistant Déploiement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode interactif et, sur la page **Spécifiez des options pour les partitions et les rôles** , spécifiez de nouvelles options de déploiement pour les partitions et les rôles.  
  
     —ou—  
  
-   Exécutez l'Assistant Déploiement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'invite de commandes en mode fichier de réponses. (Pour plus d’informations sur le mode fichier de réponses, consultez [Exécution de l’Assistant Déploiement d’Analysis Services](../../analysis-services/multidimensional-models/running-the-analysis-services-deployment-wizard.md).)  
  
     —ou—  
  
-   Ouvrez le \< *nom du projet*> .deploymentoptions dans un éditeur de texte et manuellement modifier les options. Les options de PartitionDeployment sont DeployPartitions, RetainPartitions. Les options de RoleDeployment sont DeployRolesAndMembers, DeployRolesRetainMembers, RetainRoles.
  
## <a name="see-also"></a>Voir aussi  
 [Spécification de la cible d’Installation](../../analysis-services/multidimensional-models/deployment-script-files-specifying-the-installation-target.md)   
 [Spécification des paramètres de Configuration pour le déploiement de solutions](../../analysis-services/multidimensional-models/deployment-script-files-solution-deployment-config-settings.md)   
 [Spécification d’Options de traitement](../../analysis-services/multidimensional-models/deployment-script-files-specifying-processing-options.md)  
  
  
