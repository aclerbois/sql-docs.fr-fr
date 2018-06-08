---
title: Étape 5 Train et enregistrer un modèle de Python à l’aide de T-SQL | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 173df47b5862a892efcd0c5e7064349daedf7efb
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="step-5-train-and-save-a-python-model-using-t-sql"></a>Étape 5 : L’apprentissage et enregistrer un modèle de Python à l’aide de T-SQL
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie d’un didacticiel, [analytique Python de la base de données pour les développeurs SQL](sqldev-in-database-python-for-sql-developers.md). 

Dans cette étape, vous apprenez former un modèle d’apprentissage à l’aide des packages Python **scikit-en savoir plus** et **revoscalepy**. Ces bibliothèques Python sont déjà installés avec SQL Server Machine Learning Services.

Vous chargez les modules et appelez les fonctions nécessaires pour créer et de l’apprentissage du modèle à l’aide d’une procédure stockée SQL Server. Le modèle requiert les fonctionnalités de données que vous conçues dans les leçons précédentes. Enfin, vous enregistrez le modèle formé dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table.

> [!IMPORTANT]
> Il y a eu plusieurs modifications dans le **revoscalepy** package nécessitant des petites modifications dans le code pour ce didacticiel. Consultez le [modifications](sqldev-py6-operationalize-the-model.md#changes) à la fin de ce didacticiel. 
> 
> Si vous avez installé les Services de Python à l’aide d’une version préliminaire de SQL Server 2017, nous vous recommandons de mettre à niveau vers la dernière version. 

## <a name="split-the-sample-data-into-training-and-testing-sets"></a>Fractionner les exemples de données en jeux d’apprentissage et jeux de test

1. Vous pouvez utiliser la procédure stockée **TrainTestSplit** pour diviser les données dans le nyctaxi\_exemple de table en deux parties : nyctaxi\_exemple\_d’apprentissage et nyctaxi\_exemple\_test. 

    Cette procédure stockée doit déjà être créée pour vous, mais vous pouvez exécuter le code suivant pour la créer :

    ```SQL
    CREATE PROCEDURE [dbo].[TrainTestSplit] (@pct int)
    AS
    
    DROP TABLE IF EXISTS dbo.nyctaxi_sample_training
    SELECT * into nyctaxi_sample_training FROM nyctaxi_sample WHERE (ABS(CAST(BINARY_CHECKSUM(medallion,hack_license)  as int)) % 100) < @pct
    
    DROP TABLE IF EXISTS dbo.nyctaxi_sample_testing
    SELECT * into nyctaxi_sample_testing FROM nyctaxi_sample
    WHERE (ABS(CAST(BINARY_CHECKSUM(medallion,hack_license)  as int)) % 100) > @pct
    GO
    ```

2. Pour diviser vos données à l’aide d’une division personnalisée, exécutez la procédure stockée et tapez un entier qui représente le pourcentage de données alloués au jeu d’apprentissage. Par exemple, l’instruction suivante affecte à 60 % des données au jeu d’apprentissage.

    ```SQL
    EXEC TrainTestSplit 60
    GO
    ```

## <a name="build-a-logistic-regression-model"></a>Générer un modèle de régression logistique

Une fois les données a été préparées, vous pouvez l’utiliser pour former un modèle. Pour cela en appelant une procédure procédure qui s’exécute du code Python, en prenant comme entrée de la table de données d’apprentissage. Pour ce didacticiel, vous créez deux modèles, les deux modèles de classification binaire :

+ La procédure stockée **TrainTipPredictionModelRxPy** crée un modèle de prévision de Conseil avec le **revoscalepy** package.
+ La procédure stockée **TrainTipPredictionModelSciKitPy** crée un modèle de prévision de Conseil avec le **scikit-en savoir plus** package.

Chaque procédure stockée utilise les données d’entrée afin de créer et d’effectuer l’apprentissage d’un modèle de régression logistique. Tout le code Python est encapsulé dans la procédure stockée système, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Pour faciliter le former à nouveau le modèle sur de nouvelles données, vous encapsulez l’appel à sp_execute_exernal_script d’une autre procédure stockée et transmettez les nouvelles données d’apprentissage en tant que paramètre. Cette section vous guidera tout au long de ce processus.

### <a name="traintippredictionmodelscikitpy"></a>TrainTipPredictionModelSciKitPy

1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ouvrez une nouvelle **requête** fenêtre et exécutez l’instruction suivante pour créer la procédure stockée _TrainTipPredictionModelSciKitPy_.  La procédure stockée contient une définition des données d’entrée, vous n’avez pas besoin de fournir une requête d’entrée.

    ```SQL
    DROP PROCEDURE IF EXISTS TrainTipPredictionModelSciKitPy;
    GO

    CREATE PROCEDURE [dbo].[TrainTipPredictionModelSciKitPy] (@trained_model varbinary(max) OUTPUT)
    AS
    BEGIN
      EXEC sp_execute_external_script
      @language = N'Python',
      @script = N'
      import numpy
      import pickle
      from sklearn.linear_model import LogisticRegression
      
      ##Create SciKit-Learn logistic regression model
      X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
      y = numpy.ravel(InputDataSet[["tipped"]])
      
      SKLalgo = LogisticRegression()
      logitObj = SKLalgo.fit(X, y)
      
      ##Serialize model
      trained_model = pickle.dumps(logitObj)
      ',
      @input_data_1 = N'
      select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, 
      dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
      from nyctaxi_sample_training
      ',
      @input_data_1_name = N'InputDataSet',
      @params = N'@trained_model varbinary(max) OUTPUT',
      @trained_model = @trained_model OUTPUT;
      ;
    END;
    GO
    ```

2. Exécutez ce qui suit les instructions SQL pour insérer le modèle formé dans la table nyc\_taxi_models.

    ```SQL
    DECLARE @model VARBINARY(MAX);
    EXEC TrainTipPredictionModelSciKitPy @model OUTPUT;
    INSERT INTO nyc_taxi_models (name, model) VALUES('SciKit_model', @model);
    ```

    Le traitement des données et l’ajustement du modèle peuvent prendre quelques minutes. Les messages qui sont redirigés vers Python **stdout** flux sont affichés dans le **Messages** fenêtre de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Par exemple :

    *Messages STDOUT du script externe :*
  *C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\revoscalepy*

3. Ouvrez la table *nyc\_taxi_models*. Vous pouvez voir qu’une nouvelle ligne a été ajoutée, avec le modèle sérialisé dans la colonne _model_.

    *linear_model* *0x800363736B6C6561726E2E6C696E6561...*

### <a name="traintippredictionmodelrxpy"></a>TrainTipPredictionModelRxPy

Cette procédure stockée utilise le nouveau **revoscalepy** package, qui est un nouveau package pour Python. Il contient des objets, de transformation et algorithmes similaires à ceux fournis pour la langue de R **RevoScaleR** package. 

À l’aide de **revoscalepy**, vous pouvez créer des contextes de calcul à distance, le calcul de déplacement des données entre les contextes de transformer des données et effectuer l’apprentissage des modèles prédictifs à l’aide des algorithmes populaires tels que de la régression logistique et linéaire, les arbres de décision, et plus. Pour plus d’informations, consultez [What ' s revoscalepy ?](../python/what-is-revoscalepy.md)

1. Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], ouvrez une nouvelle **requête** fenêtre et exécutez l’instruction suivante pour créer la procédure stockée _TrainTipPredictionModelRxPy_.  Étant donné que la procédure stockée contient déjà une définition des données d’entrée, vous n’avez pas besoin de fournir une requête d’entrée.

    ```SQL
    DROP PROCEDURE IF EXISTS TrainTipPredictionModelRxPy;
    GO

    CREATE PROCEDURE [dbo].[TrainTipPredictionModelRxPy] (@trained_model varbinary(max) OUTPUT)
    AS
    BEGIN
    EXEC sp_execute_external_script 
      @language = N'Python',
      @script = N'
    import numpy
    import pickle
    from revoscalepy.functions.RxLogit import rx_logit
    
    ## Create a logistic regression model using rx_logit function from revoscalepy package
    logitObj = rx_logit("tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance", data = InputDataSet);
    
    ## Serialize model
    trained_model = pickle.dumps(logitObj)
    ',
    @input_data_1 = N'
    select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, 
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
    from nyctaxi_sample_training
    ',
    @input_data_1_name = N'InputDataSet',
    @params = N'@trained_model varbinary(max) OUTPUT',
    @trained_model = @trained_model OUTPUT;
    ;
    END;
    GO
    ```

    Cette procédure stockée effectue les étapes suivantes dans le cadre de l’apprentissage du modèle :

    - La requête SELECT s’applique à la fonction scalaire personnalisée _fnCalculateDistance_ pour calculer la distance directe entre les emplacements de prélèvement et de remise. Les résultats de la requête sont stockés dans la variable d’entrée de Python par défaut, `InputDataset`.
    - La variable binaire _incliné_ est utilisé comme le *étiquette* ou de la colonne de résultat et le modèle est adapté à l’aide de ces colonnes de fonctionnalités : _passenger_count_, _trip_ distance_, _trip_time_in_secs_, et _direct_distance_.
    - Le modèle formé est sérialisé et stocké dans la variable de Python `logitObj`. En ajoutant le mot clé de T-SQL OUTPUT, vous pouvez ajouter la variable comme sortie de la procédure stockée. Dans l’étape suivante, cette variable est utilisée pour insérer le code binaire du modèle dans une table de base de données _nyc_taxi_models_. Ce mécanisme permet simple stocker et réutiliser les modèles.

2. Exécutez la procédure stockée comme suit pour insérer l’objet d’un apprentissage **revoscalepy** modèle dans la table _nyc\_taxi\_modèles.

    ```SQL
    DECLARE @model VARBINARY(MAX);
    EXEC TrainTipPredictionModelRxPy @model OUTPUT;
    
    INSERT INTO nyc_taxi_models (name, model) VALUES('revoscalepy_model', @model);
    ```

    Le traitement des données et l’ajustement du modèle peuvent prendre un certain temps. Les messages qui sont redirigés vers Python **stdout** flux sont affichés dans le **Messages** fenêtre de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Par exemple :

    *Messages STDOUT du script externe :*
  *C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\PYTHON_SERVICES\lib\site-packages\revoscalepy*

3. Ouvrez la table *nyc_taxi_models*. Vous pouvez voir qu’une nouvelle ligne a été ajoutée, avec le modèle sérialisé dans la colonne _model_.

    *rx_model* *0x8003637265766F7363616c...*

Dans l’étape suivante, vous utilisez les modèles formés pour créer des prédictions.

## <a name="next-step"></a>Étape suivante

[Étape 6 : Mettre le modèle de Python à l’aide de SQL Server](sqldev-py6-operationalize-the-model.md)

## <a name="previous-step"></a>Étape précédente

[Étape 4 : Créer des caractéristiques de données à l’aide de T-SQL](sqldev-py5-train-and-save-a-model-using-t-sql.md)
