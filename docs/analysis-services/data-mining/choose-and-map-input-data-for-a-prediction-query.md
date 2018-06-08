---
title: Choisir et mapper les données d’entrée pour une requête de prédiction | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0781c35dfe7bcc1ea99be3d68fcbb839d5f9374b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="choose-and-map-input-data-for-a-prediction-query"></a>Choisir et mapper les données d'entrée pour une requête de prédiction
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez des prédictions à partir d'un modèle d'exploration de données, vous le faites généralement en alimentant de nouvelles données dans le modèle. (Les modèles de série chronologique, qui peuvent faire des prédictions basées sur des données historiques uniquement, font exception.) Pour fournir de nouvelles données au modèle, vous devez vous assurer que les données sont disponibles dans une vue de source de données. Si vous connaissez à l'avance les données que vous allez utiliser pour la prédiction, vous pouvez les inclure dans la vue de source de données utilisée pour créer le modèle. Sinon, vous devrez peut-être créer une vue de source de données. Pour plus d’informations sur les vues de source de données, consultez [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md).  
  
 Parfois les données dont vous avez besoin peuvent être contenues dans plusieurs tables d'une jointure un-à-plusieurs. Tel est le cas avec les données utilisées pour les modèles d'association ou les modèles Sequence Clustering, qui utilisent une table de cas liée à une table imbriquée qui contient les détails du produit ou de la transaction. Si votre modèle utilise une structure de table de cas imbriquée, les données que vous utilisez pour la prédiction doivent également avoir une structure de table de cas imbriquée.  
  
> [!WARNING]  
>  Vous ne pouvez pas ajouter de nouvelles colonnes ou mapper des colonnes qui se trouvent dans une vue de source de données différente. La vue de source de données que vous sélectionnez doit contenir toutes les colonnes dont vous avez besoin pour la requête de prédiction.  
  
 Après avoir identifié les tables qui contiennent les données que vous allez utiliser pour les prédictions, vous devez mapper les colonnes des données externes aux colonnes du modèle d'exploration de données. Par exemple, si votre modèle prédit le comportement d'achat de clients en fonction des statistiques démographiques et des réponses aux enquêtes, vos données d'entrée contiennent des informations qui correspondent généralement à ce qui est dans le modèle. Vous n'avez pas besoin d'avoir des données correspondantes pour chaque colonne, mais plus le nombre de colonnes correspondant est élevé, meilleurs sont les résultats. Si vous essayez de mapper des colonnes qui ont des types de données différents, vous pouvez obtenir une erreur. Dans ce cas, vous pouvez définir un calcul nommé dans la vue de source de données pour convertir les nouvelles données de la colonne en type de données requis par le modèle. Pour plus d’informations, consultez [Définir des calculs nommés dans une vue de source de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-named-calculations-in-a-data-source-view-analysis-services.md).  
  
 Lorsque vous choisissez les données à utiliser pour la prédiction, certaines colonnes de la source de données sélectionnée peuvent être mappées automatiquement aux colonnes du modèle d’exploration de données, selon la similitude des noms et le type de données correspondant. Vous pouvez utiliser la boîte de dialogue **Modifier le mappage** dans **Prédiction de modèle d’exploration de données** pour modifier les colonnes qui sont mappées, supprimer les mappages inappropriés, ou créer des mappages pour les colonnes existantes. L’aire de conception **Prédiction de modèle d’exploration de données** prend également en charge la modification par glisser-déplacer des connexions.  
  
-   Pour créer une connexion, sélectionnez une colonne dans la table **Modèle d’exploration de données** et faites-la glisser vers la colonne correspondante dans la table **Sélectionner une ou plusieurs tables d’entrée** .  
  
-   Pour supprimer une connexion, sélectionnez la ligne de connexion et appuyez sur la touche Suppr.  
  
 La procédure suivante explique comment modifier les jointures qui ont été créées entre la table de cas et une table imbriquée utilisées comme entrées dans une requête de prédiction, à l’aide de la boîte de dialogue **Spécifier la jointure imbriquée** .  
  
### <a name="select-an-input-table"></a>Sélectionner une table d'entrée  
  
1.  Dans la table **Sélectionner une ou plusieurs tables d’entrée** de l’onglet **Graphique d’analyse de précision de l’exploration de données** du Concepteur d’exploration de données dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur **Sélectionner la table de cas**.  
  
     La boîte de dialogue **Sélectionner une table** s’affiche pour vous permettre de sélectionner la table qui contient les données sur lesquelles vous voulez baser les requêtes.  
  
2.  Dans la boîte de dialogue **Sélectionner une table** , sélectionnez une source de données dans la liste **Source de données** .  
  
3.  Sous **Nom de la table/vue**, sélectionnez la table qui contient les données à utiliser pour tester les modèles.  
  
4.  Cliquez sur **OK**.  
  
     Les colonnes de la structure d'exploration de données sont mappées automatiquement aux colonnes portant le même nom dans la table d'entrée.  
  
### <a name="change-the-way-that-input-data-is-mapped-to-the-model"></a>Modifier la façon dont les données d'entrée sont mappées au modèle  
  
1.  Dans le Concepteur d’exploration de données de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], sélectionnez l’onglet **Prédiction de modèle d’exploration de données** .  
  
2.  Dans le menu **Modèle d’exploration de données** , sélectionnez **Modifier les connexions**.  
  
     La boîte de dialogue **Modifier le mappage** s’ouvre. Dans cette boîte de dialogue, la colonne **Colonne du modèle d’exploration de données** répertorie les colonnes dans la structure d’exploration de données sélectionnée. La colonne **Colonne de table** répertorie les colonnes dans la source de données externe que vous avez choisie dans la boîte de dialogue **Sélectionner une ou plusieurs tables d’entrée** . Les colonnes de la source de données externe sont mappées aux colonnes du modèle d'exploration de données.  
  
3.  Sous **Colonne de table**, sélectionnez la ligne correspondante à la colonne du modèle d’exploration de données à mapper.  
  
4.  Sélectionnez une nouvelle colonne dans la liste de colonnes disponibles de la source de données externe. Sélectionnez l'élément vide dans la liste pour supprimer le mappage de colonnes.  
  
5.  Cliquez sur **OK**.  
  
     Les nouveaux mappages de colonnes s'affichent dans le Concepteur.  
  
### <a name="remove-a-relationship-between-input-tables"></a>Supprimer une relation entre des tables d'entrée  
  
1.  Dans la table **Sélectionner une ou plusieurs tables d’entrée** de l’onglet **Prédiction de modèle d’exploration de données** du Concepteur d’exploration de données de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cliquez sur **Modifier la jointure**.  
  
     La boîte de dialogue **Spécifier la jointure imbriquée** s’ouvre.  
  
2.  Sélectionnez une relation.  
  
3.  Cliquez sur **Supprimer la relation**.  
  
4.  Cliquez sur **OK**.  
  
     La relation entre la table de cas et la table imbriquée est supprimée.  
  
### <a name="create-a-new-relationship-between-input-tables"></a>Créer une relation entre des tables d'entrée  
  
1.  Dans la table **Sélectionner une ou plusieurs tables d’entrée** de l’onglet **Prédiction de modèle d’exploration de données** du Concepteur d’exploration de données, cliquez sur **Modifier la jointure**.  
  
     La boîte de dialogue **Spécifier la jointure imbriquée** s’ouvre.  
  
2.  Cliquez sur **Ajouter une relation**.  
  
     La boîte de dialogue **Créer une relation** s’ouvre.  
  
3.  Sélectionnez la clé de la table imbriquée dans **Colonnes sources**.  
  
4.  Sélectionnez la clé de la table de cas dans **Colonnes de destination**.  
  
5.  Cliquez sur **OK** dans la boîte de dialogue **Créer une relation** .  
  
6.  Cliquez sur **OK** dans la boîte de dialogue **Spécifier la jointure imbriquée** .  
  
     Une relation est créée entre la table de cas et la table imbriquée.  
  
### <a name="add-a-nested-table-to-the-input-tables-of-a-prediction-query"></a>Ajouter une table imbriquée aux tables d'entrées d'une requête de prédiction  
  
1.  Sous l’onglet **Prédiction de modèle d’exploration de données** du Concepteur d’exploration de données, cliquez sur **Sélectionner la table de cas** pour ouvrir la boîte de dialogue **Sélectionner une table** .  
  
    > [!NOTE]  
    >  Vous ne pouvez pas ajouter de table imbriquée aux entrées, sauf si vous avez spécifié une table de cas. L'utilisation d'une table imbriquée nécessite que le modèle d'exploration de données que vous utilisez pour la prédiction utilise également une table imbriquée.  
  
2.  Dans la boîte de dialogue **Sélectionner une table** , sélectionnez une source de données dans la liste **Source de données** , puis sélectionnez la table dans la vue de source de données qui contient les données de cas. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Cliquez sur **Sélectionner la table imbriquée** pour ouvrir la boîte de dialogue **Sélectionner une table** .  
  
4.  Dans la boîte de dialogue **Sélectionner une table** , sélectionnez une source de données dans la liste **Source de données** , puis sélectionnez la table dans la vue de source de données qui contient les données imbriquées. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Si une relation existe, les colonnes du modèle d'exploration de données sont mappées automatiquement aux colonnes portant le même nom dans la table d'entrée. Vous pouvez modifier la relation entre la table imbriquée et la table de cas en cliquant sur **Modifier la jointure**qui ouvre la boîte de dialogue **Créer une relation** .  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes de prédiction & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/prediction-queries-data-mining.md)  
  
  
