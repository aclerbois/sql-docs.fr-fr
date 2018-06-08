---
title: Exemples de requêtes modèle de Clustering de séquence | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e14fd39a1e917532b61b9f55415281e53598e564
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="sequence-clustering-model-query-examples"></a>Sequence Clustering Model Query Examples
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez soit créer une requête de contenu, qui fournit des détails sur les informations stockées dans le modèle, soit créer une requête de prédiction, qui utilise les séquences du modèle pour effectuer des prédictions basées sur les nouvelles données que vous fournissez. Pour un modèle Sequence Clustering, les requêtes de contenu fournissent en général des détails supplémentaires à propos des clusters trouvés ou les transitions dans ces clusters. Vous pouvez également extraire les métadonnées relatives au modèle en utilisant une requête.  
  
 Les requêtes de prédiction sur un modèle Sequence Clustering font en général des recommandations basées sur les séquences et transitions, sur les attributs non-séquence inclus dans le modèle ou sur une combinaison de séquence et attributs non-séquence.  
  
 Cette section explique comment créer des requêtes pour les modèles basés sur l'algorithme MSC (Microsoft Sequence Clustering). Pour obtenir des informations générales sur la création de requêtes, consultez [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md).  
  
 **Content Queries**  
  
 [Utilisation de l'ensemble de lignes de schéma d'exploration de données pour retourner les paramètres du modèle](#bkmk_Query1)  
  
 [Obtention d'une liste de séquences pour un état](#bkmk_Query2)  
  
 [Utilisation des procédures stockées système](#bkmk_Query3)  
  
 **Requêtes de prédiction**  
  
 [Prédiction d'un ou de plusieurs états suivants](#bkmk_Query4)  
  
##  <a name="bkmk_ContentQueries"></a> Recherche d'informations sur le modèle Sequence Clustering  
 Pour créer des requêtes pertinentes sur le contenu d'un modèle d'exploration de données, vous devez comprendre la structure du contenu du modèle et les types d'informations stockés dans les différents types de nœuds. Pour plus d’informations, consultez [Contenu du modèle d’exploration de données pour les modèles Sequence Clustering &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-sequence-clustering-models.md).  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : utilisation de l'ensemble de lignes de schéma d'exploration de données pour retourner les paramètres du modèle  
 En interrogeant l'ensemble de lignes de schéma d'exploration de données, vous pouvez rechercher plusieurs types d'informations sur le modèle, dont les métadonnées de base, sa date et heure de création, le moment où il a été traité pour la dernière fois, le nom de la structure d'exploration de données sur laquelle il est basé et la colonne utilisée comme attribut prévisible.  
  
 La requête suivante retourne les paramètres utilisés pour générer et procéder à l'apprentissage du modèle, `[Sequence Clustering]`. Vous pouvez créer ce modèle dans  la leçon 5 du [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c).  
  
```  
SELECT MINING_PARAMETERS   
from $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = 'Sequence Clustering'  
```  
  
 Résultats de l'exemple :  
  
|MINING_PARAMETERS|  
|------------------------|  
|CLUSTER_COUNT=15,MINIMUM_SUPPORT=10,MAXIMUM_STATES=100,MAXIMUM_SEQUENCE_STATES=64|  
  
 Notez que ce modèle a été construit en utilisant la valeur par défaut de 10 pour CLUSTER_COUNT. Lorsque vous spécifiez un nombre différent de zéro de clusters pour CLUSTER_COUNT, l'algorithme cherche à trouver ce nombre. Toutefois, l'algorithme peut trouver plus ou moins de clusters dans le processus d'analyse. Dans ce cas, l'algorithme a trouvé que 15 clusters ont correspondu le mieux aux données d'apprentissage. Par conséquent, la liste de valeurs de paramètre pour le modèle terminé indique le nombre de clusters tel qu'il a été déterminé par l'algorithme, et non la valeur indiquée lors de la création du modèle.  
  
 En quoi ce comportement diffère-t-il par rapport au fait de permettre à l'algorithme de déterminer le meilleur nombre de clusters ? Pour l'expérience, vous pouvez créer un autre modèle de clustering qui utilise ces mêmes données, mais en définissant CLUSTER_COUNT sur 0. Lorsque vous faites cela, l'algorithme détecte 32 clusters. Par conséquent, vous limitez le nombre de résultats en utilisant la valeur par défaut de 10 pour CLUSTER_COUNT.  
  
 La valeur de 10 est utilisée par défaut parce que la réduction du nombre de clusters simplifie pour la plupart des personnes la navigation dans les regroupements des données et leur compréhension. Toutefois, chaque modèle et chaque jeu de données sont différents. Vous pouvez tester différents nombres de clusters pour voir quelle valeur de paramètre donne le modèle le plus exact.  
  
###  <a name="bkmk_Query2"></a> Requête d'exemple 2 : réception d'une liste de séquences pour un état  
 Le contenu du modèle d'exploration de données stocke les séquences trouvées dans les données d'apprentissage comme un premier état couplé à une liste de tous les deuxièmes états. Le premier état est utilisé comme étiquette pour la séquence et les deuxièmes états connexes sont appelés des transitions.  
  
 Par exemple, la requête suivante retourne la liste complète de premiers états dans le modèle, avant que les séquences soient regroupées dans des clusters.  Vous pouvez obtenir cette liste en retournant la liste des séquences (NODE_TYPE = 13) qui ont le nœud racine du modèle comme parent (PARENT_UNIQUE_NAME = 0). Le mot clé FLATTENED simplifie la lecture des résultats.  
  
> [!NOTE]  
>  Le nom des colonnes, PARENT_UNIQUE_NAME, support et probabilité, doivent être mis entre crochets afin de le distinguer des mots clés réservés du même nom.  
  
```  
SELECT FLATTENED NODE_UNIQUE_NAME,  
(SELECT ATTRIBUTE_VALUE AS [Product 1],  
[Support] AS [Sequence Support],   
[Probability] AS [Sequence Probability]  
FROM NODE_DISTRIBUTION) AS t  
FROM [Sequence Clustering].CONTENT  
WHERE NODE_TYPE = 13  
AND [PARENT_UNIQUE_NAME] = 0  
```  
  
 Résultats partiels :  
  
|NODE_UNIQUE_NAME|Produit 1|Support de séquence|Probabilité de la séquence|  
|------------------------|---------------|----------------------|--------------------------|  
|1081327|Manquant|0|#######|  
|1081327|All-Purpose Bike Stand|17|0.00111|  
|1081327|Bike Wash|64|0.00418|  
|1081327|(les lignes 4-36 sont omises)|||  
|1081327|Women's Mountain Shorts|506|0.03307|  
  
 La liste de séquences dans le modèle est toujours triée alphabétiquement en ordre croissant. Le classement des séquences est important parce que vous pouvez rechercher les transitions connexes en regardant le numéro d'ordre de la séquence. La valeur **Missing** est toujours transition 0.  
  
 Par exemple, dans les résultats précédents, le produit "Women's Mountain Shorts" est le numéro de séquence 37 du modèle. Vous pouvez utiliser ces informations pour consulter tous les produits qui ont été achetés après "Women's Mountain Shorts".  
  
 Pour cela, référencez d'abord la valeur retournée pour NODE_UNIQUE_NAME dans la requête précédente, pour obtenir l'ID du nœud qui contient toutes les séquences du modèle. Entrez cette valeur dans la requête comme l'ID du nœud parent pour obtenir uniquement les transitions incluses dans ce nœud, qui contient une liste de toutes les séquences du modèle. Toutefois, si vous souhaitez consulter la liste de transitions d'un cluster spécifique, vous pouvez entrer l'ID du nœud de cluster et consulter uniquement les séquences associées à ce cluster.  
  
```  
SELECT NODE_UNIQUE_NAME  
FROM [Sequence Clustering].CONTENT  
WHERE NODE_DESCRIPTION = 'Transition row for sequence state 37'  
AND [PARENT_UNIQUE_NAME] = '1081327'  
```  
  
 Résultats de l'exemple :  
  
|NODE_UNIQUE_NAME|  
|------------------------|  
|1081365|  
  
 Le nœud représenté par cet ID contient une liste des séquences qui suivent le produit « Women's Mountain Shorts », avec les valeurs de support et de probabilité.  
  
```  
SELECT FLATTENED  
(SELECT ATTRIBUTE_VALUE AS Product2,  
[Support] AS [P2 Support],  
[Probability] AS [P2 Probability]  
FROM NODE_DISTRIBUTION) AS t  
FROM [Sequence Clustering].CONTENT  
WHERE NODE_UNIQUE_NAME = '1081365'  
```  
  
 Résultats de l'exemple :  
  
|t.Product2|t.P2 Support|t.P2 Probability|  
|----------------|------------------|----------------------|  
|Manquant|230.7419|0.456012|  
|Classic Vest|8.16129|0.016129|  
|Cycling Cap|60.83871|0.120235|  
|Half-Finger Gloves|30.41935|0.060117|  
|Long-Sleeve Logo Jersey (Pull-over à manches longues)|86.80645|0.171554|  
|Racing Socks|28.93548|0.057185|  
|Short-Sleeve Classic Jersey|60.09677|0.118768|  
  
 Notez que le support des diverses séquences en rapport avec Women's Mountain Shorts est 506 dans le modèle. Les valeurs de support pour les transitions donnent également un total de 506. Toutefois, les nombres ne sont pas des nombres entiers, ce qui semble un peu étrange lorsque l'on s'attend à ce que le support représente simplement un nombre de cas qui contiennent chaque transition. Toutefois, la probabilité de toute transition dans un cluster doit être pondérée par sa probabilité d'appartenir à ce cluster particulier parce que la méthode de création des clusters calcule l'appartenance partielle.  
  
 Par exemple, s'il y a quatre clusters, une séquence particulière peut avoir 40 % de chance d'appartenir au cluster 1, 30 % de chance d'appartenir au cluster 2, 20 % de chance d'appartenir au cluster 3 et 100 % de chance d'appartenir au cluster 4. Lorsque l'algorithme a déterminé le cluster auquel la transition a le plus de chance d'appartenir, il pondère les probabilités au sein du cluster par la probabilité a priori du cluster.  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3 : utilisation des procédures stockées système  
 Vous pouvez voir à partir de ces exemples de requête que les informations stockées dans le modèle sont complexes et que vous devrez peut-être créer plusieurs requêtes pour obtenir les informations dont vous avez besoin. Toutefois, la visionneuse de l'algorithme MSC fournit un jeu performant d'outils pour parcourir graphiquement les informations contenues dans une séquence de modèle Sequence Clustering. Vous pouvez également utiliser la visionneuse pour interroger et extraire dans le modèle.  
  
 Dans la plupart des cas, les informations présentées dans la visionneuse de l'algorithme MSC sont créées en utilisant des procédures stockées système Analysis Services pour interroger le modèle. Vous pouvez écrire des requêtes Data Mining Extensions (DMX) sur le contenu du modèle pour extraire les mêmes informations, mais les procédures stockées système Analysis Services fournissent un raccourci commode lors de l'exploration ou du test des modèles.  
  
> [!NOTE]  
>  Les procédures stockées système sont utilisées pour le traitement interne par le serveur et par les clients que Microsoft fournit pour interagir avec le serveur Analysis Services. Par conséquent, Microsoft se réserve le droit de les modifier n'importe quand. Bien qu'ils soient décrits ici pour plus de commodité, nous ne prenons pas en charge leur utilisation dans un environnement de production. Pour garantir stabilité et compatibilité dans un environnement de production, vous devez toujours écrire vos propres requêtes en utilisant DMX.  
  
 Cette section fournit quelques exemples sur la façon d'utiliser les procédures stockées système pour créer des requêtes sur un modèle Sequence Clustering :  
  
#### <a name="cluster-profiles-and-sample-cases"></a>Profils du cluster et cas d'exemple  
 L'onglet Profils du cluster affiche une liste des clusters dans le modèle, la taille de chaque cluster et un histogramme qui indique les états inclus dans le cluster. Il existe deux procédures stockées système que vous pouvez utiliser dans les requêtes pour extraire des informations semblables :  
  
-   `GetClusterProfile` retourne les caractéristiques du cluster, avec toutes les informations trouvées dans la table NODE_DISTRIBUTION du cluster.  
  
-   `GetNodeGraph` retourne les nœuds et les contours qui peuvent être utilisés pour construire une représentation graphique mathématique des clusters, correspondant à ce que vous voyez sur le premier onglet de la vue Sequence Clustering. Les nœuds sont des clusters et les contours représentent des poids ou force.  
  
 L'exemple suivant illustre la manière d'utiliser la procédure stockée système `GetClusterProfiles`pour retourner tous les clusters du modèle, avec leurs profils respectifs. Cette procédure stockée exécute une série d'instructions DMX qui retournent le jeu complet de profils dans le modèle. Toutefois, pour utiliser cette procédure stockée, vous devez connaître l'adresse du modèle.  
  
 `CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterProfiles('Sequence Clustering', 2147483647, 0)`  
  
 L'exemple suivant illustre la façon d'extraire le profil pour un cluster spécifique, Cluster 12, en utilisant la procédure stockée système `GetNodeGraph`et en spécifiant l'ID du cluster, qui est habituellement le même que le numéro dans le nom du cluster.  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetNodeGraph('Sequence Clustering','12',0)  
```  
  
 Si vous omettez l'ID du cluster, comme affiché dans la requête suivante, `GetNodeGraph` retourne une liste triée et aplatie de tous les profils :  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetNodeGraph('Sequence Clustering','',0)  
```  
  
 L'onglet **Profil du cluster** affiche également un histogramme de cas d'exemple du modèle. Ces cas d'exemple représentent les cas idéalisés pour le modèle. Ces cas ne sont pas stockés dans le modèle de la même façon que les données d'apprentissage ; vous devez utiliser une syntaxe spéciale pour extraire les cas d'exemple du modèle.  
  
```  
SELECT * FROM [Sequence Clustering].SAMPLE_CASES WHERE IsInNode('12')  
```  
  
 Pour plus d’informations, consultez [SELECT FROM &#60;modèle&#62;.SAMPLE_CASES &#40;DMX&#41;](../../dmx/select-from-model-sample-cases-dmx.md).  
  
#### <a name="cluster-characteristics-and-cluster-discrimination"></a>Caractéristiques et discrimination du cluster  
 L'onglet **Caractéristiques du cluster** résume les attributs principaux de chaque cluster, classés par probabilité. Vous pouvez trouver le nombre de cas appartenant à un cluster ainsi que des informations relatives à la distribution des cas dans le cluster : chaque caractéristique a un certain support. Pour consulter les caractéristiques d'un cluster particulier, vous devez connaître l'ID du cluster.  
  
 Les exemples suivants utilisent la procédure stockée système `GetClusterCharacteristics`pour retourner toutes les caractéristiques du Cluster 12 qui ont un score de probabilité supérieur au seuil spécifié de 0,0005.  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterCharacteristics('Sequence Clustering','12',0.0005)  
```  
  
 Pour retourner les caractéristiques de tous les clusters, vous pouvez laisser l'ID du cluster vide.  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterCharacteristics('Sequence Clustering','',0.0005)  
```  
  
 L'exemple suivant appelle la procédure stockée système `GetClusterDiscrimination` pour comparer les caractéristiques du Cluster 1 et du Cluster 12.  
  
```  
CALL System.Microsoft.AnalysisServices.System.DataMining.Clustering.GetClusterDiscrimination('Sequence Clustering','1','12',0.0005,true)  
```  
  
 Si vous souhaitez écrire votre propre requête en DMX pour comparer deux clusters, ou comparer un cluster avec son complément, vous devez commencer par extraire un jeu de caractéristiques, puis extraire les caractéristiques du cluster qui vous intéresse et comparer les deux jeux. Ce scénario est plus compliqué et nécessite généralement un traitement client.  
  
#### <a name="states-and-transitions"></a>États et transitions  
 L'onglet **Transitions d'état** de l'algorithme Microsoft Sequence Clustering effectue des requêtes compliquées interroge sur le principal pour extraire et comparer les statistiques de clusters différents. Reproduire ces résultats requiert une requête plus complexe et un traitement client.  
  
 Toutefois, vous pouvez utiliser les requêtes DMX décrites dans l'exemple 2 de la section [Requêtes de contenu](#bkmk_ContentQueries)pour extraire des probabilités et des états pour les séquences ou pour les transitions individuelles.  
  
## <a name="using-the-model-to-make-predictions"></a>Utilisation du modèle pour réaliser des prédictions  
 Les requêtes de prédiction sur un modèle Sequence Clustering peuvent utiliser beaucoup des fonctions de prédiction utilisées avec d'autres modèles de clustering. De plus, vous pouvez utiliser la fonction de prédiction spéciale, [PredictSequence &#40;DMX&#41;](../../dmx/predictsequence-dmx.md), pour effectuer des recommandations ou prédire les états suivants.  
  
###  <a name="bkmk_Query4"></a> Requête d'exemple 4 : prédiction d'un ou de plusieurs états suivants  
 Vous pouvez utiliser la fonction [PredictSequence &#40;DMX&#41;](../../dmx/predictsequence-dmx.md) pour prévoir l’état suivant le plus probable, compte tenu d’une valeur. Vous pouvez également prédire plusieurs états suivants. Par exemple, vous pouvez retourner une liste des trois premiers produits qu'il est probable qu'un client achète, pour présenter une liste de recommandations.  
  
 La requête d'exemple suivante est une requête singleton de prédiction qui retourne les cinq premières prédictions, avec leur probabilité. Vous devez utiliser une table imbriquée, `[v Assoc Seq Line Items]`, comme référence de colonne lorsque vous effectuez des prédictions parce que le modèle inclut une table imbriquée. En outre, lorsque vous fournissez des valeurs comme entrée, vous devez joindre à la fois les colonnes de la table de cas et de la table imbriquée, comme indiqué dans les instructions SELECT imbriquées.  
  
```  
SELECT FLATTENED PredictSequence([v Assoc Seq Line Items], 7)  
FROM [Sequence Clustering]  
NATURAL PREDICTION JOIN  
(SELECT  (SELECT 1 as [Line Number],  
   'All-Purpose Bike Stand' as [Model]) AS [v Assoc Seq Line Items])   
AS t  
```  
  
 Résultats de l'exemple :  
  
|Expression.$Sequence|Expression.Line Number|Expression.Model|  
|--------------------------|----------------------------|----------------------|  
|1||Cycling Cap|  
|2||Cycling Cap|  
|3||Sport-100|  
|4||Long-Sleeve Logo Jersey (Pull-over à manches longues)|  
|5||Half-Finger Gloves|  
|6||All-Purpose Bike Stand|  
|7||All-Purpose Bike Stand|  
  
 Il y a trois colonnes dans les résultats, alors que vous pourriez vous attendre à n'en trouver qu'une, car la requête retourne toujours une colonne pour la table de cas. Les résultats sont aplatis, sinon la requête retournerait une colonne unique contenant deux colonnes de table imbriquée.  
  
 La colonne $sequence est une colonne retournée par défaut par la fonction `PredictSequence` pour commander les résultats de prédiction. La colonne `[Line Number]`est requise pour correspondre aux clés de séquence dans le modèle, mais les clés ne sont pas des sorties.  
  
 De façon intéressante, les premières séquences prédites après All-Purpose Bike Stand sont Cycling Cap et Cycling Cap. Il ne s'agit pas d'une erreur. Selon la façon dont les données sont présentées au client et selon la façon dont elles sont groupées lors de l'apprentissage du modèle,  il est tout à fait possible d'obtenir des séquences de ce type. Par exemple, un client peut acheter une casquette (rouge), puis une autre casquette (bleue), ou en acheter deux à la fois s'il n'y avait aucune façon de spécifier la quantité.  
  
 Les valeurs des lignes 6 et 7 sont des espaces réservés. Lorsque vous atteignez la fin de la chaîne de transitions possibles, plutôt que terminer les résultats de prédiction, la valeur passée comme une entrée est ajoutée aux résultats. Par exemple, si vous avez augmenté le nombre de prédictions jusqu'à 20, les valeurs des lignes 6-20 sont toutes les mêmes, All-Purpose Bike Stand.  
  
## <a name="function-list"></a>Liste de fonctions  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Toutefois, l'algorithme MSC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering) prend en charge les fonctions supplémentaires répertoriées dans le tableau suivant.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[Cluster &#40;DMX&#41;](../../dmx/cluster-dmx.md)|Retourne le cluster qui est le plus susceptible de contenir le cas d'entrée|  
|[ClusterDistance &#40;DMX&#41;](../../dmx/clusterdistance-dmx.md)|Retourne la distance séparant le cas d'entrée du cluster spécifié ou, si aucun cluster n'est indiqué, la distance séparant le cas d'entrée du cluster le plus probable.<br /><br /> CEtte fonction peut être utilisée avec n'importe quel type de modèle de clustering (EM, K-Means, etc.), mais les résultats diffèrent selon l'algorithme.|  
|[ClusterProbability &#40;DMX&#41;](../../dmx/clusterprobability-dmx.md)|Retourne la probabilité que le cas d'entrée appartient au cluster spécifié.|  
|[IsInNode &#40;DMX&#41;](../../dmx/isinnode-dmx.md)|Indique si le nœud spécifié contient le cas courant.|  
|[PredictAdjustedProbability & #40 ; DMX & #41 ;](../../dmx/predictadjustedprobability-dmx.md)|Retourne la probabilité ajustée d'un état spécifié.|  
|[PredictAssociation &#40;DMX&#41;](../../dmx/predictassociation-dmx.md)|Prévoit une appartenance associative.|  
|[PredictCaseLikelihood &#40;DMX&#41;](../../dmx/predictcaselikelihood-dmx.md)|Retourne le degré de vraisemblance de l'intégration d'un cas d'entrée au modèle existant.|  
|[PredictHistogram & #40 ; DMX & #41 ;](../../dmx/predicthistogram-dmx.md)|Retourne une table qui représente un histogramme de la prévision d'une colonne donnée.|  
|[PredictNodeId & #40 ; DMX & #41 ;](../../dmx/predictnodeid-dmx.md)|Retourne le Node_ID du nœud dans lequel le cas est classé.|  
|[PredictProbability & #40 ; DMX & #41 ;](../../dmx/predictprobability-dmx.md)|Retourne la probabilité pour un état spécifié.|  
|[PredictSequence &#40;DMX&#41;](../../dmx/predictsequence-dmx.md)|Prédit les valeurs de séquence pour un ensemble spécifié de données de séquence.|  
|[PredictStdev &#40;DMX&#41;](../../dmx/predictstdev-dmx.md)|Retourne l'écart-type prévu pour la colonne spécifiée.|  
|[PredictSupport & #40 ; DMX & #41 ;](../../dmx/predictsupport-dmx.md)|Retourne la valeur de support pour un état spécifié.|  
|[PredictVariance & #40 ; DMX & #41 ;](../../dmx/predictvariance-dmx.md)|Retourne la variance d'une colonne spécifiée.|  
  
 Pour obtenir la liste des fonctions communes à tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)], consultez [Fonctions de prédiction générales &#40;DMX&#41;](../../dmx/general-prediction-functions-dmx.md). Pour la syntaxe de fonctions spécifiques, consultez [Fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)   
 [Microsoft Sequence Clustering algorithme techniques de référence](../../analysis-services/data-mining/microsoft-sequence-clustering-algorithm-technical-reference.md)   
 [Algorithme de Microsoft Sequence Clustering](../../analysis-services/data-mining/microsoft-sequence-clustering-algorithm.md)   
 [Contenu du modèle d’exploration de données pour les modèles de Clustering de séquence & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-sequence-clustering-models.md)  
  
  
