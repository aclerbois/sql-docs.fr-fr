---
title: Déplacement des données entre SQL Server et le fichier XDF (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 6eb2ed7bdda7fab662048d7e8da692253cf9c164
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="move-data-between-sql-server-and-xdf-file-sql-and-r-deep-dive"></a>Déplacement des données entre SQL Server et le fichier XDF (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Dans cette étape, vous apprenez à utiliser un fichier XDF pour transférer des données entre les contextes de calcul locaux et distants. Le stockage des données dans un fichier XDF permet de vous permet d’effectuer des transformations sur les données.

Lorsque vous avez terminé, utilisez les données dans le fichier pour créer un nouveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table. La fonction [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) peuvent appliquer des transformations aux données et effectue la conversion entre des trames de données et de fichiers .xdf.
  
## <a name="create-a-sql-server-table-from-an-xdf-file"></a>Créer une table SQL Server à partir d’un fichier XDF

Dans cet exercice, vous utilisez à nouveau les données d’une fraude de carte de crédit. Dans ce scénario, vous avez été invité à effectuer des analyses supplémentaires sur les utilisateurs dans les États de la Californie, de l’Oregon et de Washington. Pour être plus efficace, que vous avez décidé de stocker les données pour seulement ces États sur votre ordinateur local et utiliser uniquement le sexe de variables, détenteur de la carte, l’état et équilibre.

1. Réutiliser la `stateAbb` variable que vous avez créée précédemment pour identifier les niveaux à inclure et les écrire dans une nouvelle variable, `statesToKeep`.
  
    ```R
    statesToKeep <- sapply(c("CA", "OR", "WA"), grep, stateAbb)
    statesToKeep
    ```
    **Résultats**
    
    CA|- ou -|WA
    ----|----|----
    5|38|48
    
2. Définir les données que vous souhaitez obtenir de SQL Server, à l’aide un [!INCLUDE[tsql](../../includes/tsql-md.md)] requête.  Vous utiliserez cette variable en tant que le *inData* argument pour **rxImport**.
  
    ```R
    importQuery <- paste("SELECT gender,cardholder,balance,state FROM",  sqlFraudTable,  "WHERE (state = 5 OR state = 38 OR state = 48)")
    ```
  
    Vérifiez que la requête ne contient aucun caractère masqué, tel qu’un saut de ligne ou une tabulation.
  
3. Ensuite, définissez les colonnes à utiliser lorsque vous travaillez avec les données dans R. Par exemple, dans le plus petit jeu de données, vous devez uniquement trois niveaux de facteur, car la requête retourne les données de seulement trois états.  Appliquer le `statesToKeep` variable pour identifier les niveaux corrects à inclure.
  
    ```R
    importColInfo <- list(
        gender = list( type = "factor",  levels = c("1", "2"), newLevels = c("Male", "Female")),
        cardholder = list(  type = "factor",  levels = c("1", "2"), newLevels = c("Principal", "Secondary")),
        state = list(   type = "factor",  levels = as.character(statesToKeep), newLevels = names(statesToKeep))
            )
    ```
  
4. Définissez le contexte de calcul **local**, car vous voulez que toutes les données disponibles sur votre ordinateur local.
  
    ```R
    rxSetComputeContext("local")
    ```
    
    Le [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqlserverdata) fonction peut importer des données à partir de n’importe quelle source de données pris en charge dans un fichier XDF local. À l’aide d’une copie locale des données est pratique lorsque vous souhaitez effectuer de nombreuses autres analyses sur les données, mais que vous souhaitez éviter d’exécuter la même requête plusieurs fois.

5. Créer l’objet de source de données en passant les variables définies précédemment en tant qu’arguments à **RxSqlServerData**.
  
    ```R
    sqlServerImportDS <- RxSqlServerData(
        connectionString = sqlConnString,
        sqlQuery = importQuery,
        colInfo = importColInfo)
    ```
  
6. Appelez **rxImport** pour écrire les données dans un fichier nommé `ccFraudSub.xdf`, dans le répertoire de travail actuel.
  
    ```R
    localDS <- rxImport(inData = sqlServerImportDS,
        outFile = "ccFraudSub.xdf",
        overwrite = TRUE)
    ```
  
    Le `localDs` objet retourné par la **rxImport** fonction est un léger **RxXdfData** objet de source de données qui représente le `ccFraud.xdf` le fichier de données stocké localement sur le disque.
  
7. Appelez [rxGetVarInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf) dans le fichier XDF pour vérifier que le schéma de données est le même.
  
    ```R
    rxGetVarInfo(data = localDS)
    ```

    **Résultats**
    
    *rxGetVarInfo(data = localDS)*

    *Var 1 : sexe, Type : facteur, aucun niveau de facteur disponible*

    *Var 2 : titulaire de la carte, Type : facteur, aucun niveau de facteur disponible*

    *Var 3 : solde, Type : entier Min./Max. : (0, 22463)*

    *Var 4 : état, Type : facteur, aucun niveau de facteur disponible*
  
8. Vous pouvez maintenant appeler différentes fonctions R pour analyser l’objet `localDs`, comme vous le feriez avec les données sources sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par exemple, vous pouvez résumer par sexe :
  
    ```R
    rxSummary(~gender + cardholder + balance + state, data = localDS)
    ```

L’utilisation des contextes de calcul et de différentes sources de données n’ayant plus de secret pour vous, il est temps d’essayer quelque chose d’amusant. Dans la leçon suivante et finale, vous créez une simulation simple qui exécute une fonction R personnalisée sur le serveur distant.

## <a name="next-step"></a>Étape suivante

[Créer une simulation simple](../../advanced-analytics/tutorials/deepdive-create-a-simple-simulation.md)

## <a name="previous-step"></a>Étape précédente

[Analyser des données dans un contexte de calcul local](../../advanced-analytics/tutorials/deepdive-analyze-data-in-local-compute-context.md)



