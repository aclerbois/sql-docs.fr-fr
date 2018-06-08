---
title: Étape 6 tiens le modèle de Python à l’aide de SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: aedd6beeb720c24a6960950abc6a29c1bf89a5fa
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="step-6-operationalize-the-python-model-using-sql-server"></a>Étape 6 : Mettre le modèle de Python à l’aide de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie d’un didacticiel, [analytique Python de la base de données pour les développeurs SQL](sqldev-in-database-python-for-sql-developers.md). 

Dans cette étape, vous apprenez à *opérationnaliser* les modèles que vous avez formé et enregistré à l’étape précédente.

Dans ce scénario, à l’Opérationnalisation signifie en production pour calculer les scores du déploiement du modèle. L’intégration avec SQL Server rend cette assez simple, car vous pouvez incorporer le code Python dans une procédure stockée. Pour obtenir des prédictions à partir du modèle en fonction de nouvelles entrées, simplement appeler la procédure stockée à partir d’une application et transmettre les nouvelles données.

Cette leçon illustre deux méthodes pour créer des prédictions basées sur un modèle de Python : calcul de score et le score de ligne par ligne du lot.

- **Calcul du score du lot :** pour fournir plusieurs lignes de données d’entrée, passez une requête SELECT comme argument à la procédure stockée. Le résultat est une table d’observations correspondant aux cas d’entrée.
- **Calculer les scores individuels :** passer un ensemble de valeurs de paramètre individuelles en tant qu’entrée.  La procédure stockée retourne une seule ligne ou valeur.

Tout le code Python que nécessaire pour calculer le score est fourni dans le cadre des procédures stockées.

| Nom de la procédure stockée | Traitement par lots ou unique | Source de modèle|
|----|----|----|
|PredictTipRxPy|lot| modèle de revoscalepy|
|PredictTipSciKitPy|lot |scikit-en savoir plus de modèle|
|PredictTipSingleModeRxPy|ligne unique| modèle de revoscalepy|
|PredictTipSingleModeSciKitPy|ligne unique| scikit-en savoir plus de modèle|

## <a name="batch-scoring"></a>Calcul du score du lot

Les deux premières procédures stockées illustrent la syntaxe de base pour encapsuler un appel de prédiction Python dans une procédure stockée. Les deux procédures stockées requièrent une table de données en tant qu’entrées.

- Le nom du modèle exact à utiliser est fourni en tant que paramètre d’entrée à la procédure stockée. La procédure stockée charge le modèle sérialisé à partir de la table de base de données `nyc_taxi_models`.table, à l’aide de l’instruction SELECT dans la procédure stockée.
- Le modèle sérialisé est stocké dans la variable de Python `mod` pour un traitement supplémentaire à l’aide de Python.
- Les nouveaux cas qui doivent être évaluées sont obtenues à partir de la [!INCLUDE[tsql](../../includes/tsql-md.md)] requête spécifiée dans `@input_data_1`. Lors de la lecture des données de requête, les lignes sont enregistrées dans la trame de données par défaut, `InputDataSet`.
- Les deux procédures stockées utilisent des fonctions à partir de `sklearn` pour calculer une mesure de précision, AUC (zone sous la courbe). Les mesures de précision telles que AUC ne peuvent être générées que si vous fournissez également l’étiquette cible (le _incliné_ colonne). Prédictions n’avez pas besoin de l’étiquette cible (variable `y`), mais le calcul de mesures de précision.

    Par conséquent, si vous n’avez pas les étiquettes de cible pour les données à noter, vous pouvez modifier la procédure stockée pour supprimer les calculs AUC et retourner uniquement les probabilités de Conseil dans les fonctionnalités (variable `X` dans la procédure stockée).

### <a name="predicttipscikitpy"></a>PredictTipSciKitPy

La procédure stockée doit déjà avoir été créée pour vous. Si vous ne le trouvez pas, exécutez les instructions T-SQL suivantes pour créer les procédures stockées.

Cette procédure stockée requiert un modèle basé sur le scikit-en savoir plus de package, car elle utilise les fonctions spécifiques à ce package :

+ La trame de données contenant des entrées est passée à la `predict_proba` fonction du modèle de régression logistique, `mod`. Le `predict_proba` (fonction) (`probArray = mod.predict_proba(X)`) retourne un **float** qui représente la probabilité que recevra un Conseil (de n’importe quel volume).

```SQL
CREATE PROCEDURE [dbo].[PredictTipSciKitPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
  DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
  EXEC sp_execute_external_script
    @language = N'Python',
    @script = N'
        import pickle;
        import numpy;
        from sklearn import metrics
        
        mod = pickle.loads(lmodel2)
        
        X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
        y = numpy.ravel(InputDataSet[["tipped"]])
        
        probArray = mod.predict_proba(X)
        probList = []
        for i in range(len(probArray)):
          probList.append((probArray[i])[1])
        
        probArray = numpy.asarray(probList)
        fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
        aucResult = metrics.auc(fpr, tpr)
        print ("AUC on testing data is: " + str(aucResult))
        
        OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
        ',  
    @input_data_1 = @inquery,
    @input_data_1_name = N'InputDataSet',
    @params = N'@lmodel2 varbinary(max)',
    @lmodel2 = @lmodel2
  WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttiprxpy"></a>PredictTipRxPy

Cette procédure stockée utilise les mêmes entrées et crée le même type de résultats en tant que la procédure précédente, mais utilise des fonctions à partir de la **revoscalepy** package fourni avec l’apprentissage de SQL Server.

> [!NOTE] 
> Le code de cette procédure stockée a légèrement changé entre les versions préliminaires et la version RTM, afin de refléter les modifications apportées au package revoscalepy. Consultez le [modifications](#changes) table pour plus d’informations.

```SQL
CREATE PROCEDURE [dbo].[PredictTipRxPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
  DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models2 where name = @model);

  EXEC sp_execute_external_script 
    @language = N'Python',
    @script = N'
      import pickle;
      import numpy;
      from sklearn import metrics
      from revoscalepy.functions.RxPredict import rx_predict;
      
      mod = pickle.loads(lmodel2)
      X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
      y = numpy.ravel(InputDataSet[["tipped"]])
      
      probArray = rx_predict(mod, X)
      prob_list = prob_array["tipped_Pred"].values 
      
      probArray = numpy.asarray(probList)
      fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
      aucResult = metrics.auc(fpr, tpr)
      print ("AUC on testing data is: " + str(aucResult))
      OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
      ',    
    @input_data_1 = @inquery,
    @input_data_1_name = N'InputDataSet',
    @params = N'@lmodel2 varbinary(max)',
    @lmodel2 = @lmodel2
  WITH RESULT SETS ((Score float));
END
GO
```

## <a name="run-batch-scoring-using-a-select-query"></a>Exécuter le traitement par lots de calcul de score à l’aide d’une requête SELECT

Les procédures stockées **PredictTipSciKitPy** et **PredictTipRxPy** requièrent deux paramètres d’entrée : 

- La requête qui Récupère les données pour calculer les scores
- Le nom d’un modèle formé

En passant les arguments à la procédure stockée, vous pouvez sélectionner un modèle particulier ou modifier les données utilisées pour calculer les scores.

1. Pour utiliser le **scikit-en savoir plus** de modèle pour le calcul de score, appelez la procédure stockée **PredictTipSciKitPy**, en passant le nom du modèle et de chaîne de requête en tant qu’entrées.

    ```SQL
    DECLARE @query_string nvarchar(max) -- Specify input query
      SET @query_string='
      select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
      dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
      from nyctaxi_sample_testing'
    EXEC [dbo].[PredictTipSciKitPy] 'SciKit_model', @query_string;
    ```

    La procédure stockée retourne les probabilités prévues pour chaque voyage qui a été passée dans le cadre de la requête d’entrée. 
    
    Si vous utilisez SSMS (SQL Server Management Studio) pour l’exécution de requêtes, les probabilités seront affiche sous forme de table dans le **résultats** volet. Le **Messages** volet génère les mesures de précision (AUC ou zone sous la courbe) avec une valeur de 0,56 environ.

2. Pour utiliser le **revoscalepy** de modèle pour le calcul de score, appelez la procédure stockée **PredictTipRxPy**, en passant le nom du modèle et de chaîne de requête en tant qu’entrées.

    ```SQL
    EXEC [dbo].[PredictTipRxPy] 'revoscalepy_model', @query_string;
    ```

## <a name="single-row-scoring"></a>Ligne unique de calcul de score

Au lieu de score par lot, vous pouvez parfois à passer dans un cas unique, l’obtention des valeurs à partir d’une application et de retourner un résultat unique en fonction de ces valeurs. Par exemple, vous pouvez définir une feuille de calcul Excel, l’application web ou le rapport pour appeler la procédure stockée et passez-la à entrées sélectionnées par les utilisateurs ou non typé.

Dans cette section, vous allez apprendre à créer des prévisions uniques en appelant les deux procédures stockées :

+ [PredictTipSingleModeSciKitPy](#PredictTipSingleModeSciKitPy) est conçu pour calculer les scores seule ligne à l’aide de la scikit-en savoir plus le modèle.
+ [PredictTipSingleModeRxPy](#PredictTipSingleModeRxPy) est conçu pour calculer les scores seule ligne à l’aide du modèle revoscalepy.
+ Si vous n’avez pas un modèle entraîné encore, retournez à [étape 5](sqldev-py5-train-and-save-a-model-using-t-sql.md)!

Les deux prennent les modèles en tant qu’entrée d’une série de valeurs uniques, telles que le nombre de passagers, distance du voyage et ainsi de suite. Une fonction table, `fnEngineerFeatures`, est utilisé pour convertir des valeurs de latitude et longitude à partir des entrées à une nouvelle fonctionnalité, directe de distance. [Leçon 4](sqldev-py4-create-data-features-using-t-sql.md) contient une description de cette fonction table.

Les deux procédures stockées créent un score basé sur le modèle de Python.

> [!NOTE]
> 
> Il est important que vous fournissez toutes les fonctionnalités d’entrée requises par le modèle de Python lorsque vous appelez la procédure stockée à partir d’une application externe. Pour éviter les erreurs, vous devrez peut-être convertir les données d’entrée à un type de données Python, en plus de la validation de type de données et la longueur des données.

### <a name="predicttipsinglemodescikitpy"></a>PredictTipSingleModeSciKitPy

Prenez une minute pour examiner le code de la procédure stockée qui effectue le calcul de score à l’aide de la **scikit-en savoir plus** modèle.

```SQL
CREATE PROCEDURE [dbo].[PredictTipSingleModeSciKitPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
    '
  DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models2 where name = @model);
  EXEC sp_execute_external_script 
    @language = N'Python',
    @script = N'
      import pickle;
      import numpy;
      
      # Load model and unserialize
      mod = pickle.loads(model)
      
      # Get features for scoring from input data
      X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
      
      # Score data to get tip prediction probability as a list (of float)
      probList = []
      probList.append((mod.predict_proba(X)[0])[1])
      
      # Create output data frame
      OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
    ',
    @input_data_1 = @inquery,
    @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
      @trip_time_in_secs int ,
      @pickup_latitude float ,
      @pickup_longitude float ,
      @dropoff_latitude float ,
      @dropoff_longitude float',
      @model = @lmodel2,
      @passenger_count =@passenger_count ,
      @trip_distance=@trip_distance,
      @trip_time_in_secs=@trip_time_in_secs,
      @pickup_latitude=@pickup_latitude,
      @pickup_longitude=@pickup_longitude,
      @dropoff_latitude=@dropoff_latitude,
      @dropoff_longitude=@dropoff_longitude
  WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttipsinglemoderxpy"></a>PredictTipSingleModeRxPy

La procédure stockée suivante effectue le calcul de score à l’aide de la **revoscalepy** modèle.

```SQL
CREATE PROCEDURE [dbo].[PredictTipSingleModeRxPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
    SELECT * FROM [dbo].[fnEngineerFeatures]( 
      @passenger_count,
      @trip_distance,
      @trip_time_in_secs,
      @pickup_latitude,
      @pickup_longitude,
      @dropoff_latitude,
      @dropoff_longitude)
    '
  DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models2 where name = @model);
  EXEC sp_execute_external_script 
    @language = N'Python',
    @script = N'
      import pickle;
      import numpy;
      from revoscalepy.functions.RxPredict import rx_predict;
      
      # Load model and unserialize
      mod = pickle.loads(model)
      
      # Get features for scoring from input data
      X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
      
      # Score data to get tip prediction probability as a list (of float)
      
      probArray = rx_predict(mod, X)
      
      probList = []
      prob_list = prob_array["tipped_Pred"].values
      
      # Create output data frame
      OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
      ',
    @input_data_1 = @inquery,
    @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
      @trip_time_in_secs int ,
      @pickup_latitude float ,
      @pickup_longitude float ,
      @dropoff_latitude float ,
      @dropoff_longitude float',
      @model = @lmodel2,
      @passenger_count =@passenger_count ,
      @trip_distance=@trip_distance,
      @trip_time_in_secs=@trip_time_in_secs,
      @pickup_latitude=@pickup_latitude,
      @pickup_longitude=@pickup_longitude,
      @dropoff_latitude=@dropoff_latitude,
      @dropoff_longitude=@dropoff_longitude
  WITH RESULT SETS ((Score float));
END
GO
```

### <a name="generate-scores-from-models"></a>Générer des scores à partir de modèles

Une fois que les procédures stockées ont été créées, il est facile de générer un score basé sur un modèle. Il suffit d’ouvrir un nouveau **requête** fenêtre et les paramètres de type ou de coller pour chacune des colonnes de la fonctionnalité. Les sept requis sont des valeurs pour ces colonnes de fonctionnalités, dans l’ordre :
    
+ *passenger_count*
+ *trip_distance* v*trip_time_in_secs*
+ *pickup_latitude*
+ *pickup_longitude*
+ *dropoff_latitude*
+ *dropoff_longitude*

1. Pour générer une prédiction à l’aide de la **revoscalepy** modèle, exécutez cette instruction :
  
    ```SQL
    EXEC [dbo].[PredictTipSingleModeRxPy] 'revoscalepy_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

2. Pour générer un score à l’aide de la **scikit-en savoir plus** modèle, exécutez cette instruction :

    ```SQL
    EXEC [dbo].[PredictTipSingleModeSciKitPy] 'linear_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

La sortie à partir de deux procédures est la probabilité d’une info-bulle est payée pour le voyage taxi avec les fonctionnalités ou les paramètres spécifiés.

### <a name="changes"></a> Modifications

Cette section répertorie les modifications de code utilisé dans ce didacticiel. Ces modifications ont été apportées afin de refléter les dernières **revoscalepy** version. Pour obtenir de l’API, consultez [Python fonction référence de la bibliothèque](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference).

| Modifier les détails | Remarques|
| ----|----|
| supprimé `import pandas` dans tous les exemples| pandas maintenant chargés par défaut|
| fonction `rx_predict_ex` remplacé par `rx_predict`| Exigent des versions RTM et en version préliminaire `rx_predict_ex`|
| fonction `rx_logit_ex` remplacé par `rx_logit`| Exigent des versions RTM et en version préliminaire `rx_logit_ex`|
| ` probList.append(probArray._results["tipped_Pred"])` remplacé par `prob_list = prob_array["tipped_Pred"].values`| mises à jour des API|

Si vous avez installé les Services de Python à l’aide d’une version préliminaire de SQL Server 2017, nous vous recommandons de mettre à niveau que vous. Vous pouvez également mettre à niveau uniquement les composants de Python et R à l’aide de la dernière version du serveur de Machine Learning. Pour plus d’informations, consultez [à l’aide de la liaison pour mettre à niveau une instance de SQL Server](../r/use-sqlbindr-exe-to-upgrade-an-instance-of-sql-server.md).

## <a name="conclusions"></a>Conclusions

Dans ce didacticiel, vous avez appris à utiliser avec le code Python incorporé dans des procédures stockées. L’intégration avec [!INCLUDE[tsql](../../includes/tsql-md.md)] rend plus facile à déployer des modèles de Python pour la prédiction et d’incorporer le recyclage de modèle dans le cadre d’un workflow de données d’entreprise.

## <a name="previous-step"></a>Étape précédente

[Étape 5 : L’apprentissage et enregistrez un modèle de Python](sqldev-py5-train-and-save-a-model-using-t-sql.md)

## <a name="see-also"></a>Voir aussi

[Machine Learning Services avec Python](../python/sql-server-python-services.md)
