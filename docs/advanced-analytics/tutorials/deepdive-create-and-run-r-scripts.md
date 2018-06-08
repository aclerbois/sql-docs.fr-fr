---
title: Créer et exécuter des scripts R (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 1ff4b72b535f97ba0132dd5e2712b56f90effb10
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="create-and-run-r-scripts-sql-and-r-deep-dive"></a>Créer et exécuter des scripts R (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Maintenant que vous avez configuré vos sources de données et établi un ou plusieurs contextes de calcul, vous êtes prêt à exécuter des scripts R haute performance à l’aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Dans cette leçon, le contexte de calcul de serveur vous permet d’effectuer certaines tâches courantes d’apprentissage :

- Visualiser des données et générer des statistiques de synthèse
- Créer un modèle de régression linéaire
- Créer un modèle de régression logistique
- Affecter un score à de nouvelles données et créer un histogramme des scores

## <a name="change-compute-context-to-the-server"></a>Modification de contexte pour le serveur de calcul

Avant d’exécuter tout code R, vous devez spécifier le contexte de calcul *actuel* ou *actif* .

1. Pour activer un contexte de calcul que vous avez déjà défini à l’aide de R, utilisez la fonction **rxSetComputeContext** , comme indiqué ici :
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
    Dès que vous exécutez cette instruction, tous les calculs suivants ont lieu le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordinateur spécifié dans le *sqlCompute* paramètre.
  
2. Si vous préférez exécuter le code R sur votre station de travail, vous pouvez rebasculer le contexte de calcul vers l’ordinateur local à l’aide du mot clé  **local** .
  
    ```R
    rxSetComputeContext ("local")
    ```
  
    Pour obtenir la liste des autres mots clés pris en charge par cette fonction, tapez `help("rxSetComputeContext")` sur une ligne de commande R.
  
3. Une fois que vous avez spécifié un contexte de calcul, il reste actif jusqu’à ce que vous le changiez. Cependant, tout script R qui *ne peut pas* être exécuté dans un contexte de serveur distant sera exécuté localement.

## <a name="compute-some-summary-statistics"></a>Calcul des statistiques de résumé

Pour voir comment le contexte de calcul fonctionne, essayez de générer des statistiques de synthèse à l’aide du `sqlFraudDS` source de données.  N’oubliez pas, que l’objet de source de données définit uniquement les données que vous utilisez ; Il ne change pas le contexte de calcul.

+ Pour exécuter le résumé localement, utilisez **rxSetComputeContext** et spécifiez le _local_ (mot clé).
+ Pour créer les mêmes calculs sur l’ordinateur SQL Server, basculez vers le contexte de calcul SQL que vous avez défini précédemment.

1. Appelez le [rxSummary](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsummary) fonction et de passer des arguments requis, tels que la formule et de la source de données et affecter les résultats à la variable `sumOut`.
  
    ```R
    sumOut <- rxSummary(formula = ~gender + balance + numTrans + numIntlTrans + creditLine, data = sqlFraudDS)
    ```
  
    Le langage R fournit de nombreuses fonctions de synthèses, mais **rxSummary** prend en charge l’exécution sur les différents contextes de calcul à distance, y compris [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur les fonctions similaires, consultez [des résumés de données à l’aide de RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-data-summaries).
  
2. Lorsque le traitement est terminé, vous pouvez imprimer le contenu de la `sumOut` variable à la console.
  
    ```R
    sumOut
    ```
  
    > [!NOTE]
    > Si vous essayez d’imprimer les résultats avant qu’ils n’aient été retournés à partir de l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez obtenir une erreur.

**Résultats**

*Summary Statistics Results for: ~gender + balance + numTrans +*

 *numIntlTrans + creditLine*

 *Data: sqlFraudDS (RxSqlServerData Data Source)*

 *Number of valid observations: 10000*

 *Name  Mean    StdDev  Min Max ValidObs    MissingObs*

 *balance       4075.0318 3926.558714            0   25626 100000*

 *numTrans        29.1061   26.619923 0     100 10000    0           100000*

 *numIntlTrans     4.0868    8.726757 0      60 10000    0           100000*

 *creditLine 9.1856 9.870364 1 75 10000 0 100000*

 *Nombres de catégorie pour le sexe*

 *Number of categories: 2*

 *Number of valid observations: 10000*

 *Number of missing observations: 0*

 *gender Counts*

 *Male   6154*

  *Female 3846*

## <a name="add-maximum-and-minimum-values"></a>Ajoutez les valeurs maximales et minimales

D’après les statistiques de synthèse calculées, vous avez trouvé des informations utiles sur les données que vous souhaitez placer dans la source de données afin de les utiliser dans d’autres calculs. Par exemple, les valeurs minimales et maximale peuvent être utilisés pour calculer les histogrammes. Pour cette raison, nous allons ajouter les valeurs haute et bas à la **RxSqlServerData** source de données.

Heureusement [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] inclut des fonctions optimisées qui peuvent être efficacement converti données entières pour les données catégoriques facteur.

1. Commencez par configurer des variables temporaires.
  
    ```R
    sumDF <- sumOut$sDataFrame
    var <- sumDF$Name
    ```
  
2. Utilisez la variable `ccColInfo` créée précédemment pour définir les colonnes dans la source de données.
  
    En outre, ajoutez quelques nouvelles colonnes calculées (`numTrans`, `numIntlTrans`, et `creditLine`) à la collection de colonnes.
  
    ```R 
    ccColInfo <- list(
        gender = list(type = "factor",
          levels = c("1", "2"), 
          newLevels = c("Male", "Female")),
        cardholder = list(type = "factor",
          levels = c("1", "2"), 
          newLevels = c("Principal", "Secondary")), 
        state = list(type = "factor", 
          levels = as.character(1:51), 
          newLevels = stateAbb), 
        balance  = list(type = "numeric"),
        numTrans = list(type = "factor", 
          levels = as.character(sumDF[var == "numTrans", "Min"]:sumDF[var == "numTrans", "Max"])),
        numIntlTrans = list(type = "factor",  
            levels = as.character(sumDF[var == "numIntlTrans", "Min"]:sumDF[var =="numIntlTrans", "Max"])),
        creditLine = list(type = "numeric")
            )
    ```
  
3. Ayant de mises à jour la collection de colonnes, s’appliquent à l’instruction suivante pour créer une version mise à jour de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données que vous avez défini précédemment.
  
    ```R
    sqlFraudDS <- RxSqlServerData(
        connectionString = sqlConnString,
        table = sqlFraudTable,
        colInfo = ccColInfo,
        rowsPerRead = sqlRowsPerRead)
    ```
  
    Le `sqlFraudDS` source de données inclut désormais les nouvelles colonnes ajoutées à l’aide `ccColInfo`.
  

À ce stade, les modifications affectent uniquement l’objet de source de données dans R ; Aucune nouvelle donnée n’a encore été écrit à la table de base de données. Toutefois, vous pouvez utiliser les données capturées dans le `sumOut` variable à créer des visualisations et des résumés. Dans l’étape suivante vous découvrez comment effectuer cette opération lors du changement de contextes de calcul.

> [!TIP]
> Si vous oubliez le contexte de calcul que vous utilisez, exécutez `rxGetComputeContext()`.  Une valeur de retour de « Contexte de calcul RxLocalSeq » indique que vous exécutez dans le contexte de calcul local.

## <a name="next-step"></a>Étape suivante

[Visualiser des données SQL Server à l’aide de R](../../advanced-analytics/tutorials/deepdive-visualize-sql-server-data-using-r.md)

## <a name="previous-step"></a>Étape précédente

[Définir et utiliser des contextes de calcul](../../advanced-analytics/tutorials/deepdive-define-and-use-compute-contexts.md)
