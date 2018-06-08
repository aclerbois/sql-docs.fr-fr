---
title: Présentation de package de Python revoscalepy dans l’apprentissage de SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 450aa7cc002da9b42379330141f34ee33eedbde6
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="introducing-revoscalepy-in-sql-server-machine-learning"></a>Présentation de revoscalepy dans l’apprentissage de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

**revoscalepy** est une nouvelle bibliothèque de Python fourni par Microsoft pour prendre en charge l’informatique distribuée, de calcul distant contextes et des algorithmes de hautes performances pour les développeurs Python.

Il est basé sur le **RevoScaleR** package r, qui a été fourni dans Microsoft R Server et SQL Server R Services et vise à fournir les mêmes fonctionnalités :

+ Prend en charge plusieurs contextes de calcul, locales et distantes
+ Fournit des fonctions équivalentes à celles de RevoScaleR pour la visualisation et de transformation de données
+ Fournit des versions de Python d’algorithmes d’apprentissage automatique RevoScaleR pour le traitement distribué ou parallèle
+ Amélioration des performances, y compris l’utilisation des bibliothèques de mathématiques Intel

MicrosoftML packages sont également fournis pour R et Python. Pour plus d’informations, consultez [à l’aide de MicrosoftML dans SQL Server](../using-the-microsoftml-package.md)

## <a name="versions-and-supported-platforms"></a>Versions et plateformes prises en charge

Le **revoscalepy** module n’est disponible uniquement lorsque vous installez un des produits Microsoft suivants :

+ Apprentissage des Services, dans SQL Server 2017
+ Microsoft Machine Learning Server 9.2.0 ou version ultérieure

Pour obtenir la dernière version de revoscalepy, installez à jour Cumulative 1 pour SQL Server 2017. Elle inclut de nombreuses améliorations dans Python, y compris :

+ Une nouvelle fonction de Python, `rx_create_col_info`, qui obtient des informations de schéma à partir d’une source de données SQL Server, telles que [rxCreateColInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxcreatecolinfo) pour 
+ Améliorations apportées aux [rx_exec](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-exec) pour prendre en charge les scénarios parallèles à l’aide de la `RxLocalParallel` contexte de calcul. 

## <a name="supported-functions-and-data-types"></a>Prise en charge les fonctions et types de données

Cette section fournit une vue d’ensemble des types de données Python et des nouvelles fonctions de Python pris en charge dans les **revoscalepy** module, en commençant par la version de SQL Server 2017 CTP 2.0. 

Pour obtenir la dernière liste de fonctions dans les bibliothèques de Python émis à ce jour, consultez ces liens :

+ [revoscalepy pour Python](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

+ [Microsoftml pour Python](https://docs.microsoft.com/r-server/python-reference/microsoftml/microsoftml-package)

### <a name="data-types-data-sources-and-compute-contexts"></a>Types de données, les sources de données et les contextes de calcul

SQL Server et les Python utilisent différents types de données dans certains cas. Pour obtenir la liste des mappages entre les types de données SQL et Python, consultez [Python bibliothèques et Types de données](python-libraries-and-data-types.md).

Les sources de données prises en charge pour l’apprentissage avec Python dans SQL Server inclut les sources de données ODBC, base de données SQL Server et les fichiers locaux, y compris les fichiers XDF.

Vous créez l’objet de source de données à l’aide des fonctions répertoriées dans le tableau suivant. Après avoir défini la source de données, vous avez chargé ou transformer les données à l’aide appropriée [(fonction) ETL](#bkmk_etl).

> [!IMPORTANT]
> Nombre de noms de fonction ont changé depuis la version initiale de Python dans CTP 2.0.

**Données de SQL Server**

+ Utilisez [RxSqlServerData](https://docs.microsoft.com/r-server/python-reference/revoscalepy/rxsqlserverdata) pour définir une source de données à partir d’une requête ou une table
+ Utilisez [RxInSqlServer](https://docs.microsoft.com/r-server/python-reference/revoscalepy/rxinsqlserver) pour créer un contexte de calcul de SQL Server
+ Utilisez [RxOdbcData](https://docs.microsoft.com/r-server/python-reference/revoscalepy/rxodbcdata) pour créer une source de données à partir d’une connexion ODBC

**revoscalepy** prend également en charge la [source de données XDF](https://docs.microsoft.com/r-server/python-reference/revoscalepy/rxxdfdata), utilisé pour le déplacement des données entre la mémoire et d’autres sources de données.

> [!TIP]
> Si vous ne connaissez pas le concept de sources de données ou les contextes de calcul, nous vous recommandons de commencer par la lecture sur le fonctionnement informatique distribuée pour l’apprentissage de [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction).


### <a name="bkmk_algorithms"></a>Apprentissage automatique et fonctions de synthèse

Les algorithmes d’apprentissage automatique suivants et les fonctions de synthèse de RevoScaleR sont incluses dans SQL Server 2017, à partir de CTP 2.0.

| Fonction|  Description|Remarques|
| ------ | ------ |------ |
|`rx_btrees` | Ajuster les arbres de décision augmentés de gradient stochastique|`rx_btrees_ex` dans CTP 2.0|
|`rx_dforest` | Ajuster les forêts décisionnelles classification et de régression|`rx_dforest_ex` dans CTP 2.0|
|`rx_dtree` | Arbres de classification et de régression adaptés |`rx_dtree_ex` dans CTP 2.0|
|`rx_lin_mod` | Créer un modèle linéaire|`rx_lin_mod_ex` dans CTP 2.0|
|`rx_logit` | Créer un modèle de régression logistique|`rx_logit_ex` dans CTP 2.0|
|`rx_predict` | Générer des prédictions à partir d’un modèle formé|`rx_predict_ex` dans CTP 2.0|
|`rx_summary` | Générer un résumé du modèle||

Nouveaux algorithmes d’apprentissage automatique sont également fournies par la version Python de [MicrosoftML](https://docs.microsoft.com/r-server/python-reference/microsoftml/microsoftml-package):

| Fonction|  Description|
| ------ | ------ |
|`rx_fast_forest` |Créez un modèle de forêt de décision|
|`rx_fast_linear` | Régression linéaire avec élévation de coordonnées double stochastique|
|`rx_fast_trees` | Créer un modèle d’arbre augmenté |
|`rx_logistic_regression` | Créer un modèle de régression logistique|
|`rx_neural_network` | Créer un modèle de réseau neuronal personnalisable |
|`rx_oneclass_svm` | Crée un modèle SVM sur un jeu de données déséquilibré, pour la détection d’anomalie|

> [!TIP]
> La plupart de ces algorithmes sont déjà fournies sous forme de modules dans Azure Machine Learning.

MicrosoftML pour Python inclut également des transformations et les fonctions d’assistance, telles que :

+ `rx_predict` génère des prédictions à partir d’un modèle formé et peut être utilisé pour calculer les scores en temps réel
+ fonctions de la personnalisation d’image
+ fonctions d’extraction de traitement et l’opinion de texte

Pour plus d’informations, consultez [présentation MicrosoftML](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)


> [!NOTE]
> La Communauté Python utilise les conventions de codage qui peuvent être différentes de celui que vous êtes habitué, y compris toutes les lettres minuscules et des traits de soulignement au lieu de casse mixte pour les noms de paramètre. En outre, peut-être que vous avez remarqué que la **revoscalepy** bibliothèque est toujours en minuscule. C'est juste ! Une autre convention Python.
> 
> Consultez les conseils sur la documentation de référence Python Microsoft R: [aide de la fonction Python][aide sur la fonction Python](https://docs.microsoft.com/r-server/python-reference/introducing-python-package-reference)

## <a name="examples"></a>Exemples

Vous pouvez exécuter le code qui inclut **revoscalepy** fonctionne localement ou dans un contexte de calcul à distance. Vous pouvez également exécuter Python à l’intérieur de SQL Server en incorporant le code Python dans une procédure stockée.

Lorsque vous exécutez localement, vous en général, exécutez un script Python à partir de la ligne de commande ou dans un environnement de développement Python, puis spécifiez un contexte de calcul de SQL Server à l’aide d’une de la **revoscalepy** fonctions. Vous pouvez utiliser le contexte de calcul à distance pour l’ensemble du code, ou pour des fonctions individuelles. Par exemple, vous pouvez souhaiter décharger l’apprentissage du modèle sur le serveur pour utiliser les données les plus récentes et d’éviter le déplacement des données.

Si vous souhaitez placer un script Python complet à l’intérieur de la procédure stockée, [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql), nous vous recommandons de réécrire le code comme une fonction unique qui a défini clairement les entrées et sorties. Entrées et sorties doivent être **pandas** des trames de données. Dans ce cas, vous pouvez appeler la procédure stockée à partir de n’importe quel client qui prend en charge T-SQL, facilement transmettre des requêtes SQL en tant qu’entrées et enregistrer les résultats dans des tables SQL. Pour obtenir un exemple, consultez [Analytique de Python dans base de données pour les développeurs SQL](../tutorials/sqldev-in-database-python-for-sql-developers.md).

### <a name="using-remote-compute-contexts"></a>À l’aide de contextes de calcul à distance

+ [Créer un modèle à l’aide de revoscalepy](../tutorials/use-python-revoscalepy-to-create-model.md)

  Cet exemple montre comment exécuter les Python à l’aide d’une instance de SQL Server en tant que le contexte de calcul.

### <a name="using-a-stored-procedure"></a>À l’aide d’une procédure stockée

+ [Exécutez Python à l’aide de T-SQL](../tutorials/run-python-using-t-sql.md)

  Cet exemple montre comment le mécanisme de base de l’appel de script Python qui est incorporé dans une procédure stockée.

### <a name="using-revoscalepy-with-microsoftml"></a>À l’aide de revoscalepy avec MicrosoftML

Les fonctions de Python pour MicrosoftML sont intégrées avec les contextes de calcul et les sources de données qui sont fournies dans revoscalepy. Par conséquent, vous utilisez un algorithme MicrosoftML pour définir et effectuer l’apprentissage d’un modèle dans Python et a les fonctions revoscalepy permet d’exécuter le code Python localement ou dans un contexte de calcul de SQl Server.

Importer uniquement les modules dans votre code Python, puis faire référence aux fonctions individuelles que vous avez besoin.

```Python
from microsoftml.modules.logistic_regression.rx_logistic_regression import rx_logistic_regression
from revoscalepy.functions.RxSummary import rx_summary
from revoscalepy.etl.RxImport import rx_import_datasource
```

### <a name="requirements"></a>Spécifications

Pour exécuter le code Python dans SQL Server, vous devez avoir installé SQL Server 2017 avec la fonctionnalité de **Machine Learning Services**et le langage Python. Les versions antérieures de SQL Server ne gèrent pas l’intégration de Python.

> [!NOTE]
> Distributions d’ouvrir la source de Python ne gèrent pas les contextes de calcul de SQL Server. Toutefois, si vous avez besoin de publier et utiliser des applications Python à partir de Windows, vous pouvez installer Microsoft Machine Learning Server sans installation de SQL Server. Pour plus d’informations, consultez [installer SQL Server 2017 Machine Learning Server (autonome)](../install/sql-machine-learning-standalone-windows-install.md).

## <a name="get-more-help"></a>Obtenir de l’aide

Une documentation complète pour ces API seront disponible lorsque le produit est lancé. En attendant, nous recommandons que vous référencez la fonction correspondante dans les bibliothèques RevoScaleR ou MicrosoftML.

+ [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler).
+ [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml/microsoftml)

Vous pouvez obtenir de l’aide sur n’importe quelle fonction Python par l’importation du module, puis en appelant `help()`. Par exemple, en cours d’exécution `help(revoscalepy)` à partir de votre environnement IDE Python retourne une liste de toutes les fonctions dans le module revoscalepy, avec leurs signatures.

Si vous utilisez les outils Python pour Visual Studio, vous pouvez utiliser IntelliSense pour obtenir de l’aide de syntaxe et de l’argument. Pour plus d’informations, consultez [Python prend en charge dans Visual Studio](http://docs.microsoft.com/visualstudio/python/installation)et téléchargez l’extension qui correspond à votre version de Visual Studio. Vous pouvez utiliser Python avec Visual Studio 2015 et 2017 ou des versions antérieures.

## <a name="see-also"></a>Voir aussi

[Didacticiels Python](../tutorials/sql-server-python-tutorials.md)
