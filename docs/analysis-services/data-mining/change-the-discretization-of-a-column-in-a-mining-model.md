---
title: Modifier la discrétisation d’une colonne dans un modèle d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e9d6b2c75becad147e196534bb4d366dff01a13d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="change-the-discretization-of-a-column-in-a-mining-model"></a>Modifier la discrétisation d'une colonne dans un modèle d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] discrétise automatiquement les valeurs, autrement dit, il place les données dans une colonne numérique, dans certains scénarios. Par exemple, si vos données contiennent des données numériques continues et que vous créez un modèle d'arbre de décision, chaque colonne de données continues est intégrée automatiquement, selon la distribution des données. Si vous souhaitez contrôler la discrétisation des données, vous devez modifier les propriétés de la colonne de structure d'exploration de données, qui contrôle l'utilisation des données dans le modèle.  
  
 Pour obtenir des informations générales sur la façon de définir des propriétés dans un modèle d’exploration de données, consultez [Colonnes de modèle d’exploration de données](../../analysis-services/data-mining/mining-model-columns.md).  
  
### <a name="to-display-the-properties-for-a-mining-model-column"></a>Pour afficher les propriétés d'une colonne de modèle d'exploration de données  
  
1.  Sous l’onglet **Modèles d’exploration de données** du Concepteur d’exploration de données, cliquez avec le bouton droit sur l’en-tête de colonne contenant le nom du modèle d’exploration de données ou sur la ligne dans la grille qui contient le nom de l’algorithme d’exploration de données, puis sélectionnez **Propriétés**.  
  
     La fenêtre **Propriétés** affiche les propriétés associées au modèle d'exploration de données dans son ensemble.  
  
2.  Dans la colonne **Structure** près du côté gauche de l'écran, cliquez sur la colonne qui contient les données numériques continues que vous souhaitez discrétiser.  
  
     L'affichage de la fenêtre **Propriétés** se modifie et affiche uniquement les propriétés associées à cette colonne.  
  
### <a name="to-change-the-discretization-method"></a>Pour modifier la méthode de discrétisation  
  
1.  Dans la fenêtre **Propriétés d'exploration de données** , cliquez sur la zone de texte en regard de **Contenu**, et sélectionnez **Discretized** dans la liste déroulante.  
  
     La fenêtre <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationBucketCount%2A> et <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationMethod%2A> sont maintenant activées.  
  
2.  Sous l’onglet **Propriétés** , cliquez sur la zone de texte en regard de <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationMethod%2A> et sélectionnez l’une des valeurs suivantes : **Automatic**, **EqualAreas**ou **Cluster**.  
  
    > [!NOTE]  
    >  Si l’utilisation de la colonne est définie sur **Ignorer**, la fenêtre **Propriétés** pour la colonne est vide.  
  
     La nouvelle valeur est appliquée lorsque vous sélectionnez un élément différent dans le concepteur.  
  
3.  Sous l’onglet **Propriétés** , cliquez sur la zone de texte en regard de <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationBucketCount%2A> et tapez une valeur numérique.  
  
    > [!NOTE]  
    >  Si vous modifiez ces propriétés, la structure doit être traitée à nouveau, avec tous les modèles auxquels s'applique le nouveau paramètre.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches liées aux modèles d’exploration de données et procédures](../../analysis-services/data-mining/mining-model-tasks-and-how-tos.md)  
  
  
