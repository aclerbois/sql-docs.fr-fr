---
title: Exemples de requêtes de modèle de série chronologique | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fb280c856b6e7231c078bf830be4a10f9ecc4723
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="time-series-model-query-examples"></a>Exemples de requêtes de modèle de série chronologique
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez soit créer une requête de contenu, qui fournit des détails sur les modèles (ou séquences) découverts au cours de l'analyse, soit créer une requête de prédiction, qui utilise les séquences du modèle pour effectuer des prédictions pour les nouvelles données. Par exemple, une requête de contenu pour un modèle de série chronologique peut fournir des détails supplémentaires sur les structures périodiques détectées, tandis qu'une requête de prédiction peut vous donner des prédictions pour les 5 à 10 tranches de temps suivantes. Vous pouvez également extraire les métadonnées relatives au modèle en utilisant une requête.  
  
 Cette section explique comment créer les deux types de requêtes pour les modèles basés sur l'algorithme MTS (Microsoft Time Series).  
  
 **Requêtes de contenu**  
  
 [Récupération d'indices de périodicité pour le modèle](#bkmk_Query1)  
  
 [Récupération de l'équation d'un modèle ARIMA](#bkmk_Query2)  
  
 [Récupération de l'équation d'un modèle ARTxp](#bkmk_Query3)  
  
 **Requêtes de prédiction**  
  
 [Présentation du remplacement et de l'extension de données de série chronologique](#bkmk_ReplaceExtend)  
  
 [Exécution de prédictions avec EXTEND_MODEL_CASES](#bkmk_EXTEND)  
  
 [Exécution de prédictions avec REPLACE_MODEL_CASES](#bkmk_REPLACE)  
  
 [Substitution de valeurs manquantes dans les modèles de séries chronologiques](#bkmk_MissingValues)  
  
## <a name="getting-information-about-a-time-series-model"></a>Obtention d'informations sur un modèle de série chronologique  
 Une requête de contenu de modèle peut fournir des informations de base sur le modèle, telles que les paramètres qui ont été utilisés lors de la création du modèle et l'heure du dernier traitement du modèle. L'exemple suivant illustre la syntaxe de base pour l'interrogation du contenu du modèle à l'aide des ensembles de lignes de schéma d'exploration de données.  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : extraction d'indications de périodicité pour le modèle  
 Vous pouvez récupérer les périodicités qui ont été détectées dans la série chronologique en interrogeant l'arbre ARIMA ou l'arbre ARTXP. Toutefois, les périodicités dans le modèle terminé peuvent ne pas être les mêmes que les périodes que vous avez spécifiées comme indications lors de la création du modèle. Pour extraire les indications qui ont été fournies comme paramètres lors de la création du modèle, vous pouvez interroger l'ensemble de lignes de schéma du contenu du modèle d'exploration de données en utilisant l'instruction DMX suivante :  
  
```  
SELECT MINING_PARAMETERS   
FROM $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = '<model name>'  
```  
  
 Résultats partiels :  
  
|MINING_PARAMETERS|  
|------------------------|  
|COMPLEXITY_PENALTY = 0,1, MINIMUM_SUPPORT = 10, PERIODICITY_HINT ={1,3},...|  
  
 L'indication de périodicité par défaut, {1}, apparaît dans tous les modèles ; cet exemple de modèle a été créé avec une indication supplémentaire qui peut ne pas être présente dans le modèle final.  
  
> [!NOTE]  
>  Pour une meilleure lisibilité, les résultats ont été tronqués ici.  
  
  
###  <a name="bkmk_Query2"></a> Exemple de requête 2 : extraction de l'équation d'un modèle ARIMA  
 Vous pouvez extraire l'équation d'un modèle ARIMA en interrogeant tout nœud dans un arbre individuel. N'oubliez pas que chaque arbre d'un modèle ARIMA représente une périodicité différente et, s'il y a plusieurs séries de données, chaque série de données aura son propre ensemble d'arbres de périodicités. Par conséquent, pour extraire l'équation correspondant à une série de données spécifique, vous devez tout d'abord identifier l'arbre.  
  
 Par exemple, le préfixe TA vous indique que le nœud appartient à un arbre ARIMA, alors que le préfixe TS est utilisé pour les arbres ARTXP. Vous pouvez rechercher tous les arbres racine ARIMA en interrogeant le contenu du modèle pour les nœuds ayant une valeur NODE_TYPE de 27. Vous pouvez également utiliser la valeur de ATTRIBUTE_NAME pour rechercher le nœud racine ARIMA pour une série de données particulière. Cet exemple de requête recherche les nœuds ARIMA qui représentent les quantités vendues du modèle R250 dans la région Europe.  
  
```  
SELECT NODE_UNIQUE_NAME  
FROM Forecasting.CONTENT  
WHERE ATTRIBUTE_NAME = 'R250 Europe: Quantity"  
AND NODE_TYPE = 27  
```  
  
 À l'aide de cet ID de nœud, vous pouvez extraire des informations sur l'équation ARIMA pour cet arbre. L'instruction DMX suivante extrait la forme abrégée de l'équation ARIMA pour la série de données. Elle extrait également l'ordonnée à l'origine de la table imbriquée, NODE_DISTRIBUTION. Dans cet exemple, l'équation s'obtient en référençant l'ID unique de nœud TA00000007. Cependant, vous devrez peut-être utiliser un autre ID de nœud et vous obtiendrez éventuellement des résultats légèrement différents de votre modèle.  
  
```  
SELECT FLATTENED NODE_CAPTION as [Short equation],   
(SELECT ATTRIBUTE_NAME, ATTRIBUTE_VALUE   
FROM NODE_DISTRIBUTION) as t  
FROM Forecasting.CONTENT  
WHERE NODE_NAME = 'TA00000007'  
```  
  
 Résultats de l'exemple :  
  
|Équation abrégée|t.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|  
|--------------------|-----------------------|------------------------|  
|ARIMA (2,0,7)x(1,0,2)(12)|R250 Europe:Quantity(Intercept)|15.24….|  
|ARIMA (2,0,7)x(1,0,2)(12)|R250 Europe:Quantity(Periodicity)|1|  
|ARIMA (2,0,7)x(1,0,2)(12)|R250 Europe:Quantity(Periodicity)|12|  
  
 Pour en savoir plus sur l’interprétation de ces informations, consultez [Mining Model Content for Time Series Models &#40;Analysis Services - Data Mining&#41;](../../analysis-services/data-mining/mining-model-content-for-time-series-models-analysis-services-data-mining.md).  
  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3 : récupération de l'équation d'un modèle ARTXP  
 Pour un modèle ARTXP, des informations différentes sont stockées à chaque niveau de l'arbre. Pour plus d’informations sur la structure d’un modèle ARTxp et sur l’interprétation de ces informations dans l’équation, consultez [Mining Model Content for Time Series Models &#40;Analysis Services - Data Mining&#41;](../../analysis-services/data-mining/mining-model-content-for-time-series-models-analysis-services-data-mining.md).  
  
 L'instruction DMX suivante extrait les informations de l'arbre ARTxp qui représente la quantité de ventes du modèle R250 en Europe.  
  
> [!NOTE]  
>  Le nom de la colonne de la table imbriquée, VARIANCE, doit être placé entre parenthèses afin de le distinguer du mot clé réservé du même nom. Les colonnes de la table imbriquée, PROBABILITY et SUPPORT, ne sont pas incluses car elles sont vides dans la plupart des cas.  
  
```  
SELECT NODE_CAPTION as [Split information],   
(SELECT ATTRIBUTE_NAME, ATTRIBUTE_VALUE,  
   [VARIANCE]  
   FROM NODE_DISTRIBUTION) AS t  
FROM Forecasting.CONTENT  
WHERE NODE_ATTRIBUTE_NAME = 'R250 Europe:Quantity'  
AND NODE_TYPE = 15  
```  
  
 Pour en savoir plus sur l’interprétation de ces informations, consultez [Mining Model Content for Time Series Models &#40;Analysis Services - Data Mining&#41;](../../analysis-services/data-mining/mining-model-content-for-time-series-models-analysis-services-data-mining.md).  
  
  
## <a name="creating-predictions-on-a-time-series-model"></a>Création de prédictions sur un modèle de série chronologique  
 À compter de [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)], vous pouvez ajouter de nouvelles données à un modèle de série chronologique et les incorporer automatiquement au modèle. Vous ajoutez de nouvelles données à un modèle d'exploration de données de série chronologique de l'une des deux manières suivantes :  
  
-   Utilisez une instruction **PREDICTION JOIN** pour joindre des données d’une source externe aux données d’apprentissage.  
  
-   Utilisez une requête singleton de prédiction pour fournir les données un secteur à la fois. Pour plus d’informations sur la création d’une requête de prédiction singleton, consultez [Outils de requête d’exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md).  
  
###  <a name="bkmk_ReplaceExtend"></a> Pour comprendre le comportement des opérations de remplacement et d'extension  
 Lorsque vous ajoutez de nouvelles données à un modèle de série chronologique, vous pouvez spécifier s'il faut étendre ou remplacer les données d'apprentissage :  
  
-   **Extension :** lorsque vous étendez une série de données, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ajoute les nouvelles données à la fin des données d'apprentissage existantes. Le nombre de cas d'apprentissage augmente également.  
  
     L'extension des cas de modèle est utile pour mettre à jour continuellement le modèle avec de nouvelles données. Par exemple, si vous souhaitez que le jeu d'apprentissage grandisse au fil du temps, il suffit d'étendre le modèle.  
  
     Pour étendre les données, vous devez créer une instruction **PREDICTION JOIN** sur un modèle de série chronologique, spécifiez la source des nouvelles données et utilisez l’argument **EXTEND_MODEL_CASES** .  
  
-   **Replacement :** lorsque vous remplacez les données dans la série de données, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] conserve le modèle appris, mais utilise les nouvelles valeurs pour remplacer tout ou une partie des cas d'apprentissage existants. Par conséquent, la taille des données d'apprentissage ne change jamais, mais les cas eux-mêmes sont remplacés continuellement par des données plus récentes. Si vous fournissez suffisamment de nouvelles données, vous pouvez remplacer les données d'apprentissage par une série entièrement nouvelle.  
  
     Le remplacement des cas de modèles s'avère utile lorsque vous souhaitez effectuer l'apprentissage d'un modèle sur un ensemble de cas, puis appliquer ce modèle à une série de données différente.  
  
     Pour remplacer les données, créez une instruction **PREDICTION JOIN** sur un modèle de série chronologique, spécifiez la source des nouvelles données et utilisez l’argument **REPLACE_MODEL_CASES** .  
  
> [!NOTE]  
>  Vous ne pouvez pas effectuer de prédictions historiques lorsque vous ajoutez de nouvelles données.  
  
 Les prédictions commencent toujours à l'horodatage qui termine le jeu d'apprentissage d'origine, que vous étendiez ou remplaciez les données d'apprentissage. En d’autres termes, si vos nouvelles données contiennent n tranches horaires et que vous demandez des prédictions pour les étapes 1 à n, les prédictions coïncideront avec la même période que les nouvelles données et vous n’obtiendrez aucune nouvelle prédiction.  
  
 Pour obtenir de nouvelles prédictions pour des périodes sans qu’elles se chevauchent avec les nouvelles données, vous devez faire commencer les prédictions à la tranche horaire n+1 ou demander des tranches horaires supplémentaires.  
  
 Par exemple, supposez que le modèle existant a des données pour six mois. Vous souhaitez étendre ce modèle en ajoutant les chiffres de vente des trois derniers mois. En même temps, vous souhaitez effectuer une prédiction concernant les trois prochains mois. Pour obtenir uniquement les nouvelles prédictions lorsque vous ajoutez les nouvelles données, spécifiez la tranche de temps 4 comme point de départ et la tranche de temps 7 comme point de terminaison. Vous pourriez également demander un total de six prédictions, mais les tranches de temps pour les trois premières prédictions et celles des nouvelles données ajoutées se chevaucheraient.  
  
 Pour obtenir des exemples de requêtes et des informations supplémentaires sur la syntaxe d’utilisation des arguments **REPLACE_MODEL_CASES** et **EXTEND_MODEL_CASES**, consultez [PredictTimeSeries &#40;DMX&#41;](../../dmx/predicttimeseries-dmx.md).  
  
  
###  <a name="bkmk_EXTEND"></a> Exécution de prédictions avec EXTEND_MODEL_CASES  
 Le comportement de prédiction diffère selon que vous étendez ou remplacez les cas de modèle. Lorsque vous étendez un modèle, les nouvelles données sont jointes à la fin de la série et la taille du jeu d'apprentissage augmente. Toutefois, les tranches de temps utilisées pour les requêtes de prédiction commencent toujours à la fin de la série d'origine. Par conséquent, si vous ajoutez trois nouveaux points de données et que vous demandez six prédictions, les trois premières prédictions retournées et les nouvelles données se chevauchent. Dans ce cas, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne les nouveaux points de données réels au lieu d'effectuer une prédiction, jusqu'à ce que tous les nouveaux points de données soient utilisés. Ensuite, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] effectue des prédictions sur la base de la série composite.  
  
 Ce comportement vous permet d'ajouter de nouvelles données, puis d'afficher vos chiffres de vente réels dans le graphique de prédiction, au lieu de voir des projections.  
  
 Par exemple, pour ajouter trois nouveaux points de données et effectuer trois nouvelles prédictions, vous procéderiez comme suit :  
  
-   Créez une instruction **PREDICTION JOIN** sur un modèle de série chronologique et spécifiez la source de trois mois de nouvelles données.  
  
-   Demandez des prédictions pour six tranches de temps. Pour ce faire, spécifiez 6 tranches de temps, où le point de départ est la tranche de temps 1 et le point de terminaison est la tranche de temps 7. Cela est vrai uniquement pour EXTEND_MODEL_CASES.  
  
-   Pour obtenir uniquement les nouvelles prédictions, vous spécifiez la tranche de temps 4 comme point de départ et la tranche de temps 7 comme point de terminaison.  
  
-   Vous devez utiliser l’argument **EXTEND_MODEL_CASES**.  
  
     Les chiffres de vente réels sont retournés pour les trois premières tranches de temps et des prédictions basées sur le modèle étendu sont retournées pour les trois tranches de temps suivantes.  
  
  
###  <a name="bkmk_REPLACE"></a> Exécution de prédictions avec REPLACE_MODEL_CASES  
 Lorsque vous remplacez les cas dans un modèle, la taille du modèle reste identique mais [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] remplace les cas individuels dans le modèle. Ceci est utile pour les prédictions croisées et les scénarios dans lesquels la maintenance d'une taille de jeu de données d'apprentissage cohérente est importante.  
  
 Par exemple, l'un de vos magasins a des données de ventes insuffisantes. Vous pourriez créer un modèle général en faisant la moyenne des ventes pour tous les magasins dans une région particulière et former ensuite un modèle. Ensuite, pour effectuer des prédictions pour le magasin dont les données de ventes sont insuffisantes, vous créez une instruction **PREDICTION JOIN** sur les nouvelles données de ventes uniquement pour ce magasin. Dans ce cas, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] conserve les modèles dérivés du modèle régional, mais remplace les cas d'apprentissage existants par les données du magasin individuel. En conséquence, vos valeurs de prédiction seront plus proches des lignes de tendance pour le magasin individuel.  
  
 Quand vous utilisez l’argument **REPLACE_MODEL_CASES** , [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ajoute continuellement de nouveaux cas à la fin de l’ensemble de cas et supprime un nombre correspondant au début de l’ensemble de cas. Si vous ajoutez davantage de nouvelles données qu'il n'en existait dans le jeu d'apprentissages d'origine, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ignore les données les plus anciennes. Si vous fournissez suffisamment de nouvelles valeurs, les prédictions peuvent être basées sur des données complètement nouvelles.  
  
 Par exemple, vous avez formé votre modèle sur un jeu de données de cas qui contenait 1000 lignes. Vous ajoutez ensuite 100 lignes de nouvelles données. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] supprime les 100 premières lignes du jeu d'apprentissage et ajoute les 100 lignes de nouvelles données à la fin du jeu pour un total de 1 000 lignes. Si vous ajoutez 1100 lignes de nouvelles données, seules les 1000 lignes les plus récentes sont utilisées.  
  
 Voici un autre exemple. Pour ajouter trois mois de nouvelles données et effectuer trois nouvelles prédictions, vous procéderiez comme suit :  
  
-   Créez une instruction **PREDICTION JOIN** sur un modèle de série chronologique et utilisez l’argument **REPLACE_MODEL_CASE** .  
  
-   Spécifiez la source de trois mois de nouvelles données. Ces données peuvent provenir d'une source complètement différente des données d'apprentissage d'origine.  
  
-   Demandez des prédictions pour six tranches de temps. Pour cela, spécifiez 6 tranches de temps ou spécifiez que le point de départ est la tranche de temps 1 et le point de terminaison est la tranche de temps 7.  
  
    > [!NOTE]  
    >  Contrairement à **EXTEND_MODEL_CASES**, vous ne pouvez pas retourner les mêmes valeurs que celles que vous avez ajoutées comme données d’entrée. Les six valeurs retournées sont des prédictions basées sur le modèle mis à jour, qui inclut à la fois les données anciennes et nouvelles.  
  
    > [!NOTE]  
    >  Avec REPLACE_MODEL_CASES, commençant à la valeur timestamp 1, vous obtenez de nouvelles prédictions basées sur les nouvelles données, lesquelles remplacent les anciennes données d'apprentissage.  
  
 Pour obtenir des exemples de requêtes et des informations supplémentaires sur la syntaxe d’utilisation des arguments **REPLACE_MODEL_CASES** et **EXTEND_MODEL_CASES**, consultez [PredictTimeSeries &#40;DMX&#41;](../../dmx/predicttimeseries-dmx.md).  
  
  
###  <a name="bkmk_MissingValues"></a> Substitution de valeurs manquantes dans les modèles de séries chronologiques  
 Quand vous ajoutez de nouvelles données à un modèle de série chronologique en utilisant une instruction **PREDICTION JOIN** , le nouveau dataset ne peut pas avoir de valeurs manquantes. Si une série est incomplète, le modèle doit fournir les valeurs manquantes en utilisant une valeur Null, une moyenne numérique, une moyenne numérique spécifique ou une valeur prédite. Si vous spécifiez **EXTEND_MODEL_CASES**, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] remplace les valeurs manquantes par des prédictions basées sur le modèle d’origine. Si vous utilisez **REPLACE_MODEL_CASES**, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] remplace les valeurs manquantes par la valeur que vous spécifiez dans le paramètre *MISSING_VALUE_SUBSTITUTION* .  
  
## <a name="list-of-prediction-functions"></a>Liste des fonctions de prédiction  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Toutefois, l'algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) prend en charge les fonctions supplémentaires décrites dans le tableau suivant.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[Décalage & #40 ; DMX & #41 ;](../../dmx/lag-dmx.md)|Retourne un nombre de tranches de temps entre la date du cas en cours et la dernière date du jeu d'apprentissage.<br /><br /> En règle générale, cette fonction est utilisée pour identifier les cas d'apprentissage récents afin de vous permettre de récupérer des données détaillées sur les cas.|  
|[PredictNodeId & #40 ; DMX & #41 ;](../../dmx/predictnodeid-dmx.md)|Retourne l'ID de nœud pour la colonne prédictible spécifiée.<br /><br /> En règle générale, cette fonction est utilisée pour identifier le nœud qui a généré une valeur prédite particulière afin que vous puissiez examiner les cas associés au nœud ou récupérer l'équation et d'autres détails.|  
|[PredictStdev & #40 ; DMX & #41 ;](../../dmx/predictstdev-dmx.md)|Retourne l'écart type des prédictions dans la colonne prédictible spécifiée.<br /><br /> Cette fonction remplace l'argument INCLUDE_STATISTICS, qui n'est pas pris en charge pour les modèles de séries chronologiques.|  
|[PredictVariance & #40 ; DMX & #41 ;](../../dmx/predictvariance-dmx.md)|Retourne la variance des prédictions pour la colonne prédictible spécifiée.<br /><br /> Cette fonction remplace l'argument INCLUDE_STATISTICS, qui n'est pas pris en charge pour les modèles de séries chronologiques.|  
|[PredictTimeSeries & #40 ; DMX & #41 ;](../../dmx/predicttimeseries-dmx.md)|Retourne des valeurs prédites historiques ou des valeurs prédites futures pour des séries chronologiques.<br /><br /> Vous pouvez aussi interroger des modèles de séries chronologiques en utilisant la fonction de prédiction générale, [Predict &#40;DMX&#41;](../../dmx/predict-dmx.md).|  
  
 Pour obtenir la liste des fonctions communes à tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)], consultez [Fonctions de prédiction générales &#40;DMX&#41;](../../dmx/general-prediction-functions-dmx.md). Pour la syntaxe de fonctions spécifiques, consultez [Fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)   
 [Algorithme de série chronologique de Microsoft](../../analysis-services/data-mining/microsoft-time-series-algorithm.md)   
 [Référence technique de Microsoft Time Series algorithme](../../analysis-services/data-mining/microsoft-time-series-algorithm-technical-reference.md)   
 [Contenu du modèle d’exploration de données pour les modèles de série chronologique & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-time-series-models-analysis-services-data-mining.md)  
  
  
