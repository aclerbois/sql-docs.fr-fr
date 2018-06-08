---
title: À l’aide du Package MicrosoftML avec SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 4f94d2eaaaa9fc70014e5d333dede4919ca198f9
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="using-the-microsoftml-package-with-sql-server"></a>À l’aide du package MicrosoftML avec SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Le [ **MicrosoftML** ](https://msdn.microsoft.com/microsoft-r/microsoftml-introduction) package qui est fourni avec Microsoft R Server et SQL Server 2017 inclut plusieurs des algorithmes d’apprentissage. Ces API ont été développés par Microsoft pour les applications d’apprentissage interne et ont été affinées au fil des années pour prendre en charge des performances élevées sur les données volumineuses, à l’aide de traitement multicœur et rapide des données de diffusion en continu. MicrosoftML inclut également de nombreuses transformations de texte et de traitement d’image.

Dans SQL Server 2017 CTP 2.0, la prise en charge a été ajoutée pour le langage Python. Le **microsoftml** le package pour Python contient des fonctions équivalentes à celles dans le package MicrosoftML pour R. 

+ **MicrosoftML pour R**

    Référence de présentation et de package : [MicrosoftML : algorithmes d’apprentissage automatique R](https://docs.microsoft.com/en-us/r-server/r-reference/microsoftml/microsoftml-package)

    R étant respectant la casse, assurez-vous que vous référencez le nom correctement lors du chargement du package.

+ **Microsoftml pour Python**

    Référence de présentation et de package : [microsoftml (bibliothèque de fonctions pour Python)](https://docs.microsoft.com/r-server/python-reference/microsoftml/microsoftml-package). 

## <a name="whats-in-microsoftml"></a>Nouveautés dans MicrosoftML

MicrosoftML contient une série de transformations qui ont été optimisées pour les performances et algorithmes d’apprentissage.

### <a name="machine-learning-algorithms"></a>Algorithmes d’apprentissage automatique

-  Modèles linéaires : `rxFastLinear` un apprenant linéaire repose sur stochastique élévation coordonnée double qui peut être utilisée pour la classification binaire ou de régression. Le modèle prend en charge la régularisation L1 et L2.

- Modèles de forêt arborescence et la décision de décision : `rxFastTree` est un algorithme d’arbre de décision augmentés initialement appelé FastRank, qui a été développée pour une utilisation dans Bing. Il s’agit de l’un des apprentissages les plus rapides et les plus populaires. Il prend en charge la régression et la classification binaire.

  `rxFastForest` un modèle de régression logistique est basé sur la méthode de forêt aléatoire. Il est similaire à la fonction `rxLogit` dans RevoScaleR, mais il prend en charge la régularisation L1 et L2. Il prend en charge la régression et la classification binaire.

- La régression logistique : `rxLogisticRegression` est un modèle de régression logistique similaire à la `rxLogit` fonction dans RevoScaleR, avec prise en charge supplémentaire pour la régularisation L1 et L2. Prend en charge la classification binaire ou multiclasse.

- Réseaux neuronaux : le `rxNeuralNet` fonction prend en charge la classification binaire, la classification multiclasse et régression à l’aide de réseaux neuronaux. Personnalisable et prend en charge les réseaux compliquées avec accélération GPU, à l’aide d’un processeur unique.

- Détection des anomalies.  Le `rxOneClassSvm` (fonction) est basée sur la méthode SVM mais est optimisée pour la classification binaire non équilibré de jeux de données.

### <a name="transformation-functions"></a>Fonctions de transformation

MicrosoftML comprend de nombreuses fonctions spécialisées qui sont utiles pour la transformation des données et l’extraction des fonctionnalités.

- Capacités de traitement de texte incluent la `featurizeText` et `getSentiment` fonctions. Vous pouvez compter n-grammes, détecter la langue utilisée ou effectuer la normalisation du texte. Vous pouvez également effectuer le nettoyage des opérations telles que la suppression de mots vides de texte courants, ou générer des fonctionnalités de hachage ou de nombre à partir du texte.

- Sélection des fonctionnalités et des fonctionnalités telles que les fonctions de transformation, `selectFeatures` ou `getSentiment`, l’analyse des données et créent des fonctionnalités les plus utiles pour la modélisation.

- Travailler avec des variables en utilisant comme `categorical` ou `categoricalHash`, convertir des valeurs catégorielles tableaux indexés pour de meilleures performances.

- Fonctions spécifiques au traitement d’image et analytique, telles que `extractPixels` ou `featurizeImage`, permettent d’obtenir le plus d’informations des images et de traiter les images plus rapidement.

Pour plus d’informations, consultez [MicrosoftML: State-of-the-art machine learning R algorithms](https://msdn.microsoft.com/microsoft-r/microsoftml/microsoftml).

## <a name="how-to-use-microsoftml"></a>Comment utiliser MicrosoftML

Cette section décrit comment localiser et charger le package dans votre code R et Python.

+ Le package MicrosoftML pour R est installé par défaut avec Microsoft R Server 9.1.0 et dans SQL Server 2017.

    Il est également disponible pour une utilisation avec SQL Server 2016, si vous mettez à niveau les composants R pour l’instance, en utilisant le programme d’installation de Microsoft R Server comme décrit ici : [mettre à niveau une instance de SQL Server à l’aide de la liaison](r/use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md)

+ Le **microsoftml** package Python est installé par défaut avec SQL Server 2017 

   Pour utiliser ce package, nous vous recommandons de mettre à niveau vers Release Candidate 2 ou version ultérieure. Une version antérieure a été publiée avec la version RC1, mais la bibliothèque a subi une révision considérable, y compris les modifications apportées aux noms de fonctions. 

Toutefois, pour R et Python, pas du chargement du package par défaut ; Par conséquent, vous devez charger explicitement le package dans le cadre de votre code pour utiliser ses fonctions.

### <a name="calling-microsoftml-functions-from-r-in-sql-server"></a>Appel de fonctions MicrosoftML à partir de R dans SQL Server

Dans votre code R, charger le package MicrosoftML et appeler ses fonctions, comme tout autre package.

```R
library(microsoftml);
library(RevoScaleR);
logisticRegression(args);
```

**Remarques**

+ Le package MicrosoftML est entièrement intégré avec le pipeline de traitement des données fourni par le package RevoScaleR. Par conséquent, vous pouvez utiliser le package MicrosoftML dans n’importe quel contexte de calcul basées sur Windows, y compris une instance de SQL Server qui a des extensions d’apprentissage machine activées.

    Toutefois, MicrosoftML requiert une référence à RevoScaleR et ses fonctions afin d’utiliser à distance les contextes de calcul.

### <a name="calling-microsoftml-functions-from-python-in-sql-server"></a>Appel de fonctions microsoftml à partir de Python dans SQL Server

Pour appeler des fonctions à partir du package, dans votre code Python, importez le **microsoftml** du package, puis importez **revoscalepy** si vous avez besoin d’utiliser les contextes de calcul à distance ou de source de données ou de connectivité connexe objets. Ensuite référencer les fonctions individuelles que vous avez besoin.

```Python
from microsoftml.modules.logistic_regression.rx_logistic_regression import rx_logistic_regression
from revoscalepy.functions.RxSummary import rx_summary
from revoscalepy.etl.RxImport import rx_import_datasource
```

**Remarques**

+ Dans SQL Server 2017, **microsoftml** est un module Python35 compatibles. 

+ Les fonctions dans **microsoftml** sont intégrés avec les calcul des contextes et sources de données qui sont pris en charge par **revoscalepy**. Par conséquent, vous pouvez utiliser la **microsoftml** package Python pour créer et évaluer des modèles dans n’importe quel contexte de calcul basées sur Windows, y compris une instance de SQL Server qui a des extensions de machine learning. activé.

    Toutefois, **microsoftml** pour Python requiert une référence à **revoscalepy** et ses fonctions afin d’utiliser à distance les contextes de calcul.

Pour plus d’informations sur revoscalepy, consultez :

+ [Qu’est revoscalepy](python/what-is-revoscalepy.md)

+ [bibliothèque de fonctions revoscalepy](https://docs.microsoft.com/en-us/r-server/python-reference/revoscalepy/revoscalepy-package) 
