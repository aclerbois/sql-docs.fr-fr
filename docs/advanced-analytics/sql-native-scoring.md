---
title: Calcul de score native | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e08cb48335f956298c387e2a621a8e8de7a91a98
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="native-scoring"></a>Calcul de score en natif
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cette rubrique décrit les fonctionnalités de SQL Server 2017 permettant de calculer les scores sur les modèles d’apprentissage automatique en temps quasi réel.

+ Qu’est natif et calculer les scores en temps réel de calcul de score
+ Fonctionnement
+ Configuration requise et plates-formes prises en charge

## <a name="what-is-native-scoring-and-how-is-it-different-from-realtime-scoring"></a>Qu’est le score natif et comment il est différent de calculer les scores en temps réel ?

Dans SQL Server 2016, Microsoft a créé une infrastructure d’extensibilité qui permet aux scripts R doit être exécuté à partir de T-SQL. Cette structure prend en charge toute opération que vous pouvez effectuer dans R, allant de simples fonctions à formation complexes d’apprentissage des modèles. Toutefois, l’architecture du processus de double nécessite l’appel d’un processus R externe pour chaque appel, quelle que soit la complexité de l’opération. Si vous chargez un modèle dont l’apprentissage d’une table et score s’y rapportant sur les données déjà présentes dans SQL Server, la charge de l’appel du processus de R externe représente un coût de performances inutiles.

_Calcul de score_ est un processus en deux étapes. Tout d’abord, vous spécifiez un modèle préentraîné à charger à partir d’une table. Ensuite, passez de nouveau les données à la fonction, pour générer des valeurs de prédiction d’entrée (ou _scores_). L’entrée peut être tabulaires ou uniques des lignes. Vous pouvez choisir de générer une valeur de colonne unique qui représente une probabilité ou vous pouvez générer plusieurs valeurs, comme un intervalle de confiance, erreur ou autres complément utile pour la prédiction.

Lorsque l’entrée inclut plusieurs lignes de données, il est généralement plus rapide pour insérer des valeurs de prédiction dans une table dans le cadre du processus de calcul de score.  Générer un score unique est plus courant dans un scénario où obtenir les valeurs d’entrée à partir d’une demande de formulaire ou d’utilisateur et le score de retour à une application cliente. Pour améliorer les performances lors de la génération de scores successives, SQL Server peut mettre en cache le modèle afin qu’il peut être rechargé en mémoire.

Pour prendre en charge rapide de calcul de score, Machine Learning Services SQL Server (et serveur de Microsoft Machine Learning) fournissent des bibliothèques de calcul de score intégrées qui fonctionnent dans R ou T-SQL. Il existe différentes options, selon la version que vous avez.

**Calcul de score en natif**

+ La fonction PREDICT dans Transact-SQL prend en charge _score native_ dans n’importe quelle instance de SQL Server 2017. Il nécessite uniquement que vous disposez d’un modèle déjà formé, que vous pouvez appeler à l’aide de T-SQL. Calcul de score natif à l’aide de T-SQL, présente les avantages :

    + Aucune configuration supplémentaire n’est requise.
    + Le runtime R n’est pas appelé. Il est inutile d’installer R.

**Calculer les scores en temps réel**

+ **sp_rxPredict** est une procédure stockée pour en temps réel de calcul de score qui peut être utilisé pour générer des scores à partir de n’importe quel type de modèle pris en charge, sans appeler le runtime R.

  Cette procédure stockée est également disponible dans SQL Server 2016, si vous mettez à niveau les composants de R à l’aide du programme d’installation autonome de Microsoft R Server. sp_rxPredict est également pris en charge dans SQL Server 2017. Par conséquent, vous pouvez utiliser cette fonction lors de la génération de scores avec un type de modèle non pris en charge par la fonction de prédiction.

+ La fonction rxPredict peut être utilisée pour calculer les scores rapide dans du code R.

Pour l’ensemble de ces méthodes, vous devez utiliser un modèle qui a été formé à l’aide d’un des algorithmes de RevoScaleR ou MicrosoftML pris en charge.

Pour obtenir un exemple d’en temps réel de calcul de score en action, consultez [fin à la fin prêt ChargeOff prédiction créé à l’aide de Azure HDInsight Spark Clusters et le Service de SQL Server 2016 R](https://blogs.msdn.microsoft.com/rserver/2017/06/29/end-to-end-loan-chargeoff-prediction-built-using-azure-hdinsight-spark-clusters-and-sql-server-2016-r-service/)

## <a name="how-native-scoring-works"></a>Comment native notation fonctionne

Calcul de score natif utilise des bibliothèques natives C++ à partir de Microsoft, qui peut lire le modèle à partir d’un format binaire spécial et générer des scores. Car un modèle peut être publié et utilisé pour calculer les scores sans avoir à appeler l’interpréteur R, la surcharge de plusieurs interactions entre les processus est réduite. Par conséquent, le score natif prend en charge les performances de prédiction beaucoup plus rapides dans les scénarios de production d’entreprise.

Pour générer des scores à l’aide de cette bibliothèque, vous appelez la fonction de calcul de score et passez les entrées requises suivantes :

+ Un modèle compatible. Consultez le [exigences](#Requirements) pour plus d’informations.
+ Données d’entrée, généralement définies comme une requête SQL

La fonction renvoie des prédictions pour les données d’entrée, ainsi que toutes les colonnes de données source que vous souhaitez passer.

Pour obtenir des exemples de code, ainsi que des instructions sur la façon de préparer les modèles dans le format binaire requis, consultez cet article :

+ [Comment effectuer le calcul de score en temps réel](r/how-to-do-realtime-scoring.md)

Pour une solution complète qui inclut un score natif, consultez ces exemples à partir de l’équipe de développement SQL Server :

+ Déployer votre script ML : [à l’aide d’un modèle de Python](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/step/3.html)
+ Déployer votre script ML : [à l’aide d’un modèle R](https://microsoft.github.io/sql-ml-tutorials/R/rentalprediction/step/3.html)

## <a name="requirements"></a>Spécifications

Plateformes prises en charge sont les suivantes :

+ Services SQL Server 2017 Machine Learning (y compris Microsoft R Server 9.1.0)
    
    Calculer les scores natif à l’aide de prédire nécessite SQL Server 2017.
    Il fonctionne sur n’importe quelle version de SQL Server 2017, y compris Linux.

    Vous pouvez également effectuer en temps réel de calcul de score à l’aide de sp_rxPredict. Pour utiliser cette procédure stockée nécessite que vous activiez [intégration CLR SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/introduction-to-sql-server-clr-integration).

+ SQL Server 2016

   En temps réel de calcul de score à l’aide de sp_rxPredict est possible avec SQL Server 2016 et peut également être exécuté sur Microsoft R Server. Cette option requiert SQLCLR doit être activée et que vous installez la mise à niveau de Microsoft R Server.
   Pour plus d’informations, consultez [calculer les scores en temps réel](Real-time-scoring.md)

### <a name="model-preparation"></a>Préparation du modèle

+ Le modèle doit être formé à l’avance à l’aide d’une des prises en charge **rx** algorithmes. Pour plus d’informations, consultez [algorithmes pris en charge](#bkmk_native_supported_algos).
+ Le modèle doit être enregistré à l’aide de la nouvelle fonction de sérialisation fournie dans Microsoft R Server 9.1.0. La fonction de sérialisation est optimisée pour prendre en charge la notation rapide.

### <a name="bkmk_native_supported_algos"></a> Algorithmes qui prennent en charge la notation natif

+ Modèles de RevoScaleR

  + [rxLinMod](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxdtree)
  + [rxDForest](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxdforest)

Si vous avez besoin d’utiliser des modèles à partir de MicrosoftML, utilisez en temps réel avec sp_rxPredict de calcul de score.

### <a name="restrictions"></a>Restrictions

Les types de modèles suivants ne sont pas prises en charge :

+ Modèles contenant des autres types de transformations de R non pris en charge
+ Modèles à l’aide de la `rxGlm` ou `rxNaiveBayes` algorithmes dans RevoScaleR
+ Modèles PMML
+ Modèles créés à l’aide d’autres bibliothèques R à partir de CRAN ou autres référentiels
+ Modèles contenant toutes les autres transformations R
