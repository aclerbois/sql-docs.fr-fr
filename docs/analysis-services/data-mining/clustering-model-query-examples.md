---
title: Exemples de requêtes modèle de clustering | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f6e415c0b82738ec153b20cb79e19af59bacba16
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="clustering-model-query-examples"></a>Exemples de requêtes de modèle de clustering
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez récupérer les métadonnées sur le modèle ou créer une requête de contenu qui fournit des détails sur les séquences découvertes dans l'analyse. Une autre solution consiste à créer une requête de prédiction qui utilise les séquences dans le modèle pour faire des prédictions pour les nouvelles données. Chaque type de requête fournit des informations différentes. Par exemple, une requête de contenu peut fournir des détails supplémentaires sur les clusters identifiés, tandis qu'une requête de prédiction peut vous dire à quel cluster un nouveau point de données est le plus susceptible d'appartenir.  
  
 Cette section explique comment créer des requêtes pour les modèles basés sur l'algorithme de clustering [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
 **Requêtes de contenu**  
  
 [Obtention des métadonnées du modèle avec DMX](#bkmk_Query1)  
  
 [Récupération des métadonnées du modèle de l'ensemble de lignes de schéma](#bkmk_Query2)  
  
 [Retour d'un cluster ou d'une liste de clusters](#bkmk_Query3)  
  
 [Retour d'attributs pour un cluster](#bkmk_Query4)  
  
 [Retour d'un profil de cluster à l'aide de procédures stockées système](#bkmk_Query5)  
  
 [Recherche de facteurs de discrimination pour un cluster](#bkmk_Query6)  
  
 [Retour des cas qui appartiennent à un cluster](#bkmk_Query7)  
  
 **Requêtes de prédiction**  
  
 [Prédiction de résultats à partir d'un modèle de clustering](#bkmk_Query8)  
  
 [Détermination de l'appartenance au cluster](#bkmk_Query9)  
  
 [Retour de tous les clusters possibles avec la probabilité et la distance](#bkmk_Query10)  
  
##  <a name="bkmk_top2"></a> Recherche d'informations sur le modèle  
 Tous les modèles d'exploration de données exposent le contenu appris par l'algorithme en fonction d'un schéma standardisé : l'ensemble de lignes de schéma du modèle d'exploration de données. Vous pouvez créer des requêtes sur l'ensemble de lignes de schéma du modèle d'exploration de données en utilisant des instructions DMX (Data Mining Extension). Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vous pouvez également interroger directement les ensemble de lignes de schéma en tant que tables système.  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : obtention des métadonnées du modèle à l'aide de DMX  
 La requête suivante retourne les métadonnées de base sur le modèle de clustering `TM_Clustering`que vous avez créé dans le Didacticiel sur l'exploration de données de base. Les métadonnées disponibles dans le nœud parent d'un modèle de clustering incluent le nom du modèle, la base de données où le modèle est stocké et le nombre de nœuds enfants dans le modèle. Cette requête utilise une requête de contenu DMX pour récupérer les métadonnées du nœud parent du modèle:  
  
```  
SELECT MODEL_CATALOG, MODEL_NAME, NODE_CAPTION,   
NODE_SUPPORT, [CHILDREN_CARDINALITY], NODE_DESCRIPTION  
FROM TM_Clustering.CONTENT  
WHERE NODE_TYPE = 1  
```  
  
> [!NOTE]  
>  Vous devez mettre le nom de la colonne, CHILDREN_CARDINALITY, entre parenthèses pour le distinguer du mot clé réservé MDX (Multidimensional Expressions) qui porte le même nom.  
  
 Résultats de l'exemple :  
  
|||  
|-|-|  
|MODEL_CATALOG|TM_Clustering|  
|MODEL_NAME|Adventure Works DW|  
|NODE_CAPTION|Cluster Model|  
|NODE_SUPPORT|12939|  
|CHILDREN_CARDINALITY|10|  
|NODE_DESCRIPTION|Tous|  
  
 Pour connaître la signification de ces colonnes dans un modèle de clustering, consultez [Contenu du modèle d’exploration de données pour les modèles de clustering &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-clustering-models-analysis-services-data-mining.md).  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query2"></a> Exemple de requête 2 : récupération des métadonnées du modèle de l'ensemble de lignes de schéma  
 En interrogeant l'ensemble de lignes de schéma d'exploration de données, vous pouvez obtenir les mêmes informations que celles retournées par une requête de contenu DMX. Toutefois, l'ensemble de lignes de schéma fournit quelques colonnes supplémentaires, telles que les paramètres utilisés lorsque le modèle a été créé, la date et l'heure auxquelles le modèle a été traité pour la dernière fois et le propriétaire du modèle.  
  
 L'exemple suivant retourne la date à laquelle le modèle a été créé, modifié et traité pour la dernière fois, ainsi que les paramètres de clustering utilisés pour générer le modèle et la taille du jeu d'apprentissage. Ces informations peuvent être utiles pour documenter le modèle ou pour déterminer les options de clustering qui ont été utilisées pour créer un modèle existant.  
  
```  
SELECT MODEL_NAME, DATE_CREATED, LAST_PROCESSED, PREDICTION_ENTITY, MINING_PARAMETERS   
from $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = 'TM_Clustering'  
```  
  
 Résultats de l'exemple :  
  
|||  
|-|-|  
|MODEL_NAME|TM_Clustering|  
|DATE_CREATED|10/12/2007 7:42:51 PM|  
|LAST_PROCESSED|10/12/2007 8:09:54 PM|  
|PREDICTION_ENTITY|Bike Buyer|  
|MINING_PARAMETERS|CLUSTER_COUNT=10,<br /><br /> CLUSTER_SEED=0,<br /><br /> CLUSTERING_METHOD=1,<br /><br /> MAXIMUM_INPUT_ATTRIBUTES=255,<br /><br /> MAXIMUM_STATES=100,<br /><br /> MINIMUM_SUPPORT=1,<br /><br /> MODELLING_CARDINALITY=10,<br /><br /> SAMPLE_SIZE=50000,<br /><br /> STOPPING_TOLERANCE=10|  
  
 [Retour en haut](#bkmk_top2)  
  
## <a name="finding-information-about-clusters"></a>Recherche d'informations sur les Clusters  
 Les requêtes de contenu les plus utiles sur les modèles de clustering retournent généralement le même type d'informations que celles auxquelles vous pouvez accéder à l'aide de **Cluster Viewer**. Il s'agit notamment des profils, des caractéristiques et de la discrimination de cluster. Cette section fournit des exemples de requêtes qui récupèrent ces informations.  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3: retour d'un cluster ou d'une liste de clusters  
 Tous les clusters ayant un type de nœud égal à 5, vous pouvez facilement récupérer une liste des clusters en interrogeant uniquement le contenu du modèle au sujet des nœuds de ce type. Vous pouvez également filtrer les nœuds retournés par probabilité ou par prise en charge, comme le montre cet exemple.  
  
```  
SELECT NODE_NAME, NODE_CAPTION ,NODE_SUPPORT, NODE_DESCRIPTION  
FROM TM_Clustering.CONTENT  
WHERE NODE_TYPE = 5 AND NODE_SUPPORT > 1000  
```  
  
 Résultats de l'exemple :  
  
|||  
|-|-|  
|NODE_NAME|002|  
|NODE_CAPTION|Cluster 2|  
|NODE_SUPPORT|1649|  
|NODE_DESCRIPTION|English Education=Graduate Degree , 32 <=Age <=48 , Number Cars Owned=0 , 35964.0771121808 <=Yearly Income <=97407.7163393957 , English Occupation=Professional , Commute Distance=2-5 Miles , Region=North America , Bike Buyer=1 , Number Children At Home=0 , Number Cars Owned=1 , Commute Distance=0-1 Miles , English Education=Bachelors , Total Children=1 , Number Children At Home=2 , English Occupation=Skilled Manual , Marital Status=S , Total Children=0 , House Owner Flag=0 , Gender=F , Total Children=2 , Region=Pacific|  
  
 Les attributs qui définissent le cluster se trouvent dans deux colonnes dans l'ensemble de lignes de schéma d'exploration de données.  
  
-   La colonne NODE_DESCRIPTION contient une liste séparée par des virgules d'attributs. Notez qu'il est possible d'abréger la liste d'attributs à des fins d'affichage.  
  
-   La table imbriquée dans la colonne NODE_DISTRIBUTION contient la liste complète des attributs du cluster. Si votre client ne prend pas en charge d'ensembles de lignes hiérarchiques, vous pouvez retourner la table imbriquée en ajoutant le mot clé FLATTENED avant la liste des colonnes SELECT. Pour plus d’informations sur l’utilisation du mot clé FLATTENED, consultez [SELECT FROM &#60;modèle&#62;.CONTENT &#40;DMX&#41;](../../dmx/select-from-model-content-dmx.md).  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query4"></a> Exemple de requête 4 : retour d'attributs pour un cluster  
 Pour chaque cluster, **Cluster Viewer** affiche un profil qui répertorie les attributs et leurs valeurs. La visionneuse affiche également un histogramme qui montre la distribution des valeurs pour toute la population de cas dans le modèle. Si vous parcourez le modèle dans la visionneuse, vous pouvez copier facilement l'histogramme de la Légende d'exploration de données et le coller dans un document Excel ou Word. Vous pouvez également utiliser le volet Caractéristiques du cluster de la visionneuse pour comparer graphiquement les attributs de différents clusters.  
  
 Toutefois, si vous devez obtenir des valeurs pour plusieurs clusters à la fois, il est plus facile d'interroger le modèle. Par exemple, lorsque vous parcourez le modèle, vous pouvez remarquer que les deux premiers clusters diffèrent d'un attribut, `Number Cars Owned`. Par conséquent, vous souhaitez extraire les valeurs pour chaque cluster.  
  
```  
SELECT TOP 2 NODE_NAME,   
(SELECT ATTRIBUTE_VALUE, [PROBABILITY] FROM NODE_DISTRIBUTION WHERE ATTRIBUTE_NAME = 'Number Cars Owned')  
AS t  
FROM [TM_Clustering].CONTENT  
WHERE NODE_TYPE = 5  
```  
  
 La première ligne du code spécifie que vous souhaitez uniquement les deux premiers clusters.  
  
> [!NOTE]  
>  Par défaut, les clusters sont classés par prise en charge. Par conséquent, la colonne NODE_SUPPORT peut être omise.  
  
 La deuxième ligne du code ajoute une instruction de sous-sélection qui retourne uniquement certaines colonnes de la colonne de table imbriquée. En outre, elle restreint les lignes de la table imbriquée à celles qui sont liées à l'attribut cible, `Number Cars Owned`. Pour simplifier l'affichage, la table imbriquée possède un alias.  
  
> [!NOTE]  
>  Vous devez mettre la colonne de table imbriquée, `PROBABILITY`, entre parenthèses car un mot clé MDX réservé porte le même nom.  
>   
>  Résultats de l'exemple :  
  
|NODE_NAME|T.ATTRIBUTE_VALUE|T.PROBABILITY|  
|----------------|------------------------|-------------------|  
|001|2|0.829207754|  
|001|1|0.109354156|  
|001|3|0.034481552|  
|001|4|0.013503302|  
|001|0|0.013453236|  
|001|Manquant|0|  
|002|0|0.576980023|  
|002|1|0.406623939|  
|002|2|0.016380082|  
|002|3|1.60E-05|  
|002|4|0|  
|002|Manquant|0|  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query5"></a> Exemple de requête 5 : retour d'un profil de cluster à l'aide de procédures stockées système  
 Pour aller plus vite, au lieu d'écrire vos propres requêtes avec DMX, vous pouvez appeler les procédures stockées système que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise pour traiter les clusters. L'exemple suivant illustre comment utiliser les procédures stockées internes pour retourner le profil pour un cluster avec l'ID 002.  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterProfiles('TM_Clustering", '002',0.0005  
```  
  
 De la même façon, vous pouvez utiliser une procédure stockée système pour retourner les caractéristiques d'un cluster spécifique, comme le montre l'exemple suivant :  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterCharacteristics('TM_Clustering", '009',0.0005  
```  
  
 Résultats de l'exemple :  
  
|Attributs|Valeurs|Fréquence|Support technique|  
|----------------|------------|---------------|-------------|  
|Number Children at Home|0|0.999999829076798|899|  
|Région|North America|0.999852875241508|899|  
|Total Children|0|0.993860958572323|893|  
  
> [!NOTE]  
>  Les procédures stockées système d'exploration de données sont destinées à une utilisation interne et [!INCLUDE[msCoName](../../includes/msconame-md.md)] se réserve le droit de les modifier si nécessaire. Pour une utilisation dans un environnement de production, nous vous recommandons de créer les requêtes avec DMX, AMO ou XMLA.  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query6"></a> Exemple de requête 6 : recherche de facteurs de discrimination pour un cluster  
 L’onglet **Discrimination de cluster** de **Cluster Viewer** vous permet de comparer facilement un cluster à un autre ou de comparer un cluster avec tous les cas restants (le complément du cluster).  
  
 Toutefois, la création de requêtes pour retourner ces informations peut être complexe, et un traitement additionnel sur le client peut être nécessaire pour stocker les résultats temporaires et comparer les résultats de deux requêtes ou plus. Pour aller plus vite, vous pouvez utiliser les procédures stockées système.  
  
 La requête suivante retourne une table unique qui indique les facteurs de discrimination principaux entre les deux clusters avec les ID de nœud 009 et 007. Les attributs avec des valeurs positives favorisent le cluster 009, tandis que les attributs avec des valeurs négatives favorisent le cluster 007.  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterDiscrimination('TM_Clustering','009','007',0.0005,true)  
```  
  
 Résultats de l'exemple :  
  
|Attributs|Valeurs|Score|  
|----------------|------------|-----------|  
|Région|North America|100|  
|English Occupation|Skilled Manual|94.9003803898654|  
|Région|Europe|-72.5041051379789|  
|English Occupation|Manuel|-69.6503163202722|  
  
 Ces informations sont les mêmes que celles présentées dans le graphique de la visionneuse **Discrimination de cluster** si vous sélectionnez le cluster 9 dans la première liste déroulante et le cluster 7 dans la deuxième. Pour comparer le cluster 9 avec son complément, vous utilisez la chaîne vide dans le deuxième paramètre, comme le montre l'exemple suivant :  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterDiscrimination('TM_Clustering','009','',0.0005,true)  
```  
  
> [!NOTE]  
>  Les procédures stockées système d'exploration de données sont destinées à une utilisation interne et [!INCLUDE[msCoName](../../includes/msconame-md.md)] se réserve le droit de les modifier si nécessaire. Pour une utilisation dans un environnement de production, nous vous recommandons de créer les requêtes avec DMX, AMO ou XMLA.  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query7"></a> Exemple de requête 7 : retour des cas qui appartiennent à un cluster  
 Si l'extraction a été activée sur le modèle d'exploration de données, vous pouvez créer des requêtes qui retournent des informations détaillées sur les cas utilisés dans le modèle. De plus, si l’extraction a été activée sur la structure d’exploration de données, vous pouvez inclure des colonnes de la structure sous-jacente à l’aide de la fonction [StructureColumn &#40;DMX&#41;](../../dmx/structurecolumn-dmx.md).  
  
 L'exemple suivant retourne deux colonnes utilisées dans le modèle (Age et Region), plus une colonne supplémentaire (First Name) qui n'a pas été utilisée dans le modèle. La requête retourne uniquement les cas qui étaient classifiés dans le Cluster 1.  
  
```  
SELECT [Age], [Region], StructureColumn('First Name')  
FROM [TM_Clustering].CASES  
WHERE IsInNode('001')  
```  
  
 Pour retourner les cas qui appartiennent à un cluster, vous devez connaître l'ID du cluster. Vous pouvez obtenir l'ID du cluster en parcourant le modèle dans l'une des visionneuses. Ou vous pouvez renommer un cluster pour en faciliter la référence, ce qui vous permet ensuite d'utiliser le nom au lieu d'un numéro d'ID. Toutefois, sachez que les noms que vous attribuez à un cluster seront perdus en cas de retraitement du modèle.  
  
 [Retour en haut](#bkmk_top2)  
  
## <a name="making-predictions-using-the-model"></a>Exécution de prédictions à l'aide du modèle  
 Bien que le clustering soit généralement utilisé pour décrire et comprendre des données, l'implémentation [!INCLUDE[msCoName](../../includes/msconame-md.md)] vous permet également de faire des prédictions sur l'appartenance au cluster et de retourner des probabilités associées à la prédiction. Cette section fournit des exemples de création de requêtes de prédiction sur des modèles de clustering. Vous pouvez faire des prédictions pour plusieurs cas, en spécifiant une source de données tabulaire, ou vous pouvez fournir une par une de nouvelles valeurs en créant une requête singleton. Pour plus de clarté, les exemples dans cette section sont tous des requêtes singleton.  
  
 Pour plus d’informations sur la création de requêtes de prédiction avec DMX, consultez [Outils de requête d’exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md).  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query8"></a> Exemple de requête 8 : prédiction de résultats à partir d'un modèle de clustering  
 Si le modèle de clustering que vous créez contient un attribut prévisible, vous pouvez utiliser le modèle pour faire des prédictions sur les résultats. Toutefois, le modèle gère différemment l'attribut prédictible selon que vous attribuez la valeur **Predict** ou **PredictOnly**à la colonne prédictible. Si vous attribuez la valeur **Predict**à la colonne, les valeurs de cet attribut sont ajoutées au modèle de clustering et apparaissent sous forme d'attributs dans le modèle fini. Toutefois, si vous attribuez la valeur **PredictOnly**à la colonne, les valeurs ne sont pas utilisées pour créer des clusters. Au lieu de cela, une fois le mode terminé, l'algorithme de clustering crée de nouvelles valeurs pour l'attribut **PredictOnly** en fonction des clusters auxquels chaque cas appartient.  
  
 La requête suivante fournit un nouveau cas unique au modèle, où les seules informations sur le cas sont l'âge et le sexe. L’instruction SELECT spécifie la paire attribut/valeur prévisible qui vous intéresse, et la fonction [PredictProbability &#40;DMX&#41;](../../dmx/predictprobability-dmx.md) indique la probabilité qu’un cas avec ces attributs ait le résultat désiré.  
  
```  
SELECT  
  [TM_Clustering].[Bike Buyer], PredictProbability([Bike Buyer],1)  
FROM  
  [TM_Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 40 AS [Age],  
  'F' AS [Gender]) AS t  
```  
  
 Exemple de résultats lorsque l'utilisation a pour valeur **Predict**:  
  
|Bike Buyer|Expression|  
|----------------|----------------|  
|1|0.592924735740338|  
  
 Exemple de résultats lorsque l'utilisation a pour valeur **PredictOnly** et que le modèle est retraité :  
  
|Bike Buyer|Expression|  
|----------------|----------------|  
|1|0.55843544003102|  
  
 Dans cet exemple, la différence dans le modèle n'est pas significative. Toutefois, il peut parfois être important de détecter les différences entre la distribution réelle de valeurs et ce que le modèle prédit. L’onglet [PredictCaseLikelihood &#40;DMX&#41;](../../dmx/predictcaselikelihood-dmx.md) est utile dans ce scénario, car elle indique la probabilité qu’un cas ce produise pour un modèle donné.  
  
 Le nombre retourné par la fonction PredictCaseLikelihood est une probabilité. Par conséquent, il est donc toujours égal à 0 ou 1, la valeur 0,5 représentant un résultat aléatoire. Un score inférieur à 0,5 signifie que le cas prédit est improbable pour le modèle donné, tandis qu'un score supérieur à 0,5 indique que le cas prédit est plus susceptible d'être adapté au modèle que de ne pas l'être.  
  
 Par exemple, la requête suivante retourne deux valeurs qui caractérisent la vraisemblance d'un nouveau cas d'exemple. La valeur non normalisée représente la probabilité pour le modèle actuel donné. Lorsque vous utilisez le mot clé NORMALIZED, le score de vraisemblance retourné par la fonction est ajusté en divisant la « probabilité avec le modèle » par la « probabilité sans le modèle ».  
  
```  
SELECT  
PredictCaseLikelihood(NORMALIZED) AS [NormalizedValue], PredictCaseLikelihood(NONNORMALIZED) AS [NonNormalizedValue]  
FROM  
  [TM_Clustering_PredictOnly]  
NATURAL PREDICTION JOIN  
(SELECT 40 AS [Age],  
  'F' AS [Gender]) AS t  
```  
  
 Résultats de l'exemple :  
  
|NormalizedValue|NonNormalizedValue|  
|---------------------|------------------------|  
|5.56438372679893E-11|8.65459953145182E-68|  
  
 Notez que les nombres dans ces résultats sont exprimés en notation scientifique.  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query9"></a> Exemple de requête 9 : identification de l'appartenance au cluster  
 Cet exemple utilise la fonction [Cluster &#40;DMX&#41;](../../dmx/cluster-dmx.md) pour retourner le cluster auquel le nouveau cas est le plus susceptible d’appartenir, et la fonction [ClusterProbability &#40;DMX&#41;](../../dmx/clusterprobability-dmx.md) pour retourner la probabilité d’appartenance à ce cluster.  
  
```  
SELECT Cluster(), ClusterProbability()  
FROM  
  [TM_Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 40 AS [Age],  
  'F' AS [Gender],  
  'S' AS [Marital Status]) AS t  
```  
  
 Résultats de l'exemple :  
  
|$CLUSTER|Expression|  
|--------------|----------------|  
|Cluster 2|0.397918596951617|  
  
 **Remarque** Par défaut, la fonction **ClusterProbability** retourne la probabilité du cluster le plus probable. Toutefois, vous pouvez spécifier un cluster différent en utilisant la syntaxe `ClusterProbability('cluster name')`. Si vous procédez ainsi, sachez que les résultats de chaque fonction de prédiction sont indépendants des autres résultats. Par conséquent, le score de probabilité dans la deuxième colonne peut faire référence à un cluster différent que le cluster nommé dans la première colonne.  
  
 [Retour en haut](#bkmk_top2)  
  
###  <a name="bkmk_Query10"></a> Exemple de requête 10 : retour de tous les clusters possibles avec la probabilité et la distance  
 Dans l'exemple précédent, le score de probabilité n'était pas très élevé. Pour déterminer si un meilleur cluster existe, vous pouvez utiliser la fonction [PredictHistogram &#40;DMX&#41;](../../dmx/predicthistogram-dmx.md) avec la fonction [Cluster &#40;DMX&#41;](../../dmx/cluster-dmx.md) pour retourner une table imbriquée qui inclut tous les clusters possibles, ainsi que la probabilité que le nouveau cas appartienne à chaque cluster. Le mot clé FLATTENED est utilisé pour remplacer l'ensemble de lignes hiérarchique par une table à plat à des fins de visualisation.  
  
```  
SELECT FLATTENED PredictHistogram(Cluster())  
From  
  [TM_Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 40 AS [Age],  
  'F' AS [Gender],  
  'S' AS [Marital Status])  
```  
  
|Expression.$CLUSTER|Expression.$DISTANCE|Expression.$PROBABILITY|  
|-------------------------|--------------------------|-----------------------------|  
|Cluster 2|0.602081403048383|0.397918596951617|  
|Cluster 10|0.719691686785675|0.280308313214325|  
|Cluster 4|0.867772590378791|0.132227409621209|  
|Cluster 5|0.931039872200985|0.0689601277990149|  
|Cluster 3|0.942359230072167|0.0576407699278328|  
|Cluster 6|0.958973668972756|0.0410263310272437|  
|Cluster 7|0.979081275926724|0.0209187240732763|  
|Cluster 1|0.999169044818624|0.000830955181376364|  
|Cluster 9|0.999831227795894|0.000168772204105754|  
|Cluster 8|1|0|  
  
 Par défaut, les résultats sont classés par probabilité. Les résultats indiquent que le Cluster 2, bien que sa probabilité soit assez faible, est toujours le mieux adapté au nouveau point de données.  
  
 **Remarque** La colonne supplémentaire `$DISTANCE`représente la distance du point de données au cluster. Par défaut, l'algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] utilise le clustering EM évolutif qui attribue plusieurs clusters à chaque point de données et qui classe les clusters possibles.  Toutefois, si vous créez votre modèle de clustering à l'aide de l'algorithme K-means, un seul cluster peut être attribué à chaque point de données, et cette requête retournerait une seule ligne. Il est nécessaire de comprendre ces différences pour interpréter les résultats de la fonction [PredictCaseLikelihood &#40;DMX&#41;](../../dmx/predictcaselikelihood-dmx.md) . Pour plus d’informations sur les différences entre le clustering EM et le clustering K-means, consultez [Références techniques relatives à l’algorithme de gestion de clusters Microsoft](../../analysis-services/data-mining/microsoft-clustering-algorithm-technical-reference.md).  
  
 [Retour en haut](#bkmk_top2)  
  
## <a name="function-list"></a>Liste de fonctions  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Toutefois, les modèles créés avec l'algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge les fonctions supplémentaires répertoriées dans le tableau suivant.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[Cluster &#40;DMX&#41;](../../dmx/cluster-dmx.md)|Retourne le cluster qui est le plus susceptible de contenir le cas d'entrée.|  
|[ClusterDistance &#40;DMX&#41;](../../dmx/clusterdistance-dmx.md)|Retourne la distance séparant le cas d'entrée du cluster spécifié ou, si aucun cluster n'est indiqué, la distance séparant le cas d'entrée du cluster le plus probable.<br /><br /> Retourne la probabilité que le cas d'entrée appartient au cluster spécifié.|  
|[ClusterProbability &#40;DMX&#41;](../../dmx/clusterprobability-dmx.md)|Retourne la probabilité que le cas d'entrée appartient au cluster spécifié.|  
|[IsDescendant & #40 ; DMX & #41 ;](../../dmx/isdescendant-dmx.md)|Détermine si un nœud est un enfant d'un autre nœud dans le modèle.|  
|[IsInNode & #40 ; DMX & #41 ;](../../dmx/isinnode-dmx.md)|Indique si le nœud spécifié contient le cas courant.|  
|[PredictAdjustedProbability & #40 ; DMX & #41 ;](../../dmx/predictadjustedprobability-dmx.md)|Retourne la probabilité pondérée.|  
|[PredictAssociation & #40 ; DMX & #41 ;](../../dmx/predictassociation-dmx.md)|Prédit l'appartenance à un dataset associatif.|  
|[PredictCaseLikelihood &#40;DMX&#41;](../../dmx/predictcaselikelihood-dmx.md)|Retourne le degré de vraisemblance de l'intégration d'un cas d'entrée au modèle existant.|  
|[PredictHistogram &#40;DMX&#41;](../../dmx/predicthistogram-dmx.md)|Retourne une table des valeurs associées à la valeur prédite actuelle.|  
|[PredictNodeId & #40 ; DMX & #41 ;](../../dmx/predictnodeid-dmx.md)|Retourne Node_ID pour chaque cas.|  
|[PredictProbability & #40 ; DMX & #41 ;](../../dmx/predictprobability-dmx.md)|Retourne la probabilité pour la valeur prédite.|  
|[PredictStdev & #40 ; DMX & #41 ;](../../dmx/predictstdev-dmx.md)|Retourne l'écart-type prévu pour la colonne spécifiée.|  
|[PredictSupport & #40 ; DMX & #41 ;](../../dmx/predictsupport-dmx.md)|Retourne la valeur de support pour un état spécifié.|  
|[PredictVariance & #40 ; DMX & #41 ;](../../dmx/predictvariance-dmx.md)|Retourne la variance d'une colonne spécifiée.|  
  
 Pour en savoir plus sur la syntaxe de fonctions spécifiques, consultez [Informations de référence sur les fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)   
 [Microsoft Clustering algorithme informations techniques de référence](../../analysis-services/data-mining/microsoft-clustering-algorithm-technical-reference.md)   
 [Algorithme de gestion de clusters Microsoft](../../analysis-services/data-mining/microsoft-clustering-algorithm.md)  
  
  
