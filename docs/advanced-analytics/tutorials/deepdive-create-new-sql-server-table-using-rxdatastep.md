---
title: Créer la nouvelle table SQL Server à l’aide de rxDataStep (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 2fe521908e34afc1ae3ec23d56cb9d5cea801f1b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="create-new-sql-server-table-using-rxdatastep-sql-and-r-deep-dive"></a>Créer la nouvelle table SQL Server à l’aide de rxDataStep (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Dans cette leçon, vous découvrez comment déplacer des données entre les images de données en mémoire, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du contexte et des fichiers locaux.

> [!NOTE]
> Cette leçon est basée sur un autre jeu de données. Le dataset de retards de billet d’avion est un jeu de données publique est largement utilisé pour une expérience d’apprentissage. Les fichiers de données utilisés dans cet exemple sont disponibles dans le même répertoire que les autres exemples de produits.

## <a name="create-sql-server-table-from-local-data"></a>Créer la table SQL Server à partir de données locales

Dans la première moitié de ce didacticiel, vous avez utilisé le **RxTextData** de fonction pour importer des données dans le code R à partir d’un fichier texte, puis utilisé le **RxDataStep** pour déplacer les données en fonction [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Cette leçon adopte une approche différente, et utilise des données à partir d’un fichier enregistrement dans le [format XDF](https://en.wikipedia.org/wiki/Extensible_Data_Format). Lorsque vous effectuez certaines transformations légers sur les données à l’aide du fichier XDF, vous enregistrez les données transformées dans un nouveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table.

**Nouveautés XDF**

Le format XDF est une norme XML développée pour les données de grande dimension et est le format de fichier natif utilisé par [Machine Learning Server](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-xdf). C’est un format de fichier binaire avec une interface R qui optimise l’analyse et le traitement des colonnes et des lignes.  Vous pouvez également vous en servir pour déplacer des données et stocker des sous-ensembles de données qui sont utiles pour l’analyse.

1. Définissez le contexte de calcul sur votre station de travail locale. **Les autorisations DDL sont nécessaires pour cette étape.**

  
    ```R
    rxSetComputeContext("local")
    ```
  
2. Définissez un nouvel objet de source de données utilisant la fonction **RxXdfData** . Pour définir une source de données XDF, spécifiez le chemin d’accès au fichier de données.  

    Vous pouvez spécifier le chemin d’accès au fichier à l’aide d’une variable de texte. Toutefois, dans ce cas, il est un raccourci pratique qui consiste à utiliser le **rxGetOption** la fonction et obtenir le fichier (AirlineDemoSmall.xdf) à partir du répertoire de données d’exemple.
  
    ```R
    xdfAirDemo <- RxXdfData(file.path(rxGetOption("sampleDataDir"),  "AirlineDemoSmall.xdf"))
    ```

3. Appelez [rxGetVarInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf) dans les données en mémoire pour afficher un résumé du dataset.
  
    ```R
    rxGetVarInfo(xdfAirDemo)
    ```

**Résultats**

*Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)*

*Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0,0167, 23,9833)*

*Var 3: DayOfWeek 7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday*

> [!NOTE]
> 
> Avez-vous remarqué que vous n’avez pas eu besoin d’appeler d’autres fonctions pour charger les données dans le fichier XDF et que vous avez pu appeler **rxGetVarInfo** dans les données immédiatement ? C’est parce que l’usage du fichier XDF constitue la méthode de stockage temporaire par défaut de RevoScaleR. En plus des fichiers XDF, le **rxGetVarInfo** fonction prend désormais en charge plusieurs types de sources.
  
4. Placez ces données dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le stockage de la table _DayOfWeek_ en tant qu’entier avec des valeurs comprises entre 1 et 7.
  
    Pour cela, définissez d’abord une source de données SQL Server.
  
    ```R
    sqlServerAirDemo <- RxSqlServerData(table = "AirDemoSmallTest", connectionString = sqlConnString)
    ```
  
5. Vérifiez si une table du même nom existe déjà et, si tel est le cas, supprimez-la.
  
    ```R
    if (rxSqlServerTableExists("AirDemoSmallTest",  connectionString = sqlConnString))  rxSqlServerDropTable("AirDemoSmallTest",  connectionString = sqlConnString)
    ```
  
6. Créez la table et chargez les données à l’aide de **rxDataStep**. Cette fonction déplace les données entre deux déjà défini les sources de données et peuvent éventuellement transformer les données en cours de route.
  
    ```R
    rxDataStep(inData = xdfAirDemo, outFile = sqlServerAirDemo,
            transforms = list( DayOfWeek = as.integer(DayOfWeek),
            rowNum = .rxStartRow : (.rxStartRow + .rxNumRows - 1) ),
            overwrite = TRUE )
    ```
  
    Il s’agit d’une table relativement volumineux, par conséquent, patientez jusqu'à ce que vous voyez un message d’état final similaire à celle-ci : *lignes lues : 200000, Total lignes traitées : 600000*.
     
7. Définissez le contexte de calcul à nouveau sur l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
8. Créez une source de données SQL Server à l’aide d’une requête SQL simple sur la nouvelle table. Cette définition ajoute des niveaux de facteur pour le *DayOfWeek* colonne, à l’aide de la *colInfo* l’argument de **RxSqlServerData**.
  
    ```R
    SqlServerAirDemo <- RxSqlServerData(
        sqlQuery = "SELECT * FROM AirDemoSmallTest",
        connectionString = sqlConnString,
        rowsPerRead = 50000,
        colInfo = list(DayOfWeek = list(type = "factor",  levels = as.character(1:7))))
    ```
  
9. Appelez **rxSummary** une fois de plus pour obtenir une synthèse des données dans votre requête.
  
    ```R
    rxSummary(~., data = sqlServerAirDemo)
    ```

## <a name="next-step"></a>Étape suivante

[Effectuer une analyse de segmentation à l’aide de rxDataStep](../../advanced-analytics/tutorials/deepdive-perform-chunking-analysis-using-rxdatastep.md)

## <a name="previous-step"></a>Étape précédente

[Charger des données en mémoire à l’aide de rxImport](../../advanced-analytics/tutorials/deepdive-load-data-into-memory-using-rximport.md)
