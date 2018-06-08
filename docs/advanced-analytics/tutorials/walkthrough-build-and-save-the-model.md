---
title: Générer un modèle R et l’enregistrer dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: c580cc3a6e5fefd7882d4fc58f6eacd6d9999f71
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="build-an-r-model-and-save-to-sql-server"></a>Générer un modèle R et l’enregistrer dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans cette étape, vous allez apprendre à créer un modèle d’apprentissage automatique et enregistrez le modèle dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="create-a-classification-model-using-rxlogit"></a>Créer un modèle de classification à l’aide de rxLogit

Le modèle que vous créez est un classifieur binaire qui prédit si le pilote taxi est susceptible d’obtenir un Conseil sur un cas particulier ou non. Vous allez utiliser la source de données que vous avez créé dans la leçon précédente pour former le classifieur Conseil, à l’aide de la régression logistique.

1. Appelez la fonction [rxLogit](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxlogit) , comprise dans le package **RevoScaleR** , pour créer un modèle de régression logistique. 

    ```R
    system.time(logitObj <- rxLogit(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = sql_feature_ds));
    ```

    L’appel qui génère le modèle est placé dans la fonction system.time. Cela vous donne le temps de générer le modèle.

2. Après avoir créé le modèle, vous pouvez le vérifier à l’aide de la `summary` la fonction et afficher les coefficients.

    ```R
    summary(logitObj);
    ```

     *Résultats*

     *Résultats de la régression logistique pour : pourboires ~ passenger_count + trip_distance + trip_time_in_secs +* direct_distance *   <br/>*Données : featureDataSource (Source de données RxSqlServerData)*
     <br/>*Dependent variable(s) : incliné*
     <br/>*Nombre total de variables indépendantes : 5*
     <br/>*Nombre d’observations valides : 17068*
     <br/>*Nombre d’observations manquantes : 0*
     <br/>*-2\*LogLikelihood : 23540.0602 (déviance qui sont resté sur les degrés de 17063 liberté)*
     <br/>*Coefficients :*
     <br/>*Estimate Std. Valeur d’erreur z Pr (> | z |)*
     <br/>*(Intercepter) - 2.509e-03 3.223e-02-0.078 0.93793*
     <br/>*passenger_count-5.753e-02 1.088e-02-5.289 1, 23E-07 \*\*\**
     <br/>*trip_distance-3.896e-02 1.466e-02-2.658 0.00786 \*\**
     <br/>*trip_time_in_secs 2.115e-04 4.336e-05 4.878 1.07e-06 \*\*\**
     <br/>*direct_distance 6.156e-02 2.076e-02 2.966 0.00302 \*\**
     <br/>*---*
     <br/>*Signif. codes:  0 ‘\*\*\*’ 0.001 ‘\*\*’ 0.01 ‘\*’ 0.05 ‘.’ 0.1 ‘ ’ 1*
     <br/>*Nombre de matrice de variance-covariance finale de condition : 48.3933*
     <br/>*Number of iterations: 4*

## <a name="use-the-logistic-regression-model-for-scoring"></a>Utiliser le modèle de régression logistique pour calculer les scores

Maintenant que le modèle est créé, vous pouvez l’utiliser pour évaluer la probabilité que le conducteur obtienne un pourboire lors d’un trajet donné.

1. Tout d’abord, utilisez le [RxSqlServerData](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxsqlserverdata) pour définir un objet de source de données pour stocker le calcul de score resul (fonction)

    ```R
    scoredOutput <- RxSqlServerData(
      connectionString = connStr,
      table = "taxiScoreOutput"  )
    ```

    + Pour simplifier cet exemple, l’entrée dans le modèle de régression logistique est la même source de données de fonctionnalité (`sql_feature_ds`) que vous avez utilisé pour l’apprentissage du modèle.  Plus généralement, vous aurez sans doute de nouvelles données avec lesquelles calculer les scores, ou vous aurez peut-être mis de côté certaines données de test et de formation.
  
    + Les résultats de prédiction seront enregistrés dans la table, _taxiscoreOutput_. Notez que le schéma de cette table n’est pas défini lorsque vous créez à l’aide de rxSqlServerData. Le schéma est obtenu à partir de la sortie de rxPredict.
  
    + Pour créer la table qui stocke les valeurs prédites, la connexion SQL exécute la fonction de données rxSqlServer doit avoir des privilèges DDL dans la base de données. Si la connexion ne peut pas créer des tables, l’instruction échoue.

2. Appelez la fonction [rxPredict](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxpredict) pour générer les résultats.

    ```R
    rxPredict(modelObject = logitObj,
        data = sql_feature_ds,
        outData = scoredOutput,
        predVarNames = "Score",
        type = "response",
        writeModelVars = TRUE, overwrite = TRUE)
    ```
    
    Si l’instruction réussit, il doit prendre un certain temps à s’exécuter. Lorsque vous avez terminé, vous pouvez ouvrir SQL Server Management Studio et vérifiez que la table a été créée et qu’elle contient la colonne de Score et l’autre sortie attendue.

## <a name="plot-model-accuracy"></a>Tracer la précision du modèle

Pour obtenir une idée de la précision du modèle, vous pouvez utiliser la [rxRoc](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxroc) fonction pour tracer la courbe d’exploitation du récepteur. Étant donné que rxRoc est une des nouvelles fonctions fournies par le package RevoScaleR qui prend en charge les contextes de calcul à distance, vous avez deux options :

+ Vous pouvez utiliser la fonction rxRoc pour exécuter le traçage dans le contexte de calcul à distance et de retourner le tracé à votre client local.

+ Vous pouvez également importer les données sur votre ordinateur client R et utiliser d’autres fonctions de traçage R pour créer le graphique de performances.

Dans cette section, vous allez faire des essais avec ces deux techniques.

### <a name="execute-a-plot-in-the-remote-sql-server-compute-context"></a>Exécuter un tracé dans le contexte de calcul distant (SQL Server)

1. Appelez la fonction rxRoc et fournir les données définies précédemment en tant qu’entrée.

    ```R
    scoredOutput = rxImport(scoredOutput);
    rxRoc(actualVarName= "tipped", predVarNames = "Score", scoredOutput);
    ```

    Cet appel renvoie les valeurs utilisées pour calculer le graphique ROC. La colonne d’étiquette est _incliné_, qui a les résultats réels que vous tentez de prédire, tandis que la _Score_ colonne possède la prédiction.

2. Pour réellement tracer le graphique, vous pouvez enregistrer l’objet ROC et puis dessinez avec le `plot` (fonction). Le graphique est créé dans le contexte de calcul à distance et renvoyé à votre environnement R.

    ```R
    scoredOutput = rxImport(scoredOutput);
    rocObjectOut <- rxRoc(actualVarName= "tipped", predVarNames = "Score", scoredOutput);
    plot(rocObjectOut);
    ```

    Afficher le graphique en ouvrant le périphérique d’affichage R, ou en cliquant sur le **tracer** fenêtre dans RStudio.

    ![Tracé ROC pour le modèle](media/rsql-e2e-rocplot.png "Tracé ROC pour le modèle")

### <a name="create-the-plots-in-the-local-compute-context-using-data-from-sql-server"></a>Créer les tracés dans le contexte de calcul local à l’aide de données de SQL Server

1. Dans le contexte de calcul local, le processus est similaire. Vous utilisez la [rxImport](https://docs.microsoft.com/r-server/r-reference/revoscaler/rximport) (fonction) pour afficher les données spécifiées dans votre environnement R local.

    ```R
    scoredOutput = rxImport(scoredOutput)
    ```

2. À l’aide des données dans la mémoire locale, vous chargez le **ROCR** du package et utiliser la fonction de prédiction à partir de ce package pour créer des prédictions de nouveau.

    ```R
    library('ROCR');
    pred <- prediction(scoredOutput$Score, scoredOutput$tipped);
    ```

3. Générer un diagramme en local, selon les valeurs stockées dans la variable de sortie `pred`.

    ```R
    acc.perf = performance(pred, measure = 'acc');
    plot(acc.perf);
    ind = which.max( slot(acc.perf, 'y.values')[[1]] );
    acc = slot(acc.perf, 'y.values')[[1]][ind];
    cutoff = slot(acc.perf, 'x.values')[[1]][ind];
    ```

    ![Traçage des performances d’un modèle à l’aide de R](media/rsql-e2e-performanceplot.png "Traçage des performances d’un modèle à l’aide de R")

> [!NOTE]
> Vos graphiques peuvent différer de ces options, selon le nombre de points de données utilisé.

## <a name="deploy-the-model"></a>Déployer le modèle

Après avoir créé un modèle et a établi qu’elle fonctionne correctement, vous souhaiterez probablement déployer sur un site où les utilisateurs ou les personnes de votre organisation faire en utilisation du modèle, ou peut-être recycler et étalonner le modèle sur une base régulière. Ce processus est parfois appelé *Opérationnalisation* un modèle.

Étant donné que [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] vous permet d’appeler un modèle R avec un [!INCLUDE[tsql](../../includes/tsql-md.md)] une procédure stockée, il est facile d’utiliser R dans une application cliente.

Toutefois, avant de pouvoir appeler le modèle à partir d’une application externe, vous devez l’enregistrer dans la base de données utilisée pour la production. Dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], les modèles formés sont stockés sous forme binaire, dans une seule colonne de type **varbinary(max)**.

Par conséquent, déplacer un modèle formé depuis R vers SQL Server comprend les étapes suivantes :

+ Sérialisation du modèle dans une chaîne hexadécimale

+ Transmission de l’objet sérialisé à la base de données

+ Enregistrement du modèle dans une colonne varbinary(max)

Dans cette section, vous découvrez comment conserver le modèle et comment l’appeler pour effectuer des prédictions.

1. Revenez à votre environnement R local si vous ne l’utilisez pas déjà, sérialisation du modèle et enregistrez-le dans une variable.

    ```R
    rxSetComputeContext("local");
    modelbin <- serialize(logitObj, NULL);
    modelbinstr=paste(modelbin, collapse="");
    ```

2. Ouvrez une connexion ODBC à l’aide **RODBC**.

    ```R
    library(RODBC);
    conn <- odbcDriverConnect(connStr);
    ```

    Vous pouvez omettre l’appel à RODBC si vous avez déjà le package chargé.

3. Appelez la procédure stockée créée par le script PowerShell, pour stocker la représentation binaire du modèle dans une colonne dans la base de données.

    ```R
    q <- paste("EXEC PersistModel @m='", modelbinstr,"'", sep="");
    sqlQuery (conn, q);
    ```

    L’enregistrement d’un modèle dans une table nécessite uniquement une instruction INSERT. Toutefois, il est plus facile lorsque encapsulée dans une procédure stockée, tel que _PersistModel_.

    > [!NOTE]
    > Si vous obtenez une erreur tels que « l’autorisation EXECUTE a été refusée sur l’objet PersistModel », assurez-vous que votre nom d’utilisateur a l’autorisation. Vous pouvez accorder des autorisations explicites sur simplement la procédure stockée en exécutant une instruction T-SQL comme suit : `GRANT EXECUTE ON [dbo].[PersistModel] TO <user_name>`

4. Une fois que vous avez créé un modèle et enregistré dans une base de données, vous pouvez l’appeler directement à partir de [!INCLUDE[tsql](../../includes/tsql-md.md)] de code, à l’aide de la procédure stockée système, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

    Toutefois, avec n’importe quel modèle que vous utilisez souvent, il est plus facile d’encapsuler la requête d’entrée et l’appel au modèle, ainsi que d’autres paramètres, dans une procédure stockée personnalisée.

    Voici le code complet d’une telle procédure stockée. Nous vous recommandons de créer une procédure stockée comme celle-ci pour le rendre plus facile à gérer et mettre à jour vos modèles R dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    ```tsql
    CREATE PROCEDURE [dbo].[PersistModel]  @m nvarchar(max)
    AS
    BEGIN
      SET NOCOUNT ON;
      INSERT INTO nyc_taxi_models (model) values (convert(varbinary(max),@m,2))
    END
    ```

  > [!NOTE]
  > Utilisez le **SET NOCOUNT ON** clause afin d’éviter des résultats supplémentaires définit d’interférer avec les instructions SELECT.


Dans la leçon suivante et finale, vous apprenez à effectuer sur le modèle enregistré à l’aide de calcul de score [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="next-lesson"></a>Leçon suivante

[Déployer le modèle R et à utiliser dans SQL](walkthrough-deploy-and-use-the-model.md)

## <a name="previous-lesson"></a>Leçon précédente

[Créer des fonctionnalités de données à l’aide de R et SQL](walkthrough-create-data-features.md)

