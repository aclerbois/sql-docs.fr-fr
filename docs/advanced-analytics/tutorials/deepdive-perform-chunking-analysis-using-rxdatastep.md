---
title: Effectuer une analyse de segmentation à l’aide de rxDataStep (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 5db0acfb90c200442489cd7c3ec464223195eec6
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="perform-chunking-analysis-using-rxdatastep-sql-and-r-deep-dive"></a>Effectuer une analyse de segmentation à l’aide de rxDataStep (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Dans cette leçon, vous utilisez la **rxDataStep** de fonction pour traiter les données dans des segments, plutôt que d’exiger que l’ensemble du dataset être chargé en mémoire et traité à la fois, comme dans R. traditionnel Le **rxDataStep** fonctions lit les données dans le bloc, applique des fonctions R pour chaque segment de données à son tour et puis l’enregistre la synthèse des résultats pour chaque segment dans un commun [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données. Lorsque toutes les données ont été lues, les résultats sont combinés.

> [!TIP]
> Pour cette leçon, vous calculez une table d’urgence à l’aide du `table` fonction dans R. Cet exemple est destiné uniquement à des fins pédagogiques. 
> 
> Si vous avez besoin générer des jeux de données réelles, nous vous recommandons d’utiliser le **rxCrossTabs** ou **rxCube** dans les fonctions **RevoScaleR**, qui sont optimisés pour ce type d’opération.

## <a name="partition-data-by-values"></a>Partitionner les données par valeurs

1. Créez une fonction R personnalisée qui appelle la R `table` fonction sur chaque segment de données et le nom de la nouvelle fonction `ProcessChunk`.
  
    ```R
    ProcessChunk <- function( dataList) {
    # Convert the input list to a data frame and compute contingency table
    chunkTable <- table(as.data.frame(dataList))
  
    # Convert table output to a data frame with a single row
    varNames <- names(chunkTable)
    varValues <- as.vector(chunkTable)
    dim(varValues) <- c(1, length(varNames))
    chunkDF <- as.data.frame(varValues)
    names(chunkDF) <- varNames
  
    # Return the data frame, which has a single row
    return( chunkDF )
    }
    ```

2. Définissez le contexte de calcul sur le serveur.
  
    ```R
    rxSetComputeContext( sqlCompute )
    ```
  
3. Définir une source de données SQL Server pour stocker les données que vous traitez. Commencez par attribuer une requête SQL à une variable. Ensuite, utilisez cette variable dans le *sqlQuery* argument d’un nouveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données.
  
  
    ```R
    dayQuery <-  "SELECT DayOfWeek FROM AirDemoSmallTest"
    inDataSource <- RxSqlServerData(sqlQuery = dayQuery,
        connectionString = sqlConnString,
        rowsPerRead = 50000,
        colInfo = list(DayOfWeek = list(type = "factor",
            levels = as.character(1:7))))
    ```
4. Si vous le souhaitez, vous pouvez exécuter **rxGetVarInfo** sur cette source de données. À ce stade, il contient une seule colonne : *Var 1 : DayOfWeek, tapez : ne facteur, aucun niveau de facteur disponible*
     
5. Avant d’appliquer cette variable de facteur à la source de données, créez une autre table pour stocker les résultats intermédiaires. Là encore, vous utilisez simplement la fonction RxSqlServerData pour définir les données, makign, assurez-vous de supprimer des tables existantes du même nom.
  
    ```R
    iroDataSource = RxSqlServerData(table = "iroResults",   connectionString = sqlConnString)
    # Check whether the table already exists.
    if (rxSqlServerTableExists(table = "iroResults",  connectionString = sqlConnString))  { rxSqlServerDropTable( table = "iroResults", connectionString = sqlConnString) }
    ```
  
7.  Appeler la fonction personnalisée `ProcessChunk` pour transformer les données qu’il est lu à l’aide en tant que le *transformfunc est utilisé* l’argument de la **rxDataStep** (fonction).
  
    ```R
    rxDataStep( inData = inDataSource, outFile = iroDataSource, transformFunc = ProcessChunk, overwrite = TRUE)
    ```
  
8.  Pour afficher les résultats intermédiaires de `ProcessChunk`, affecter les résultats de **rxImport** à une variable et puis génère les résultats dans la console.
  
    ```R
    iroResults <- rxImport(iroDataSource)
    iroResults
    ```

**Résultats partiels**

|      |    1  |   2   |  3   |  4   |  5  |   6   |  7 |
| --- | ---  | --- | ---  |  ---  | ---  | ---  | --- |
| 1 | 8228 | 8924 | 6916 | 6932 | 6944 | 5602 | 6454 |
| 2  | 8321  | 5351 | 7329 | 7411 | 7409 | 6487 | 7692 |

9. Pour calculer les résultats finaux de l’ensemble des segments, additionnez les colonnes et affichez les résultats dans la console.

    ```R
    finalResults <- colSums(iroResults)
    finalResults
    ```

 **Résultats**
  1  |   2  |   3  |   4  |   5  |   6  |   7
---  |   ---  |   ---  |   ---  |   ---  |   ---  |   ---
97975 | 77725 | 78875 | 81304 | 82987 | 86159 | 94975 

10. Pour supprimer la table de résultats intermédiaires, effectuez un appel à **rxSqlServerDropTable**.
  
    ```R
    rxSqlServerDropTable( table = "iroResults", connectionString = sqlConnString)
    ```

## <a name="next-step"></a>Étape suivante

[Analyser des données dans un contexte de calcul local](../../advanced-analytics/tutorials/deepdive-analyze-data-in-local-compute-context.md)

## <a name="previous-step"></a>Étape précédente

[Créer une table SQL Server à l’aide de rxDataStep](../../advanced-analytics/tutorials/deepdive-create-new-sql-server-table-using-rxdatastep.md)
