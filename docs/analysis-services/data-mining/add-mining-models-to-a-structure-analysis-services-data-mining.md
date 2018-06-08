---
title: Ajouter des modèles d’exploration de données à une Structure (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c32140e639a0e79b8736036392104f593a305b30
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="add-mining-models-to-a-structure-analysis-services---data-mining"></a>Ajouter des modèles d'exploration de données à une structure (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une structure d'exploration de données est destinée à prendre en charge plusieurs modèles d'exploration de données. Par conséquent, une fois l'exécution de l'Assistant terminée, vous pouvez ouvrir la structure et ajouter de nouveaux modèles d'exploration de données. Chaque fois que vous créez un modèle, vous pouvez utiliser un algorithme différent, modifier les paramètres, ou appliquer des filtres pour utiliser un autre sous-ensemble des données.  
  
## <a name="adding-new-mining-models"></a>Ajout de nouveaux modèles d'exploration de données  
 Lorsque vous utilisez l'Assistant Exploration de données pour créer un nouveau modèle d'exploration de données, par défaut, vous devez toujours d'abord créer une structure d'exploration de données. L'Assistant vous donne ensuite la possibilité d'ajouter un modèle d'exploration de données initial à la structure. Toutefois, vous n'êtes pas obligé de créer un modèle immédiatement après. Si vous créez uniquement la structure, vous n'avez pas besoin de prendre de décision concernant la colonne à utiliser comme attribut prédictible ou la façon d'utiliser les données dans un modèle particulier. Il vous suffit de définir la structure de données générale que vous souhaitez utiliser ultérieurement. Par la suite, vous pouvez utiliser le [Concepteur d’exploration de données](../../analysis-services/data-mining/data-mining-designer.md) pour ajouter de nouveaux modèles d’exploration de données basés sur la structure.  
  
> [!NOTE]  
>  Dans DMX, l'instruction CREATE MINING MODEL commence par le modèle d'exploration de données. Autrement dit, vous définissez votre choix de modèle d'exploration de données, et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère automatiquement la structure sous-jacente. Par la suite, vous pouvez continuer à ajouter de nouveaux modèles d’exploration de données à cette structure à l’aide de l’instruction ALTER STRUCTURE… ADD MODEL.  
  
## <a name="choosing-an-algorithm"></a>Choix d'un algorithme  
 Lorsque vous ajoutez un nouveau modèle à une structure existante, la première chose à faire est de sélectionner un algorithme d'exploration de données à utiliser dans ce modèle. Le choix de l'algorithme est important car chaque algorithme effectue un type d'analyse différent et a des exigences différentes.  
  
 Lorsque vous sélectionnez un algorithme qui est incompatible avec vos données, un avertissement s'affiche. Dans certains cas, vous devrez peut-être ignorer les colonnes qui ne peuvent pas être traitées par l'algorithme. Dans d'autres cas, l'algorithme effectuera automatiquement les ajustements. Par exemple, si votre structure contient des données numériques et que l'algorithme ne peut fonctionner qu'avec des valeurs discrètes, il regroupera automatiquement les valeurs numériques dans des plages discrètes. Dans certains cas, vous devrez peut-être d'abord corriger les données manuellement, en choisissant une clé ou un attribut prédictible.  
  
 Il n'est pas nécessaire de modifier l'algorithme lorsque vous créez un modèle. Bien souvent, vous pouvez obtenir des résultats très différents en utilisant le même algorithme, mais en filtrant les données, ou en modifiant un paramètre tel que la méthode de clustering ou la taille minimale du jeu d'éléments. Nous vous recommandons de faire des essais avec plusieurs modèles afin de voir quels paramètres produisent les meilleurs résultats.  
  
 Notez que tous les nouveaux modèles doivent être traités avant de pouvoir être utilisés.  
  
## <a name="specifying-the-usage-of-columns-in-a-new-mining-model"></a>Spécification de l'utilisation de colonnes dans un modèle d'exploration de données  
 Lorsque vous ajoutez de nouveaux modèles d'exploration de données à une structure d'exploration de données existante, vous devez spécifier la façon dont chaque colonne de données doit être utilisée par le modèle. Suivant le type d'algorithme que vous choisissez pour le modèle, certains de ces choix peut être effectués par défaut. Si vous ne spécifiez pas de type d'utilisation pour une colonne, celle-ci ne sera pas incluse dans la structure d'exploration de données. Toutefois, les données de la colonne peuvent encore être disponibles pour l'extraction, si le modèle prend en charge cette fonctionnalité.  
  
 Les colonnes de la structure d'exploration de données qui sont utilisées par le modèle (si la valeur Ignorer ne leur a pas été affectée) doivent être une clé, une colonne d'entrée, une colonne prédictible, ou une colonne prédictible dont les valeurs sont également utilisées en tant qu'entrées pour le modèle.  
  
-   Les colonnes clés contiennent un identificateur unique pour chaque ligne d'une table. Certains modèles d'exploration de données, tels que ceux basés sur les algorithmes MSC (Microsoft Sequence Clustering) ou MTS (Microsoft Time Series), peuvent contenir plusieurs colonnes clés. Toutefois, ces multiples clés ne sont pas des clés composées au sens relationnel ; elles doivent être sélectionnées pour prendre en charge l'analyse de séries chronologiques et Sequence Clustering.  
  
-   Les colonnes d'entrée fournissent les informations à partir desquelles les prédictions sont effectuées. L’Assistant Exploration de données fournit la fonctionnalité **Suggérer** , qui est activée quand vous sélectionnez une colonne prédictible. Si vous cliquez sur ce bouton, l'Assistant échantillonne les valeurs prédictibles et détermine les autres colonnes de la structure qui constituent de bonnes variables. Il rejette les colonnes clés ou les autres colonnes comportant de nombreuses valeurs uniques, et suggère les colonnes qui semblent être en corrélation avec le résultat.  
  
     Cette fonctionnalité est particulièrement pratique lorsque les datasets contiennent plus de colonnes que nécessaire pour générer un modèle d'exploration de données. La fonctionnalité **Suggérer** calcule un score compris entre 0 et 1, qui décrit la relation entre chaque colonne du jeu de données et la colonne prédictible. En fonction de ce score, la fonctionnalité suggère les colonnes à utiliser comme entrée pour le modèle d'exploration de données. Si vous utilisez la fonctionnalité **Suggérer** , vous pouvez utiliser les colonnes suggérées, modifier les choix pour les adapter à vos besoins ou ignorer les suggestions.  
  
-   Les colonnes prédictibles contiennent les informations que vous tentez de prévoir dans le modèle d'exploration de données. Vous pouvez sélectionner plusieurs colonnes comme attributs prédictibles. Les modèles de clustering sont l'exception car un attribut prédictible est facultatif.  
  
     Suivant le type de modèle, la colonne prédictible devra peut-être être un type de données spécifique : par exemple, un modèle de régression linéaire requiert une colonne numérique comme valeur prédite ; l'algorithme Naïve Bayes requiert une valeur discrète (et toutes les entrées doivent également être discrètes).  
  
## <a name="specifying-column-content"></a>Spécification du contenu des colonnes  
 Pour certaines colonnes, vous devrez peut-être spécifier le *contenu de colonne*. Dans l'exploration de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la propriété Type de contenu de chaque colonne de données indique à l'algorithme comment il doit traiter les données dans cette colonne. Par exemple, si vos données ont une colonne Income, vous devez spécifier que la colonne contient des nombres continus en définissant Continu comme type de contenu. Vous pouvez aussi spécifier que les nombres dans la colonne Revenu doivent être regroupés dans des compartiments en attribuant au contenu le type Discrétisé et en indiquant éventuellement le nombre exact de compartiments. Vous pouvez créer des modèles distincts qui gèrent les colonnes différemment : par exemple, vous pouvez faire des essais avec un modèle qui répartit les clients en trois groupes d'âge et un autre modèle qui répartit les clients en dix groupes d'âge.  
  
## <a name="see-also"></a>Voir aussi  
 [Les Structures d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md)   
 [Créer une Structure d’exploration de données relationnelles](../../analysis-services/data-mining/create-a-relational-mining-structure.md)   
 [Propriétés du modèle d’exploration de données](../../analysis-services/data-mining/mining-model-properties.md)   
 [Colonnes du modèle d’exploration de données](../../analysis-services/data-mining/mining-model-columns.md)  
  
  
