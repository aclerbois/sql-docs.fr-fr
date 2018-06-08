---
title: Conteneur de séquences | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sequencecontainer.f1
helpviewer_keywords:
- Sequence container
- grouping control flows
- containers [Integration Services], Sequence
- subset control flow [Integration Services]
ms.assetid: 7731f91e-b8b3-4d96-a0d9-73f568547cb3
caps.latest.revision: 48
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 572ef34e4ffc661312cf6c640306ae263f4e62b0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sequence-container"></a>conteneur de séquences
  Le conteneur de séquences définit un flux de contrôle représentant un sous-ensemble du flux de contrôle du package. Les conteneurs de séquences regroupent le package en plusieurs flux de contrôle distincts contenant chacun un ou plusieurs conteneurs et tâches exécutés dans le flux de contrôle global du package.  
  
 Le conteneur de séquences peut inclure plusieurs tâches et d'autres conteneurs. L'ajout de tâches et de conteneurs à un conteneur de séquences est identique à l'ajout de ces éléments à un package. La seule différence est que vous faites glisser les tâches et les conteneurs dans le conteneur de séquences au lieu de les glisser dans le conteneur de package. Si le conteneur de séquences inclut plusieurs tâches ou conteneurs, vous pouvez les connecter à l'aide de contraintes de précédence de la même manière que dans un package. Pour plus d’informations, consultez [Contraintes de précédence](../../integration-services/control-flow/precedence-constraints.md).  
  
 L'utilisation d'un conteneur de séquences présente de nombreux avantages :  
  
-   Désactivation des groupes de tâches afin de concentrer le débogage du package sur un seul sous-ensemble de son flux de contrôle  
  
-   Centralisation de la gestion des propriétés de plusieurs tâches en définissant les propriétés sur un conteneur de séquences plutôt que sur les différentes tâches  
  
     Vous pouvez ainsi définir la propriété **Disable** du conteneur de séquences et lui affecter la valeur **True** pour désactiver toutes les tâches et tous les conteneurs du conteneur de séquences.  
  
-   Attribution d'une étendue aux variables utilisées par un groupe de tâches et de conteneurs apparentés  
  
-   Regroupement de nombreuses tâches pour vous permettre de les gérer plus facilement, en réduisant et en développant le conteneur de séquences.  
  
     Vous pouvez aussi créer des groupes de tâches, qui peuvent être réduits et développés à partir de la zone **Groupe** . Cependant, la zone **Groupe** est une fonctionnalité disponible au moment de la conception qui ne contient aucune propriété et n’affiche aucun comportement au moment de l’exécution. Pour plus d’informations, consultez [Grouper ou dissocier des composants](../../integration-services/group-or-ungroup-components.md)  
  
-   Configurez un attribut de transaction sur le conteneur de séquences afin de définir une transaction pour un sous-ensemble du flux de contrôle du package. Ainsi, vous pouvez gérer les transactions de façon plus précise.  
  
     Par exemple, si un conteneur de séquences comprend deux tâches apparentées, l'une supprimant des données d'une table et l'autre insérant celles-ci dans une table, vous pouvez configurer une transaction de manière à ce que l'action de suppression soit annulée en cas d'échec de l'action d'insertion. Pour plus d’informations, consultez [Transactions Integration Services](../../integration-services/integration-services-transactions.md).  
  
## <a name="configuration-of-the-sequence-container"></a>Configuration du conteneur de séquences  
 Le conteneur de séquences ne possède pas d’interface utilisateur personnalisée et vous ne pouvez le configurer que dans la fenêtre **Propriétés** de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou par programmation.  
  
 Pour plus d’informations sur la définition par programmation de ces propriétés, consultez la documentation relative à la classe **T:Microsoft.SqlServer.Dts.Runtime.Sequence** dans le Guide du développeur.  
  
## <a name="related-tasks"></a>Related Tasks  
 Pour plus d’informations sur la définition des propriétés du composant dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], consultez [Définir les propriétés d’une tâche ou d’un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b).  
  
## <a name="see-also"></a> Voir aussi  
 [Ajouter ou supprimer une tâche ou un conteneur dans un flux de contrôle](../../integration-services/control-flow/add-or-delete-a-task-or-a-container-in-a-control-flow.md)   
 [Connecter des tâches et des conteneurs à l’aide d’une contrainte de précédence par défaut](http://msdn.microsoft.com/library/8f31f15f-98ff-4c35-b41f-8b8cfd148d75)   
 [Conteneurs Integration Services](../../integration-services/control-flow/integration-services-containers.md)  
  
  
