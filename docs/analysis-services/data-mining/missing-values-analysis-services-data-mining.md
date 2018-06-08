---
title: Les valeurs manquantes (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 662fdd55fc5929fe56734b9894bf971962ff2a7b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="missing-values-analysis-services---data-mining"></a>Valeurs manquantes (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Gérer les  *valeurs manquantes* correctement fait partie intégrante d’une modélisation efficace. Cette section explique ce que sont les valeurs manquantes et décrit les fonctionnalités fournies dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour gérer les valeurs manquantes lors de la génération de structures et de modèles d'exploration de données.  
  
## <a name="definition-of-missing-values-in-data-mining"></a>Définition des valeurs manquantes dans l'exploration de données  
 Une valeur manquante peut avoir plusieurs significations. Entre autres possibilités, le champ n'était pas applicable, l'événement n'a pas eu lieu ou bien les données n'étaient pas disponibles. Il se peut que la personne qui a saisi les données ne connaissait pas la valeur exacte ou ne s'est pas inquiété de l'absence de remplissage d'un champ.  
  
 Toutefois, il existe de nombreux scénarios d'exploration de données dans lesquels des valeurs manquantes fournissent des informations importantes. La signification des valeurs manquantes dépend principalement du contexte. Par exemple, une valeur manquante pour la date dans une liste de factures a une signification fondamentalement différente de l'absence de date dans une colonne qui indique la date d'embauche d'un employé. Généralement, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] traite les valeurs manquantes en tant qu’éléments informatifs et ajuste les probabilités pour incorporer les valeurs manquantes dans ses calculs. Ce faisant, vous pouvez vous assurer que les modèles sont équilibrés et qu'ils ne pèsent pas trop lourdement sur les cas.  
  
 Ainsi, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit deux mécanismes profondément différents pour la gestion et le calcul des valeurs manquantes. La première méthode contrôle la gestion des valeurs Null au niveau de la structure d'exploration de données. La deuxième méthode diffère dans l’implémentation pour chaque algorithme, mais définit généralement comment les valeurs manquantes sont traitées et comptabilisées dans les modèles qui autorisent les valeurs Null.  
  
## <a name="specifying-handling-of-nulls"></a>Spécification de la gestion des valeurs Null  
 Dans votre source de données, les valeurs manquantes peuvent être représentées de plusieurs manières : sous la forme de valeurs Null, comme cellules vides dans une feuille de calcul, comme valeur N/A ou tout autre code, ou encore en tant que valeur artificielle, par exemple 9999. Toutefois, dans le contexte de l'exploration de données, seules les valeurs Null sont considérées comme des valeurs manquantes. Si vos données contiennent des valeurs d'espace réservé au lieu de valeurs Null, elles peuvent affecter les résultats du modèle ; vous devez donc les remplacer par des valeurs Null ou déduire les valeurs correctes si possible. Divers outils sont à votre disposition pour déduire et fournir les valeurs adéquates, notamment la transformation de recherche et la tâche du profileur de données dans SQL Server Integration Services, ou bien l'outil Remplir à partir de l'exemple fourni dans les compléments d'exploration de données pour Excel.  
  
 Si la tâche que vous modélisez spécifie qu’une colonne ne doit jamais comporter de valeurs manquantes, vous devez appliquer l’indicateur de modélisation **NOT_NULL** à cette colonne quand vous définissez la structure d’exploration de données. Cet indicateur signale que le traitement doit échouer si un cas ne possède pas de valeur adéquate. Si cette erreur se produit lors du traitement d'un modèle, vous pouvez enregistrer l'erreur et prendre des mesures pour corriger les données fournies au modèle.  
  
## <a name="calculation-of-the-missing-state"></a>Calcul de l'état manquant  
 Pour l'algorithme d'exploration de données, les valeurs manquantes sont des éléments informatifs. Dans les tables de cas, **Manquant** est un état valide comme tout autre état. De plus, un modèle d'exploration de données peut utiliser d'autres valeurs pour prédire si une valeur est manquante. En d'autres termes, le fait qu'une valeur soit manquante n'est pas considéré comme une erreur.  
  
 Quand vous créez un modèle d’exploration, un état **Manquant** est ajouté automatiquement au modèle pour toutes les colonnes discrètes. Par exemple, si la colonne d’entrée [Sexe] contient deux valeurs possibles, Homme et Femme, une troisième valeur est ajoutée automatiquement pour représenter la valeur **Manquant** , et l’histogramme qui affiche la distribution de toutes les valeurs pour la colonne indique toujours le nombre de cas avec des valeurs **Manquant** . S'il ne manque aucune valeur dans la colonne Sexe, l'histogramme indique que l'état manquant est présent dans 0 cas.  
  
 Le raisonnement derrière l’inclusion par défaut de l’état **Manquant** se justifie par le fait que vos données peuvent ne pas comporter des exemples de toutes les valeurs possibles. En effet, il est illogique qu’un modèle exclue une possibilité uniquement sur la base de l’absence d’un exemple dans les données. Par exemple, si les données de ventes d'un magasin indiquent que tous les clients ayant acheté un produit particulier sont des femmes, vous n'allez pas créer un modèle prédisant que seules les femmes sont susceptibles d'acheter le produit. À la place, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ajoute un espace réservé pour la valeur inconnue supplémentaire, appelé **Manquant**, comme moyen de prendre en compte les autres états possibles.  
  
 Par exemple, le tableau suivant indique la distribution des valeurs pour le nœud (All) dans le modèle d'arbre de décision créé pour le didacticiel Bike Buyer. Dans le scénario de l'exemple, la colonne [Bike Buyer] est l'attribut prédictible, où 1 indique « Oui » et 0 « Non ».  
  
|Valeur|Cas|  
|-----------|-----------|  
|0|9296|  
|1|9098|  
|Manquant|0|  
  
 Cette distribution montre qu'environ la moitié des clients a acheté un vélo et que l'autre moitié n'en n'a pas acheté. Ce jeu de données particulier est très propre ; ainsi, chaque cas a une valeur dans la colonne [Bike Buyer] et le nombre de valeurs **Manquant** est de 0. Toutefois, si un cas comporte une valeur NULL dans la colonne [Bike Buyer], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] compte cette ligne comme un cas avec une valeur **Manquant** .  
  
 Si l’entrée est une colonne continue, le modèle tabule deux états possibles pour l’attribut : **Existant** et **Manquant**. En d'autres termes, soit la colonne contient une valeur d'un type de données numérique, soit elle ne contient aucune valeur. Pour les cas qui possèdent une valeur, le modèle calcule la moyenne, l'écart type et d'autres statistiques significatives. Pour les cas qui n’ont aucune valeur, le modèle indique le nombre des valeurs **Manquant** et ajuste les prédictions en conséquence. La méthode d'ajustement de la prédiction, décrite dans la section suivante, diffère selon l'algorithme.  
  
> [!NOTE]  
>  Pour les attributs dans une table imbriquée, les valeurs manquantes ne constituent pas des éléments informatifs. Par exemple, si un client n’a pas acheté de produit, la table **Products** imbriquée ne contient pas de ligne correspondant à ce produit, et le modèle d’exploration de données ne crée pas d’attribut pour le produit manquant. Toutefois, si vous vous intéressez aux clients qui n'ont pas acheté certains produits, vous pouvez créer un modèle filtré d'après l'inexistence des produits dans la table imbriquée, en utilisant une instruction NOT EXISTS dans le filtre de modèle. Pour plus d’informations, consultez [Appliquer un filtre à un modèle d’exploration de données](../../analysis-services/data-mining/apply-a-filter-to-a-mining-model.md).  
  
## <a name="adjusting-probability-for-missing-states"></a>Ajustement de la probabilité pour les états manquants  
 Outre le dénombrement des valeurs, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] calcule la probabilité d'une valeur quelconque dans le jeu de données. Cela vaut également pour la valeur **Manquant** . Par exemple, le tableau suivant montre les probabilités pour les cas dans l’exemple précédent :  
  
|Valeur|Cas|Probabilité|  
|-----------|-----------|-----------------|  
|0|9296|50.55%|  
|1|9098|49.42%|  
|Manquant|0|0.03%|  
  
 Il peut paraître étrange que la probabilité de la valeur **Manquant** soit de 0,03 % avec un nombre de cas égal à 0. En fait, ce comportement est normal et représente un ajustement qui permet au modèle de gérer les valeurs inconnues correctement.  
  
 En général, la probabilité correspond au nombre de cas favorables divisé par tous les cas possibles. Dans cet exemple, l'algorithme calcule la somme des cas qui remplissent une condition particulière ([Bike Buyer] = 1 ou [Bike Buyer] = 0), puis divise ce nombre par le nombre total de lignes. Toutefois, pour tenir compte des cas **Manquant** , la valeur 1 est ajoutée au nombre de cas possibles. Ainsi, la probabilité pour le cas inconnu n'est plus nulle, mais un nombre très petit indiquant que l'état est seulement improbable, mais pas impossible.  
  
 L’ajout de la petite valeur **Manquant** ne change pas le résultat du prédicteur ; cela permet toutefois une meilleure modélisation dans les scénarios où les données historiques n’incluent pas tous les résultats possibles.  
  
> [!NOTE]  
>  Les fournisseurs d'exploration de données diffèrent par la manière dont ils gèrent les valeurs manquantes. Par exemple, certains fournisseurs supposent que les données manquantes dans une colonne imbriquée constituent une représentation incomplète, mais que les données manquantes dans une colonne non imbriquée manquent de façon aléatoire.  
  
 Si vous êtes certain que tous les résultats sont spécifiés dans vos données et que vous souhaitez empêcher l'ajustement des probabilités, vous devez définir l'indicateur de modélisation NOT_NULL sur la colonne dans la structure d'exploration de données.  
  
> [!NOTE]  
>  Chaque algorithme, notamment les algorithmes personnalisés que vous avez pu obtenir d’un plug-in tiers, peut gérer les valeurs manquantes différemment.  
  
### <a name="special-handling-of-missing-values-in-decision-tree-models"></a>Gestion spéciale des valeurs manquantes dans les modèles d'arbre de décision  
 L'algorithme MDT (Microsoft Decision Trees) ne calcule pas les probabilités pour les valeurs manquantes de la même façon que les autres algorithmes. Au lieu d’ajouter simplement la valeur 1 au nombre total de cas, l’algorithme d’arbres de décision prend en compte l’état **Manquant** à l’aide d’une formule légèrement différente.  
  
 Dans un modèle d’arbres de décision, la probabilité de l’état **Manquant** est calculé comme suit :  
  
 StateProbability = (NodePriorProbability) * (StateSupport + 1) / (NodeSupport + TotalStates)  
  
L’algorithme MDT fournit un ajustement supplémentaire qui permet à l’algorithme de compenser la présence de filtres sur le modèle, ce qui peut entraîner de nombreux États à exclure pendant l’apprentissage.  
  
 Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], si un état est présent pendant l’apprentissage mais que sa prise en charge est de zéro dans un certain nœud, l’ajustement standard est effectué. Cependant, si un état ne survient jamais au cours de l'apprentissage, l'algorithme définit la probabilité à zéro. Cet ajustement s’applique non seulement à l’état **Manquant** , mais aussi aux autres états qui existent dans les données d’apprentissage et qui ont une prise en charge de zéro à la suite du filtrage de modèle.  
  
 Cet ajustement supplémentaire se traduit par la formule suivante :  
  
 StateProbability = 0.0 si cet état a une prise en charge de 0.0 dans le jeu d’apprentissage  
  
 ELSE StateProbability = (NodePriorProbability) * (StateSupport + 1) / (NodeSupport + TotalStatesWithNonZeroSupport)  
  
 L'effet net de cet ajustement est de maintenir la stabilité de l'arbre.  
  
## <a name="related-tasks"></a>Tâches associées  
 Les rubriques suivantes fournissent des informations supplémentaires sur la manière de gérer les valeurs manquantes.  
  
|Tâches|Liens|  
|-----------|-----------|  
|Ajouter des indicateurs à différentes colonnes du modèle pour contrôler la gestion des valeurs manquantes|[Afficher ou modifier la modélisation des indicateurs & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/view-or-change-modeling-flags-data-mining.md)|  
|Définir des propriétés sur un modèle d'exploration de données pour contrôler la gestion des valeurs manquantes|[Modifier les propriétés d’un modèle d’exploration de données](../../analysis-services/data-mining/change-the-properties-of-a-mining-model.md)|  
|Découvrir comment spécifier des indicateurs de modélisation dans DMX|[Indicateurs de modélisation & #40 ; DMX & #41 ;](../../dmx/modeling-flags-dmx.md)|  
|Modifier la façon dont la structure d'exploration de données gère les valeurs manquantes|[Modifier les propriétés d’une Structure d’exploration de données](../../analysis-services/data-mining/change-the-properties-of-a-mining-structure.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Contenu du modèle d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md)   
 [Modélisation des indicateurs & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/modeling-flags-data-mining.md)  
  
  
