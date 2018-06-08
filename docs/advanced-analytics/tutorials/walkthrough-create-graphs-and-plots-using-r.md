---
title: Créer des graphiques et des graphiques à l’aide de SQL et R (procédure pas à pas) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 4d4b573bc2aa0cc48e7c158edafa0dd604e6fc87
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585471"
---
# <a name="create-graphs-and-plots-using-sql-and-r-walkthrough"></a>Créer des graphiques et des graphiques à l’aide de SQL et R (procédure pas à pas)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans cette partie de la procédure pas à pas, vous découvrez les techniques permettant de générer des graphiques et des cartes à l’aide de R avec des données SQL Server. Vous créez un histogramme simple, pour faire la main et ensuite développez un tracé de mappage plus complexe.

### <a name="create-a-histogram"></a>Créer un histogramme

1. Générez le premier tracé à l’aide de la fonction [rxHistogram](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxdatasource) .  La fonction rxHistogram fournit des fonctionnalités similaires à celles dans les packages R open source, mais peut s’exécuter dans un contexte d’exécution à distance.

    ```R
    # Plot fare amount on SQL Server and return the plot
    start.time <- proc.time()
    rxHistogram(~fare_amount, data = inDataSource, title = "Fare Amount Histogram")
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2), " seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate plot.", sep=""))
    ```

2. L’image est retourné dans l’appareil graphique R pour votre environnement de développement.  Par exemple, dans RStudio, cliquez sur la fenêtre **Plot** (Tracer).  Dans [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)], une fenêtre graphique distincte est ouverte.

    ![Utilisation de rxHistogram pour tracer les prix](media/rsql-e2e-rxhistogramresult.png "Utilisation de rxHistogram pour tracer les prix")

    > [!NOTE]
    > Votre graphique recherche différents ?
    >  
    > C’est parce que _inDataSource_ utilise uniquement les lignes du haut 1000. L’ordre des lignes à l’aide de TOP étant non déterministe en l’absence d’une clause ORDER BY, il est probable que les données et le graphique résultant peuvent varier.
    > Cette image a été générée à l’aide de 10 000 lignes de données environ. Nous vous recommandons de faire des essais avec différents nombres de lignes pour obtenir différents graphiques, et de noter le temps nécessaire pour retourner les résultats dans votre environnement.

### <a name="create-a-map-plot"></a>Créer un tracé de carte

En règle générale, les serveurs de base de données bloquent l’accès à Internet. Cela peut être gênant lors de l’utilisation de packages R que vous doivent télécharger des mappages ou autres images pour générer des graphiques. Toutefois, il est une solution de contournement qui peuvent s’avérer utile lors du développement de vos propres applications. En fait, vous générez la représentation sous forme de carte sur le client et puis superposer les points qui sont stockés en tant qu’attributs dans la table SQL Server sur la carte.

1. Définition de la fonction qui crée l’objet de traçage. La fonction personnalisée *mapPlot* crée un nuage qui utilise des emplacements de la collecte taxi et trace le nombre de trajets démarré à partir de chaque emplacement. Elle utilise les packages **ggplot2** et  **ggmap** , qui doivent déjà être installés et chargés.

    ```R
    mapPlot <- function(inDataSource, googMap){
        library(ggmap)
        library(mapproj)
        ds <- rxImport(inDataSource)
        p <- ggmap(googMap)+
        geom_point(aes(x = pickup_longitude, y =pickup_latitude ), data=ds, alpha =.5,
    color="darkred", size = 1.5)
        return(list(myplot=p))
    }
    ```

    + Le *mapPlot* fonction accepte deux arguments : un objet de données, ce qui vous avez défini précédemment à l’aide de RxSqlServerData, et le client transmet la représentation sous forme de carte.
    + Dans la ligne commençant par le *ds* variable, utilisé pour charger des données de mémoire à partir de la source de données créée précédemment rxImport *inDataSource*. (Cette source de données contient uniquement les 1000 lignes ; si vous souhaitez créer une carte avec plus de points de données, vous pouvez remplacer une autre source de données).
    + Chaque fois que vous utilisez **open source** fonctions R, les données doivent être chargées dans des trames de données dans la mémoire locale. Toutefois, en appelant le [rxImport](https://docs.microsoft.com/r-server/r-reference/revoscaler/rximport) (fonction), vous pouvez exécuter dans la mémoire de contexte de calcul à distance.

2. Modifier le contexte de calcul en local et charger les bibliothèques requises pour créer les mappages.

    ```R
    rxSetComputeContext("local")
    library(ggmap)
    library(mapproj)
    gc <- geocode("Times Square", source = "google")
    googMap <- get_googlemap(center = as.numeric(gc), zoom = 12, maptype = 'roadmap', color = 'color');
    ```

    + La variable `gc` stocke un jeu de coordonnées pour Times Square, NY.

    + La ligne commençant par `googmap` génère une carte avec les coordonnées spécifiées au centre.

3. Basculez vers le contexte de calcul de SQL Server et afficher les résultats, en encapsulant la fonction de traçage dans [rxExec](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxexec) comme indiqué ici. La fonction rxExec fait partie de la **RevoScaleR** package et l’exécution de prend en charge les fonctions R arbitraire dans un contexte de calcul à distance.

    ```R
    rxSetComputeContext(sqlcc)
    myplots <- rxExec(mapPlot, inDataSource, googMap, timesToRun = 1)
    plot(myplots[[1]][["myplot"]]);
    ````

    + Les données cartographiques dans `googMap` est passé en tant qu’argument à la fonction exécutée à distance *mapPlot*. Les mappages ont été générées dans votre environnement local, elles doivent être passées à la fonction afin de créer le tracé dans le contexte de SQL Server.

    + Lorsque la ligne commençant par `plot` s’exécute, les données rendues est sérialisé à l’environnement R local afin que vous puissiez l’afficher dans votre client de R.

    > [!NOTE]
    > Si vous utilisez SQL Server dans une machine virtuelle Azure, vous pouvez obtenir une erreur à ce stade. C’est parce qu’une règle de pare-feu par défaut dans Azure bloque l’accès au réseau par le code R. Pour plus d’informations sur la façon de corriger cette erreur, consultez [l’installation de R Services dans une machine virtuelle Azure](../r/installing-sql-server-r-services-on-an-azure-virtual-machine.md).

4. L’image suivante montre le tracé de sortie. Les emplacements de montée en taxi sont ajoutés à la carte sous forme de points rouges. Votre image peut se présenter différente, selon le nombre d’emplacements se trouvent dans la source de données que vous avez utilisé.

    ![Traçage des courses en taxi à l’aide d’une fonction R personnalisée](media/rsql-e2e-mapplot.png "Traçage des courses en taxi à l’aide d’une fonction R personnalisée")

## <a name="next-lesson"></a>Leçon suivante

[Créer des fonctionnalités de données à l’aide de R et SQL](walkthrough-create-data-features.md)

## <a name="previous-lesson"></a>Leçon précédente

[Résumer les données à l’aide de R](walkthrough-view-and-summarize-data-using-r.md)
