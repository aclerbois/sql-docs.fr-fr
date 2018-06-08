---
title: Utiliser des projets et des bases de données dans le développement d’Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 018f937ed07c202aad17d5e7758f41a3082c4870
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="work-with-analysis-services-projects-and-databases-in-development"></a>Utiliser des projets et des bases de données dans le développement d’Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez développer une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode projet ou en mode en ligne.  
  
## <a name="single-developer"></a>Développeur unique  
 Si un seul développeur est chargé du développement de l'intégralité de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et des objets qui la constituent, celui-ci peut utiliser [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en mode projet ou en mode en ligne à tout moment pendant le cycle de vie de la solution Business Intelligence. Dans ce cas, le choix du mode n'est pas vraiment essentiel. La maintenance d'un fichier de projet hors connexion intégré à un système de contrôle de la source présente de nombreux avantages, tels que l'archivage et la restauration. Cependant, avec un seul et unique développeur vous n'aurez aucun problème lié à la communication des modifications entre les développeurs.  
  
## <a name="multiple-developers"></a>Plusieurs développeurs  
 Si plusieurs développeurs collaborent sur une solution Business Intelligence, des problèmes surviendront s'ils ne travaillent pas en mode projet avec contrôle de la source dans la plupart des cas, si ce n'est tous. Le contrôle de la source permet à deux développeurs de ne pas modifier simultanément le même objet.  
  
 Par exemple, supposons qu'un développeur travaille en mode projet et apporte des modifications à des objets sélectionnés. Pendant qu'il effectue ces modifications, supposons qu'un autre développeur modifie la base de données déployée en mode en ligne. Un problème surviendra lorsque le premier développeur tentera de déployer son projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] modifié. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] détectera que des objets ont été modifiés dans la base de données déployée et invitera le développeur à remplacer l'intégralité de la base de données. Cela entraînera le remplacement des modifications apportées par le deuxième développeur. Étant donné que [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ne dispose d'aucun moyen de résoudre les modifications entre l'instance de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et les objets contenus dans le projet qui seront remplacés, la seule solution pour le premier développeur consiste à ignorer toutes ses modifications et à créer un nouveau projet sur la base de la version actuelle de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
  
