---
title: Mesures dans le rapport de Validation croisée | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 00a8d7d0e05d4fa4a714011e18ec1162eb7b68e7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="measures-in-the-cross-validation-report"></a>Mesures dans le rapport de validation croisée
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Pendant la validation croisée, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] divise les données d'une structure d'exploration de données en plusieurs sections croisées, puis teste de manière itérative la structure et tous les modèles d'exploration de données associés. D'après cette analyse, il produit un jeu de mesures de précision standard pour la structure et chaque modèle.  
  
 Le rapport contient certaines informations de base sur le nombre de replis dans les données et le volume de données dans chaque repli, ainsi qu'un ensemble de mesures générales qui décrivent la distribution des données. En comparant les mesures générales de chaque section croisée, vous pouvez évaluer la fiabilité de la structure ou du modèle.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] affiche également un ensemble de mesures détaillées pour les modèles d'exploration de données. Ces mesures dépendent du type de modèle et du type d'attribut qui est analysé (discret ou continu, par exemple).  
  
 Cette section fournit une liste des mesures contenues dans le rapport de **validation croisée** , ainsi que leur signification. Pour plus d’informations sur la façon dont chaque mesure est calculée, consultez [Formules de validation croisée](../../analysis-services/data-mining/cross-validation-formulas.md).  
  
## <a name="list-of-measures-in-the-cross-validation-report"></a>Liste de mesures dans le rapport de validation croisée  
 Le tableau suivant répertorie les mesures qui apparaissent dans le rapport de validation croisée. Les mesures sont regroupées par *type de test*, fourni dans la colonne gauche du tableau suivant. La colonne de droite répertorie le nom de la mesure tel qu'il apparaît dans le rapport, et fournit une brève explication de sa signification.  
  
|type de test|Mesures et descriptions|  
|---------------|-------------------------------|  
|Clustering|Mesures qui s'appliquent aux modèles de clustering|  
||**Probabilité de cas**:<br />                      Cette mesure indique habituellement la probabilité qu'un cas appartienne à un cluster particulier. Pour la validation croisée, les scores sont additionnés, puis divisés par le nombre de cas ; ici, le score est une vraisemblance moyenne de cas.|  
|classification.|Mesures qui s'appliquent aux modèles de classification|  
||**Vrai positif**/**Vrai négatif**/**Faux positif**/**Faux négatif**:<br /><br /> Nombre de lignes ou valeurs dans la partition où l'état prédit correspond à l'état cible et où la probabilité prédite est supérieure au seuil spécifié.<br /><br /> Les cas qui ont des valeurs manquantes pour l'attribut cible sont exclus, ce qui signifie que le nombre de toutes les valeurs peut ne pas s'additionner.|  
||**Succès/échec**:<br />                      Nombre de lignes ou valeurs dans la partition où l'état prédit correspond à l'état cible, et où le probabilité prédite est supérieure à 0.|  
|Vraisemblance|Les mesures de vraisemblance s'appliquent à plusieurs types de modèle.|  
||**Élévation**:<br />                      Rapport entre la probabilité de prédiction réelle et la probabilité marginale dans les scénarios de test. Les lignes qui ont des valeurs manquantes pour l'attribut cible sont exclues.<br /><br /> Cette mesure affiche généralement le degré d'amélioration de la probabilité des résultats cibles lorsque le modèle est utilisé.|  
||**Racine carrée de l’erreur moyenne**:<br />                      Racine carrée de l'erreur moyenne pour tous les cas de la partition, divisée par le nombre de cas dans la partition, en excluant les cas avec des valeurs manquantes pour l'attribut cible.<br /><br /> RMSE est un estimateur souvent utilisé pour les modèles prévisionnels. Le score calcule la moyenne des résiduels pour chaque cas et génère un seul indicateur de l'erreur modèle.|  
||**Score du journal**:<br />                      Logarithme des valeurs de probabilité réelle pour chaque cas, additionnées, puis divisées par le nombre de lignes dans le dataset d'entrée, en excluant les cas avec des valeurs manquantes pour l'attribut cible.<br /><br /> Étant donné que la probabilité est représentée comme une fraction décimale, les scores du journal sont toujours un nombre négatif. Un nombre plus proche de 0 représente un meilleur score. Alors que les scores bruts peuvent avoir des distributions très irrégulières ou asymétriques, un score de journal est semblable à un pourcentage.|  
|Estimation|Mesures qui s'appliquent uniquement aux modèles d'évaluation, qui prédisent un attribut numérique continu.|  
||**Racine carrée de l’erreur moyenne**:<br />                      Erreur moyenne lorsque la valeur prédite est comparée à la valeur réelle.<br /><br /> RMSE est un estimateur souvent utilisé pour les modèles prévisionnels. Le score calcule la moyenne des résiduels pour chaque cas et génère un seul indicateur de l'erreur modèle.|  
||**Erreur d’absolue moyenne**:<br />                      Erreur moyenne lorsque les valeurs prédites sont comparées aux valeurs réelles, calculées sous la forme d'une moyenne de la somme absolue des erreurs.<br /><br /> L'erreur absolue moyenne est utile pour comprendre la justesse des prédictions par rapport aux valeurs réelles. Un faible score signifie que les prédictions étaient plus précises.|  
||**Log Score**:<br />                      Logarithme des valeurs de probabilité réelle pour chaque cas, additionnées, puis divisées par le nombre de lignes dans le dataset d'entrée, en excluant les cas avec des valeurs manquantes pour l'attribut cible.<br /><br /> Étant donné que la probabilité est représentée comme une fraction décimale, les scores du journal sont toujours un nombre négatif. Un nombre plus proche de 0 représente un meilleur score. Alors que les scores bruts peuvent avoir des distributions très irrégulières ou asymétriques, un score de journal est semblable à un pourcentage.|  
|Agrégats|Les mesures d'agrégat fournissent une indication de la variation des résultats pour chaque partition.|  
||**Moyenne**:<br />                      Moyenne des valeurs de partition pour une mesure donnée.|  
||**Écart type**:<br />                      Moyenne de l'écart par rapport à la moyenne d'une mesure spécifique, sur toutes les partitions d'un modèle.<br /><br /> Pour la validation croisée, une valeur plus élevée pour ce score implique une variation substantielle entre les replis.|  
  
## <a name="see-also"></a>Voir aussi  
 [Test et Validation & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-data-mining.md)  
  
  
