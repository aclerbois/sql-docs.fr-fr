---
title: Ajouter ou supprimer une tâche ou un conteneur dans un flux de contrôle | Microsoft Docs
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
helpviewer_keywords:
- containers [Integration Services], adding
- adding tasks
- adding containers
- tasks [Integration Services], adding
ms.assetid: 653084c6-87a3-45d5-b458-914ecf24d56a
caps.latest.revision: 46
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8209b848bb2434fd157c83f1e5aaa32783fb8402
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-or-delete-a-task-or-a-container-in-a-control-flow"></a>Ajouter ou supprimer une tâche ou un conteneur dans un flux de contrôle
  Quand vous travaillez dans le concepteur de flux de contrôle, la boîte à outils du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] énumère les tâches proposées par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour créer le flux de contrôle d’un package. Pour plus d’informations sur la boîte à outils, consultez [Boîte à outils SSIS](../../integration-services/ssis-toolbox.md).  
  
 Un package peut inclure plusieurs instances de la même tâche. Chaque instance d'une tâche est identifiée de manière unique dans le package et vous pouvez configurer chaque instance différemment.  
  
 Si vous supprimez une tâche, les contraintes de précédence connectant la tâche à d'autres tâches et les conteneurs du flux de contrôle sont également supprimés.  
  
 Les procédures ci-dessous décrivent comment ajouter ou supprimer une tâche ou un conteneur dans le flux de contrôle d'un package.  
  
## <a name="add-a-task-or-a-container-to-a-control-flow"></a>Ajouter une tâche ou un conteneur à un flux de contrôle  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** .  
  
4.  Pour ouvrir la **boîte à outils**, cliquez sur **Boîte à outils** dans le menu **Affichage** .  
  
5.  Développez **Éléments de flux de contrôle** et **Tâches du plan de maintenance**.  
  
6.  Faites glisser des tâches et des conteneurs de la **boîte à outils** vers l’aire de conception de l’onglet **Flux de contrôle** .  
  
7.  Connectez une tâche ou un conteneur du flux de contrôle du package à un autre composant du flux de contrôle en faisant glisser son connecteur vers cet autre composant.  
  
8.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="delete-a-task-or-a-container-from-a-control-flow"></a>Supprimer une tâche ou un conteneur d’un flux de contrôle  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir. Procédez de l’une des manières suivantes :  
  
    -   Cliquez sur l’onglet **Flux de contrôle** , cliquez avec le bouton droit sur la tâche ou le conteneur à supprimer, puis cliquez sur **Supprimer**.  
  
    -   Ouvrez **Explorateur de package**. Dans le dossier **Exécutables** , cliquez avec le bouton droit sur la tâche ou le conteneur à supprimer, puis cliquez sur **Supprimer**.  
  
3.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="set-the-properties-of-a-task-or-container"></a>Définir les propriétés d’une tâche ou d’un conteneur
Vous pouvez définir la plupart des propriétés des tâches et des conteneurs à l’aide de la fenêtre **Propriétés** . Les seules exceptions sont les propriétés des collections de tâches et les propriétés trop complexes à définir dans la fenêtre **Propriétés** . Par exemple, vous ne pouvez pas configurer l’énumérateur utilisé par le conteneur de boucles Foreach dans la fenêtre **Propriétés** . Vous devez utiliser un éditeur de tâche ou de conteneur pour définir ces propriétés complexes. La plupart des éditeurs de tâche et de conteneur possèdent plusieurs nœuds contenant chacun des propriétés connexes. Le nom du nœud indique l'objet des propriétés contenues dans le nœud.  
  
 Les procédures suivantes décrivent comment définir les propriétés d’une tâche ou d’un conteneur en utilisant les fenêtres **Propriétés** ou l’éditeur de tâche ou de conteneur correspondant.  
  
### <a name="set-the-properties-of-a-task-or-container-with-the-properties-window"></a>Définir les propriétés d’une tâche ou d’un conteneur à l’aide de la fenêtre Propriétés  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** .  
  
4.  Sur l’aire de conception de l’onglet **Flux de contrôle** , cliquez avec le bouton droit sur la tâche ou le conteneur, puis cliquez sur **Propriétés**.  
  
5.  Dans la fenêtre **Propriétés** , mettez à jour la valeur de la propriété.  
  
    > [!NOTE]  
    >  Vous pouvez définir les propriétés en tapant une valeur directement dans la zone de texte ou en sélectionnant une valeur dans une liste. Néanmoins, certaines propriétés sont plus complexes et disposent d'un éditeur de propriétés personnalisées. Pour définir la propriété, cliquez dans la zone de texte, puis sur le bouton Générer **(…)** pour ouvrir l’éditeur personnalisé.  
  
6.  Si vous le souhaitez, créez des expressions de propriété afin de mettre à jour de manière dynamique les propriétés de la tâche ou du conteneur. Pour plus d’informations, consultez [Ajouter ou modifier une Expression de propriété](../../integration-services/expressions/add-or-change-a-property-expression.md).  
  
7.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
### <a name="set-the-properties-of-a-task-or-container-with-the-task-or-container-editor"></a>Définir les propriétés d’une tâche ou d’un conteneur à l’aide de l’éditeur de tâche ou de conteneur  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l'onglet **Flux de contrôle** .  
  
4.  Sur l’aire de conception de l’onglet **Flux de contrôle** , cliquez avec le bouton droit sur la tâche ou le conteneur, puis cliquez sur **Modifier** pour ouvrir l’éditeur de tâche ou de conteneur correspondant.  
  
     Pour plus d’informations sur la configuration du conteneur de boucles For, consultez [Configurer un conteneur de boucles For](http://msdn.microsoft.com/library/b9cd7ea7-b198-4a35-8b16-6acf09611ca5).  
  
     Pour plus d’informations sur la configuration du conteneur de boucles Foreach, consultez [Configurer un conteneur de boucles Foreach](http://msdn.microsoft.com/library/519c6f96-5e1f-47d2-b96a-d49946948c25).  
  
    > [!NOTE]  
    >  Le conteneur de séquences ne possède pas d'éditeur personnalisé.  
  
5.  Si l'éditeur de tâche ou de conteneur comporte plusieurs nœuds, cliquez sur celui qui contient la propriété à définir.  
  
6.  Si vous le souhaitez, cliquez sur **Expressions** , puis dans la page **Expressions** , créez des expressions de propriété afin de mettre à jour dynamiquement les propriétés de la tâche ou du conteneur. Pour plus d’informations, consultez [Ajouter ou modifier une Expression de propriété](../../integration-services/expressions/add-or-change-a-property-expression.md).  
  
7.  Mettez à jour la valeur de la propriété.  
  
8.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a> Voir aussi  
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Conteneurs Integration Services](../../integration-services/control-flow/integration-services-containers.md)   
 [Flux de contrôle](../../integration-services/control-flow/control-flow.md)  
  
  
