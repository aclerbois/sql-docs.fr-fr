---
title: Utiliser des projets et des bases de données de Production Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f90af41c397da20fb26c73bebc6723b9a3cf6270
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="work-with-analysis-services-projects-and-databases-in-production"></a>Utiliser des projets et des bases de données de Production Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Après avoir développé et déployé une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans une instance d'[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous devez décider de la façon dont vous souhaitez modifier les objets dans la base de données déployée. Certaines modifications, telles que celles associées aux rôles de sécurité, au partitionnement et aux paramètres de stockage, peuvent être effectuées à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. D’autres modifications peuvent être effectuées uniquement en utilisant [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], en mode projet ou en mode en ligne (comme ajouter des attributs ou des hiérarchies définies par l’utilisateur).  
  
 Dès que vous apportez une modification à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] déployée à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode en ligne, le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui a été utilisé pour le déploiement devient obsolète. Si un développeur modifie le projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et déploie le projet modifié, un message l'invitera à remplacer la totalité de la base de données. S'il remplace la base de données, cette dernière doit également être traitée. Le problème devient complexe si les modifications effectuées directement dans la base de données déployée par l’équipe de production n’ont pas été communiquées à l’équipe de développement, car cette dernière ne comprendra pas pourquoi ses modifications n’apparaissent plus dans la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 Vous disposez de plusieurs méthodes vous permettant d'utiliser les outils SQL Server [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour éviter les problèmes inhérents à cette situation.  
  
-   Méthode 1 : si une modification est apportée à une version de production d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , utilisez [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour créer un nouveau projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] basé sur la version modifiée de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Ce nouveau projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut être archivé dans le système de contrôle du code source comme copie principale du projet. Cette méthode fonctionne indépendamment du fait que les modifications ont été effectuées dans la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode en ligne.  
  
-   Méthode 2 : modifiez seulement la version de production d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode projet. Cette méthode vous permet d’utiliser les options disponibles dans l’Assistant Déploiement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour conserver les modifications effectuées par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], notamment les rôles de sécurité et les paramètres de stockage. Les paramètres liés à la conception sont donc conservés dans le fichier de projet (les paramètres de stockage et les rôles de sécurité peuvent être ignorés) et le serveur en ligne est utilisé pour les paramètres de stockage et les rôles de sécurité.  
  
-   Méthode 3 : modifiez seulement la version de production d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode en ligne. Étant donné que les deux outils fonctionnent sur le même serveur en ligne, la version sera synchronisée.  
  
  
