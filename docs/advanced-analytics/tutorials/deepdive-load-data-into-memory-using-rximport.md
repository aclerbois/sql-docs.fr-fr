---
title: Charger des données en mémoire à l’aide de rxImport (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: ea5a977f1504a245e270a93a876ac617d8aa6852
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="load-data-into-memory-using-rximport-sql-and-r-deep-dive"></a>Charger des données dans la mémoire à l’aide de rxImport (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Le [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) fonction peut être utilisée pour déplacer des données à partir d’une source de données dans une trame de données dans la mémoire de session, ou dans un fichier XDF sur le disque. Si vous ne spécifiez pas de fichier de destination, les données sont placées en mémoire sous la forme d’une trame de données.

Dans cette étape, vous apprenez à obtenir des données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis utilisez le **rxImport** afin de placer les données d’intérêt dans un fichier local. De cette façon, vous pouvez les analyser dans le contexte de calcul local à plusieurs reprises, sans devoir réinterroger la base de données.

## <a name="extract-a-subset-of-data-from-sql-server-to-local-memory"></a>Extraire un sous-ensemble de données à partir de SQL Server vers la mémoire locale

Vous avez décidé que vous souhaitez examiner uniquement les personnes risque élevé plus en détail. La table source dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est grande, vous pouvez obtenir les informations concernant uniquement les clients à haut risque. Ensuite, vous chargez les données dans une trame de données dans la mémoire de la station de travail.

1. Réinitialisez le contexte de calcul sur votre station de travail locale.

    ```R
    rxSetComputeContext("local")
    ```

2. Créez un objet de source de données SQL Server en fournissant une instruction SQL valide dans le paramètre *sqlQuery* . Cet exemple récupère une partie des observations ayant les scores de risque les plus élevés. De cette façon, seules les données dont vous avez vraiment besoin sont placées dans la mémoire locale.

    ```R
    sqlServerProbDS \<- RxSqlServerData(
        sqlQuery = paste("SELECT * FROM ccScoreOutput2",
        "WHERE (ccFraudProb > .99)"),
        connectionString = sqlConnString)
    ```

3. Appelez la fonction [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) pour lire les données dans une trame de données dans la session locale de R.

    ```R
    highRisk <- rxImport(sqlServerProbDS)
    ```

    Si l’opération a réussi, vous devez voir un message d’état comme celle-ci : « lignes lues : 35, traités lignes Total : 35, temps Total de segment : 0.036 secondes »

4. Maintenant que les observations à haut risque sont d’une trame de données en mémoire, vous pouvez utiliser diverses fonctions R pour manipuler la trame de données. Par exemple, vous pouvez classer les clients par leur score de risque et imprimer la liste des clients qui présentent le risque le plus élevé.

    ```R
    orderedHighRisk <- highRisk[order(-highRisk$ccFraudProb),]
    row.names(orderedHighRisk) <- NULL
    head(orderedHighRisk)
    ```

**Résultats**

*ccFraudLogitScore   state gender cardholder balance numTrans numIntlTrans creditLine ccFraudProb1*

*9.786345    SD   Male  Principal   23456       25            5 75   0.99994382*

*9.433040    FL Female  Principal   20629       24           28 75   0.99992003*

*8.556785    NY Female  Principal   19064       82           53 43   0.99980784*

*8.188668    AZ Female  Principal   19948       29            0 75   0.99972235*

*7.551699    NY Female  Principal   11051       95            0 75   0.99947516*

*7.335080    NV   Male  Principal   21566        4            6  75   0.9993482*

## <a name="more-about-rximport"></a>En savoir plus sur rxImport

Vous pouvez utiliser **rxImport** non seulement pour déplacer des données, mais aussi pour transformer les données pendant leur lecture. Par exemple, vous pouvez spécifier le nombre de caractères pour les colonnes à largeur fixe, fournir une description des variables, définir des niveaux pour les colonnes de facteur et même créer des niveaux à utiliser après l’importation.

Le **rxImport** fonction assigne des noms de variables aux colonnes pendant le processus d’importation, mais vous pouvez indiquer des noms de variables à l’aide de la *colInfo* paramètre, ou modifier les types de données à l’aide de la *colClasses* paramètre.

En spécifiant des opérations supplémentaires dans le paramètre *transforms* , vous pouvez effectuer un traitement élémentaire sur chaque segment de données lu.

## <a name="next-step"></a>Étape suivante

[Créer une table SQL Server à l’aide de rxDataStep](../../advanced-analytics/tutorials/deepdive-create-new-sql-server-table-using-rxdatastep.md)

## <a name="previous-step"></a>Étape précédente

[Transformer des données à l’aide de R](../../advanced-analytics/tutorials/deepdive-transform-data-using-r.md)

