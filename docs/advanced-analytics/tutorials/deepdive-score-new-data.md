---
title: Un score de nouvelles données (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 2de06b0159c432ac1d53d9e51bbdf0cd820efd7a
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="score-new-data-sql-and-r-deep-dive"></a>Un score de nouvelles données (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Dans cette étape, vous utilisez le modèle de régression logistique que vous avez créé précédemment, pour créer des scores pour un autre jeu de données qui utilise des variables indépendantes mêmes en tant qu’entrées.

> [!NOTE]
> Vous avez besoin des privilèges d’administration DDL pour certaines de ces étapes.

## <a name="generate-and-save-scores"></a>Générez et enregistrez des scores
  
1. Mettre à jour la source de données que vous avez configurée précédemment, `sqlScoreDS`, pour ajouter les informations de colonne requis.
  
    ```R
    sqlScoreDS <- RxSqlServerData(
        connectionString = sqlConnString,
        table = sqlScoreTable,
        colInfo = ccColInfo,
        rowsPerRead = sqlRowsPerRead)
    ```
  
2. Pour vous assurer de que ne pas perdre les résultats, créez un nouvel objet de source de données. Ensuite, utilisez le nouvel objet de source de données pour remplir une nouvelle table dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données.
  
    ```R
    sqlServerOutDS <- RxSqlServerData(table = "ccScoreOutput",
        connectionString = sqlConnString,
        rowsPerRead = sqlRowsPerRead )
    ```
    À ce stade, la table n’a pas encore été créée. Cette instruction définit simplement un conteneur pour les données.
     
3. Vérifiez le contexte de calcul actuel et définissez-le sur le serveur si nécessaire.
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
4. Avant d’exécuter la fonction de prédiction qui génère les résultats, vous devez vérifier l’existence d’une table de sortie. Dans le cas contraire, vous obtenez une erreur lorsque vous avez tenté d’écrire la nouvelle table.
  
    Pour ce faire, appelez les fonctions **rxSqlServerTableExists** et **rxSqlServerDropTable**, en passant le nom de la table en tant qu’entrée.
  
    ```R
    if (rxSqlServerTableExists("ccScoreOutput"))     rxSqlServerDropTable("ccScoreOutput")
    ```
  
    -  La fonction **rxSqlServerTableExists** interroge le pilote ODBC et retourne TRUE si la table existe, FALSE dans le cas contraire.
    -  La fonction **rxSqlServerDropTable** exécute l’instruction DDL et retourne la valeur TRUE si la table est correctement supprimée, FALSE dans le cas contraire.
    - Référencer des deux fonctions se trouvent ici : [rxSqlServerDropTable](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqlserverdroptable)
  
5. Vous êtes maintenant prêt à utiliser le [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) pour créer les scores de fonction et les enregistrer dans la nouvelle table définie dans la source de données `sqlScoreDS`.
  
    ```R
    rxPredict(modelObject = logitObj,
        data = sqlScoreDS,
        outData = sqlServerOutDS,
        predVarNames = "ccFraudLogitScore",
          type = "link",
        writeModelVars = TRUE,
        overwrite = TRUE)
    ```
  
    La fonction **rxPredict** est une autre fonction qui peut être exécutée dans les contextes de calcul distants. Vous pouvez utiliser la fonction **rxPredict** pour créer des scores à partir de modèles créés à l’aide de [rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod), [rxLogit](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlogit)ou [rxGlm](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxglm).
  
    - Le paramètre *writeModelVars* a ici la valeur **TRUE** . Cela signifie que les variables qui ont été utilisées pour l’estimation figureront dans la nouvelle table.
  
    - Le paramètre *predVarNames* spécifie la variable où les résultats seront stockés. Ici vous passez une variable, `ccFraudLogitScore`.
  
    - Le paramètre *type* de **rxPredict** définit la manière dont les prédictions sont calculées. Spécifiez le mot clé **réponse** pour générer des scores en fonction de l’échelle de la variable de réponse. Ou bien, utilisez le mot clé **lien** pour générer des scores en fonction de la fonction de liaison sous-jacente, auquel cas les prédictions sont créées à l’aide d’une échelle logistique.

6. Après un certain temps, vous pouvez actualiser la liste de tables dans Management Studio pour voir la nouvelle table et ses données.

7. Pour ajouter des variables supplémentaires aux prédictions de sortie, utilisez l’argument *extraVarsToWrite*.  Par exemple, dans le code suivant, la variable `custID` est ajouté à partir de la table de données de calcul de score dans la table de sortie de prédictions.
  
    ```R
    rxPredict(modelObject = logitObj,
            data = sqlScoreDS,
            outData = sqlServerOutDS,
            predVarNames = "ccFraudLogitScore",
              type = "link",
            writeModelVars = TRUE,
            extraVarsToWrite = "custID",
            overwrite = TRUE)
    ```

## <a name="display-scores-in-a-histogram"></a>Affichage des résultats dans un histogramme

Une fois la nouvelle table a été créée, vous pouvez calculer et afficher un histogramme des scores prédits 10 000. Le calcul est plus rapide si vous spécifiez les valeurs supérieure et inférieure, par conséquent, obtenez ceux de la base de données et les ajoutez à vos données de travail.

1. Créer une nouvelle source de données, `sqlMinMax`, qui interroge la base de données pour obtenir les valeurs supérieure et inférieure.
  
    ```R
    sqlMinMax <- RxSqlServerData(
        sqlQuery = paste("SELECT MIN(ccFraudLogitScore) AS minVal,",
        "MAX(ccFraudLogitScore) AS maxVal FROM ccScoreOutput"),
        connectionString = sqlConnString)
    ```

     Dans cet exemple, vous allez voir combien il est facile d’utiliser les objets de source de données **RxSqlServerData** pour définir des datasets arbitraires basés sur des fonctions ou des requêtes SQL, ou des procédures stockées, puis de les utiliser dans votre code R. La variable ne stocke pas les valeurs réelles, mais seulement la définition de la source de données. La requête est exécutée pour générer les valeurs uniquement lorsque vous l’utilisez dans une fonction comme **rxImport**.
      
2. Appelez le [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) afin de placer les valeurs dans une trame de données qui peut être partagée entre plusieurs contextes de calcul.
  
    ```R
    minMaxVals <- rxImport(sqlMinMax)
    minMaxVals \<- as.vector(unlist(minMaxVals))
  
    ```
     **Résultats**
     
     *> minMaxVals*
     
     *[1] -23.970256   9.786345*
  
3. Maintenant que les valeurs minimale et maximales sont disponibles, utilisez les valeurs pour créer une autre source de données pour les scores générés.
  
    ```R
    sqlOutScoreDS <- RxSqlServerData(sqlQuery = "SELECT ccFraudLogitScore FROM ccScoreOutput",
        connectionString = sqlConnString,
        rowsPerRead = sqlRowsPerRead,
            colInfo = list(ccFraudLogitScore = list(
                low = floor(minMaxVals[1]),
                        high = ceiling(minMaxVals[2]) ) ) )
    ```

4. Utilisez l’objet de source de données `sqlOutScoreDS` pour obtenir les scores et calculer et afficher un histogramme. Ajoutez le code pour définir le contexte de calcul, si nécessaire.
  
    ```R
    # rxSetComputeContext(sqlCompute)
    rxHistogram(~ccFraudLogitScore, data = sqlOutScoreDS)
    ```
  
    **Résultats**
  
    ![Histogramme complexe créé par R](media/rsql-sue-complex-histogram.png "Histogramme complexe créé par R")
  
## <a name="next-step"></a>Étape suivante

[Transformer des données à l’aide de R](../../advanced-analytics/tutorials/deepdive-transform-data-using-r.md)

## <a name="previous-step"></a>Étape précédente

[Créer des modèles](../../advanced-analytics/tutorials/deepdive-create-models.md)


