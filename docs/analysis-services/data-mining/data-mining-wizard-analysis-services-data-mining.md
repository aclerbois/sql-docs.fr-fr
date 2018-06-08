---
title: Assistant exploration de données (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 936a09c286b3732b6259d1ada9c8e720ab812074
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-mining-wizard-analysis-services---data-mining"></a>Assistant Exploration de données (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’Assistant Exploration de données inclus dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] démarre chaque fois que vous ajoutez une nouvelle structure d’exploration de données à un projet d’exploration de données. L'Assistant vous permet de choisir une source de données et de configurer une vue de source de données qui définit les données à utiliser pour l'analyse, puis vous aide à créer un modèle.  
  
 Pendant la dernière phase de l'Assistant, vous pouvez éventuellement diviser vos données en jeux d'apprentissage et de test, et activer certaines fonctionnalités telles que l'extraction.  
  
## <a name="what-to-know-before-you-start"></a>À lire avant de commencer  
 Voici ce que vous devez savoir avant de commencer l'Assistant.  
  
-   Allez-vous créer la structure et les modèles d'exploration de données à partir d'une base de données relationnelle ou à partir d'un cube existant d'une base de données OLAP ?  
  
-   Quelles colonnes contiennent les clés qui identifient de manière unique un enregistrement de cas ?  
  
-   Quelles colonnes ou quels attributs voulez-vous utiliser pour la prédiction ? Quelles colonnes ou quels attributs convient-il d'utiliser comme entrée pour l'analyse ?  
  
-   Quel algorithme utiliser ? Les algorithmes fournis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ont tous des caractéristiques différentes et produisent des résultats différents. Heureusement, vous n'êtes pas limité à un modèle pour chaque ensemble de données. Aussi, n'hésitez pas à faire des essais en ajoutant différents modèles.  
  
-   Devez-vous être en mesure de tester vos modèles sur un ensemble de données unifiées ? Si tel est le cas, envisagez d'utiliser l'option pour définir des données à réserver pour le test. Vous pouvez choisir un pourcentage, et imposer un nombre spécifié de lignes, si vous le souhaitez.  
  
##  <a name="BKMK_Using_DM_Wizard"></a> Démarrage de l'Assistant Exploration de données  
 Pour utiliser l’Assistant Exploration de données, vous devez avoir ouvert une solution dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] qui contient au moins un exploration de données ou un projet OLAP.  
  
-   Si votre solution est prête pour l’exploration de données, vous pouvez simplement cliquer avec le bouton droit sur le nœud **Structures d’exploration de données** dans l’Explorateur du solutions, puis sélectionner **Nouvelle structure d’exploration de données** pour démarrer l’Assistant.  
  
-   Si votre solution ne contient aucun projet existant, vous pouvez ajouter un nouveau projet d'exploration de données. Dans le menu **Fichier** , sélectionnez **Nouveau**, puis **Projet**. Veillez à sélectionner le modèle **Projet multidimensionnel et d’exploration de données Analysis Services**.  
  
-   Vous pouvez également utiliser l'Assistant Importation d'Analysis Services pour obtenir des métadonnées d'une solution d'exploration de données existante. Toutefois, vous ne pouvez pas sélectionner les différents objets à importer ; l'ensemble de la base de données est importé, y compris tout cube, toute vue de source de données, etc. Notez également que la nouvelle solution créée par l'intermédiaire de l'importation est automatiquement configurée pour utiliser la base de données par défaut locale. Vous devrez peut-être la remplacer par une autre instance avant de pouvoir traiter ou parcourir les objets, et si vous effectuez l'importation à partir d'une version antérieure d'[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous devrez peut-être mettre à jour les références aux fournisseurs.  
  
 Vous allez ensuite créer la structure d'exploration de données et un modèle d'exploration de données associé. Vous pouvez également créer uniquement la structure d'exploration de données et ajouter des modèles ultérieurement, mais le plus simple est généralement de créer un modèle de test en premier.  
  
###  <a name="BKMK_Relational"></a>Vs relationnelles. modèles d'exploration de données OLAP  
 L'option importante suivante que vous avez est d'utiliser une source de données relationnelles ou de baser votre modèle sur des données multidimensionnelles (OLAP).  
  
 À ce stade, l'Assistant Exploration de données se divise en deux branches, selon que votre source de données est relationnelle ou dans un cube. Tout le reste excepté le processus de sélection des données est identique (le choix de l'algorithme, la capacité à ajouter un ensemble de données d'exclusion, etc.), mais la sélection de données de cube est un peu plus complexe que d'utiliser des données relationnelles. (Vous obtenez également des options supplémentaires à la fin si vous créez un modèle basé sur un cube.)  
  
 Consultez les rubriques suivantes pour une procédure pas-à-pas de chaque option plus en détail :  
  
 [Créer une Structure d’exploration de données relationnelles](../../analysis-services/data-mining/create-a-relational-mining-structure.md)  
 Vous guide dans les décisions à prendre lors de la création d'un modèle d'exploration de données relationnel.  
  
 [Créer une Structure d’exploration de données OLAP](../../analysis-services/data-mining/create-an-olap-mining-structure.md)  
 Décrit les options supplémentaires et les sélections à effectuer lors de la sélection de données à partir d'un cube OLAP.  
  
> [!NOTE]  
>  Vous n'avez pas besoin d'avoir un cube ou une base de données OLAP pour effectuer l'exploration de données. Sauf si vos données sont déjà stockées dans un cube, ou si vous voulez explorer des dimensions OLAP ou les résultats de calculs ou d'agrégations OLAP, nous vous recommandons d'utiliser une table relationnelle ou une source de données pour l'exploration de données.  
  
### <a name="choosing-an-algorithm"></a>Choix d'un algorithme  
 Vous devez ensuite décider de l'algorithme à utiliser lors du traitement de vos données. Cette décision peut être difficile à prendre. Chaque algorithme fourni dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] a des fonctionnalités différentes et produit des résultats différents. Vous pouvez donc faire des essais et tenter différents modèles avant de déterminer celui qui est le plus approprié pour vos données et votre problème d'entreprise. Pour obtenir une explication des tâches pour lesquelles chaque algorithme est le plus approprié, consultez la rubrique suivante :  
  
 [Algorithmes d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)  
  
 Là encore, vous pouvez créer plusieurs modèles en utilisant différents algorithmes, ou bien modifier les paramètres des algorithmes pour créer différents modèles. Vous n'êtes pas bloqué dans votre choix de l'algorithme, et il est conseillé de créer différents modèles sur les mêmes données.  
  
### <a name="define-the-data-used-for-modeling"></a>Définir les données utilisées pour la modélisation  
 En plus de choisir les données d’une source, vous devez spécifier la table de la vue de source de données qui contient les *données de cas*. La table de cas sera utilisée pour l'apprentissage du modèle d'exploration de données, et en tant que telle, elle doit contenir les entités que vous voulez analyser : par exemple, les clients et leurs informations démographiques. Chaque cas doit être unique, et doit être identifiable par une *clé de cas*.  
  
 En plus de spécifier la table de cas, vous pouvez inclure des *tables imbriquées* dans vos données. Une table imbriquée contient généralement des informations supplémentaires sur les entités de la table de cas, telles que les transactions effectuées par le client ou les attributs ayant une relation plusieurs-à-un avec l'entité. Par exemple, une table imbriquée jointe à la table de cas **Customers** peut inclure une liste de produits que chaque client a achetés. Dans un modèle qui analyse le trafic sur un site Web, la table imbriquée peut inclure les séquences de pages que l'utilisateur a visitées. Pour plus d’informations, consultez [Tables imbriquées &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/nested-tables-analysis-services-data-mining.md).  
  
### <a name="additional-features"></a>Fonctionnalités supplémentaires  
 Pour vous aider à choisir les bonnes données, et à configurer correctement les sources de données, l'Assistant Exploration de données fournit les fonctionnalités supplémentaires suivantes :  
  
-   **Détection automatique des types de données**: l’Assistant examine l’unicité et la distribution de valeurs de colonnes, puis recommande le meilleur type de données, et propose un type d’utilisation pour les données. Vous pouvez remplacer ces suggestions en sélectionnant des valeurs dans une liste.  
  
-   **Suggestions pour les variables**: vous pouvez cliquer sur une boîte de dialogue et démarrer un analyseur qui calcule les corrélations entre les colonnes incluses dans le modèle, et qui détermine si des colonnes sont des prédicteurs probables de l’attribut de résultats, étant donné la configuration du modèle jusqu’à présent. Vous pouvez remplacer ces suggestions en tapant des valeurs différentes.  
  
-   **Sélection des fonctionnalités**: la plupart des algorithmes détectent automatiquement les colonnes qui sont de bons prédicteurs et les utilisent de préférence. Dans les colonnes qui contiennent trop de valeurs, la *sélection des fonctionnalités* est appliquée pour réduire la cardinalité des données et d’améliorer des chances de trouver un modèle explicite. Vous pouvez affecter le comportement de sélection des fonctionnalités à l'aide de paramètres de modèle.  
  
-   **Découpage automatique de cube en tranches**: si votre modèle d’exploration de données repose sur une source de données OLAP, la possibilité de découper le modèle en tranches à l’aide d’attributs de cube est automatiquement fournie. Cela est pratique pour créer des modèles basés sur des sous-ensembles de données de cube.  
  
### <a name="completing-the-wizard"></a>Fin de l'Assistant  
 La dernière étape de l'Assistant consiste à nommer la structure d'exploration de données et le modèle d'exploration de données associé. Selon le type de modèle que vous avez créés, vous pouvez également disposer des options importantes suivantes :  
  
-   Si vous sélectionnez **Accepter l’extraction**, la possibilité d’effectuer une *extraction* est activée dans le modèle. Avec l'extraction, les utilisateurs qui disposent des autorisations appropriées peuvent explorer les données sources utilisées pour générer le modèle.  
  
-   Si vous créez un modèle OLAP, vous pouvez sélectionner les options **Créer un nouveau cube d’exploration de données**ou **Créer une dimension d’exploration de données**. Ces deux options facilitent le parcours du modèle terminé et l'extraction des données sous-jacentes.  
  
 Lorsque vous avez terminé toutes les étapes de l'Assistant Exploration de données, vous utilisez le Concepteur d'exploration de données pour modifier la structure et les modèles d'exploration de données, voir la précision du modèle, afficher les caractéristiques de la structure et des modèles ou effectuer des prédictions à l'aide des modèles.  
  
 [Retour au début](#BKMK_Using_DM_Wizard)  
  
## <a name="related-content"></a>Contenu connexe  
 Pour en savoir plus sur les décisions à prendre lors de la création d'un modèle d'exploration de données, consultez les liens suivants :  
  
 [Algorithmes d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)  
  
 [Contenu des Types de & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/content-types-data-mining.md)  
  
 [Types de données & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/data-types-data-mining.md)  
  
 [Sélection des fonctionnalités & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/feature-selection-data-mining.md)  
  
 [Les valeurs manquantes & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/missing-values-analysis-services-data-mining.md)  
  
 [Extraction sur les modèles d’exploration de données](../../analysis-services/data-mining/drillthrough-on-mining-models.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Outils d’exploration de données](../../analysis-services/data-mining/data-mining-tools.md)   
 [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md)  
  
  
