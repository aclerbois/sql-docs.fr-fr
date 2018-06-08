---
title: Tâches Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- scripts [Integration Services], tasks
- files [Integration Services], task options
- workflow tasks [Integration Services]
- Integration Services, tasks
- adding package tasks
- tasks [Integration Services], listed
- SSIS tasks
- SSIS, tasks
- control flow [Integration Services], tasks
- tasks [Integration Services]
- grouping tasks
- tasks [Integration Services], about tasks
- SQL Server Integration Services tasks
- data preparation tasks [Integration Services]
- directory operations [Integration Services]
ms.assetid: 75c8901d-6966-4af3-abe5-10af6dd9313b
caps.latest.revision: 51
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a0d4a80a0dbbeb521777654483f5dc4cfdf9242a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-tasks"></a>Tâches Integration Services
  Les tâches sont des éléments de flux de contrôle qui définissent des unités de travail qui sont exécutées dans un flux de contrôle de package. Un package [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est composé d’une ou plusieurs tâches. Si le package contient plusieurs tâches, elles sont connectées et organisées dans le flux de contrôle par des contraintes de priorité.  
  
 Vous pouvez également écrire des tâches personnalisées à l'aide d'un langage de programmation qui prend en charge COM, tel que Visual Basic, ou d'un langage de programmation .NET, tel que C#.  
  
 Le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , outil graphique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] permettant de manipuler les packages, met à votre disposition une aire de conception pour la création des flux de contrôle de package, ainsi que des éditeurs personnalisés pour la configuration des tâches. Vous pouvez aussi programmer le modèle objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour créer des packages par programmation.  
  
## <a name="types-of-tasks"></a>Types de tâches  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend les types de tâches suivants.  
  
 Tâche de flux de données  
 Tâche qui exécute les flux de données pour extraire les données, pour appliquer les transformations au niveau des colonnes et pour charger des données.  
  
 Tâches de préparation des données  
 Ces tâches effectuent les processus suivants : copie de fichiers et de répertoires, téléchargement de fichiers et de données, exécution de méthodes Web, application d'opérations aux documents XML et profilage des données à des fins de nettoyage.  
  
 Tâches de flux de travail  
 Tâches qui communiquent avec d'autres processus pour exécuter les packages, exécuter les programmes ou les fichiers de commandes, gérer les échanges de messages entre les packages, envoyer des messages électroniques, lire les données WMI (Windows Management Instrumentation) et observer les événements WMI.  
  
 Tâches SQL Server  
 Tâches qui accèdent aux objets et aux données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et qui les copient, les insèrent, les suppriment et les modifient.  
  
 Tâches de script  
 Tâches qui étendent les fonctionnalités des packages à l'aide de scripts.  
  
 Tâches Analysis Services  
 Tâches qui créent, modifient, suppriment et traitent les objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 Tâches de maintenance  
 Tâches qui réalisent des fonctions d'administration telles que la sauvegarde et la réduction des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la reconstruction et la réorganisation des index ou l'exécution des travaux d'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Tâches personnalisées  
 Vous pouvez aussi écrire des tâches personnalisées à l'aide d'un langage de programmation qui prend en charge COM, tel que Visual Basic, ou d'un langage de programmation .NET, tel que C#. Si vous voulez accéder à votre tâche personnalisée dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vous pouvez créer et inscrire une interface utilisateur pour la tâche. Pour plus d’informations, consultez [Développement d’une tâche personnalisée](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="configuration-of-tasks"></a>Configuration des tâches  
 Un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peut contenir une seule tâche, par exemple une tâche d'exécution SQL qui supprime des enregistrements dans une table de base de données lors de l'exécution du package. Les packages contiennent cependant en général plusieurs tâches et chacune d'elles est définie de manière à être exécutée dans le contexte du flux de contrôle du package. Les gestionnaires d'événements, qui sont des flux de travail exécutés en réponse aux événements d'exécution, peuvent également avoir des tâches.  
  
 Pour plus d’informations sur l’ajout d’une tâche à un package à l’aide du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Ajouter ou supprimer une tâche ou un conteneur dans un flux de contrôle](../../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
 Pour plus d’informations sur l’ajout d'une tâche à un package par programmation, consultez [Ajout de tâches par programme](../../integration-services/building-packages-programmatically/adding-tasks-programmatically.md).  
  
 Chaque tâche peut être configurée séparément à l’aide des boîtes de dialogue personnalisées du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou de la fenêtre Propriétés de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Un package peut contenir plusieurs tâches du même type — par exemple, six tâches d'exécution SQL — et chaque tâche peut être configurée différemment. Pour plus d’informations, consultez [Définir les propriétés d’une tâche ou d’un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b).  
  
## <a name="tasks-connections-and-groups"></a>Connexions et groupes des tâches  
 Si la tâche contient plusieurs tâches, elles sont connectées et organisées dans le flux de contrôle par des contraintes de priorité. Pour plus d’informations, consultez [Contraintes de précédence](../../integration-services/control-flow/precedence-constraints.md).  
  
 Les tâches peuvent être regroupées et exécutées en tant qu'unité de travail unique ou répétées dans une boucle. Pour plus d’informations, consultez [Conteneur de boucles Foreach](../../integration-services/control-flow/foreach-loop-container.md), [Conteneur de boucles For](../../integration-services/control-flow/for-loop-container.md)et [Conteneur de séquences](../../integration-services/control-flow/sequence-container.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 [Ajouter ou supprimer une tâche ou un conteneur dans un flux de contrôle](../../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
  
