---
title: MNN (Microsoft Neural Network) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 54896093b887985fc658e823f7d277347a70f0ea
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-neural-network-algorithm"></a>Algorithme MNN (Microsoft Neural Network)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) est une implémentation de l’architecture de réseau neuronal à succès et modulable destinée à l’apprentissage automatique (Machine Learning).  L’algorithme opère en testant chaque état possible de l’attribut d’entrée par rapport à chaque état possible de l’attribut prévisible, et en calculant des probabilités pour chaque combinaison en fonction des données d’apprentissage. Vous pouvez utiliser ces probabilités à des fins de classification ou de régression pour prédire un résultat en fonction de certains attributs d’entrée. Un réseau neuronal peut aussi être utile pour l’analyse d’association.  
  
 Quand vous créez un modèle d’exploration de données avec l’algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network), vous pouvez inclure plusieurs sorties. L’algorithme crée alors plusieurs réseaux. Le nombre de réseaux contenus dans un même modèle d’exploration de données dépend du nombre d’états (ou de valeurs d’attribut) contenus dans les colonnes d’entrée, mais aussi du nombre de colonnes prédictibles qu’utilise le modèle d’exploration de données et du nombre d’états contenus dans ces colonnes.  
  
## <a name="example"></a>Exemple  
 L'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) permet d'analyser des données d'entrée complexes, telles que les données d'un processus commercial ou de fabrication, ou des problèmes d'entreprise pour lesquels une quantité significative de données d'apprentissage est disponible, mais pour lesquels des règles ne peuvent pas être facilement dérivées en utilisant d'autres algorithmes.  
  
 Voici quelques suggestions de scénarios d'utilisation de l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) :  
  
-   analyse marketing et de promotion des ventes, par exemple pour mesurer le succès d’une campagne de marketing direct ou radiophonique ;  
  
-   prédiction des mouvements de stock, des fluctuations monétaires ou d’autres informations financières extrêmement inconstantes à partir des données d’historique ;  
  
-   analyse de processus industriels et de fabrication ;  
  
-   exploration de texte ;  
  
-   tout modèle de prédiction qui analyse des relations complexes entre des entrées nombreuses et des sorties en nombre relativement plus limité.  
  
## <a name="how-the-algorithm-works"></a>Fonctionnement de l'algorithme  
 L’algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) crée un réseau composé d’au maximum trois couches de nœuds (parfois appelés *neurones*). Ces couches sont constituées d’une *couche d’entrée*, d’une *couche masquée*et d’une *couche de sortie*.  
  
 **Couche d’entrée :** les nœuds d’entrée définissent toutes les valeurs d’attribut d’entrée pour le modèle d’exploration de données et leurs probabilités.  
  
 **Couche masquée :** les nœuds masqués reçoivent les entrées des nœuds d’entrée et fournissent les sorties aux nœuds de sortie. La couche masquée correspond à la couche où des poids sont affectés aux diverses probabilités des entrées. Un poids décrit la pertinence ou l’importance d’une entrée donnée par rapport au nœud masqué. Plus le poids assigné à une entrée est élevé, plus la valeur de cette entrée sera importante. Les poids peuvent être négatifs, ce qui implique que l'entrée peut désactiver un résultat spécifique au lieu de l'activer.  
  
 **Couche de sortie :** les nœuds de sortie représentent les valeurs d’attributs prédictibles du modèle d’exploration de données.  
  
 Pour en savoir plus sur la construction et le notation des couches d’entrée, des couches masquées et des couches de sortie, consultez [Informations techniques de référence sur l’algorithme MNN (Microsoft Neural Network)](../../analysis-services/data-mining/microsoft-neural-network-algorithm-technical-reference.md).  
  
## <a name="data-required-for-neural-network-models"></a>Données requises pour les modèles Neural Network  
 Un modèle de réseau neuronal doit contenir une colonne clé, une ou plusieurs colonnes d'entrée et une ou plusieurs colonnes prédictibles.  
  
 Les modèles d’exploration de données qui utilisent l’algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) sont fortement influencés par les valeurs que vous spécifiez pour les paramètres accessibles à l’algorithme. Les paramètres définissent comment les données sont échantillonnées, comment elles sont distribuées ou comment leur distribution est prévue dans chaque colonne, ainsi que le moment où la sélection de fonctionnalité est invoquée pour limiter les valeurs utilisées dans le modèle final.  
  
 Pour plus d’informations sur la définition des paramètres permettant de personnaliser le comportement d’un modèle, consultez [Informations techniques de référence sur l’algorithme MNN (Microsoft Neural Network)](../../analysis-services/data-mining/microsoft-neural-network-algorithm-technical-reference.md).  
  
## <a name="viewing-a-neural-network-model"></a>Affichage d’un modèle Neural Network  
 Pour utiliser les données et voir comment le modèle met en corrélation les entrées et les sorties, vous pouvez utiliser la **Visionneuse de l’algorithme MNN (Microsoft Neural Network)**. Cette visionneuse personnalisée vous permet de définir des filtres sur les attributs d’entrée et leurs valeurs et présentent des graphiques qui illustrent leur impact sur les sorties. Les info-bulles de la visionneuse indiquent la probabilité et l’élévation associées à chaque paire de valeurs d’entrée et de sortie. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse de l’algorithme MNN (Microsoft Neural Network)](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-neural-network-viewer.md).  
  
 Le moyen le plus simple d’explorer la structure du modèle est d’utiliser la **Visionneuse de l’arborescence de contenu générique Microsoft**. Vous pouvez consulter les entrées, les sorties et les réseaux créés par le modèle et cliquer sur n’importe quel nœud pour le développer et consulter les statistiques liées aux nœuds des couches d’entrée, de sortie ou masquée. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse de l’arborescence de contenu générique Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-generic-content-tree-viewer.md).  
  
## <a name="creating-predictions"></a>Création de prédictions  
 Une fois le modèle traité, vous pouvez utiliser le réseau et les poids stockés au sein de chaque nœud pour faire des prédictions. Un modèle de réseau neuronal prend en charge la régression, l’association et l’analyse de classification. Par conséquent, la signification de chaque prédiction peut être différente. Vous pouvez également interroger le modèle lui-même pour examiner les corrélations qui ont été trouvées et extraire les statistiques connexes. Pour obtenir des exemples de création de requêtes sur un modèle de réseau neuronal, consultez [Exemples de requêtes de modèle de réseau neuronal](../../analysis-services/data-mining/neural-network-model-query-examples.md).  
  
 Pour obtenir des informations générales sur la création d’une requête sur un modèle d’exploration de données, consultez [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md).  
  
## <a name="remarks"></a>Notes  
  
-   Ne prend pas en charge l’extraction ou les dimensions d’exploration de données. Cela est dû au fait que la structure des nœuds du modèle d'exploration de données ne correspond pas nécessairement directement aux données sous-jacentes.  
  
-   Ne prend pas en charge la création de modèles au format de langage PMML (Predictive Model Markup Language).  
  
-   Prend en charge l'utilisation de modèles d'exploration de données OLAP.  
  
-   Ne prend pas en charge la création de dimensions d’exploration de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Référence technique de Microsoft Neural Network algorithme](../../analysis-services/data-mining/microsoft-neural-network-algorithm-technical-reference.md)   
 [Contenu du modèle d’exploration de données pour les modèles de réseau neuronal & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-neural-network-models-analysis-services-data-mining.md)   
 [Exemples de requête de modèle de réseau neuronal](../../analysis-services/data-mining/neural-network-model-query-examples.md)   
 [Algorithme Microsoft Logistic Regression](../../analysis-services/data-mining/microsoft-logistic-regression-algorithm.md)  
  
  
