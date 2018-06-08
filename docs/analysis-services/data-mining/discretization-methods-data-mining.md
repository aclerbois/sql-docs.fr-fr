---
title: Méthodes de discrétisation (exploration de données) | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 610108ce4edb6e3beb5c13398d0a79eca200bdba
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discretization-methods-data-mining"></a>Méthodes de discrétisation (exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Certains algorithmes utilisés pour créer des modèles d’exploration de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] nécessitent des types de contenu spécifiques pour pouvoir fonctionner correctement. Par exemple, l'algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes ne peut pas utiliser de colonnes continues comme entrée ni prédire des valeurs continues. En outre, certaines colonnes peuvent contenir tellement de valeurs que l'algorithme ne peut pas identifier facilement les motifs intéressants qui ressortent des données et qui vont servir à créer un modèle.  
  
 Dans ce cas, vous pouvez discrétiser les données des colonnes afin de pouvoir utiliser les algorithmes pour créer un modèle d'exploration de données. La*discrétisation* est le processus consistant à mettre des valeurs dans des compartiments afin d’obtenir un nombre limité d’états possibles. Les compartiments eux-mêmes sont traités comme des valeurs discrètes et ordonnées. Vous pouvez discrétiser les colonnes de nombres et de chaînes.  
  
 Plusieurs méthodes vous permettent de discrétiser des données. Si votre solution d’exploration de données utilise des données relationnelles, vous pouvez déterminer le nombre de compartiments à utiliser pour le regroupement des données en définissant la valeur de la propriété <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationBucketCount%2A> . Le nombre de compartiments par défaut est 5.  
  
 Si votre solution d’exploration de données utilise les données d’un cube OLAP (Online Analytical Processing), l’algorithme d’exploration de données calcule automatiquement le nombre de compartiments à générer, en utilisant l’équation suivante, où « n » est le nombre de valeurs distinctes de données dans la colonne :  
  
 `Number of Buckets = sqrt(n)`  
  
 Si vous ne voulez pas qu’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] calcule le nombre de compartiments, vous pouvez utiliser la propriété <xref:Microsoft.AnalysisServices.DimensionAttribute.DiscretizationBucketCount%2A> pour spécifier manuellement le nombre de compartiments.  
  
 Le tableau suivant décrit les méthodes que vous pouvez utiliser pour discrétiser des données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
|Méthode de discrétisation| Description|  
|---------------------------|-----------------|  
|**AUTOMATIC**|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] détermine la méthode de discrétisation à utiliser.|  
|**CLUSTERS**|L'algorithme divise les données en groupes en échantillonnant les données d'apprentissage, en initialisant à un certain nombre de points aléatoires, puis en exécutant plusieurs itérations de l'algorithme MC (Microsoft Clustering) à l'aide de la méthode de clustering EM (expectation-maximisation). La méthode **CLUSTERS** est utile, car elle fonctionne sur n’importe quelle courbe de distribution. Cependant, elle nécessite une durée de traitement plus longue que les autres méthodes de discrétisation.<br /><br /> Cette méthode peut uniquement être utilisée sur des colonnes numériques.|  
|**EQUAL_AREAS**|L'algorithme divise les données en groupes contenant un nombre égal de valeurs. Cette méthode convient surtout aux courbes de distribution normales, mais elle n'est pas efficace si la distribution comprend un nombre élevé de valeurs dans un groupe resserré de valeurs continues. Par exemple, si la moitié des éléments a un coût de 0, la moitié des données se trouvera sous un point unique de la courbe. Dans ce type de distribution, cette méthode disperse les données pour tenter d'établir une discrétisation équivalente dans plusieurs zones, ce qui engendre une représentation inexacte des données.|  
  
## <a name="remarks"></a>Notes  
  
-   Vous pouvez utiliser la méthode **EQUAL_AREAS** pour discrétiser des chaînes.  
  
-   La méthode **CLUSTERS** utilise un échantillon aléatoire de 1 000 enregistrements pour discrétiser les données. Utilisez la méthode **EQUAL_AREAS** si vous ne voulez pas que l’algorithme échantillonne les données.  
  
  
  
## <a name="see-also"></a>Voir aussi  
 [Contenu des Types de & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/content-types-data-mining.md)   
 [Contenu des Types de & #40 ; DMX & #41 ;](../../dmx/content-types-dmx.md)   
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Les Structures d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md)   
 [Types de données & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/data-types-data-mining.md)   
 [Colonnes de Structure d’exploration de données](../../analysis-services/data-mining/mining-structure-columns.md)   
 [Distributions de colonnes &#40;exploration de données&#41;](../../analysis-services/data-mining/column-distributions-data-mining.md)  
  
  
