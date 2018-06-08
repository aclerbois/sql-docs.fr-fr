---
title: Créer un rapport de Validation croisée | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 88d3af205a1e92ac07a4c841c80f2abea463de9b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-cross-validation-report"></a>Créer un rapport de validation croisée
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Cette rubrique vous guide tout au long de la création d'un rapport de validation croisée à l'aide de l'onglet Graphique d'analyse de précision dans le Concepteur d'exploration de données. Pour obtenir des informations d’ordre général sur l’apparence du rapport de validation croisée et sur les mesures statistiques qu’il inclut, consultez [Validation croisée &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md).  
  
 Un rapport de validation croisée est fondamentalement différent d'un graphique d'analyse de précision, tel qu'un graphique de courbes d'élévation ou une matrice de classification.  
  
-   La validation croisée évalue la distribution globale des données utilisées dans un modèle ou une structure ; par conséquent, vous ne spécifiez pas de jeu de données de test. La validation croisée utilise toujours uniquement les données d'origine exploitées pour l'apprentissage du modèle ou la structure d'exploration de données.  
  
-   La validation croisée ne peut être exécutée que par rapport à des résultats prédictibles uniques. Si la structure prend en charge des modèles qui ont des attributs prédictibles différents, vous devez créer des rapports distincts pour chaque sortie prédictible.  
  
-   Seuls les modèles ayant un rapport avec la structure actuellement sélectionnée sont disponibles pour la validation croisée.  
  
-   Si la structure actuellement sélectionnée prend en charge une combinaison de modèles de clustering et d’autres modèles, quand vous cliquez sur **Obtenir les résultats**, la procédure stockée de validation croisée charge automatiquement les modèles qui ont la même colonne prédite et ignorent les modèles de clustering qui ne partagent pas le même attribut prédictible.  
  
-   Vous pouvez créer un rapport de validation croisée sur un modèle de clustering qui n'a pas d'attribut prédictible uniquement si la structure d'exploration de données ne prend en charge aucun autre attribut prédictible.  
  
### <a name="select-a-mining-structure"></a>Sélectionner une structure d'exploration de données  
  
1.  Ouvrez le Concepteur d'exploration de données dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
2.  Dans l'Explorateur de solutions, ouvrez la base de données qui contient la structure ou le modèle auquel est destiné le rapport que vous créez.  
  
3.  Double-cliquez sur la structure d'exploration de données pour ouvrir la structure et ses modèles associés dans le Concepteur d'exploration de données.  
  
4.  Cliquez sur l'onglet **Graphique d'analyse de précision de l'exploration de données** .  
  
5.  Cliquez sur l'onglet **Validation croisée** .  
  
### <a name="set-cross-validation-options"></a>Définir les options de validation croisée  
  
1.  Sous l'onglet **Validation croisée** , pour **Nombre de replis**, cliquez sur la flèche vers le bas pour sélectionner un nombre entre 1 et 10. La valeur par défaut est 10.  
  
     Le **Nombre de replis** représente le nombre des partitions qui seront créées dans le jeu de données d'origine. Si vous définissez le Nombre de replis à 1, le jeu d'apprentissage est utilisé sans partitionnement.  
  
2.  Pour **Attribut cible**, cliquez sur la flèche vers le bas et sélectionnez une colonne dans la liste. Si le modèle est un modèle de clustering, sélectionnez **#Cluster** pour indiquer que le modèle n’a pas d’attribut prédictible. Notez que la valeur, **#Cluster**, est disponible uniquement quand la structure d’exploration de données ne prend pas en charge d’autres types d’attributs prédictibles.  
  
     Vous ne pouvez sélectionner qu'un seul attribut prédictible par rapport. Par défaut, tous les modèles connexes qui ont le même attribut prédictible sont inclus dans le rapport.  
  
3.  Pour **Nombre maximal de cas**, entrez un nombre qui est assez grand pour fournir un exemple représentatif de données lorsque les données sont fractionnées parmi le nombre spécifié de plis. Si le nombre est supérieur au nombre de cas dans le jeu d'apprentissage du modèle, tous les cas sont utilisés.  
  
     Si le jeu de données d'apprentissage est très important, la définition de la valeur de **Nombre maximal de cas** limite le nombre total de cas traités, et permet au rapport de se terminer plus vite. Cependant, vous ne devez pas définir une valeur trop basse pour **Nombre maximal de cas** au risque de manquer de données suffisantes pour la validation croisée.  
  
4.  Éventuellement, pour **État cible**, tapez la valeur de l'attribut prédictible que vous souhaitez modéliser. Par exemple, si la colonne (Bike Buyer) a deux valeurs possibles, 1 (Oui) et 2 (Non), vous pouvez entrer la valeur 1 pour évaluer la précision du modèle uniquement pour le résultat souhaité.  
  
    > [!NOTE]  
    >  Si vous n’entrez pas de valeur, l’option **Seuil cible** n’est pas disponible et le modèle est évalué pour toutes les valeurs possibles de l’attribut prédictible.  
  
5.  Éventuellement, pour **Seuil cible**, tapez un nombre décimal entre 0 et 1 pour spécifier la probabilité minimale nécessaire à une prédiction pour être comptabilisée comme exacte.  
  
     Pour obtenir des conseils supplémentaires sur la manière de définir des seuils de probabilité, consultez [Mesures dans le rapport de validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md).  
  
6.  Cliquez sur **Obtenir les résultats**.  
  
### <a name="print-the-cross-validation-report"></a>Imprimer le rapport de validation croisée  
  
1.  Cliquez avec le bouton droit sur le rapport complété sous l’onglet **Validation croisée** .  
  
2.  Dans le menu contextuel, cliquez sur **Imprimer** ou **Aperçu avant impression** pour afficher d'abord un aperçu du rapport.  
  
### <a name="create-a-copy-of-the-report-in-microsoft-excel"></a>Créer une copie du rapport dans Microsoft Excel  
  
1.  Cliquez avec le bouton droit sur le rapport complété sous l’onglet **Validation croisée** .  
  
2.  Dans le menu contextuel, cliquez sur **Sélectionner tout**.  
  
3.  Cliquez avec le bouton droit sur le texte sélectionné, puis sélectionnez **Copier**.  
  
4.  Collez la sélection dans un classeur Excel ouvert. Si vous utilisez l'option **Coller** , le rapport est collé dans Excel au format HTML, ce qui conserve la mise en forme des lignes et des colonnes. Si vous collez le rapport en utilisant les options **Collage spécial** pour le texte ou le texte Unicode, le rapport est collé au format séparé par des lignes.  
  
## <a name="see-also"></a>Voir aussi  
 [Mesures dans le rapport de Validation croisée](../../analysis-services/data-mining/measures-in-the-cross-validation-report.md)  
  
  
