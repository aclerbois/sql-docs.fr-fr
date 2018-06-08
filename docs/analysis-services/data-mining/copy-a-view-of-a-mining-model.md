---
title: Copie d’une vue d’un modèle d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 685d4ce90312e9f34dcf9b8a6e0c94b419f37ee4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="copy-a-view-of-a-mining-model"></a>Copier une vue d'un modèle d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’onglet **Visionneuse de modèle d’exploration de données** du Concepteur d’exploration de données de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] utilise une visionneuse distincte pour chaque type de modèle d’exploration de données. Plusieurs des visionneuses ont des composants à partir desquels vous pouvez copier le contenu vers le Presse-papiers, puis coller le contenu dans un document ou un logiciel de manipulation d'image. Cette fonctionnalité est disponible avec les composants suivants :  
  
-   Diagramme de cluster dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Cluster Viewer et la Visionneuse de l'algorithme MSC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Cluster)  
  
-   Arbre de décision dans la Visionneuse d’arborescence [!INCLUDE[msCoName](../../includes/msconame-md.md)] et la Visionneuse de l’algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series)  
  
-   Transitions d'état dans la Visionneuse de l'algorithme MSC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Cluster)  
  
-   Réseau de dépendances dans la Visionneuse de l'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules), la Visionneuse de l'algorithme MNB ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes) et la Visionneuse d'arborescence [!INCLUDE[msCoName](../../includes/msconame-md.md)]  
  
-   Contenu du modèle d’exploration de données à partir du volet Détails du nœud de la Visionneuse de l’arborescence de contenu générique [!INCLUDE[msCoName](../../includes/msconame-md.md)]  
  
 Vous pouvez copier la représentation complète du modèle d'exploration de données ou simplement la partie visible dans la visionneuse.  
  
> [!WARNING]  
>  Lorsque vous copiez un modèle à l'aide de la visionneuse, il ne crée pas de nouvel objet de modèle. Pour créer un modèle, vous devez utiliser l'Assistant, ou le Concepteur d'exploration de données. Pour plus d’informations, consultez [Créer une copie d’un modèle d’exploration de données](../../analysis-services/data-mining/make-a-copy-of-a-mining-model.md).  
  
### <a name="to-copy-the-complete-model-to-the-clipboard"></a>Pour copier le modèle complet dans le Presse-papiers  
  
1.  Dans la liste **Modèle d’exploration de données** sous l’onglet **Visionneuse de modèle d’exploration de données** , sélectionnez le modèle d’exploration de données à afficher.  
  
2.  Sélectionnez l’onglet approprié, tel que l’onglet **Réseau de dépendances** , puis cliquez sur **Copier le graphique entier** dans la barre d’outils de l’onglet.  
  
### <a name="to-copy-the-visible-piece-of-the-model-to-the-clipboard"></a>Pour copier la partie visible du modèle vers le Presse-papiers  
  
1.  Dans la liste **Modèle d’exploration de données** sous l’onglet **Visionneuse de modèle d’exploration de données** , sélectionnez le modèle d’exploration de données à afficher.  
  
2.  Sélectionnez l’onglet approprié, tel que l’onglet **Réseau de dépendances** , puis effectuez un zoom avant ou arrière sur le modèle au niveau approprié.  
  
3.  Cliquez sur **Copier la vue du graphique** dans la barre d’outils de l’onglet sélectionné.  
  
### <a name="to-copy-the-mining-model-content-to-the-clipboard"></a>Pour copier le contenu du modèle d'exploration de données dans le Presse-papiers  
  
1.  Dans la liste **Modèle d’exploration de données** sous l’onglet **Visionneuse de modèle d’exploration de données** , sélectionnez le modèle d’exploration de données à afficher.  
  
2.  Dans la liste déroulante **Visionneuse** , sélectionnez **Visionneuse de l’arborescence de contenu générique Microsoft**.  
  
3.  Dans le volet **Légende du nœud (ID unique)** , cliquez sur un nœud.  
  
4.  Cliquez avec le bouton droit sur le volet **Détails du nœud** , puis sélectionnez **Sélectionner tout**.  
  
5.  Cliquez de nouveau avec le bouton droit sur le volet **Détails du nœud** , puis sélectionnez **Copier**.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches de la visionneuse modèle d’exploration de données et de procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)  
  
  
