---
title: Parcourir un modèle à l’aide de la visionneuse d’arborescences Microsoft | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 26d56116652e389e60edfb269ce2e23c14bb824f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="browse-a-model-using-the-microsoft-tree-viewer"></a>Explorer un modèle à l'aide de la visionneuse d'arborescences Microsoft
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La Visionneuse d’arborescence [!INCLUDE[msCoName](../../includes/msconame-md.md)] de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] affiche les arbres de décision qui sont générés avec l’algorithme MDT ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees). L'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) est un algorithme d'arbre de décision hybride qui prend en charge à la fois la classification et la régression. Par conséquent, vous pouvez également utiliser cette visionneuse pour consulter les modèles fondés sur l'algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression). L’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) est utilisé pour la modélisation prédictive des attributs discrets et continus. Pour plus d’informations sur cet algorithme, consultez [Algorithme MDT (Microsoft Decision Trees)](../../analysis-services/data-mining/microsoft-decision-trees-algorithm.md).  
  
> [!NOTE]  
>  La visionneuse de l'arborescence de contenu générique [!INCLUDE[msCoName](../../includes/msconame-md.md)] permet de consulter des informations détaillées relatives aux équations utilisées dans le modèle et les motifs découverts. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse de l’arborescence de contenu générique Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-generic-content-tree-viewer.md) ou [Visionneuse de l’arborescence de contenu générique Microsoft &#40;exploration de données&#41;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c).  
  
##  <a name="BKMK_TabsPanes"></a> Onglets de la visionneuse  
 Lorsque vous parcourez un modèle d'exploration de données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], le modèle s'affiche sous l'onglet **Visionneuse de modèle d'exploration de données** du Concepteur d'exploration de données, à l'aide de la visionneuse appropriée pour ce modèle. La Visionneuse d'arborescences [!INCLUDE[msCoName](../../includes/msconame-md.md)] contient les onglets et volets suivants :  
  
-   [Arbre de décision](#BKMK_DecisionTree)  
  
-   [Réseau de dépendances](#BKMK_DependencyNetwork)  
  
-   [Légende d'exploration de données](#BKMK_MiningLegend)  
  
###  <a name="BKMK_DecisionTree"></a> Arbre de décision  
 Lorsque vous créez un modèle d'arbre de décision, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère un arbre distinct pour chaque attribut prévisible. Vous pouvez afficher un arbre spécifique en le sélectionnant dans la liste **Arborescence** sous l’onglet **Arbre de décision** de la visionneuse.  
  
 Un arbre de décision se compose d’une série de fractionnements, le fractionnement le plus important (tel que déterminé par l’algorithme) figurant à gauche de la visionneuse dans le nœud **Tout** . Des fractionnements supplémentaires apparaissent à droite. La division se trouvant dans le nœud **Tout** est la plus importante car elle contient l’instruction conditionnelle de fractionnement la plus forte du dataset et est, par conséquent, à l’origine de la première division.  
  
 Vous pouvez développer ou réduire certains nœuds dans l'arbre pour afficher ou masquer les fractionnements qui apparaissent après chaque nœud. Vous pouvez également utiliser les options de l’onglet **Arbre de décision** pour modifier l’affichage de l’arbre. Utilisez le curseur **Afficher le niveau** pour régler le nombre de niveaux affichés dans l’arbre. Utilisez **Expansion par défaut** pour définir le nombre de niveaux par défaut affichés pour tous les arbres du modèle.  
  
#### <a name="predicting-discrete-attributes"></a>Prédiction d'attributs discrets  
 Lorsqu'une arborescence est générée avec un attribut prévisible discret, la visionneuse affiche les éléments suivants dans chaque nœud de l'arbre :  
  
-   la condition qui est à l'origine de la division ;  
  
-   un histogramme représentant la distribution des états de l'attribut prévisible, classés par fréquence.  
  
 Vous pouvez utiliser l’option **Histogramme** pour modifier le nombre d’états apparaissant dans les histogrammes de l’arbre. Cette option est utile si l'attribut prévisible possède de nombreux états. Les états apparaissent dans un histogramme par ordre de fréquence de gauche à droite ; si le nombre d'états que vous avez choisi d'afficher est inférieur au nombre total d'états de l'attribut, les états les moins fréquents sont affichés collectivement en gris. Pour voir le nombre exact de chaque état dans un nœud, placez le curseur sur le nœud pour afficher une info-bulle ou sélectionnez le nœud pour afficher ses détails dans la **Légende d’exploration de données**.  
  
 La couleur d’arrière-plan de chaque nœud représente la concentration de cas de l’état d’attribut spécifique que vous sélectionnez à l’aide de l’option **Arrière-plan** . Vous pouvez utiliser cette option pour mettre en évidence les nœuds contenant une cible spécifique qui vous intéresse.  
  
#### <a name="predicting-continuous-attributes"></a>Prédiction d'attributs continus  
 Lorsqu'un arbre est généré avec un attribut prévisible continu, la visionneuse affiche un graphique en losange, au lieu d'un histogramme, pour chaque nœud de l'arbre. Le graphique en forme de losange comporte une ligne qui représente la plage de l'attribut. Le losange est situé à la moyenne du nœud et la largeur du losange représente la variance de l'attribut sur ce nœud. Un losange plus étroit indique que le nœud peut créer une prédiction plus précise. La visionneuse affiche également l'équation de régression qui sert à déterminer la division du nœud.  
  
#### <a name="additional-decision-tree-display-options"></a>Options d'affichage supplémentaires pour les arbres de décision  
 Lorsque l’extraction est activée pour un modèle d’arbre de décision, vous pouvez accéder aux cas d’apprentissage qui prennent en charge un nœud en cliquant avec le bouton droit sur ce nœud dans l’arbre et en sélectionnant **Extraire**. Vous pouvez activer l’extraction dans l’Assistant Exploration de données ou en spécifiant la propriété d’extraction sur le modèle d’exploration de données sous l’onglet **Modèles d’exploration de données** .  
  
 Vous pouvez utiliser les options de zoom sous l’onglet **Arbre de décision** pour faire un zoom avant ou arrière sur un arbre ou utiliser **Ajuster la taille** pour afficher l’intégralité du modèle dans l’écran de la visionneuse. Si l’arbre est trop grand pour être ajusté à la taille de l’écran, vous pouvez utiliser l’option **Navigation**pour naviguer dans l’arbre. Si vous cliquez sur **Navigation** , une nouvelle fenêtre de navigation s’ouvre pour vous permettre de sélectionner les sections du modèle à afficher.  
  
 Vous pouvez également copier l'image de l'arborescence dans le Presse-papiers afin de pouvoir la coller dans des documents ou dans un logiciel de manipulation d'images. Sélectionnez **Copier la vue du graphique** pour copier uniquement la section de l’arborescence qui est visible dans la visionneuse ou sélectionnez **Copier le graphique entier** pour copier tous les nœuds développés de l’arborescence.  
  
 [Retour au début](#BKMK_TabsPanes)  
  
###  <a name="BKMK_DependencyNetwork"></a> Réseau de dépendances  
 Le **Réseau de dépendances** affiche les dépendances entre les attributs d’entrée et les attributs prévisibles du modèle. Le curseur situé à gauche de la visionneuse agit comme un filtre lié à la force des dépendances. Si vous déplacez le curseur vers le bas, la visionneuse affiche uniquement les liens les plus forts.  
  
 Lorsque vous sélectionnez un nœud, la visionneuse met en surbrillance les dépendances propres à ce nœud. Par exemple, si vous choisissez un nœud prévisible, la visionneuse met également en surbrillance chacun des nœuds permettant de prédire le nœud prévisible.  
  
 Si la visionneuse contient un grand nombre de nœuds, vous pouvez rechercher des nœuds spécifiques à l’aide du bouton **Rechercher un nœud** . En cliquant sur **Rechercher un nœud** , vous ouvrez la boîte de dialogue **Rechercher un nœud** dans laquelle vous pouvez utiliser un filtre pour rechercher et sélectionner des nœuds spécifiques.  
  
 La légende en bas de la visionneuse associe des codes de couleur au type de dépendance apparaissant dans le graphique. Par exemple, lorsque vous sélectionnez un nœud prévisible, le nœud prévisible est surligné en turquoise et les nœuds permettant de prédire le nœud sélectionné sont surlignés en orange.  
  
 [Retour au début](#BKMK_TabsPanes)  
  
###  <a name="BKMK_MiningLegend"></a> Légende d'exploration de données  
 La **Légende d’exploration de données** affiche les informations suivantes lorsque vous sélectionnez un nœud dans le modèle d’arbre de décision :  
  
-   le nombre de cas dans le nœud, décomposés en fonction des différents états de l'attribut prévisible ;  
  
-   la probabilité de chaque cas de l'attribut prévisible pour le nœud ;  
  
-   un histogramme contenant le nombre de chaque état de l'attribut prévisible ;  
  
-   les conditions requises pour atteindre un nœud spécifique, également appelées le *chemin du nœud*.  
  
-   Pour les modèles de régression linéaire, formule de la régression.  
  
 Vous pouvez ancrer et utiliser la **Légende d’exploration de données** de la même façon que l’Explorateur de solutions.  
  
 [Retour au début](#BKMK_TabsPanes)  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme d’arbres de décision Microsoft](../../analysis-services/data-mining/microsoft-decision-trees-algorithm.md)   
 [Visionneuses de modèle d’exploration de données & #40 ; Générateur de modèles d’exploration de données & #41 ;](http://msdn.microsoft.com/library/4ba391d5-c97b-4848-ba7c-7d096fa4b7dd)   
 [Tâches de la visionneuse modèle d’exploration de données et de procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)   
 [Outils d’exploration de données](../../analysis-services/data-mining/data-mining-tools.md)   
 [Visionneuses de modèle d’exploration de données](../../analysis-services/data-mining/data-mining-model-viewers.md)  
  
  
