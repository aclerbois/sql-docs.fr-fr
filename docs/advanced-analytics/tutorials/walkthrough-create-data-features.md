---
title: Créer des fonctionnalités de données à l’aide de R et SQL (procédure pas à pas) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 226b8f2977f527d22a09fcbb4ab460e05285f858
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="create-data-features-using-r-and-sql-walkthrough"></a>Créer des fonctionnalités de données à l’aide de R et SQL (procédure pas à pas)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L’ingénierie des données est un aspect important du Machine Learning. Données nécessitent souvent une transformation avant de pouvoir l’utiliser pour la modélisation prédictive. Si les données n’ont pas les caractéristiques dont vous avez besoin, vous pouvez les créer à partir de valeurs existantes.

Pour cette tâche de modélisation, plutôt que d’utiliser les valeurs brutes de longitude et de latitude des lieux de prise en charge et de dépose, vous souhaitez avoir la distance en miles entre les deux emplacements. Pour créer cette fonctionnalité, vous calculez la distance directe linéaire entre deux points, à l’aide de la [formule haversine](https://en.wikipedia.org/wiki/Haversine_formula).

Dans cette étape, nous comparons les deux méthodes différentes pour la création d’une fonctionnalité à partir des données :

- À l’aide d’une fonction R personnalisée
- À l’aide d’une fonction personnalisée de T-SQL dans [!INCLUDE[tsql](../../includes/tsql-md.md)]

L’objectif est de créer un nouveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jeu de données qui incluant les colonnes d’origine et la nouvelle fonctionnalité numérique, *direct_distance*.

## <a name="featurization-using-r"></a>Personnalisation à l’aide de R

Le langage R est connu pour ses bibliothèques statistiques riches et variées, mais vous devrez peut-être créer des transformations de données personnalisées.

Tout d’abord, nous allons le faire R les utilisateurs sont habitués à : obtenir les données sur votre ordinateur portable et ensuite d’exécuter une fonction R personnalisée, *ComputeDist*, qui calcule la distance linéaire entre deux points spécifiés par les valeurs de latitude et longitude.

1. N’oubliez pas que l’objet de source de données que vous avez créé précédemment obtient uniquement les lignes du haut 1000. Par conséquent, nous allons définir une requête qui obtient toutes les données.

    ```R
    bigQuery <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,  pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude FROM nyctaxi_sample";
    ```

2. Créer une nouvelle source de données SQL Server à l’aide de la requête.

    ```R
    featureDataSource <- RxSqlServerData(sqlQuery = bigQuery,colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric", dropoff_longitude = "numeric", dropoff_latitude = "numeric", passenger_count  = "numeric", trip_distance  = "numeric", trip_time_in_secs  = "numeric", direct_distance  = "numeric"), connectionString = connStr);
    ```

    - [RxSqlServerData](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxsqlserverdata) peut prendre une requête composée d’une requête SELECT valide, fournie comme argument à la _sqlQuery_ paramètre ou le nom d’un objet table, fourni en tant que le _table_ paramètre.
    
    - Si vous souhaitez des exemples de données à partir d’une table, vous devez utiliser le _sqlQuery_ paramètre, définir des paramètres d’échantillonnage à l’aide de la clause TABLESAMPLE de T-SQL et définir le _rowBuffering_ l’argument de la valeur FALSE.

3. Exécutez le code suivant pour créer la fonction R personnalisée. ComputeDist accepte deux paires de valeurs de latitude et longitude et calcule la distance linéaire entre elles, en retournant la distance en miles.

    ```R
    env <- new.env();
    env$ComputeDist <- function(pickup_long, pickup_lat, dropoff_long, dropoff_lat){
      R <- 6371/1.609344 #radius in mile
      delta_lat <- dropoff_lat - pickup_lat
      delta_long <- dropoff_long - pickup_long
      degrees_to_radians = pi/180.0
      a1 <- sin(delta_lat/2*degrees_to_radians)
      a2 <- as.numeric(a1)^2
      a3 <- cos(pickup_lat*degrees_to_radians)
      a4 <- cos(dropoff_lat*degrees_to_radians)
      a5 <- sin(delta_long/2*degrees_to_radians)
      a6 <- as.numeric(a5)^2
      a <- a2+a3*a4*a6
      c <- 2*atan2(sqrt(a),sqrt(1-a))
      d <- R*c
      return (d)
    }
    ```
  
    + La première ligne définit un nouvel environnement. En R, un environnement peut être utilisé pour encapsuler des espaces de noms dans des packages et autres.  Vous pouvez utiliser la fonction `search()` pour afficher les environnements dans votre espace de travail. Pour afficher les objets dans un environnement spécifique, tapez `ls(<envname>)`.
    + Les lignes commençant par `$env.ComputeDistance` contiennent le code qui définit la formule de haversine, qui calcule la *distance orthodromique* entre deux points sur une sphère.

4. Définir la fonction, vous appliquez aux données pour créer une nouvelle colonne de fonctionnalité, *direct_distance*. mais avant d’exécuter la transformation, modifier le contexte de calcul en local.

    ```R
    rxSetComputeContext("local");
    ```

5. Appelez le [rxDataStep](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxdatastep) , la fonction pour obtenir la fonctionnalité données d’ingénierie et appliquer le `env$ComputeDist` fonction aux données en mémoire.

    ```R
    start.time <- proc.time();
  
    changed_ds <- rxDataStep(inData = featureEngineeringQuery,
    transforms = list(direct_distance=ComputeDist(pickup_longitude,pickup_latitude, dropoff_longitude, dropoff_latitude),
    tipped = "tipped", fare_amount = "fare_amount", passenger_count = "passenger_count",
    trip_time_in_secs = "trip_time_in_secs",  trip_distance="trip_distance",
    pickup_datetime = "pickup_datetime",  dropoff_datetime = "dropoff_datetime"),
    transformEnvir = env,
    rowsPerRead=500,
    reportProgress = 3);
  
    used.time <- proc.time() - start.time;
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""));
    ```

    + La fonction rxDataStep prend en charge différentes méthodes de modification des données en place. Pour plus d’informations, consultez l’article : [transformation et le sous-ensemble des données dans Microsft R](https://docs.microsoft.com/r-server/r/how-to-revoscaler-data-transform)
    
    Toutefois, quelques points à noter concernant rxDataStep : 
    
    Dans d’autres sources de données, vous pouvez utiliser les arguments *varsToKeep* et *varsToDrop*, mais ceux-ci ne sont pas pris en charge pour les sources de données SQL Server. Par conséquent, dans cet exemple, nous avons utilisé le _transforme_ argument pour spécifier les colonnes SQL directes et les colonnes transformés. En outre, lorsque en cours d’exécution dans SQL Server contexte de calcul, le _inData_ argument peut uniquement accepter une source de données SQL Server.

    Le code précédent peut également produire un message d’avertissement s’exécutent sur des jeux de données plus volumineux. Lorsque le nombre de lignes multiplié par le nombre de colonnes en cours de création dépasse une valeur définie (la valeur par défaut est 3 000 000), rxDataStep renvoie un avertissement, et le nombre de lignes dans la trame de données retourné sera tronqué. Pour supprimer l’avertissement, vous pouvez modifier le _maxRowsByCols_ argument dans la fonction rxDataStep. Toutefois, si _maxRowsByCols_ est trop volumineux, vous pouvez rencontrer des problèmes lors du chargement de la trame de données en mémoire.

7. Si vous le souhaitez, vous pouvez appeler [rxGetVarInfo](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxgetvarinfo) pour inspecter le schéma de la source de données transformées.

    ```R
    rxGetVarInfo(data = changed_ds);
    ```

## <a name="featurization-using-transact-sql"></a>Personnalisation à l’aide de Transact-SQL

À présent, créez une fonction SQL personnalisée, *ComputeDist*, pour accomplir la même tâche que la fonction R personnalisée.

1. Définissez une nouvelle fonction SQL personnalisée, nommée *fnCalculateDistance*. Le code de cette fonction SQL définie par l’utilisateur est fourni dans le cadre du script PowerShell que vous avez exécuté pour créer et configurer la base de données.  La fonction doit déjà exister dans votre base de données.

    Si elle n’existe pas, utilisez SQL Server Management Studio pour générer la fonction dans la même base de données que celle où sont stockées les données de taxi.

    ```sql
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
    -- User-defined function calculates the direct distance between two geographical coordinates.
    RETURNS
    AS
    BEGIN
      DECLARE @distance decimal(28, 10)
      -- Convert to radians
      SET @Lat1 = @Lat1 / 57.2958
      SET @Long1 = @Long1 / 57.2958
      SET @Lat2 = @Lat2 / 57.2958
      SET @Long2 = @Long2 / 57.2958
      -- Calculate distance
      SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
      --Convert to miles
      IF @distance <> 0
      BEGIN
        SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
      END
      RETURN @distance
    END
    ```

2. Exécutez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante à partir de toute application prenant en charge [!INCLUDE[tsql](../../includes/tsql-md.md)] pour voir comment la fonction se comporte.

    ```sql
    SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance,
    pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude
    FROM nyctaxi_sample
    ```
3. Définir cette fonction, il est facile créer les fonctionnalités souhaitées à l’aide de SQL, puis insérer les valeurs directement dans une nouvelle table :

    ```
    SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance,
    pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude
    INTO NewFeatureTable
    FROM nyctaxi_sample
    ```

4. Toutefois, voyons comment appeler la fonction SQL personnalisée à partir du code R. Tout d’abord, stocker la personnalisation de la la requête SQL dans une variable de R.

    ```R
    featureEngineeringQuery = "SELECT tipped, fare_amount, passenger_count,
        trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
        dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance,
        pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude
        FROM nyctaxi_sample
        tablesample (1 percent) repeatable (98052)"
    ```
  
    > [!TIP]
    > Cette requête a été modifiée pour obtenir un plus petit échantillon de données, afin d’accélérer cette procédure pas à pas. Vous pouvez supprimer la clause TABLESAMPLE si vous souhaitez obtenir les données ; Toutefois, selon votre environnement, il ne peut pas être possible de charger le DataSet complet dans R, ce qui entraîne une erreur.
  
5. Utilisez les lignes de code suivantes pour appeler la fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] à partir de votre environnement R et l’appliquer aux données définies dans *featureEngineeringQuery*.
  
    ```R
    featureDataSource = RxSqlServerData(sqlQuery = featureEngineeringQuery,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
             dropoff_longitude = "numeric", dropoff_latitude = "numeric",
             passenger_count  = "numeric", trip_distance  = "numeric",
              trip_time_in_secs  = "numeric", direct_distance  = "numeric"),
      connectionString = connStr)
    ```
  
6.  Maintenant que la nouvelle fonctionnalité est créée, appelez **rxGetVarsInfo** pour créer un résumé des données dans la table des composants.
  
    ```R
    rxGetVarInfo(data = featureDataSource)
    ```

    *Résultats*

    ```
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: numeric
    Var 4: trip_time_in_secs, Type: numeric
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: direct_distance, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: pickup_longitude, Type: numeric
    Var 11: dropoff_latitude, Type: numeric
    Var 12: dropoff_longitude, Type: numeric
    ```

    > [!NOTE]
    > Dans certains cas, vous pouvez obtenir une erreur semblable à celui-ci : *d’une autorisation EXECUTE l’a été refusée sur l’objet 'fnCalculateDistance'* dans ce cas, assurez-vous que la connexion que vous utilisez dispose des autorisations pour exécuter des scripts et de créer des objets sur la base de données, et pas seulement sur l’instance.
    > Vérifiez le schéma de l’objet, fnCalculateDistance. Si l’objet a été créé par le propriétaire de la base de données et votre nom d’utilisateur auquel appartient le rôle db_datareader, vous devez donner des autorisations explicites pour exécuter le script de la connexion.

## <a name="comparing-r-functions-and-sql-functions"></a>Comparaison des fonctions R et les fonctions SQL

N’oubliez pas de cette partie du code utilisé à la fois, le code R ?

```R
start.time <- proc.time()
<your code here>
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""))
```

Vous pouvez essayer d’utiliser cela avec l’exemple de fonction personnalisée de SQL pour voir la durée pendant laquelle la transformation de données lors de l’appel d’une fonction SQL prend. En outre, essayez de basculer les contextes de calcul avec rxSetComputeContext et comparer le minutage.

Vos heures peuvent varier considérablement, selon la vitesse de votre réseau et votre configuration matérielle. Dans les configurations que nous avons testé, le [!INCLUDE[tsql](../../includes/tsql-md.md)] approche de la fonction a été plus rapide que l’utilisation d’une fonction R personnalisée. Par conséquent, nous avons utilisé le [!INCLUDE[tsql](../../includes/tsql-md.md)] fonction pour ces calculs dans les étapes suivantes.

> [!TIP]
> Très souvent, des fonctionnalités à l’aide d’ingénierie [!INCLUDE[tsql](../../includes/tsql-md.md)] sera plus rapide que R. Par exemple, T-SQL inclut fenêtrage rapide et des fonctions de classement qui peuvent être appliquées aux calculs de science des données courantes telles que la restauration des moyennes mobiles et *n*-vignettes. Choisissez la méthode la plus efficace en fonction de vos données et de vos tâches.

## <a name="next-lesson"></a>Leçon suivante

[Générez un modèle R et enregistrez à SQL](walkthrough-build-and-save-the-model.md)

## <a name="previous-lesson"></a>Leçon précédente

[Afficher et synthétiser des données à l’aide de R](walkthrough-view-and-summarize-data-using-r.md)

