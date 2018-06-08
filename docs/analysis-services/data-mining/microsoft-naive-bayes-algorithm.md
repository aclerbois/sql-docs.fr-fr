---
title: L’algorithme Microsoft Naive Bayes | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b3cbe50437011bc97ba4f4e1e246ee85e89495c1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-naive-bayes-algorithm"></a>Algorithme MNB (Microsoft Naive Bayes)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’algorithme MNB ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes) est un algorithme de classification basé sur les théorèmes de Bayes. Vous pouvez l’utiliser pour une modélisation exploratoire et prédictive. Le terme Naïve dans le nom Naïve Bayes est dérivé du fait que l'algorithme utilise des techniques bayésiennes, mais ne prend pas en compte les dépendances qui peuvent exister.  
  
 Cet algorithme est informatiquement moins lourd que d’autres algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Ainsi, il est utile pour générer rapidement des modèles d’exploration de données permettant de découvrir les relations entre les colonnes d’entrée et les colonnes prédictibles. Vous pouvez utiliser cet algorithme pour effectuer l'exploration initiale de données et appliquer ensuite les résultats pour créer des modèles d'exploration de données supplémentaires avec d'autres algorithmes qui sont informatiquement plus lourds et plus précis.  
  
## <a name="example"></a>Exemple  
 Dans le cadre d'une stratégie promotionnelle continue, le service marketing de la société Adventure Works Cycle a décidé de cibler les clients potentiels en envoyant des prospectus. Afin de réduire les coûts de la campagne, ils ne veulent envoyer des prospectus qu'aux clients susceptibles de répondre. La société stocke des informations dans une base de données sur des statistiques démographiques et la réponse à un publipostage antérieur. Ils souhaitent utiliser ces données pour déterminer si les statistiques démographiques, telles que l'âge et la situation géographique, peuvent permettre de prédire la réponse à une promotion, en comparant les clients potentiels aux clients existants qui présentent des caractéristiques similaires. Plus particulièrement, ils veulent déterminer les différences entre les clients ayant acheté un vélo et ceux qui n'en ont pas acheté.  
  
 En utilisant l'algorithme MNB ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes), le service marketing peut rapidement prédire un résultat pour un profil de client spécifique et peut ainsi déterminer quels clients sont les plus susceptibles de répondre aux prospectus. En utilisant la Visionneuse de l’algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], le service marketing peut aussi identifier visuellement les colonnes d’entrée contribuant aux réponses positives aux prospectus.  
  
## <a name="how-the-algorithm-works"></a>Fonctionnement de l'algorithme  
 L’algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes calcule la probabilité de tous les états de chaque colonne d’entrée, en fonction de chaque état possible de la colonne prédictible.  
  
 Pour comprendre le fonctionnement, utilisez la Visionneuse de l'algorithme MNB ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes) dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] (comme illustré dans le graphique suivant) pour explorer visuellement la manière dont l'algorithme distribue les états.  
  
 ![Distribution Naive bayes des états](../../analysis-services/data-mining/media/naive-bayes.gif "distribution Naive bayes des États")  
  
 Dans ce cas, la Visionneuse de l’algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes répertorie chacune des colonnes d’entrée du dataset et montre comment les états de chaque colonne sont distribués, en fonction de chaque état de la colonne prédictible.  
  
 Vous utiliserez cette vue du modèle pour identifier les colonnes d'entrée qui jouent un rôle important dans la différenciation des états de la colonne prédictible.  
  
 Par exemple, dans la ligne de Commute Distance illustrée ici, la distribution des valeurs d’entrée est visiblement différente pour les acheteurs par rapport aux non-acheteurs. Cela vous indique que l'entrée, Commute Distance = 0-1 mile, est un prédicteur potentiel.  
  
 La visionneuse fournit également des valeurs pour les distributions. Vous pouvez ainsi voir que pour les clients qui effectuent un trajet de un à deux miles pour aller au travail, la probabilité qu’ils achètent un vélo est de 0,387, tandis que la probabilité qu’ils n’en achètent pas est de 0,287. Dans cet exemple, l'algorithme utilise les données numériques, provenant des caractéristiques du client (telles que la distance domicile-travail), pour prédire si un client va ou non acheter un vélo.  
  
 Pour plus d’informations sur l’utilisation de la Visionneuse de l’algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes, consultez [Explorer un modèle à l’aide de la visionneuse de l’algorithme MNB (Microsoft Naive Bayes)](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-naive-bayes-viewer.md).  
  
## <a name="data-required-for-naive-bayes-models"></a>Données requises pour les modèles Naive Bayes  
 Lorsque vous préparez des données à utiliser dans l’apprentissage d'un modèle Naive Bayes, vous devez vous familiariser avec les spécifications liées à l'algorithme, y compris la quantité de données requise et leur mode d'utilisation.  
  
 Les spécifications liées à un modèle Naive Bayes se présentent comme suit :  
  
-   **Colonne à index unique** Chaque modèle doit contenir une colonne numérique ou une colonne de texte qui identifie de façon unique chaque enregistrement. Les clés composées ne sont pas autorisées.  
  
-   **Colonnes d’entrée** Dans un modèle Naive Bayes, toutes les colonnes doivent être discrètes ou discrétisées. Pour plus d’informations sur la façon de discrétiser les colonnes, consultez [Méthodes de discrétisation &#40;exploration de données&#41;](../../analysis-services/data-mining/discretization-methods-data-mining.md).  
  
-   **Les variables doivent être indépendantes.** Pour un modèle Naive Bayes, il est également important de garantir que les attributs d'entrée sont indépendants les uns des autres. Cela est particulièrement important lorsque vous utilisez le modèle pour effectuer une prédiction. Si vous utilisez deux colonnes de données qui sont déjà étroitement liées, l’effet est de multiplier l’influence de ces colonnes, qui peuvent masquer d’autres facteurs qui influencent les résultats.  
  
     Inversement, la capacité de l'algorithme à identifier les corrélations entre les variables est utile lorsque vous explorez un modèle ou un dataset, pour d'identifier les relations entre des entrées.  
  
-   **Au moins une colonne prévisible** L’attribut prédictible doit contenir des valeurs discrètes ou discrétisées.  
  
     Les valeurs de la colonne prédictible peuvent être traitées comme entrées. Cette approche peut être utile lorsque vous explorez un nouveau dataset, afin de rechercher des relations entre les colonnes.  
  
## <a name="viewing-the-model"></a>Affichage du modèle  
 Pour explorer le modèle, vous pouvez utiliser la **Visionneuse de l’algorithme MNB (Microsoft Naive Bayes)**. Elle illustre la manière dont les attributs d’entrée sont liés à l’attribut prédictible. Elle présente également un profil détaillé de chaque cluster, une liste des attributs qui permettent de distinguer les clusters les uns des autres, ainsi que les caractéristiques du jeu de données d'apprentissage complet. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse de l’algorithme MNB (Microsoft Naive Bayes)](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-naive-bayes-viewer.md).  
  
 Si vous voulez en savoir plus, vous pouvez parcourir le modèle dans la [Visionneuse de l’arborescence de contenu générique Microsoft &#40;exploration de données&#41;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c). Pour plus d’informations sur le type d’informations stockées dans le modèle, consultez [Contenu du modèle d’exploration de données pour les modèles Naive Bayes &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-naive-bayes-models-analysis-services-data-mining.md).  
  
## <a name="making-predictions"></a>Élaboration de prédictions  
 Après l'apprentissage d'un modèle, les résultats sont stockés sous la forme d'un jeu de modèles que vous pouvez explorer ou utiliser pour effectuer des prédictions.  
  
 Vous pouvez créer des requêtes pour obtenir des prédictions sur la manière dont les nouvelles données sont liées à l'attribut prédictible, ou vous pouvez extraire des statistiques qui décrivent les corrélations recherchées par le modèle.  
  
 Pour plus d’informations sur la façon de créer des requêtes sur un modèle d’exploration de données, consultez [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md). Pour obtenir des exemples montrant comment utiliser des requêtes avec un modèle Naive Bayes, consultez [Exemples de requêtes de modèle Naive Bayes](../../analysis-services/data-mining/naive-bayes-model-query-examples.md).  
  
## <a name="remarks"></a>Notes  
  
-   Prend en charge l'utilisation du langage PMML (Predictive Model Markup Language) pour créer des modèles d'exploration de données.  
  
-   Prend en charge l’extraction.  
  
-   Ne prend pas en charge la création de dimensions d’exploration de données.  
  
-   Prend en charge l'utilisation de modèles d'exploration de données OLAP.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Sélection des fonctionnalités & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/feature-selection-data-mining.md)   
 [Exemples de requêtes de modèle Naive Bayes](../../analysis-services/data-mining/naive-bayes-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour les modèles Naive Bayes & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-naive-bayes-models-analysis-services-data-mining.md)   
 [Référence technique de Microsoft Naive Bayes algorithme](../../analysis-services/data-mining/microsoft-naive-bayes-algorithm-technical-reference.md)  
  
  
