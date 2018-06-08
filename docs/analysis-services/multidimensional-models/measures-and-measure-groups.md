---
title: Mesures et groupes de mesures | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 04eb5a41bec6e9abb62cfde516f2dad2ff820521
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="measures-and-measure-groups"></a>Mesures et groupes de mesures
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un cube inclut des *mesures* contenues dans des *groupes de mesures*, une logique métier, ainsi qu'une collection de dimensions qui donnent le contexte d'évaluation des données numériques fournies par une mesure. Les mesures et les groupes de mesures constituent chacun un composant essentiel d'un cube. Un cube ne peut pas exister sans au moins un de chacun de ces composants.  
  
 Cette rubrique décrit les [Measures](#bkmk_measure) et les [Measure Groups](#bkmk_mg). Elle contient également le tableau suivant, avec des liens vers des procédures de création et de configuration de mesures et de groupes de mesures.  
  
|**Lien**|**Description**|  
|--------------|---------------------|  
|[Créer des mesures et groupes de mesures dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/create-measures-and-measure-groups-in-multidimensional-models.md)|Choisissez entre les différentes approches permettant de créer des mesures et des groupes de mesures.|  
|[Configurer les propriétés de mesure](../../analysis-services/multidimensional-models/configure-measure-properties.md)|Si vous avez utilisé l'Assistant Cube pour entreprendre la création de votre cube, vous devrez peut-être modifier la méthode d'agrégation, appliquer un format de données, définir la visibilité de la mesure dans les applications clientes ou éventuellement ajouter une expression de mesure pour manipuler les données avant que les valeurs soient agrégées.|  
|[Configurer les propriétés du groupe de mesures](../../analysis-services/multidimensional-models/configure-measure-group-properties.md)|Dans un modèle multidimensionnel, un groupe de mesures équivaut à une table de faits de l'entrepôt de données source. Les propriétés définies sur un groupe de mesures vous permettent de spécifier des comportements de mise en cache, le stockage et les directives de traitement qui fonctionnent collectivement au niveau du groupe de mesures. La configuration de partitions est en partie déterminée par les propriétés définies sur les objets groupes de mesures.|  
|[Utilisez les fonctions d’agrégation](../../analysis-services/multidimensional-models/use-aggregate-functions.md)|Découvrez les méthodes d'agrégation qui peuvent être affectées à une mesure.|  
|[Définir le comportement semi-additif](../../analysis-services/multidimensional-models/define-semiadditive-behavior.md)|Le comportement semi-additif fait référence aux agrégations qui sont valides pour certaines dimensions, mais pas pour d'autres. Un solde de compte bancaire en est un exemple courant. Vous pouvez agréger des soldes par client et par région, mais pas sur une période donnée. Par exemple, il n'est pas souhaitable d'ajouter des soldes du même compte sur plusieurs jours consécutifs. Pour définir un comportement semi-additif, utilisez l'Assistant Ajouter Business Intelligence.|  
|[Groupes de mesures liés](../../analysis-services/multidimensional-models/linked-measure-groups.md)|Réaffectez un groupe de mesures existant à d'autres cubes de la même base de données ou à des bases de données Analysis Services différentes.|  
  
##  <a name="bkmk_measure"></a> Measures  
 Une mesure représente une colonne qui contient des données, généralement numériques, quantifiables et agrégeables. Les mesures représentent certains aspects de l'activité d'organisation, exprimés en termes monétaires (par exemple, le chiffre d'affaires, les marges ou les coûts), sous forme de nombres (niveaux de stock ou nombre d'employés, de clients ou de commandes) ou sous la forme d'un calcul plus complexe qui intègre la logique métier.  
  
 Chaque cube doit posséder au moins une mesure, mais la plupart en ont plusieurs, parfois même des centaines. D'un point de vue structurel, une mesure est souvent mappée à une colonne source d'une table de faits, cette colonne fournissant les valeurs utilisées pour charger la mesure. Vous pouvez également définir une mesure à l'aide de MDX.  
  
 Les mesures sont contextuelles et s'appliquent à des données numériques dans un contexte qui est déterminé par tout membre de dimension inclus dans la requête. Par exemple, une mesure qui calcule les ventes du revendeur ( **Reseller Sales** ) est associée à un opérateur **Sum** et ajoute les montants des ventes pour chaque membre de dimension inclus dans la requête. Que la requête spécifie des produits individuels, qu'elle effectue un classement dans une catégorie ou qu'elle soit segmentée par tranches horaires ou par situation géographique, la mesure doit produire une opération qui est valide pour les dimensions incluses dans la requête.  
  
 Dans cet exemple, **Reseller Sales** est agrégé en différents niveaux dans la hiérarchie **Sales Territory** .  
  
 ![Tableau croisé dynamique avec des mesures et dimensions appelées](../../analysis-services/multidimensional-models/media/ssas-keyconcepts-pivot1-measures-dimensions.png "tableau croisé dynamique avec des mesures et dimensions appelées")  
  
 Les mesures produisent des résultats valides lorsque la table de faits qui contient les données numériques sources contient également des pointeurs vers des tables de dimension utilisées dans la requête. Avec l'exemple Reseller Sales, si chaque ligne qui stocke un montant de ventes stocke également un pointeur vers une table de produits, une table de dates ou une table de secteurs de vente, les requêtes qui incluent des membres de ces dimensions sont correctement résolues.  
  
 Que se passe-t-il si la mesure n'est pas associée aux dimensions utilisées dans la requête ? En général, Analysis Services affiche la mesure par défaut, et la valeur est la même pour tous les membres. Dans cet exemple, les ventes sur Internet ( **Internet Sales**), qui mesurent la vente directe correspondant aux commandes passées par les clients à l’aide du catalogue en ligne, n’ont aucune relation avec l’organisation commerciale.  
  
 ![Valeurs de mesure de tableau croisé dynamique affichant répétées](../../analysis-services/multidimensional-models/media/ssas-unrelatedmeasure.PNG "tableau croisé dynamique affichant répétées des valeurs de mesure")  
  
 Pour réduire les risques de rencontrer ces comportements dans une application cliente, vous pouvez créer plusieurs cubes ou perspectives dans la même base de données, et vous assurer que chaque cube ou perspective contient uniquement les objets associés. Les relations que vous devez vérifier sont celles entre le groupe de mesures (mappé à la table de faits) et les dimensions.  
  
##  <a name="bkmk_mg"></a> Measure Groups  
 Dans un cube, les mesures sont groupées par leurs tables de faits sous-jacentes en groupes de mesures. Les groupes de mesures sont utilisés pour associer des dimensions à des mesures. Les groupes de mesures sont aussi utilisés pour les mesures dont le comportement d'agrégation est de type comptage de valeurs. En effet, le fait de placer chaque mesure de comptage de valeurs dans son propre groupe de mesures optimise le traitement des agrégations.  
  
 Un objet <xref:Microsoft.AnalysisServices.MeasureGroup> simple se compose d’informations de base telles que le nom du groupe, le mode de stockage et le mode de traitement. Il contient également les éléments qui le constituent : les mesures, les dimensions et les partitions qui forment la composition du groupe de mesures.  
  
## <a name="see-also"></a>Voir aussi  
 [Cubes dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/cubes-in-multidimensional-models.md)   
 [Créer des mesures et groupes de mesures dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/create-measures-and-measure-groups-in-multidimensional-models.md)  
  
  
