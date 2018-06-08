---
title: Définition d’un Cube | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 80c877341fc4ad9a952d94081d99f5a9c121fb1f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="lesson-2-2---defining-a-cube"></a>Leçon 2-2-définition d’un Cube
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

L'Assistant Cube vous aide à définir les groupes de mesures et les dimensions d'un cube. Dans la tâche suivante, vous allez utiliser l'Assistant Cube pour générer un cube.  
  
### <a name="to-define-a-cube-and-its-properties"></a>Pour définir un cube et ses propriétés  
  
1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur **Cubes**, puis cliquez sur **Nouveau cube**. L'Assistant Cube apparaît.  
  
2.  Dans la page **Bienvenue dans l’Assistant Cube** , cliquez sur **Suivant**.  
  
3.  Dans la page **Sélectionner la méthode de création** , vérifiez que l’option **Utiliser des tables existantes** est sélectionnée, puis cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner les tables de groupes de mesures** , vérifiez que la vue de source des données **Adventure Works DW 2012** est sélectionnée.  
  
5.  Cliquez sur **Suggérer** pour que l’Assistant Cube suggère les tables à utiliser pour créer les groupes de mesures.  
  
    L’Assistant examine les tables et suggère **InternetSales** comme table de groupes de mesures. Les tables de groupes de mesures, également appelées tables de faits, contiennent les mesures qui présentent un intérêt particulier pour vous, comme le nombre d’unités vendues.  
  
6.  Cliquez sur **Suivant**.  
  
7.  Dans la page **Sélectionner des mesures** , vérifiez les mesures sélectionnées dans le groupe de mesures **Internet Sales** , puis désactivez les cases à cocher correspondant aux mesures suivantes :  
  
    -   **Promotion Key**  
  
    -   **Currency Key**  
  
    -   **Sales Territory Key**  
  
    -   **Revision Number**  
  
    Par défaut, l'Assistant sélectionne comme mesures toutes les colonnes numériques de la table de faits qui ne sont pas liées à des dimensions. Toutefois, ces quatre colonnes ne sont pas vraiment des mesures. Les trois premières sont des valeurs clé qui lient la table de faits aux tables de dimension qui ne sont pas utilisées dans la première version de ce cube.  
  
8.  Cliquez sur **Suivant**.  
  
9. Dans la page **Sélectionner des dimensions existantes** , vérifiez que la dimension **Date** que vous avez créée précédemment est sélectionnée, puis cliquez sur **Suivant**.  
  
10. Dans la page **Sélectionner de nouvelles dimensions** , sélectionnez les nouvelles dimensions à créer. Pour cela, vérifiez que les cases **Customer**, **Geography**et **Product** sont cochées, puis décochez la case **InternetSales** .  
  
11. Cliquez sur **Suivant**.  
  
12. Dans la page **Fin de l’Assistant** , changez le nom du cube en **Analysis Services Tutorial**. Dans le volet Aperçu, vous pouvez voir le groupe de mesures **InternetSales** et ses mesures. Vous pouvez également voir les dimensions **Date**, **Customer** et **Product** .  
  
13. Cliquez sur **Terminer** pour terminer l’Assistant.  
  
    Dans l’Explorateur de solutions, dans le projet Didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , le cube du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] apparaît dans le dossier **Cubes** et les dimensions de base de données Customer et Product apparaissent dans le dossier **Dimensions** . De plus, au centre de l'environnement de développement, l'onglet Structure de cube affiche le cube du didacticiel [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] .  
  
14. Dans la barre d’outils de l’onglet Structure de cube, choisissez un niveau de **Zoom** de 50 % pour voir plus facilement les tables de dimension et de faits du cube. Notez que la table de faits est jaune et que les tables de dimension sont bleues.  
  
15. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
## <a name="next-task-in-lesson"></a>Tâche suivante de la leçon  
[Ajout d’attributs aux Dimensions](../analysis-services/lesson-2-3-adding-attributes-to-dimensions.md)  
  
## <a name="see-also"></a>Voir aussi  
[Cubes dans les modèles multidimensionnels](../analysis-services/multidimensional-models/cubes-in-multidimensional-models.md)  
[Dimensions dans les modèles multidimensionnels](../analysis-services/multidimensional-models/dimensions-in-multidimensional-models.md)  
  
  
  
