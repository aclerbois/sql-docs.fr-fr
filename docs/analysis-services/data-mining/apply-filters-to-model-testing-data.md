---
title: Appliquer des filtres pour modéliser les données de test | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 03eb6a8b8a459f0d5d0769d1f7af16f06b81f560
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="apply-filters-to-model-testing-data"></a>Appliquer des filtres aux données de test du modèle
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Quand vous spécifiez une source de données externe à utiliser pour tester un modèle, vous pouvez éventuellement appliquer un filtre pour restreindre les données d’entrée. Par exemple, vous pouvez tester le modèle spécifiquement pour les prédictions sur des clients dans une certaine plage de revenus.  
  
 Par exemple, dans le scénario de publipostage ciblé AdventureWorks, vous pouvez créer une expression de filtre telle que la suivante sur ProspectiveBuyer, qui est la table contenant les données de test, et restreindre les cas de test par plage de revenus :  
  
 `[YearlyIncome] = '50000'`  
  
 Le comportement des filtres est légèrement différent, selon que vous filtrez les données d'apprentissage du modèle ou un jeu de données de test :  
  
-   Lorsque vous définissez un filtre sur un jeu de données de test, vous créez une clause WHERE sur les données entrantes. Si vous filtrez un jeu de données d'entrée utilisé pour évaluer un modèle, l'expression de filtre est traduite en instruction Transact-SQL et appliquée à la table d'entrée lorsque le graphique est créé. En conséquence, le nombre de scénarios de test peut être réduit de façon significative.  
  
-   Lorsque vous appliquez un filtre à un modèle d'exploration de données, l'expression de filtre que vous créez est traduite en instruction DMX (Data Mining Extensions) et appliquée au modèle individuel. Par conséquent, lorsque vous appliquez un filtre à un modèle, seul un sous-ensemble des données d'origine est utilisé pour l'apprentissage du modèle. Cela peut poser des problèmes si vous filtrez le modèle d'apprentissage avec un jeu de critères, afin que le modèle soit optimisé pour un certain ensemble de données, puis testez le modèle avec un autre jeu de critères.  
  
-   Si vous avez défini un jeu de données de test au moment de la création de la structure, les cas de modèles utilisés pour l’apprentissage incluent uniquement les cas qui se trouvent dans le jeu d’apprentissage de la structure d’exploration de données **et** qui remplissent les conditions du filtre. Ainsi, quand vous testez un modèle et sélectionnez l’option **Utiliser des scénarios de test de modèle d’exploration de données**, les scénarios de test incluent uniquement les scénarios qui se trouvent dans le jeu de test de la structure d’exploration de données et qui remplissent les conditions du filtre. Toutefois, si vous n'avez pas défini de jeu de données d'exclusion, les cas de modèles utilisés pour le test incluent tous les cas du jeu de données qui remplissent les conditions du filtre.  
  
-   Les conditions de filtre que vous appliquez sur un modèle affectent également les requêtes d'extraction sur les cas de modèle.  
  
 En résumé, lorsque vous testez plusieurs modèles, même si tous les modèles sont basés sur la même structure d'exploration de données, vous devez savoir que les modèles utilisent éventuellement différents sous-ensembles de données pour l'apprentissage et le test. Cela peut avoir les effets suivants sur les graphiques d'analyse de précision :  
  
-   Le nombre total de cas dans les jeux de test peut varier entre les modèles testés.  
  
-   Les pourcentages pour chaque modèle peuvent ne pas s'aligner dans le graphique, si les modèles utilisent différents sous-ensembles de données d'apprentissage ou de données de test.  
  
 Pour déterminer si un modèle contient un filtre prédéfini qui peut affecter les résultats, vous pouvez rechercher la propriété **Filtre** dans le volet **Propriété** , ou vous pouvez interroger le modèle à l’aide des ensembles de lignes de schéma d’exploration de données. Par exemple, la requête suivante retourne le texte de filtre pour le modèle spécifié :  
  
 `SELECT [FILTER] FROM $system.DMSCHEMA_MINING_MODELS WHERE MODEL_NAME = 'name of model’`  
  
> [!WARNING]  
>  Si vous souhaitez supprimer le filtre d'un modèle d'exploration de données existant, ou modifier les conditions de filtre, vous devez retraiter le modèle d'exploration de données.  
  
 Pour plus d’informations sur les types de filtres applicables et sur l’évaluation des expressions de filtre, consultez [Syntaxe de filtre de modèle et exemples&#40;Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/model-filter-syntax-and-examples-analysis-services-data-mining.md).  
  
### <a name="create-a-filter-on-external-testing-data"></a>Créer un filtre sur des données de test externes  
  
1.  Double-cliquez sur la structure d'exploration de données qui contient le modèle à tester pour ouvrir le Concepteur d'exploration de données.  
  
2.  Sélectionnez l’onglet **Graphique d’analyse de précision de l’exploration de données** , puis l’onglet **Sélection d’entrée** .  
  
3.  Sous l’onglet **Sélection d’entrée** , sous **Sélectionner le jeu de données à utiliser pour le graphique d’analyse de précision**, sélectionnez l’option **Spécifier un autre jeu de données**.  
  
4.  Cliquez sur le bouton Parcourir **(…)** pour ouvrir une boîte de dialogue et choisir le jeu de données externes.  
  
5.  Choisissez la table de cas, et ajoutez une table imbriquée si nécessaire. Mappez les colonnes du modèle aux colonnes du jeu de données externes selon les besoins. Fermez la boîte de dialogue **Spécifier le mappage des colonnes** pour enregistrer la définition de table source.  
  
6.  Cliquez sur **Ouvrir l’Éditeur de filtre** pour définir un filtre pour le jeu de données.  
  
     La boîte de dialogue **Filtre de jeu de données** s’ouvre. Si la structure contient une table imbriquée, vous pouvez créer un filtre en deux parties. Tout d’abord, définissez des conditions sur la table de cas à l’aide de la boîte de dialogue **Filtre de jeu de données** , puis définissez des conditions sur les lignes imbriquées à l’aide de la boîte de dialogue **Filtre** .  
  
7.  Dans la boîte de dialogue **Filtre de jeu de données** , cliquez sur la ligne supérieure dans la grille, sous **Colonne de la structure d’exploration de données**, puis sélectionnez une table ou une colonne dans la liste.  
  
     Si la vue de source de données contient plusieurs tables ou une table imbriquée, vous devez d'abord sélectionner le nom de la table. Autrement, vous pouvez sélectionner directement des colonnes de la table de cas.  
  
     Ajoutez une nouvelle ligne pour chaque colonne que vous souhaitez filtrer.  
  
8.  Utilisez les colonnes **Opérateur**et **Valeur** pour définir comment la colonne est filtrée.  
  
     **Remarque** Tapez les valeurs sans utiliser de guillemets.  
  
9. Cliquez sur la zone de texte **et/ou** et sélectionnez un opérateur logique pour définir la manière dont les conditions multiples sont combinées.  
  
10. Éventuellement, cliquez sur le bouton Parcourir **(…)** sur la droite de la zone de texte **Valeur** pour ouvrir la boîte de dialogue **Filtre** et définir des conditions sur la table imbriquée ou sur les différentes colonnes de table de cas.  
  
11. Vérifiez que les conditions de filtrage complétées sont correctes en affichant le texte dans le volet **Expression** .  
  
12. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     La condition de filtre est appliquée à la source de données lorsque vous créez le graphique d'analyse de précision.  
  
## <a name="see-also"></a>Voir aussi  
 [Choisir et mapper les données de test du modèle](../../analysis-services/data-mining/choose-and-map-model-testing-data.md)   
 [Données de Table à l’aide d’imbriquées comme entrée pour un graphique de précision](../../analysis-services/data-mining/using-nested-table-data-as-an-input-for-an-accuracy-chart.md)   
 [Choisir un Type de graphique d’analyse de précision et le jeu de Options de graphique](../../analysis-services/data-mining/choose-an-accuracy-chart-type-and-set-chart-options.md)  
  
  
