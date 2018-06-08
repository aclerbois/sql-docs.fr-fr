---
title: Extension de packages avec des scripts | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- SQL Server Integration Services, scripting
- SSIS, scripting
- scripts [Integration Services], about scripting
ms.assetid: 67fe18ef-f3aa-41d4-9b9d-5defd4618c4b
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 913c73dda06bda9a00e2eceebcdf74c70d88901f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="extending-packages-with-scripting"></a>Extension de packages avec des scripts
  Si vous constatez que les composants intégrés [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ne satisfont pas vos besoins, vous pouvez étendre la puissance d'[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en codant vos propres extensions. Vous disposez de deux options distinctes pour étendre vos packages : vous pouvez écrire du code dans les puissants wrappers fournis par la tâche de script et le composant Script, ou vous pouvez entièrement créer des extensions [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] personnalisées, dérivées des classes de base fournies par le modèle objet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Cette section explore le plus simple des deux options : étendre des packages à l'aide de scripts.  
  
 La tâche de script et le composant Script vous permettent d'étendre le flux de contrôle et le flux de données d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec un minimum de code. Les deux objets utilisent l’environnement de développement [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) et les langages de programmation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#. Par ailleurs, ils bénéficient de toutes les fonctionnalités offertes par la bibliothèque de classes [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], ainsi que des assemblys personnalisés. La tâche de script et le composant Script permettent aux développeurs de créer des fonctionnalités personnalisées sans devoir écrire tout le code d'infrastructure généralement requis lors du développement d'une tâche personnalisée ou d'un composant de flux de données personnalisé.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Comparaison de la tâche de script et du composant Script](../../integration-services/extending-packages-scripting/comparing-the-script-task-and-the-script-component.md)  
 Examine les ressemblances et les différences entre la tâche de script et le composant Script.  
  
 [Comparaison des solutions de script et des objets personnalisés](../../integration-services/extending-packages-scripting/comparing-scripting-solutions-and-custom-objects.md)  
 Examine les critères à utiliser pour choisir entre une solution de script et le développement d'un objet personnalisé.  
  
 [Référencement d’autres assemblys dans les solutions de script](../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md)  
 Examine les étapes requises pour référencer et utiliser des assemblys et des espaces de noms externes dans un projet de script.  
  
 [Extension du package à l’aide de la tâche de script](../../integration-services/extending-packages-scripting/task/extending-the-package-with-the-script-task.md)  
 Explique comment créer des tâches personnalisées à l'aide de la tâche de script. Une tâche est généralement appelée une fois par package exécuté, ou une fois pour chaque source de données ouverte par un package.  
  
 [Extension du flux de données avec le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md)  
 Explique comment créer des sources, transformations et destinations de flux de données personnalisées à l'aide du composant Script. Un composant de flux de données est généralement appelé une fois pour chaque ligne de données traitée.  
  
## <a name="reference"></a>Référence  
 [Guide de référence des erreurs et des messages propres à Integration Services](../../integration-services/integration-services-error-and-message-reference.md)  
 Répertorie les codes d'erreur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prédéfinis avec leur nom symbolique et leur description.  
  
## <a name="related-sections"></a>Sections connexes  
 [Extension de packages avec des objets personnalisés](../../integration-services/extending-packages-custom-objects/extending-packages-with-custom-objects.md)  
 Explique comment créer des tâches personnalisées de programme, des composants de flux de données et d'autres objets de package à utiliser dans plusieurs packages.  
  
 [Génération de packages par programmation](../../integration-services/building-packages-programmatically/building-packages-programmatically.md)  
 Décrit comment créer, configurer, exécuter, charger, enregistrer et gérer des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] par programme.  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)  
  
  
