---
title: Documentation Integration Services pour les développeurs | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- Integration Services, programming
- SSIS, programming
- SQL Server Integration Services, programming
- packages [Integration Services], programming
ms.assetid: 60fe148b-a7c4-4289-ae3e-2e949fc1886c
caps.latest.revision: 76
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 53c86f77f5280e7245f4c5a5ca6c5f694138ed10
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-developer-documentation"></a>Documentation Integration Services pour les développeurs
  [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] inclut un modèle objet entièrement réécrit, auquel ont été ajoutées de nombreuses fonctionnalités, afin d'offrir un outil convivial, souple et puissant pour étendre et programmer des packages. Les développeurs peuvent étendre et programmer pratiquement tous les aspects des packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
 En tant que développeur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], vous disposez de deux méthodes de programmation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] fondamentales :  
  
-   Vous pouvez étendre des packages en écrivant des composants qui deviennent disponibles dans le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] pour fournir des fonctionnalités personnalisées dans un package.  
  
-   Vous pouvez créer, configurer et exécuter des packages par programme à partir de vos propres applications.  
  
 Si vous constatez que les composants intégrés dans [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] ne satisfont pas vos besoins, vous pouvez étendre la puissance d'[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] en codant vos propres extensions. Cette méthode propose deux options distinctes :  
  
-   Pour une utilisation ad hoc dans un seul package, vous pouvez créer une tâche personnalisée en écrivant du code dans la tâche de script, ou un composant de flux de données personnalisé en écrivant du code dans le composant Script, que vous pouvez configurer en tant que source, transformation ou destination. Ces wrappers puissants se chargent d'écrire le code d'infrastructure et vous permettent ainsi de vous consacrer exclusivement au développement de vos fonctionnalités personnalisées ; toutefois, ils sont difficilement réutilisables dans un contexte différent.  
  
-   Pour une utilisation dans plusieurs packages, vous pouvez créer des extensions [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] personnalisées, telles que des gestionnaires de connexions, des tâches, des énumérateurs, des modules fournisseurs d'informations et des composants de flux. Le modèle objet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] managé contient des classes de base qui fournissent un point de départ et facilitent considérablement le développement d'extensions personnalisées.  
  
 Si vous souhaitez créer des packages de manière dynamique, ou gérer et exécuter des packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] à l'extérieur de l'environnement de développement, vous pouvez manipuler les packages par programme. Vous pouvez charger, modifier et exécuter des packages existants, ou créer et exécuter par programme des packages entièrement nouveaux. Cette méthode vous offre une gamme continue d'options :  
  
-   Charger et exécuter un package existant sans modification.  
  
-   Charger un package existant, le reconfigurer (par exemple, spécifier une source de données différente) et l'exécuter.  
  
-   Créer un package, ajouter et configurer des composants, apporter des modifications objet par objet et propriété par propriété, l’enregistrer, puis l’exécuter.  
  
 Ces méthodes de programmation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sont décrites dans cette section et illustrées par des exemples.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Vue d’ensemble de la programmation Integration Services](../integration-services/integration-services-programming-overview.md)  
 Décrit les rôles de flux de contrôle et de flux dans le développement [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
 [Présentation des transformations synchrones et asynchrones](../integration-services/understanding-synchronous-and-asynchronous-transformations.md)  
 Décrit la distinction importante entre les sorties synchrones et les sorties asynchrones, ainsi que les composants qui les utilisent dans le flux de données.  
  
 [Utilisation de gestionnaires de connexions par programmation](../integration-services/working-with-connection-managers-programmatically.md)  
 Répertorie les gestionnaires de connexions que vous pouvez utiliser à partir du code managé, ainsi que les valeurs que les gestionnaires de connexions retournent quand le code appelle la méthode **AcquireConnection**.  
  
 [Extension de packages avec des scripts](../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
 Décrit comment étendre le flux de contrôle à l’aide de la tâche de script, ou le flux de données à l’aide du composant Script.  
  
 [Extension de packages avec des objets personnalisés](../integration-services/extending-packages-custom-objects/extending-packages-with-custom-objects.md)  
 Décrit comment créer et programmer des tâches personnalisées, des composants de flux de données et d'autres objets de package à utiliser dans plusieurs packages.  
  
 [Génération de packages par programmation](../integration-services/building-packages-programmatically/building-packages-programmatically.md)  
 Décrit comment créer, configurer et enregistrer des packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] par programme.  
  
 [Exécution et gestion de packages par programmation](../integration-services/run-manage-packages-programmatically/running-and-managing-packages-programmatically.md)  
 Décrit comment énumérer, exécuter et gérer des packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] par programme.  
  
## <a name="reference"></a>Référence  
 [Guide de référence des erreurs et des messages propres à Integration Services](../integration-services/integration-services-error-and-message-reference.md)  
 Répertorie les codes d'erreur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] prédéfinis, avec leur nom symbolique et leur description.  
  
## <a name="related-sections"></a>Sections connexes  
 [Outils de dépannage pour le développement des packages](../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
 Décrit les fonctionnalités et les outils fournis par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour le dépannage des packages lors du développement.  
  
## <a name="external-resources"></a>Ressources externes  
  
-   Exemples de code CodePlex, [Integration Services Product Samples](http://go.microsoft.com/fwlink/?LinkID=131204), sur www.codeplex.com/MSFTISProdSamples  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server Integration Services](../integration-services/sql-server-integration-services.md)  
  
  
