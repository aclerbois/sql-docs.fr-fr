---
title: Requêtes d’exploration de données | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 072ffe2e41aa75d3fe62875685b0a4aa0d9a5138
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="data-mining-queries"></a>Requêtes d’exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les requêtes d'exploration de données sont utiles à de nombreuses fins. Vous pouvez :  
  
-   Appliquer le modèle aux nouvelles données, pour créer des prédictions uniques ou multiples. Vous pouvez fournir des valeurs d'entrée sous forme de paramètres, ou dans un lot.  
  
-   Obtenir un résumé statistique des données utilisées pour l'apprentissage.  
  
-   Extraire les schémas et les règles, ou générer un profil du cas type qui représente un schéma dans le modèle.  
  
-   Extraire les formules de régression et d'autres calculs qui expliquent les schémas.  
  
-   Obtenir les cas qui conviennent à un schéma particulier.  
  
-   Récupérer des détails sur les cas utilisés dans le modèle, y compris des données non utilisées dans l'analyse.  
  
-   Recycler un modèle en ajoutant de nouvelles données ou effectuer une prédiction croisée.  
  
 Cette section fournit une vue d'ensemble des informations dont vous avez besoin pour utiliser les requêtes d'exploration de données. Elle décrit les types de requêtes que vous pouvez créer sur des objets d'exploration de données, présente les outils et les langages de requête et fournit des liens vers des exemples de requêtes que vous pouvez créer sur des modèles générés à l'aide des algorithmes fournis dans l'exploration de données SQL Server.  
  
 [Fonctionnement des requêtes d'exploration de données](#bkmk_Understand)  
  
 [Outils et interfaces de requête](#bkmk_Interfaces)  
  
 [Requêtes pour différents types de modèle](#bkmk_ModelTypes)  
  
 [Spécifications](#bkmk_Reqs)  
  
##  <a name="bkmk_Understand"></a> Fonctionnement des requêtes d'exploration de données  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend en charge les types de requêtes suivants :  
  
-   [Requêtes de prédiction & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/prediction-queries-data-mining.md)  
  
     Requêtes qui créent des inférences basées sur des schémas dans le modèle, et à partir des données d'entrée.  
  
-   [Requêtes de contenu &#40;Exploration de données&#41;](../../analysis-services/data-mining/content-queries-data-mining.md)  
  
     Requêtes qui retournent des métadonnées, des statistiques et d'autres informations sur le modèle lui-même.  
  
-   [Requêtes d’extraction & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/drillthrough-queries-data-mining.md)  
  
     Requêtes qui peuvent récupérer les données de cas sous-jacentes pour le modèle, ou même les données de la structure qui n'ont pas été utilisées dans le modèle.  
  
-   [Requêtes de définition des données &#40;Exploration de données&#41;](../../analysis-services/data-mining/data-definition-queries-data-mining.md)  
  
     Requêtes qui ne retournent pas d'informations du modèle, mais qui sont plutôt utilisées pour générer des modèles et des structures ou pour mettre à jour les données dans un modèle ou une structure.  
  
 Avant de créer des requêtes, il est recommandé de vous familiariser avec les différences qu'il existe entre les modèles créés à l'aide de chacun des algorithmes d'exploration de données fournis par SQL Server.  
  
-   Parcourez et explorez chaque type de modèle à l'aide des visionneuses personnalisées d'exploration de données fournies pour chaque type d'algorithme. Pour plus d’informations, consultez [Tâches de la visionneuse de modèle d’exploration de données et procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md).  
  
-   Examinez le contenu de modèle pour chaque type de modèle, à l'aide de la **Visionneuse de l'arborescence de contenu générique Microsoft**. Pour interpréter ces informations, consultez [Contenu du modèle d’exploration &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md).  
  
##  <a name="bkmk_Interfaces"></a> Outils et interfaces de requête  
 Vous pouvez générer des requêtes d'exploration de données en mode interactif à l'aide de l'un des outils de requête fournis par SQL Server. Le générateur de requêtes de prédiction graphique est fourni à la fois dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si vous n'avez pas utilisé le générateur de requêtes de prédiction auparavant, nous vous recommandons de suivre les étapes du [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c) pour vous familiariser avec l'interface. Pour obtenir une vue d’ensemble rapide des étapes, consultez la section consacrée à la création d’une requête dans [Créer une requête de prédiction à l’aide du Générateur de requêtes de prédiction](../../analysis-services/data-mining/create-a-prediction-query-using-the-prediction-query-builder.md).  
  
 Le générateur de requêtes de prédiction est utile pour démarrer des requêtes que vous personnaliserez ultérieurement. Vous pouvez ajouter facilement des sources de données et les mapper à des colonnes, puis basculer vers la vue DMX et personnaliser la requête en ajoutant une clause WHERE ou d'autres fonctions.  
  
 Une fois que vous êtes familiarisé avec les modèles d'exploration de données et la procédure de génération des requêtes, vous pouvez également écrire des requêtes directement à l'aide du langage d'extensions DMX (Data Mining Extensions). DMX est un langage de requête semblable à Transact-SQL, et que vous pouvez utiliser à partir de nombreux clients. DMX est l'outil de choix pour créer des prédictions personnalisées et des requêtes complexes. Pour obtenir une présentation de DMX, consultez [Création et interrogation de modèles d’exploration de données à l’aide du langage DMX : didacticiels &#40;Analysis Services - Exploration de données&#41;](http://msdn.microsoft.com/library/145b81a7-c0c3-4ca3-bb32-0b482423b9a0).  
  
 Les éditeurs DMX sont fournis à la fois dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez également utiliser le générateur de requêtes de prédiction pour démarrer vos requêtes, puis changer la vue afin d'activer l'éditeur de texte et copier l'instruction DMX dans un autre client. Pour plus d’informations, consultez [Outils de requête d’exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md).  
  
 Vous pouvez composer des instructions DMX par programmation et les envoyer depuis votre client vers le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en utilisant AMO ou XMLA. Toutefois, DMX est le langage que vous devez utiliser pour créer des requêtes sur un modèle d'exploration de données.  
  
 Vous pouvez également interroger les métadonnées, les statistiques et une partie du contenu du modèle à l'aide de vues de gestion dynamique (DMV), basées sur les ensembles de lignes de schéma d'exploration de données. Ces vues DMV facilitent la récupération d'informations relatives au modèle en tapant des instructions SELECT ; toutefois, vous ne pouvez pas créer de prédictions. Pour plus d’informations sur les vues DMV prises en charge par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Utiliser des vues de gestion dynamique &#40;DMVs&#41; pour surveiller Analysis Services](../../analysis-services/instances/use-dynamic-management-views-dmvs-to-monitor-analysis-services.md).  
  
 Enfin, vous pouvez créer des requêtes d'exploration de données à utiliser dans les packages Integration Services, à l'aide de la [Data Mining Query Task](../../integration-services/control-flow/data-mining-query-task.md)ou [Data Mining Query Transformation](../../integration-services/data-flow/transformations/data-mining-query-transformation.md). La tâche de flux de contrôle prend en charge plusieurs types de requêtes DMX, alors que la transformation de flux de données ne prend en charge que les requêtes qui utilisent des données dans le flux de données, à savoir les requêtes qui utilisent la syntaxe PREDICTION JOIN.  
  
##  <a name="bkmk_ModelTypes"></a> Requêtes pour différents types de modèle  
 L'algorithme utilisé lors de la création du modèle influence considérablement le type d'informations que vous pouvez obtenir à partir d'une requête d'exploration de données. La raison de ces différences s'explique par le fait que chaque algorithme traite les données d'une manière différente et stocke différents types de schémas. Par exemple, certains algorithmes créent des clusters ; d'autres créent des arborescences. Par conséquent, vous devrez peut-être utiliser la prédiction et les fonctions de requêtes spécialisées, selon le type de modèle avec lequel vous travaillez.  
  
 La liste suivante fournit un résumé des fonctions que vous pouvez utiliser dans les requêtes :  
  
-   **Fonctions de prédiction générales :** la fonction **Predict** est polymorphe, ce qui signifie qu'elle fonctionne avec tous les types de modèle. Cette fonction détecte automatiquement le type de modèle que vous utilisez et vous invite à saisir des paramètres supplémentaires. Pour plus d’informations, consultez [Predict &#40;DMX&#41;](../../dmx/predict-dmx.md).  
  
    > [!WARNING]  
    >  Tous les modèles ne sont pas utilisés pour effectuer des prédictions. Par exemple, vous pouvez créer un modèle de clustering qui n'a pas d'attribut prédictible. Toutefois, même si un modèle ne dispose pas d'attribut prédictible, vous pouvez créer des requêtes de prédiction qui retournent d'autres types d'informations utiles à partir du modèle.  
  
-   **Fonctions de prédiction personnalisées :** chaque type de modèle fournit un ensemble de fonctions de prédiction conçues pour utiliser les schémas créés par l'algorithme.  
  
     Par exemple, la fonction **Lag** est fournie pour les modèles de série chronologique, pour vous permettre de visualiser les données d'historique utilisées pour le modèle. Pour les modèles de clustering, les fonctions telles que **ClusterDistance** sont plus explicites.  
  
     Pour plus d'informations sur les fonctions prises en charge pour chaque type de modèle, consultez les liens suivants :  
  
    |||  
    |-|-|  
    |[Exemples de requêtes de modèle d'association](../../analysis-services/data-mining/association-model-query-examples.md)|[Algorithme MNB (Microsoft Naive Bayes)](../../analysis-services/data-mining/microsoft-naive-bayes-algorithm.md)|  
    |[Exemples de requêtes de modèle de clustering](../../analysis-services/data-mining/clustering-model-query-examples.md)|[Exemples de requête de modèle de réseau neuronal](../../analysis-services/data-mining/neural-network-model-query-examples.md)|  
    |[Exemples de requêtes de modèle d'arbre de décision](../../analysis-services/data-mining/decision-trees-model-query-examples.md)|[Sequence Clustering Model Query Examples](../../analysis-services/data-mining/sequence-clustering-model-query-examples.md)|  
    |[Exemples de requête de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md)|[Exemples de requête de modèle de série de temps](../../analysis-services/data-mining/time-series-model-query-examples.md)|  
    |[Exemples de requêtes de modèle de régression logistique](../../analysis-services/data-mining/logistic-regression-model-query-examples.md)||  
  
     Vous pouvez également appeler des fonctions VBA ou créer vos propres fonctions. Pour plus d’informations, consultez [Fonctions &#40;DMX&#41;](../../dmx/functions-dmx.md).  
  
-   **Statistiques générales :** il existe plusieurs fonctions qui peuvent être utilisées avec presque n'importe quel type de modèle et retournent un ensemble standard de statistiques descriptives, telles que l'écart type.  
  
     Par exemple, la fonction **PredictHistogram** retourne une table qui répertorie tous les états de la colonne spécifiée.  
  
     Pour plus d’informations, consultez [Fonctions de prédiction générales &#40;DMX&#41;](../../dmx/general-prediction-functions-dmx.md).  
  
-   **Statistiques personnalisées :** des fonctions de prise en charge supplémentaires sont fournies pour chaque type de modèle, afin de générer des statistiques qui sont appropriées à la tâche analytique spécifique.  
  
     Par exemple, lorsque vous utilisez un modèle de clustering, vous pouvez utiliser la fonction, **PredictCaseLikelihood**pour retourner le score de vraisemblance associé à un certain cas et cluster. Toutefois, si vous avez créé un modèle de régression linéaire, vous serez plus intéressé à récupérer le coefficient et l'ordonnée à l'origine, ce que vous pouvez effectuer à l'aide d'une requête de contenu.  
  
-   **Fonctions de contenu de modèle :** le *contenu* de tous les modèles est représenté dans un format standardisé qui vous permet de récupérer des informations avec une requête simple. Vous créez des requêtes sur le contenu du modèle à l'aide du langage DMX. Vous pouvez également obtenir un certain type de contenu de modèle à l'aide des ensembles de lignes du schéma d'exploration de données.  
  
     Dans le contenu du modèle, la signification de chaque ligne ou nœud de la table retournée diffère selon le type d'algorithme utilisé pour générer le modèle, ainsi que le type de données de la colonne. Pour plus d’informations, consultez [Requêtes de contenu &#40;Exploration de données&#41;](../../analysis-services/data-mining/content-queries-data-mining.md).  
  
##  <a name="bkmk_Reqs"></a> Spécifications  
 Avant de pouvoir créer une requête sur un modèle, le modèle d'exploration de données doit avoir été traité. Le traitement d'objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] requiert des autorisations spéciales. Pour plus d’informations sur le traitement des modèles d’exploration de données, consultez [Exigences et considérations concernant le traitement &#40;exploration de données&#41;](../../analysis-services/data-mining/processing-requirements-and-considerations-data-mining.md).  
  
 Exécuter des requêtes sur un modèle d'exploration de données requiert différents niveaux d'autorisations, selon le type de requête que vous exécutez. Par exemple, l'extraction des données de structure ou de cas nécessite généralement des autorisations supplémentaires qui peuvent être définies sur l'objet de structure d'exploration de données ou sur un objet de modèle d'exploration de données.  
  
 Toutefois, si votre requête utilise des données externes et inclut des instructions telles que OPENROWSET ou OPENQUERY, la base de données que vous interrogez doit activer ces instructions, et vous devez disposer d'une autorisation sur les objets de base de données sous-jacents.  
  
 Pour plus d’informations sur les contextes de sécurité requis pour exécuter des requêtes d’exploration de données, consultez [Vue d’ensemble de la sécurité &#40;exploration de données&#41;](../../analysis-services/data-mining/security-overview-data-mining.md)  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques de cette section présentent chaque type de requête d'exploration de données plus en détail et fournissent des liens vers des exemples détaillés de création de requêtes sur des modèles d'exploration de données.  
  
 [Requêtes de prédiction & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/prediction-queries-data-mining.md)  
  
 [Requêtes de contenu &#40;Exploration de données&#41;](../../analysis-services/data-mining/content-queries-data-mining.md)  
  
 [Requêtes d’extraction &#40;exploration de données&#41;](../../analysis-services/data-mining/drillthrough-queries-data-mining.md)  
  
 [Requêtes de définition de données & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/data-definition-queries-data-mining.md)  
  
 [Outils de requête d’exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md)  
  
## <a name="related-tasks"></a>Tâches associées  
 Utilisez ces liens pour apprendre à créer et à utiliser des requêtes d'exploration de données.  
  
|Tâches|Liens|  
|-----------|-----------|  
|Afficher des didacticiels et des procédures pas à pas sur les requêtes d'exploration de données|[Leçon 6 : création et utilisation de prédictions &#40;Didacticiel sur l’exploration de données de base&#41;](http://msdn.microsoft.com/library/b213cb58-2c40-4c89-b08b-d3c36a4afad3)<br /><br /> [Didacticiel DMX sur la prédiction de série chronologique](http://msdn.microsoft.com/library/38ea7c03-4754-4e71-896a-f68cc2c98ce2)|  
|Utiliser les outils de requête d'exploration de données dans SQL Server Management Studio et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]|[Créer une requête DMX dans SQL Server Management Studio](../../analysis-services/data-mining/create-a-dmx-query-in-sql-server-management-studio.md)<br /><br /> [Créer une requête de prédiction à l’aide du Générateur de requêtes de prédiction](../../analysis-services/data-mining/create-a-prediction-query-using-the-prediction-query-builder.md)<br /><br /> [Appliquer des fonctions de prédiction à un modèle](../../analysis-services/data-mining/apply-prediction-functions-to-a-model.md)<br /><br /> [Modifier manuellement une requête de prédiction](../../analysis-services/data-mining/manually-edit-a-prediction-query.md)|  
|Utiliser des données externes utilisées dans des requêtes de prédiction|[Choisir et mapper les données d’entrée pour une requête de prédiction](../../analysis-services/data-mining/choose-and-map-input-data-for-a-prediction-query.md)<br /><br /> [Choisir et mapper les données d’entrée pour une requête de prédiction](../../analysis-services/data-mining/choose-and-map-input-data-for-a-prediction-query.md)|  
|Utiliser les résultats de requêtes|[Afficher et enregistrer les résultats d'une requête de prédiction](../../analysis-services/data-mining/view-and-save-the-results-of-a-prediction-query.md)|  
|Utiliser les modèles de requête DMX et XMLA fournis dans Management Studio|[Créer une requête de prédiction Singleton à partir d’un modèle](../../analysis-services/data-mining/create-a-singleton-prediction-query-from-a-template.md)<br /><br /> [Create a Data Mining Query by Using XMLA](../../analysis-services/data-mining/create-a-data-mining-query-by-using-xmla.md)<br /><br /> [Utiliser des modèles Analysis Services dans SQL Server Management Studio](../../analysis-services/instances/use-analysis-services-templates-in-sql-server-management-studio.md)|  
|En savoir plus sur les requêtes de contenu et afficher des exemples|[Créer une requête de contenu sur un modèle d’exploration de données](../../analysis-services/data-mining/create-a-content-query-on-a-mining-model.md)<br /><br /> [Interroger les paramètres utilisés pour créer un modèle d'exploration de données](../../analysis-services/data-mining/query-the-parameters-used-to-create-a-mining-model.md)<br /><br /> [Requêtes de contenu &#40;Exploration de données&#41;](../../analysis-services/data-mining/content-queries-data-mining.md)|  
|Définir des options de requête et résoudre les problèmes liés aux autorisations et aux requêtes|[Modifiez la valeur de délai d’attente pour les requêtes d’exploration de données](../../analysis-services/data-mining/change-the-time-out-value-for-data-mining-queries.md)|  
|Utiliser les composants d'exploration de données dans Integration Services|[Data Mining Query Task](../../integration-services/control-flow/data-mining-query-task.md)<br /><br /> [Data Mining Query Transformation](../../integration-services/data-flow/transformations/data-mining-query-transformation.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Contenu du modèle d’exploration &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md)  
  
  
