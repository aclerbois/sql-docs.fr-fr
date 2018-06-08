---
title: Exemples de requête de modèle de réseau neuronal | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 66f7979dbcf2b547e8f24476f057616c2cd7d3cd
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="neural-network-model-query-examples"></a>Exemples de requêtes de modèle de réseau neuronal
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez créer une requête de contenu, qui fournit des détails sur les modèles (ou séquences) découverts au cours de l'analyse, ou une requête de prédiction, qui utilise les séquences du modèle pour effectuer des prédictions pour les nouvelles données. Par exemple, une requête de contenu pour un modèle de réseau neuronal peut extraire des métadonnées du modèle telles que le nombre de couches masquées. Une requête de prédiction peut également suggérer des classifications selon une entrée et fournir éventuellement des probabilités pour chaque classification.  
  
 Cette section explique comment créer des requêtes pour les modèles basés sur l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network).  
  
 **Requêtes de contenu**  
  
 [Obtention des métadonnées du modèle avec DMX](#bkmk_Query1)  
  
 [Récupération des métadonnées du modèle de l'ensemble de lignes de schéma](#bkmk_Query2)  
  
 [Récupération des attributs d'entrée du modèle](#bkmk_Query3)  
  
 [Récupération des poids de la couche masquée](#bkmk_Query4)  
  
 **Requêtes de prédiction**  
  
 [Création d'une prédiction singleton](#bkmk_Query5)  
  
## <a name="finding-information-about-a-neural-network-model"></a>Recherche d'informations sur le modèle de réseau neuronal  
 Tous les modèles d'exploration de données exposent le contenu appris par l'algorithme en fonction d'un schéma standardisé : l'ensemble de lignes de schéma du modèle d'exploration de données. Cette information fournit des détails à propos du modèle et inclut les métadonnées de base, les structures découvertes dans l'analyse et les paramètres utilisés lors du traitement. Vous pouvez créer des requêtes sur le contenu du modèle en utilisant des instructions DMX (Data Mining Extension).  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : obtention des métadonnées du modèle à l'aide de DMX  
 La requête suivante retourne des métadonnées de base sur un modèle généré en utilisant l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network). Dans un modèle de réseau neuronal, le nœud parent du modèle contient uniquement le nom du modèle, le nom de la base de données dans laquelle le modèle est stocké et le nombre de nœuds enfants. Toutefois, le nœud des statistiques marginales (NODE_TYPE = 24) fournit à la fois ces métadonnées de base et des statistiques dérivées à propos des colonnes d'entrée utilisées dans le modèle.  
  
 L'exemple de requête suivant est basé sur le modèle d'exploration de données que vous créez dans le [didacticiel sur l'exploration de données intermédiaire](http://msdn.microsoft.com/library/42c3701a-1fd2-44ff-b7de-377345bbbd6b), nommé `Call Center Default NN`. Le modèle utilise des données d'un centre d'appels pour explorer les corrélations possibles entre le personnel et le nombre d'appels, les commandes et les problèmes. L'instruction DMX récupère des données à partir du nœud des statistiques marginales du modèle de réseau neuronal. La requête inclut le mot clé FLATTENED, parce que les statistiques d'attributs d'entrée dignes d'intérêt sont stockées dans une table imbriquée, NODE_DISTRIBUTION. Toutefois, si votre fournisseur de requêtes prend en charge les ensembles de lignes hiérarchiques, vous n'avez pas besoin d'utiliser le mot clé FLATTENED.  
  
```  
SELECT FLATTENED MODEL_CATALOG, MODEL_NAME,   
(    SELECT ATTRIBUTE_NAME, ATTRIBUTE_VALUE,  
     [SUPPORT], [PROBABILITY], VALUETYPE   
     FROM NODE_DISTRIBUTION  
) AS t  
FROM [Call Center Default NN].CONTENT  
WHERE NODE_TYPE = 24  
```  
  
> [!NOTE]  
>  Vous devez indiquer entre crochets le nom des colonnes de la table imbriquée SUPPORT et PROBABILITY pour les distinguer des mots clés du même nom.  
  
 Résultats de l'exemple :  
  
|MODEL_CATALOG|MODEL_NAME|t.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|t.SUPPORT|t.PROBABILITY|t.VALUETYPE|  
|--------------------|-----------------|-----------------------|------------------------|---------------|-------------------|-----------------|  
|Adventure Works DW Multidimensional 2012|Call Center NN|Average Time Per Issue|Manquant|0|0|1|  
|Adventure Works DW Multidimensional 2012|Call Center NN|Average Time Per Issue|< 64.7094100096|11|0.407407407|5|  
  
 Pour une définition de la signification des colonnes dans l’ensemble de lignes de schéma dans le contexte d’un modèle de réseau neuronal, consultez [Contenu du modèle d’exploration de données pour les modèles de réseau neuronal &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-neural-network-models-analysis-services-data-mining.md).  
  
###  <a name="bkmk_Query2"></a> Exemple de requête 2 : récupération des métadonnées du modèle de l'ensemble de lignes de schéma  
 Vous pouvez obtenir les mêmes informations que celles retournées par une requête de contenu DMX en interrogeant l'ensemble de lignes de schéma d'exploration de données. Toutefois, l'ensemble de lignes de schéma fournit quelques colonnes supplémentaires, L'exemple de requête suivant retourne la date à laquelle le modèle a été créé, la date à laquelle il a été modifié et la date à laquelle il a été traité pour la dernière fois. La requête retourne également les colonnes prédictibles, qui ne sont pas aisément disponibles à partir du contenu du modèle, et les paramètres utilisés pour générer le modèle. Ces informations peuvent être utiles pour documenter le modèle.  
  
```  
SELECT MODEL_NAME, DATE_CREATED, LAST_PROCESSED, PREDICTION_ENTITY, MINING_PARAMETERS   
from $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = 'Call Center Default NN'  
```  
  
 Résultats de l'exemple :  
  
|||  
|-|-|  
|MODEL_NAME|Call Center Default NN|  
|DATE_CREATED|1/10/2008 5:07:38 PM|  
|LAST_PROCESSED|1/10/2008 5:24:02 PM|  
|PREDICTION_ENTITY|Durée moyenne par problème,<br /><br /> Niveau de service,<br /><br /> Nombre de commandes|  
|MINING_PARAMETERS|HOLDOUT_PERCENTAGE=30, HOLDOUT_SEED=0<br /><br /> MAXIMUM_INPUT_ATTRIBUTES=255, MAXIMUM_OUTPUT_ATTRIBUTES=255<br /><br /> MAXIMUM_STATES=100, SAMPLE_SIZE=10000, HIDDEN_NODE_RATIO=4|  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3 : extraction des attributs d'entrée du modèle  
 Vous pouvez récupérer les paires attribut-valeur d'entrée utilisées pour créer le modèle en interrogeant les nœuds enfants (NODE_TYPE = 20) de la couche d'entrée (NODE_TYPE = 18). La requête suivante retourne la liste des attributs d'entrée à partir des descriptions de nœuds.  
  
```  
SELECT NODE_DESCRIPTION  
FROM [Call Center Default NN].CONTENT  
WHERE NODE_TYPE = 2  
```  
  
 Résultats de l'exemple :  
  
|NODE_DESCRIPTION|  
|-----------------------|  
|Durée moyenne par problème=64.7094100096 - 77.4002099712|  
|Day Of Week=Fri.|  
|Opérateurs de niveau 1|  
  
 Seules quelques lignes représentatives des résultats sont montrées ici. Toutefois, vous pouvez constater que NODE_DESCRIPTION fournit des informations légèrement différentes selon le type de données de l'attribut d'entrée.  
  
-   Si l'attribut est une valeur discrète ou discrétisée, l'attribut et sa valeur ou sa plage discrétisée sont retournés.  
  
-   Si l'attribut est un type de données numériques continues, le NODE_DESCRIPTION contient uniquement le nom d'attribut. Toutefois, vous pouvez extraire la table NODE_DISTRIBUTION imbriquée pour obtenir la moyenne ou retournez le NODE_RULE pour obtenir les valeurs minimales et maximales de la plage numérique.  
  
 La requête suivante indique comment interroger la table NODE_DISTRIBUTION imbriquée pour retourner les attributs dans une colonne et leurs valeurs dans une autre colonne. Pour les attributs continus, la valeur de l'attribut est représentée par sa moyenne.  
  
```  
SELECT FLATTENED   
(SELECT ATTRIBUTE_NAME, ATTRIBUTE_VALUE  
FROM NODE_DISTRIBUTION) as t  
FROM [Call Center Default NN -- Predict Service and Orders].CONTENT  
WHERE NODE_TYPE = 21  
```  
  
 Résultats de l'exemple :  
  
|T.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|  
|-----------------------|------------------------|  
|Average Time Per Issue|64.7094100096 - 77.4002099712|  
|Jour de la semaine|Fri.|  
|Opérateurs de niveau 1|3.2962962962963|  
  
 Les valeurs de plage minimales et maximales sont stockées dans la colonne NODE_RULE et sont représentées en tant que fragment XML, comme présenté dans l'exemple suivant :  
  
```  
<NormContinuous field="Level 1 Operators">    
  <LinearNorm orig="2.83967303681711" norm="-1" />    
  <LinearNorm orig="3.75291955577548" norm="1" />    
</NormContinuous>    
```  
  
###  <a name="bkmk_Query4"></a> Exemple de requête 4 : extraction des poids de la couche masquée  
 Le contenu du modèle d'un modèle de réseau neuronal est structuré de manière à extraire facilement les détails relatifs à n'importe quel nœud du réseau. De plus, les nombres d'ID des nœuds fournissent des informations qui vous aident à identifier les relations entre les types de nœuds.  
  
 La requête suivante montre comment extraire les coefficients stockés sous un nœud particulier de la couche masquée. La couche masquée consiste en un nœud organisateur (NODE_TYPE = 19), qui contient uniquement les métadonnées, et plusieurs nœuds enfants (NODE_TYPE = 22), qui contiennent les coefficients pour les diverses combinaisons d'attributs et valeurs. Cette requête retourne uniquement les nœuds coefficient.  
  
```  
SELECT FLATTENED TOP 1 NODE_UNIQUE_NAME,   
(SELECT ATTRIBUTE_NAME, ATTRIBUTE_VALUE, VALUETYPE  
FROM NODE_DISTRIBUTION) as t  
FROM  [Call Center Default NN -- Predict Service and Orders].CONTENT  
WHERE NODE_TYPE = 22  
AND [PARENT_UNIQUE_NAME] = '40000000200000000' FROM [Call Center Default NN].CONTENT  
```  
  
 Résultats de l'exemple :  
  
|NODE_UNIQUE_NAME|t.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|t.VALUETYPE|  
|------------------------|-----------------------|------------------------|-----------------|  
|70000000200000000|6000000000000000a|-0.178616518|7|  
|70000000200000000|6000000000000000b|-0.267561918|7|  
|70000000200000000|6000000000000000c|0.11069497|7|  
|70000000200000000|6000000000000000d|0.123757712|7|  
|70000000200000000|6000000000000000e|0.294565343|7|  
|70000000200000000|6000000000000000f|0.22245318|7|  
|70000000200000000||0.188805045|7|  
  
 Les résultats partiels présentés ici montrent comment le contenu de modèle de réseau neuronal met en rapport le nœud masqué et les nœuds d'entrée.  
  
-   Les noms uniques des nœuds dans la couche masquée commencent toujours par 70000000.  
  
-   Les noms uniques des nœuds dans la couche d'entrée commencent toujours par 60000000.  
  
 Par conséquent, ces résultats vous indiquent que six coefficients différents (VALUETYPE = 7) ont été transmis au nœud dénoté par l'ID 70000000200000000. Les valeurs des coefficients se trouvent dans la colonne ATTRIBUTE_VALUE. Vous pouvez déterminer exactement à quel attribut d'entrée le coefficient s'applique en utilisant l'ID de nœud dans la colonne ATTRIBUTE_NAME. Par exemple, l’ID de nœud 6000000000000000a se rapporte à l’attribut d’entrée et à la valeur `Day of Week = 'Tue.'` Vous pouvez utiliser l’ID de nœud pour créer une requête ou vous pouvez sélectionner le nœud en utilisant la [Visionneuse de l’arborescence de contenu générique Microsoft](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c).  
  
 De la même façon, si vous interrogez la table des nœuds NODE_DISTRIBUTION dans la couche de sortie (NODE_TYPE = 23), vous pouvez consulter les coefficients pour chaque valeur de sortie. Toutefois, les pointeurs font référence aux nœuds de la couche masquée dans la couche de sortie. Pour plus d’informations, consultez [Contenu du modèle d’exploration de données pour les modèles de réseau neuronal &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-neural-network-models-analysis-services-data-mining.md).  
  
## <a name="using-a-neural-network-model-to-make-predictions"></a>Utilisation d'un modèle de réseau neuronal pour élaborer des prédictions  
 L'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) prend en charge à la fois la classification et la régression. Vous pouvez utiliser des fonctions de prédiction avec ces modèles pour fournir de nouvelles données et créer des prédictions singleton ou prédictions par lot.  
  
###  <a name="bkmk_Query5"></a> Exemple de requête 5 : création d'une prédiction singleton  
 La façon la plus simple de générer une requête de prédiction sur un modèle de réseau neuronal est d'utiliser le Générateur de requêtes de prédiction, disponible sur l'onglet **Prédiction d'exploration de données** du Concepteur d'exploration de données dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Vous pouvez parcourir le modèle dans la Visionneuse de l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) pour filtrer des attributs présentant un intérêt et voir les tendances, puis basculer vers l'onglet **Prédiction d'exploration de données** pour créer une requête et prédire de nouvelles valeurs pour ces tendances.  
  
 Par exemple, vous pouvez parcourir le modèle de centre d'appels pour voir les corrélations entre les volumes de commandes et d'autres attributs. Pour ce faire, ouvrez le modèle dans la visionneuse et pour **entrée**, sélectionnez  **\<tous les >**.  Ensuite, pour **Sortie**, sélectionnez **Nombre de commandes**. Pour **Valeur 1,** sélectionnez la plage qui représente le plus de commandes et pour **Valeur 2**, sélectionnez la plage qui représente le moins de commandes. Vous pouvez ensuite voir d'un seul coup d'œil tous les attributs que le modèle met en corrélation avec le volume de commandes.  
  
 En parcourant les résultats dans la visionneuse, vous découvrez que certains jours de la semaine se caractérisent par des volumes de commandes bas et qu'une augmentation du nombre d'opérateurs semble correspondre à des ventes plus élevées. Vous pouvez alors utiliser une requête de prédiction sur le modèle pour tester une hypothèse de simulation et chercher à savoir si l'augmentation du nombre d'opérateurs de niveau 2 un jour de volume bas permettrait d'augmenter les commandes. Pour cela, créez une requête telle que la suivante :  
  
```  
SELECT Predict([Call Center Default NN].[Number of Orders]) AS [Predicted Orders],  
PredictProbability([Call Center Default NN].[Number of Orders]) AS [Probability]  
FROM [Call Center Default NN]  
NATURAL PREDICTION JOIN   
(SELECT 'Tue.' AS [Day of Week],  
13 AS [Level 2 Operators]) AS t  
```  
  
 Résultats de l'exemple :  
  
|Commandes prédites|Probabilité|  
|----------------------|-----------------|  
|364|0.9532…|  
  
 Le volume de ventes prédit est plus élevé que la plage actuelle de ventes pour mardi et la probabilité de la prédiction est très haute. Cependant, vous pouvez créer plusieurs prédictions à l'aide d'un processus par lot pour tester diverses hypothèses sur le modèle.  
  
> [!NOTE]  
>  Les compléments d'exploration de données pour Excel 2007 fournissent des Assistants de régression logistique qui permettent de répondre facilement à des questions complexes, telles que le nombre d'opérateurs de niveau 2 qui seraient nécessaires pour améliorer le niveau de service afin d'atteindre un niveau cible pour une équipe spécifique. Les compléments d'exploration de données, qui peuvent être téléchargés gratuitement, incluent des Assistants basés sur l'algorithme MNN (Microsoft Neural Network) et/ou l'algorithme MLR (Microsoft Logistic Regression). Pour plus d’informations, consultez le site web [Data Mining Add-ins for Office 2007](http://go.microsoft.com/fwlink/?LinkID=117790) (Compléments d’exploration de données SQL Server pour Office 2007).  
  
## <a name="list-of-prediction-functions"></a>Liste des fonctions de prédiction  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Il n'existe aucune fonction de prédiction spécifique à l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network, réseau neuronal de Microsoft). Toutefois, l'algorithme prend en charge les fonctions répertoriées dans le tableau suivant.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[IsDescendant & #40 ; DMX & #41 ;](../../dmx/isdescendant-dmx.md)|Détermine si un nœud est un enfant d'un autre nœud dans le graphique de réseau neuronal.|  
|[PredictAdjustedProbability & #40 ; DMX & #41 ;](../../dmx/predictadjustedprobability-dmx.md)|Retourne la probabilité pondérée.|  
|[PredictHistogram & #40 ; DMX & #41 ;](../../dmx/predicthistogram-dmx.md)|Retourne une table des valeurs associées à la valeur prédite actuelle.|  
|[PredictVariance &#40;DMX&#41;](../../dmx/predictvariance-dmx.md)|Retourne la variance de la valeur prédite.|  
|[PredictProbability & #40 ; DMX & #41 ;](../../dmx/predictprobability-dmx.md)|Retourne la probabilité pour la valeur prédite.|  
|[PredictStdev & #40 ; DMX & #41 ;](../../dmx/predictstdev-dmx.md)|Retourne la déviance standard pour la valeur prédite.|  
|[PredictSupport & #40 ; DMX & #41 ;](../../dmx/predictsupport-dmx.md)|Pour les modèles de réseau neuronal et de régression logistique, la fonction retourne une valeur unique qui représente la taille du jeu d'apprentissage pour le modèle entier.|  
  
 Pour obtenir la liste des fonctions qui sont communes à tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)], consultez [Informations de référence sur les algorithmes (Analysis Services - Exploration de données)](https://technet.microsoft.com/library/bb895228\(v=sql.105\).aspx). Pour la syntaxe de fonctions spécifiques, consultez [Fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [MNN (Microsoft Neural Network)](../../analysis-services/data-mining/microsoft-neural-network-algorithm.md)   
 [Référence technique de Microsoft Neural Network algorithme](../../analysis-services/data-mining/microsoft-neural-network-algorithm-technical-reference.md)   
 [Contenu du modèle d’exploration de données pour les modèles de réseau neuronal & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-neural-network-models-analysis-services-data-mining.md)   
 [Leçon 5 : Génération neuronal réseau et les modèles de régression logistique & #40 ; didacticiel d’exploration de données intermédiaires & #41 ;](http://msdn.microsoft.com/library/42c3701a-1fd2-44ff-b7de-377345bbbd6b)  
  
  
