---
title: Choisir et mapper le modèle de données de test | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3c731f1a439a817abd133e14815b85a8f6d0077b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="choose-and-map-model-testing-data"></a>Choisir et mapper les données de test du modèle
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Pour créer un graphique d’analyse de précision dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous devez choisir les données à utiliser pour tester le modèle et mapper les données au modèle.  
  
 Par défaut, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise les données de test du modèle d’exploration de données, sous réserve que vous ayez créé un jeu de données d’exclusion au moment de la génération de la structure d’exploration de données. La création d'un jeu de test d'exclusion est la méthode la plus simple pour tester les modèles basés sur la même structure d'exploration de données, car les noms de colonnes et les types de données correspondront toujours au modèle, et vous pouvez être raisonnablement assuré que la distribution des données est similaire. En outre, le concepteur créera automatiquement les relations entre l'entrée et les colonnes du modèle.  
  
 Ou bien, vous pouvez spécifier une source de données externe. Pour les données externes, il existe certaines exigences supplémentaires :  
  
-   Le jeu de données externes doit être défini comme vue de source de données dans une instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
-   Le jeu de données externes doit au moins contenir une colonne qui peut être mappée à la colonne prédictible dans le modèle d'exploration de données. Vous pouvez choisir d'ignorer certaines colonnes.  
  
-   Vous ne pouvez pas ajouter de nouvelles colonnes ou mapper des colonnes dans une vue de source de données différente. La vue de source de données que vous sélectionnez doit contenir toutes les colonnes dont vous avez besoin pour la requête de prédiction.  
  
-   Si les noms des colonnes externes correspondent exactement à ceux du modèle, le concepteur les mappera automatiquement. Si les mappages sont erronés, vous pouvez les modifier, ou supprimer et créer de nouveaux mappages pour les colonnes existantes.  
  
-   Si vous utilisez une source de données externe, vous pouvez appliquer des filtres pour restreindre les données de test à un sous-ensemble approprié de cas.  
  
-   Même lorsque vous utilisez le jeu de test d'exclusion, tenez compte du fait que les filtres peuvent provoquer des différences entre les données de test associées à une structure d'exploration de données et les cas de test du modèle d'exploration de données.  
  
 Cette rubrique explique comment choisir et mapper les données de test :  
  
 [Sélectionner des tables d'entrée pour tester la précision d'un modèle d'exploration de données](#bkmk_SelectInputs)  
  
 [Mapper des colonnes aux colonnes des données de test](#bkmk_MapColumns)  
  
 [Modifier la façon dont les colonnes des données de test sont mappées au modèle](#bkmk_ChangeMappings)  
  
##  <a name="bkmk_SelectInputs"></a> Pour sélectionner des tables d'entrée pour tester la précision d'un modèle d'exploration de données  
  
1.  Dans le Concepteur d'exploration de données de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], double-cliquez sur la structure d'exploration de données qui contient les modèles pour lesquels vous souhaitez établir un graphique.  
  
2.  Sélectionnez l’onglet **Graphique d’analyse de précision de l’exploration de données** .  
  
3.  Sous l’onglet **Sélection d’entrée** de la vue **Graphique d’analyse de précision de l’exploration de données** , sélectionnez l’une des options suivantes :  
  
     **Utiliser des scénarios de test de modèle d'exploration de données**  
  
     **Utiliser des scénarios de test de structure d'exploration de données**  
  
     **Spécifier un autre jeu de données**  
  
4.  Si vous avez sélectionné **Spécifier un autre jeu de données**, cliquez éventuellement sur **Ouvrir l’Éditeur de filtre** pour créer les conditions de filtrage sur le jeu de données d’entrée. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
5.  Cliquez sur l’onglet **Graphique de courbes d’élévation** ou sur l’onglet **Matrice de classification** pour générer automatiquement le graphique en utilisant les données de test spécifiées.  
  
##  <a name="bkmk_MapColumns"></a> Pour mapper des colonnes aux colonnes des données de test  
  
1.  Pour ouvrir la structure et les modèles dans le Concepteur d'exploration de données, double-cliquez sur la structure d'exploration de données qui contient les modèles pour lesquels vous souhaitez établir un graphique.  
  
2.  Sélectionnez l’onglet **Graphique d’analyse de précision de l’exploration de données** , puis l’onglet **Sélection d’entrée** .  
  
3.  Dans **Sélection d’entrée** , sous **Sélectionner le jeu de données à utiliser pour le graphique d’analyse de précision**, sélectionnez **Spécifier un autre jeu de données**.  
  
4.  Cliquez sur le bouton Parcourir **(…)** pour ouvrir une boîte de dialogue et créer la définition du jeu de données externe.  
  
5.  Dans la boîte de dialogue **Sélectionner la structure d’exploration de données** , sélectionnez la structure d’exploration de données qui contient les modèles à utiliser, puis cliquez sur **OK**.  
  
6.  Dans la table **Sélectionner une ou plusieurs tables d’entrée** de l’onglet **Graphique d’analyse de précision de l’exploration de données** , cliquez sur **Sélectionner la table de cas** pour ouvrir la boîte de dialogue **Sélectionner une table** .  
  
7.  Dans la boîte de dialogue **Sélectionner une table** , sélectionnez une source de données dans la liste **Source de données** . Sélectionnez une table qui contient les données à utiliser dans les requêtes de prédiction pour déterminer la précision des modèles.  
  
8.  Dans la zone **Nom de la table/vue** , sélectionnez la table qui contient les données à utiliser pour tester les modèles.  
  
9. Modifiez les mappages, si nécessaire. Les colonnes de la structure d’exploration de données sont mappées automatiquement aux colonnes portant le même nom dans la table d’entrée. Pour créer des mappages manuellement, cliquez sur une colonne dans la table **Sélectionner une ou plusieurs tables d’entrée** et faites-la glisser vers la colonne correspondante dans la table **Structure d’exploration de données** . Pour supprimer un mappage, cliquez sur la ligne qui lie la colonne de la table **Structure d’exploration de données** à la colonne associée de la table **Sélectionner une ou plusieurs tables d’entrée** et appuyez sur Suppr.  
  
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="bkmk_ChangeMappings"></a> Pour modifier le mappage des données d'entrée au modèle  
  
1.  Dans le Concepteur d’exploration de données, double-cliquez sur la structure qui contient les modèles pour lesquels vous souhaitez établir un graphique.  
  
2.  Sélectionnez l’onglet **Graphique d’analyse de précision de l’exploration de données** .  
  
3.  Cliquez sur l’onglet **Sélection d’entrée** .  
  
4.  Dans **Sélectionner le jeu de données à utiliser pour le graphique d’analyse de précision**, sélectionnez l’option **Spécifier un autre jeu de données**.  
  
5.  Cliquez sur le bouton Parcourir **(…)** pour ouvrir une boîte de dialogue et créer la définition de la source de données externe.  
  
6.  Dans la boîte de dialogue **Spécifier le mappage des colonnes** , cliquez sur **Sélectionner la table de cas**.  
  
7.  Dans la boîte de dialogue Sélectionner une table, sélectionnez une vue de source de données dans la liste, puis sélectionnez la table qui contient les données de cas. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
8.  Si les tables dont vous avez besoin ne sont pas disponibles, fermez la boîte de dialogue et créez une nouvelle vue de source de données qui contient la table. Pour plus d’informations sur la création d’une vue de source de données, consultez [Définition d’une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/defining-a-data-source-view-analysis-services.md).  
  
9. Si le modèle d’exploration de données contient une table imbriquée, cliquez sur **Sélectionner la table imbriquée**, puis sélectionnez la table imbriquée dans la liste de tables de la vue de source de données. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
10. Sélectionnez la ligne de jointure du mappage à modifier, puis sélectionnez **Modifier les connexions**.  
  
     La boîte de dialogue **Modifier le mappage** s’ouvre. Dans la table de cette boîte de dialogue, **Colonne de la structure d’exploration de données** contient la liste des colonnes de la structure d’exploration de données sélectionnée, et **Colonne de table** contient la liste des colonnes des tables d’entrée qui sont associées aux colonnes de la structure d’exploration de données.  
  
11. Sous **Colonne de table**, sélectionnez la ligne qui correspond à la ligne sous **Colonne de la structure d’exploration de données** pour laquelle vous souhaitez modifier une relation. Sélectionnez une nouvelle colonne dans la liste ou l'entrée vide de la liste pour supprimer la colonne.  
  
12. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Les nouveaux mappages de colonnes sont affichés dans la boîte de dialogue **Spécifier le mappage des colonnes** . Pour supprimer un mappage, sélectionnez la ligne située entre les colonnes, puis appuyez sur la touche Suppr. Pour créer une connexion, sélectionnez une colonne dans la table **Structure d’exploration de données** , puis faites-la glisser vers la colonne correspondante de la table **Sélectionner une ou plusieurs tables d’entrée** .  
  
## <a name="see-also"></a>Voir aussi  
 [Test et de tâches de Validation et de procédures & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-tasks-and-how-tos-data-mining.md)  
  
  
