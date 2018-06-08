---
title: Calculer les scores en temps réel | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: eda95bf4cd16c5c38277e6d139c224f225c9b10a
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="realtime-scoring"></a>Calculer les scores en temps réel
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cette rubrique décrit une fonctionnalité disponible dans SQL Server 2016 et SQL Server 2017 qui prend en charge le calcul de score sur les modèles d’apprentissage automatique en temps quasi réel.

> [!TIP]
> Calcul de score natif est une implémentation spéciale d’en temps réel de calcul de score qui utilise la fonction de prédiction de T-SQL native pour calculer les scores très rapide et est disponible uniquement dans SQL Server 2017. Pour plus d’informations, consultez [score natif](sql-native-scoring.md).

## <a name="how-realtime-scoring-works"></a>Fonctionnement de calculer les scores en temps réel

Calculer les scores en temps réel sont prise en charge dans SQL Server 2017 et SQL Server 2016, sur les types de modèles spécifiques créés à l’aide d’algorithmes de RevoScaleR ou MicrosoftML pris en charge. Elle utilise des bibliothèques natives C++ pour générer des scores, en fonction de l’entrée d’utilisateur fournie à un modèle stocké dans un format binaire spécial d’apprentissage.

Car un modèle formé peut être utilisé pour calculer les scores sans avoir à appeler un runtime de langage externe, la surcharge de plusieurs processus est réduite. Cela prend en charge les performances de prédiction beaucoup plus rapides pour calculer les scores des scénarios de production. Étant donné que les données ne quittent jamais SQL Server, les résultats peuvent être générées et insérés dans une table sans aucune traduction de données entre R et SQL.

Calculer les scores en temps réel est un processus en plusieurs étapes :

1. La procédure stockée qui effectue le calcul de score doit être activée sur une base par base de données.
2. Vous chargez le modèle dont l’apprentissage au format binaire.
3. Vous fournissez de nouvelles données d’entrée, les lignes tabulaires ou uniques, en tant qu’entrée pour le modèle.
4. Pour générer des scores, appelez la procédure stockée de sp_rxPredict.

## <a name="get-started"></a>Prise en main

Pour plus d’exemples de code et des instructions, consultez [comment effectuer un score natif ou calculer les scores en temps réel](r/how-to-do-realtime-scoring.md).

Pour obtenir un exemple de comment rxPredict pouvez utilisés pour calculer les scores, consultez [fin à la fin prêt ChargeOff prédiction créé à l’aide de Azure HDInsight Spark Clusters et le Service de SQL Server 2016 R](https://blogs.msdn.microsoft.com/rserver/2017/06/29/end-to-end-loan-chargeoff-prediction-built-using-azure-hdinsight-spark-clusters-and-sql-server-2016-r-service/)

> [!TIP]
> Si vous utilisez exclusivement dans le code R, vous pouvez également utiliser le [rxPredict](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxpredict) fonction pour calculer les scores rapide.

## <a name="requirements"></a>Spécifications

Calculer les scores en temps réel sont prise en charge sur ces plateformes :

+ SQL Server 2017 Machine Learning Services
+ SQL Server R Services 2016, avec une mise à niveau de l’instance de R Services à Microsoft R Server 9.1.0 ou version ultérieure
+ Machine Learning Server (autonome)

Sur SQL Server, vous devez activer les scores de fonctionnalité à l’avance en temps réel. Il s’agit, car la fonctionnalité nécessite l’installation des bibliothèques de basé sur CLR dans SQL Server.

Pour plus d’informations concernant en temps réel de calcul de score dans un environnement distribué basé sur Microsoft R Server, reportez-vous à la [publishService](https://docs.microsoft.com/machine-learning-server/r-reference/mrsdeploy/publishservice) fonction disponible dans le [mrsDeploy package](https://docs.microsoft.com/machine-learning-server/r-reference/mrsdeploy/mrsdeploy-package), qui prend en charge les modèles de publication en temps réel de calcul de score en tant que nouvelle un service web en cours d’exécution sur le serveur de R.

### <a name="restrictions"></a>Restrictions

+ Le modèle doit être formé à l’avance à l’aide d’une des prises en charge **rx** algorithmes. Pour plus d’informations, consultez [algorithmes pris en charge](#bkmk_rt_supported_algos). En temps réel de calcul de score avec `sp_rxPredict` prend en charge les algorithmes de RevoScaleR et MicrosoftML.

+ Le modèle doit être enregistré à l’aide des nouvelles fonctions de sérialisation : [rxSerialize](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) pour R, et [rx_serialize_model](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) pour Python. Ces fonctions de sérialisation ont été optimisées pour prendre en charge la notation rapide.

+ Calculer les scores en temps réel n’utilisent pas un interpréteur interpréteur ; Par conséquent, toutes les fonctionnalités qui peuvent nécessiter un interpréteur ne sont pas pris en charge lors de l’étape de calcul de score.  Celles-ci peuvent inclure :

  + Modèles à l’aide de la `rxGlm` ou `rxNaiveBayes` algorithmes ne sont pas actuellement pris en charge

  + Modèles de RevoScaleR qui utilisent une fonction de transformation de R, ou une formule qui contient une transformation, tel que <code>A ~ log(B)</code> ne sont pas pris en charge dans le calcul de score en temps réel. Pour utiliser un modèle de ce type, nous vous recommandons d’effectuer la transformation sur le pour les données d’entrée avant de les transmettre à calculer les scores en temps réel.

+ Calculer les scores en temps réel sont actuellement optimisé pour des prédictions rapides sur les petits jeux de données, allant de quelques lignes à des centaines de milliers de lignes. Sur les jeux de données très volumineux, à l’aide de [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) peuvent être plus rapides.

### <a name="a-namebkmkrtsupportedalgosalgorithms-that-support-realtime-scoring"></a><a name="bkmk_rt_supported_algos">Algorithmes qui prennent en charge le calcul de score en temps réel

+ Modèles de RevoScaleR

  + [rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod) \*
  + [rxLogit](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlogit) \*
  + [rxBTrees](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxbtrees) \*
  + [rxDtree](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdtree) \*
  + [rxdForest](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdforest) \*
  
  Modèles marquée avec \* prennent également en charge le calcul de score natif avec la fonction de prédiction.

+ Modèles de MicrosoftML

  + [rxFastTrees](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfastlinear)

+ Transformations fournies par MicrosoftML

  + [featurizeText](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/selectFeatures)

### <a name="unsupported-model-types"></a>Types de modèles non pris en charge

Les types de modèles suivants ne sont pas prises en charge :

+ Modèles contenant des autres types de transformations de R non pris en charge
+ Modèles à l’aide de la `rxGlm` ou `rxNaiveBayes` algorithmes dans RevoScaleR
+ Modèles PMML
+ Modèles créés à l’aide d’autres bibliothèques R à partir de CRAN ou autres référentiels
+ Modèles contenant n’importe quel autre type de transformation de R autres que ceux répertoriés ici

### <a name="known-issues"></a>Problèmes connus

+ `sp_rxPredict` Retourne un message incorrect lorsqu’une valeur NULL est passée comme le modèle : « System.Data.SqlTypes.SqlNullValueException:Data dans Null ».

## <a name="next-steps"></a>Étapes suivantes

[Comment effectuer le calcul de score en temps réel](r/how-to-do-realtime-scoring.md)
