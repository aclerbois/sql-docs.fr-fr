---
title: Exploration du Cube | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d54ccdeed21f475ee90f0ec257f20c53076039ea
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-2-6---browsing-the-cube"></a>Leçon 2-6-exploration du Cube
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

Une fois le cube déployé, il est possible d'afficher les données de cube sous l'onglet **Navigateur** du Concepteur de cube et les données de dimension sous l'onglet **Navigateur** du Concepteur de dimensions. L'exploration du cube et des données de dimension permet de vérifier votre travail de façon incrémentielle. Vous pouvez vérifier que de petites modifications apportées aux propriétés, aux relations et d'autres objets ont l'effet souhaité une fois que l'objet est traité. Lorsque l'onglet Navigateur est utilisé pour afficher à la fois le cube et les données de dimension, l'onglet fournit différentes fonctions en fonction de l'objet parcouru.  
  
Pour les dimensions, l'onglet Navigateur permet d'afficher les membres ou de parcourir une hiérarchie jusqu'au nœud terminal. Vous pouvez parcourir les données de dimension dans différentes langues, en supposant que vous avez ajouté les traductions à votre modèle.  
  
Pour les cubes, l'onglet Navigateur fournit deux approches d'exploration des données. Vous pouvez utiliser le Concepteur de requêtes MDX intégré pour créer des requêtes qui renvoient un ensemble de lignes aplati à partir d'une base de données multidimensionnelle. Sinon, vous pouvez utiliser un raccourci Excel. Lorsque vous démarrez Excel à partir de [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], Excel s'ouvre avec un tableau croisé dynamique dans la feuille de calcul et une connexion prédéfinie à la base de données d'espace de travail du modèle.  
  
Excel offre généralement une meilleure navigation car vous pouvez explorer les données du cube de façon interactive, avec des axes horizontaux et verticaux pour analyser les relations entre vos données. En revanche, le Concepteur de requêtes MDX est limité à un seul axe. De plus, étant donné que l'ensemble de lignes est aplati, vous ne bénéficiez pas de la fonction d'exploration fournit par un tableau croisé dynamique Excel. Au fur et à mesure que vous ajoutez des dimensions et hiérarchies à votre cube, ce que vous ferez dans les leçons suivantes, Excel devient la solution la plus appropriée pour parcourir les données.  
  
### <a name="to-browse-the-deployed-cube"></a>Pour parcourir le cube déployé  
  
1.  Basculez vers le **Concepteur de dimensions** pour la dimension Product dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. Pour cela, double-cliquez sur la dimension **Product** du nœud **Dimensions** de l’Explorateur de solutions.  
  
2.  Cliquez sur l'onglet **Navigateur** pour afficher le membre **All** de la hiérarchie d'attribut **Product Key** . Dans la leçon 3, vous allez définir une hiérarchie utilisateur pour la dimension Product qui vous permettra de parcourir la dimension.  
  
3.  Basculez vers le **Concepteur de cube** dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]. Pour cela, double-cliquez sur le cube **Analysis Services Tutorial** dans le nœud **Cubes** de l’Explorateur de solutions.  
  
4.  Sélectionnez l'onglet **Navigateur** , puis cliquez sur l'icône **Reconnecter** dans la barre d'outils du concepteur.  
  
    Le volet gauche du concepteur affiche les objets du cube Didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . Sur le côté droit de l'onglet **Navigateur** se trouvent deux volets : le volet supérieur est le volet **Filtre** et le volet inférieur est le volet **Données** . Dans une prochaine leçon, vous utiliserez le navigateur de cube pour effectuer l'analyse.  
  
## <a name="next-lesson"></a>Leçon suivante  
[Leçon 3 : Modification des mesures, des attributs et hiérarchies](../analysis-services/lesson-3-modifying-measures-attributes-and-hierarchies.md)  
  
## <a name="see-also"></a>Voir aussi  
[Éditeur de requête MDX &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/777f2c23-1c1c-4b72-9d19-48a4866551f8)  
  
  
  
