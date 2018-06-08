---
title: Visualiser les données de SQL Server à l’aide de R (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 923b8201b6948a93f0994306269c0d3338f54c2d
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
#  <a name="visualize-sql-server-data-using-r-sql-and-r-deep-dive"></a>Visualiser les données de SQL Server à l’aide de R (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Les packages améliorés dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] incluent une série de fonctions qui ont été optimisées pour la scalabilité et le traitement parallèle. En général, ces fonctions sont précédées de **rx** ou **Rx**.

Pour cette procédure pas à pas, vous utilisez la **rxHistogram** fonction pour afficher la distribution des valeurs dans le _creditLine_ colonne par sexe.

## <a name="visualize-data-using-rxhistogram"></a>Visualiser les données à l’aide de rxHistogram

1. Le code R suivant permet d’appeler la fonction [rxHistogram](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxhistogram) et de passer une formule et une source de données. Le code peut être exécuté localement dans un premier temps, afin de voir les résultats attendus et combien de temps peut durer son exécution.
  
    ```R
    rxHistogram(~creditLine|gender, data = sqlFraudDS,  histType = "Percent")
    ```
 
    En interne, **rxHistogram** appelle la fonction [rxCube](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxcube) , qui est incluse dans le package **RevoScaleR** . **rxCube** génère une seule liste (ou la trame de données) qui contient une colonne pour chaque variable spécifiée dans la formule, plus une colonne de nombres.
    
2. À présent, définissez le contexte de calcul à l’ordinateur SQL Server distant et exécuter **rxHistogram** à nouveau.
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
 
3. Les résultats sont exactement les mêmes, puisque vous utilisez la même source de données. Cependant, les calculs sont effectués sur l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  Les résultats sont ensuite renvoyés à votre station de travail locale à des fins de traçage.
   
![Résultats de l’histogramme](media/rsql-sue-histogramresults.jpg "Résultats de l’histogramme")

4. Vous pouvez également appeler le **rxCube** la fonction et passer les résultats vers une fonction de traçage de R.  L’exemple suivant utilise **rxCube** pour calculer la moyenne de *fraudRisk* pour chaque combinaison de *numTrans* et *numIntlTrans*:
  
    ```R
    cube1 <- rxCube(fraudRisk~F(numTrans):F(numIntlTrans),  data = sqlFraudDS)
    ```
  
    Pour spécifier les groupes utilisés pour calculer les moyennes de groupe, utilisez la notation `F()` . Dans cet exemple, `F(numTrans):F(numIntlTrans)` indique que les entiers dans les variables `_numTrans` et `numIntlTrans` doivent être traités comme des variables, avec un niveau pour chaque valeur d’entier.
  
    Étant donné que les niveaux faibles et élevées ont déjà été ajoutés à la source de données `sqlFraudDS` (à l’aide de le `colInfo` paramètre), les niveaux sont automatiquement utilisés dans l’histogramme.
  
5. La valeur par défaut retourne la valeur de **rxCube** est un *rxCube objet*, qui représente un tableau croisé. Toutefois, vous pouvez utiliser la fonction [rxResultsDF](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxresultsdf) pour convertir les résultats en une trame de données facilement exploitable dans l’une des fonctions de traçage standard de R.
  
    ```R
    cubePlot <- rxResultsDF(cube1)
    ```
  
    Le **rxCube** fonction inclut un argument facultatif, *returnDataFrame* = **TRUE**, que vous pouvez utiliser pour convertir les résultats pour une trame de données directement. Par exemple :
    
    `print(rxCube(fraudRisk~F(numTrans):F(numIntlTrans), data = sqlFraudDS, returnDataFrame = TRUE))`
       
    Toutefois, la sortie de **rxResultsDF** est beaucoup plus propre et préserve le nom des colonnes sources.
  
6. Enfin, exécutez le code suivant pour créer une carte de chaleur à l’aide de la `levelplot` fonction à partir de la **treillis** package, qui est inclus avec toutes les distributions de R.
  
    ```R
    levelplot(fraudRisk~numTrans*numIntlTrans, data = cubePlot)
    ```
  
    **Résultats**
  
    ![Résultats du nuage de points](media/rsql-sue-scatterplotresults.jpg "Résultats de nuage de points")
  
Même à partir de cette analyse rapide, vous pouvez voir que le risque de fraude augmente avec le nombre de transactions et le nombre de transactions internationales.

Pour plus d’informations sur la **rxCube** (fonction) et celles des analyses croisées en général, consultez [des résumés de données à l’aide de RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-data-summaries).

## <a name="next-step"></a>Étape suivante

[Créez des modèles R à l’aide de données SQL Server](../../advanced-analytics/tutorials/deepdive-create-models.md)

## <a name="previous-step"></a>Étape précédente

[Créer et exécuter des scripts R](../../advanced-analytics/tutorials/deepdive-create-and-run-r-scripts.md)
