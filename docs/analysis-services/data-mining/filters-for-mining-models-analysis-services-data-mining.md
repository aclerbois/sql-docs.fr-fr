---
title: Filtres pour les modèles d’exploration de données (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4c678773a77b9411eb1a51dbeb85b5eeb5f08b43
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="filters-for-mining-models-analysis-services---data-mining"></a>Filtres pour les modèles d'exploration de données (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le filtrage de modèle basé sur les données est utile pour créer des modèles d'exploration de données qui utilisent les sous-ensembles de données d'une structure d'exploration de données. Le filtrage offre une certaine souplesse lorsque vous concevez vos structures d'exploration de données et sources de données, car vous pouvez créer une structure d'exploration de données unique basée sur une vue détaillée de la source de données. Vous pouvez créer ensuite des filtres pour utiliser uniquement une partie de ces données à des fins de formation et de test de divers modèles, au lieu de générer une structure différente et un modèle associé pour chaque sous-ensemble de données.  
  
 Par exemple, vous définissez la vue de la source de données sur la table Customers et les tables associées. Ensuite, vous définissez une structure d'exploration de données unique qui inclut tous les champs dont vous avez besoin. Enfin, vous créez un modèle filtré sur un attribut client particulier, tel que Region. Vous pouvez effectuer ensuite aisément une copie de ce modèle, et ne modifiez que la condition de filtre pour générer un nouveau modèle fondé sur une autre région.  
  
 Voici quelques scénarios réels où vous pourriez tirer parti de cette fonctionnalité :  
  
-   Création de modèles séparés pour les valeurs discrètes telles que le sexe, les régions, etc. Par exemple, un magasin de vêtements peut utiliser les caractéristiques démographiques de la clientèle pour générer des modèles distincts en fonction du sexe, même si les chiffres de ventes proviennent de la même source de données pour tous les clients.  
  
-   Expérimentation des modèles par la création et le test de plusieurs regroupements des mêmes données, comme les différentes tranches d'âge (20-30 ans, 20-40 ans et 20-25 ans).  
  
-   Spécification de filtres complexes sur le contenu de tables imbriquées, comme l'inclusion impérative d'un cas dans le modèle si le client a acheté au moins deux exemplaires d'un élément particulier.  
  
 Cette section explique comment générer, utiliser et gérer les filtres sur les modèles d'exploration de données.  
  
## <a name="creating-model-filters"></a>Création de filtres de modèle  
 Vous pouvez créer et appliquer des filtres de différentes façons :  
  
-   En utilisant l’onglet **Modèles d’exploration de données** du Concepteur d’exploration de données pour générer les conditions à l’aide des boîtes de dialogue de l’Éditeur de filtre.  
  
-   En tapant directement une expression de filtre dans la propriété **Filter** du modèle d’exploration de données.  
  
-   En définissant des conditions de filtrage sur un modèle par programmation, à l’aide d’AMO.  
  
### <a name="creating-model-filters-using-data-mining-designer"></a>Création des filtres de modèle à l'aide du Concepteur d'exploration de données  
 Vous filtrez un modèle dans le Concepteur d’exploration de données en modifiant la propriété **Filter** du modèle d’exploration de données. Vous pouvez taper directement une expression de filtre dans le volet **Propriétés** ou ouvrir une boîte de dialogue de filtre pour créer des conditions.  
  
 Il existe deux boîtes de dialogue de filtre. La première permet de créer des conditions appliquées à la table de cas. Si la source de données contient plusieurs tables, sélectionnez d'abord une table, puis sélectionnez une colonne et spécifiez les opérateurs et les conditions qui s'appliquent à cette colonne. Vous pouvez lier plusieurs conditions à l’aide des opérateurs **AND**/**OR** . Les opérateurs disponibles pour définir les valeurs varient selon que la colonne contient des valeurs discrètes ou continues. Par exemple, vous pouvez utiliser les opérateurs **supérieur à** et **inférieur à** avec les valeurs continues. Toutefois, pour les valeurs discrètes, vous pouvez uniquement utiliser les opérateurs **= (égal à)**, **!= (différent de)** et **IS NULL** .  
  
> [!NOTE]  
>  Le mot clé **LIKE** n’est pas pris en charge. Si vous voulez inclure plusieurs attributs discrets, vous devez créer des conditions séparées et les lier à l’aide de l’opérateur **OR** .  
  
 Si les conditions sont complexes, vous pouvez choisir la deuxième boîte de dialogue de filtre pour utiliser une table à la fois. Lorsque vous fermez la deuxième boîte de dialogue de filtre, l'expression est évaluée, puis associée aux conditions de filtrage qui ont été définies sur d'autres colonnes de la table de cas.  
  
### <a name="creating-filters-on-nested-tables"></a>Création de filtres sur les tables imbriquées  
 Si la vue de source de données contient des tables imbriquées, vous pouvez utiliser la deuxième boîte de dialogue de filtre pour créer des conditions sur les lignes des tables imbriquées.  
  
 Par exemple, si votre table de cas est liée aux clients et que la table imbriquée affiche les produits achetés par un client, vous pouvez créer un filtre pour les clients ayant acquis des articles particuliers, en utilisant la syntaxe suivante dans le filtre de table imbriquée : `[ProductName]=’Water Bottle’ OR ProductName=’Water Bottle Cage'`.  
  
 Vous pouvez également filtrer sur l’existence d’une valeur particulière de la table imbriquée en utilisant les mots clés **EXISTS** ou **NOT EXISTS** et une sous-requête. Vous pouvez ainsi créer des conditions telles que `EXISTS (SELECT * FROM Products WHERE ProductName=’Water Bottle’)`. L’instruction `EXISTS SELECT(<subquery>)` retourne **true** si la table imbriquée contient au moins une ligne incluant la valeur, `Water Bottle`.  
  
 Vous pouvez combiner les conditions de la table de cas et les conditions de la table imbriquée. Par exemple, la syntaxe suivante inclut une condition sur la table de cas (`Age > 30` ), une sous-requête sur la table imbriquée (`EXISTS (SELECT * FROM Products)`) et plusieurs conditions sur la table imbriquée (`WHERE ProductName=’Milk’  AND Quantity>2`).  
  
```  
(Age > 30 AND EXISTS (SELECT * FROM Products WHERE ProductName=’Milk’  AND Quantity>2) )  
```  
  
 Lorsque vous avez terminé de générer le filtre, le texte de filtre est analysé par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], traduit en expression DMX, puis enregistré avec le modèle.  
  
 Pour obtenir des instructions sur l’utilisation des boîtes de dialogue de filtre dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], consultez [Appliquer un filtre à un modèle d’exploration de données](../../analysis-services/data-mining/apply-a-filter-to-a-mining-model.md).  
  
## <a name="managing-mining-model-filters"></a>Gestion des filtres de modèle d'exploration de données  
 Le filtrage de modèle basé sur les données simplifie grandement la gestion des structures d'exploration de données et des modèles d'exploration de données, parce que vous pouvez créer facilement plusieurs modèles basés sur la même structure. Vous pouvez également effectuer rapidement des copies de modèles d'exploration de données existants, puis modifier uniquement la condition de filtre. Cependant, les filtres peuvent provoquer une certaine confusion.  
  
 Voici quelques questions fréquentes sur la gestion et l'interprétation des filtres dans les modèles d'exploration de données :  
  
### <a name="how-can-i-tell-whether-a-filter-is-being-used"></a>Comment puis-je savoir si un filtre est utilisé ?  
 Il existe plusieurs méthodes pour déterminer si un filtre est appliqué à un modèle :  
  
-   Dans le concepteur, cliquez sur l’onglet **Modèles d’exploration de données** , ouvrez **Propriétés**, puis consultez la propriété **Filter** du modèle d’exploration de données.  
  
-   La vue de gestion dynamique DMSCHEMA_MINING_MODELS affiche une colonne contenant le texte du filtre. Utilisez la requête suivante sur une vue de gestion dynamique pour retourner les noms des modèles et leurs filtres.  
  
    ```  
    SELECT MODEL_NAME, [FILTER]   
    FROM $SYSTEM.DMSCHEMA_MINING_MODELS  
  
    ```  
  
-   Vous pouvez obtenir la valeur de la propriété Filter de l'objet MiningModel dans AMO, ou bien consulter l'élément Filter en XMLA.  
  
 Vous pouvez également établir une convention d'affectation de noms pour que les modèles reflètent le contenu du filtre. Il est ainsi plus facile d'indiquer séparément les modèles associés.  
  
### <a name="how-can-i-save-a-filter"></a>Comment puis-je enregistrer un filtre ?  
 L'expression de filtre est enregistrée en tant que script stocké avec la table imbriquée ou le modèle d'exploration de données associé. Si vous supprimez le texte de filtre, il ne peut être restauré qu'en recréant manuellement l'expression de filtre. Par conséquent, si vous créez des expressions de filtre complexes, vous devez créer une copie de sauvegarde du texte de filtre.  
  
### <a name="why-cant-i-see-any-effects-from-the-filter"></a>Pourquoi le filtre n'a aucun effet ?  
 Chaque fois que vous changez ou ajoutez une expression de filtre, vous devez retraiter la structure et le modèle avant de pouvoir consulter les résultats du filtre.  
  
### <a name="why-do-i-see-filtered-attributes-in-prediction-query-results"></a>Pourquoi y a-t-il des attributs filtrés dans les résultats de la requête de prédiction ?  
 Lorsque vous appliquez un filtre à un modèle, il modifie uniquement la sélection des cas utilisés pour l'apprentissage du modèle. Il ne modifie pas les attributs connus sur le modèle, ni ne change ou supprime les données présentes dans la source de données. Par conséquent, les requêtes sur le modèle peuvent retourner des prédictions pour d'autres types de cas, et les listes déroulantes des valeurs utilisées par le modèle peuvent afficher des valeurs d'attribut exclues par le filtre.  
  
 Par exemple, supposez que vous effectuiez l'apprentissage du modèle [Bike Buyer] en utilisant seulement les cas qui concernent des femmes âgées de 20 à 30 ans. Vous pouvez toujours exécuter une requête de prédiction qui prédit la probabilité qu'un homme achète un vélo, ou prédire le résultat pour une femme âgée de 30 à 40 ans. La raison de cela est que les attributs et les valeurs présents dans la source de données définissent ce qui est théoriquement possible, tandis que les cas définissent les occurrences utilisées pour l'apprentissage. Toutefois, ces requêtes vont retourner des probabilités très faibles, car les données d'apprentissage ne contiennent aucun cas avec les valeurs cibles.  
  
 Si vous souhaitez masquer complètement ou rendre anonymes les valeurs d'attribut dans le modèle, plusieurs choix s'offrent à vous :  
  
-   Filtrez les données entrantes lors de la définition de la vue de source de données, ou dans la source de données relationnelle.  
  
-   Masquez ou encodez la valeur d'attribut.  
  
-   Réduisez les valeurs exclues dans une catégorie lors de la définition de la structure d'exploration de données.  
  
## <a name="related-resources"></a>Ressources connexes  
 Pour plus d’informations sur la syntaxe des filtres et obtenir des exemples d’expressions de filtre, consultez [Syntaxe de filtre de modèle et exemples &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/model-filter-syntax-and-examples-analysis-services-data-mining.md).  
  
 Pour plus d’informations sur l’utilisation des filtres de modèle lorsque vous testez un modèle d’exploration de données, consultez [Choisir un type de graphique d’analyse de précision et définir des options de graphique](../../analysis-services/data-mining/choose-an-accuracy-chart-type-and-set-chart-options.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Syntaxe de filtre de modèle et exemples &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/model-filter-syntax-and-examples-analysis-services-data-mining.md)   
 [Test et Validation & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-data-mining.md)  
  
  
