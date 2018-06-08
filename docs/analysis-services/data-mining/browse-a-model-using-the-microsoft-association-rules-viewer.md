---
title: Parcourir un modèle à l’aide de la visionneuse de règles Microsoft Association | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8eeedec727fe33b577b7bd382a8b69371428e563
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="browse-a-model-using-the-microsoft-association-rules-viewer"></a>Explorer un modèle à l'aide de la visionneuse de l'algorithme MAR (Microsoft Association Rules)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La Visionneuse de l'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] affiche les modèles d'exploration de données qui sont générés avec l'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules). L'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) est un algorithme d'association conçu pour la création de modèles d'exploration de données permettant d'effectuer des analyses de panier d'achat. Pour plus d'informations sur cet algorithme, consultez [Microsoft Association Algorithm](../../analysis-services/data-mining/microsoft-association-algorithm.md).  
  
 Voici les principales raisons pour lesquelles vous utilisez l'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) :  
  
-   pour rechercher des jeux d'éléments décrivant des éléments qui se trouvent généralement ensemble dans une transaction ;  
  
-   Pour découvrir les règles prédisant la présence d'autres éléments dans une transaction en fonction d'éléments existants  
  
> [!NOTE]  
>  La visionneuse de l'arborescence de contenu générique [!INCLUDE[msCoName](../../includes/msconame-md.md)] permet de consulter des informations détaillées relatives aux équations utilisées dans le modèle et les motifs découverts. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse de l’arborescence de contenu générique Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-generic-content-tree-viewer.md) ou [Visionneuse de l’arborescence de contenu générique Microsoft &#40;exploration de données&#41;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c).  
  
 Pour plus d’informations sur la création, l’exploration et l’utilisation d’un modèle d’exploration de données Association, consultez [Leçon 3 : Génération d’un scénario de panier d’achat &#40;Didacticiel d’exploration de données intermédiaire&#41;](http://msdn.microsoft.com/library/651eef38-772e-4d97-af51-075b1b27fc5a).  
  
##  <a name="BKMK_ViewerTabs"></a> Onglets de la visionneuse  
 Lorsque vous parcourez un modèle d'exploration de données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], le modèle s'affiche sous l'onglet **Visionneuse de modèle d'exploration de données** du Concepteur d'exploration de données, à l'aide de la visionneuse appropriée pour ce modèle. La Visionneuse de l'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) contient les onglets suivants :  
  
-   [Jeux d'éléments](#BKMK_Itemsets)  
  
-   [Règles](#BKMK_Rules)  
  
-   [Réseau de dépendances](#BKMK_Dependency)  
  
 Chaque onglet contient la case à cocher **Afficher le nom long** qui vous permet d'afficher ou de masquer, dans la règle ou le jeu d'éléments, la table d'où provient le jeu d'éléments.  
  
###  <a name="BKMK_Itemsets"></a> Jeux d'éléments  
 L'onglet **Jeux d'éléments** affiche la liste des jeux d'éléments que le modèle a identifiés comme apparaissant fréquemment ensemble. L'onglet contient une grille comportant les colonnes suivantes : **Prise en charge**, **Taille**et **Jeu d'éléments**. Pour plus d'informations sur la prise en charge, consultez [Microsoft Association Algorithm](../../analysis-services/data-mining/microsoft-association-algorithm.md). La colonne **Taille** affiche le nombre d'éléments présents dans le jeu d'éléments. La colonne **Jeu d'éléments** affiche le jeu d'éléments actuel que le modèle a découvert. Vous pouvez contrôler le format du jeu d'éléments à l'aide de la liste **Afficher** , en choisissant parmi les options suivantes :  
  
-   **Afficher le nom et la valeur de l'attribut**  
  
-   **Afficher la valeur de l'attribut uniquement**  
  
-   **Afficher le nom de l'attribut uniquement**  
  
 Vous pouvez filtrer le nombre de jeux d'éléments affichés sous cet onglet en utilisant **Prise en charge minimale** et **Taille minimale du jeu d'éléments**. Vous pouvez limiter encore plus le nombre de jeux d'éléments affichés à l'aide de **Filtrer le jeu d'éléments** en définissant une caractéristique de jeu d'éléments qui doit exister. Par exemple, si vous tapez « Water Bottle = existing », vous limitez les jeux d'éléments à ceux qui contiennent une bouteille d'eau. L'option **Filtrer le jeu d'éléments** affiche également une liste des filtres que vous avez utilisés précédemment.  
  
 Vous pouvez trier les lignes de la grille en cliquant sur un en-tête de colonne.  
  
 [Retour au début](#BKMK_ViewerTabs)  
  
###  <a name="BKMK_Rules"></a> Règles  
 L'onglet **Règles** affiche les règles découvertes par l'algorithme d'association. L'onglet **Règles** comprend une grille qui contient les colonnes suivantes : **Probabilité**, **Importance**et **Règle**. La probabilité indique la probabilité du résultat d'une règle. L'importance sert à mesurer l'utilité d'une règle. Même si la probabilité d'une règle est élevée, son utilité peut être sans importance. La colonne Importance permet de préciser ce point. Par exemple, si tous les jeux d'éléments contiennent un état spécifique d'un attribut, une règle prédisant l'état est insignifiante même si sa probabilité est très élevée. Plus l'importance est élevée, plus la règle est importante.  
  
 Vous pouvez utiliser les options **Probabilité minimale** et **Importance minimale** pour effectuer un filtrage des règles, similaire au filtrage que vous pouvez effectuer sous l'onglet **Jeux d'éléments** . Vous pouvez également utiliser **Filtrer la règle** pour filtrer une règle en fonction des états d'attribut qu'elle contient.  
  
 Vous pouvez trier les lignes de la grille en cliquant sur un en-tête de colonne.  
  
 [Retour au début](#BKMK_ViewerTabs)  
  
###  <a name="BKMK_Dependency"></a> Réseau de dépendances  
 L'onglet **Réseau de dépendances** contient une visionneuse du réseau de dépendances. Chaque nœud figurant dans la visionneuse représente un élément, par exemple « state = WA ». La flèche entre les nœuds représente l'association entre les éléments. Le sens de la flèche détermine l'association entre les éléments conformément aux règles découvertes par l'algorithme. Par exemple, si la visionneuse contient les trois éléments A, B et C, et que C est prédit par A et B, lorsque vous sélectionnez le nœud C, deux flèches pointent vers ce nœud : A vers C et B vers C.  
  
 Le curseur situé à gauche de la visionneuse agit comme un filtre lié à la probabilité des règles. Si vous déplacez le curseur vers le bas, seuls les liens les plus forts sont affichés.  
  
 [Retour au début](#BKMK_ViewerTabs)  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme Microsoft Association](../../analysis-services/data-mining/microsoft-association-algorithm.md)   
 [Tâches de la visionneuse modèle d’exploration de données et de procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)   
 [Tâches de la visionneuse modèle d’exploration de données et de procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)   
 [Outils d’exploration de données](../../analysis-services/data-mining/data-mining-tools.md)   
 [Visionneuses de modèle d’exploration de données](../../analysis-services/data-mining/data-mining-model-viewers.md)  
  
  
