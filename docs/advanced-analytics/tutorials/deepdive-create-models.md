---
title: Créez des modèles R (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: a2f6e9b23e1592073c1b21bc41e4975ffaf17075
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-r-models-sql-and-r-deep-dive"></a>Créez des modèles R (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Maintenant que vous avez enrichi les données d’apprentissage, il est temps d’analyser les données en utilisant la régression linéaire. Les modèles linéaires sont un outil important dans le monde de la prédiction analytique et le package **RevoScaleR** de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] inclut un algorithme évolutif à hautes performances.

## <a name="create-a-linear-regression-model"></a>Créer un modèle de régression linéaire

Dans cette étape, vous créez un modèle linéaire simple qui évalue le solde de carte de crédit pour les clients, à l’aide, en tant que variables indépendantes, les valeurs dans le *sexe* et *creditLine* colonnes.
  
Pour ce faire, utilisez le [rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod) (fonction), qui prend en charge des contextes de calcul à distance.
  
1. Créez une variable R pour stocker le modèle, puis appelez **rxLinMod**, en passant une formule appropriée.
  
    ```R
    linModObj <- rxLinMod(balance ~ gender + creditLine,  data = sqlFraudDS)
    ```
  
2. Pour afficher un résumé des résultats, appelez R standard `summary` fonction sur l’objet de modèle.
  
     ```R
     summary(linModObj)
     ```

Vous pouvez penser particulières qu’une fonction R standard, tels que `summary` fonctionne ici, comme dans l’étape précédente, vous définissez le contexte de calcul sur le serveur. Toutefois, même lorsque la fonction **rxLinMod** utilise le contexte de calcul à distance pour créer le modèle, il retourne également un objet qui contient le modèle pour votre station de travail locale et le stocke dans le répertoire partagé.

Par conséquent, vous pouvez exécuter des commandes R standard sur le modèle comme s’il avait été créé en utilisant le contexte « local ».

**Résultats**

```
Linear Regression Results for: balance ~ gender + creditLineData: sqlFraudDS (RxSqlServerData Data Source)
Dependent variable(s): balance
Total independent variables: 4 (Including number dropped: 1)
Number of valid observations: 10000
Number of missing observations: 0
Coefficients: (1 not defined because of singularities)

Estimate Std. Error t value Pr(>|t|) (Intercept)
3253.575 71.194 45.700 2.22e-16
gender=Male -88.813 78.360 -1.133 0.257
gender=Female Dropped Dropped Dropped Dropped
creditLine 95.379 3.862 24.694 2.22e-16
Signif. codes: 0  0.001  0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3812 on 9997 degrees of freedom
Multiple R-squared: 0.05765
Adjusted R-squared: 0.05746
F-statistic: 305.8 on 2 and 9997 DF, p-value: < 2.2e-16
Condition number: 1.0184
```

## <a name="create-a-logistic-regression-model"></a>Créer un modèle de régression logistique

Ensuite, vous créez un modèle de régression logistique qui indique si un client particulier est un risque de fraude. Vous allez utiliser le **RevoScaleR** [rxLogit](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlogit) fonction, les contextes de calcul qui prend en charge l’ajustement des modèles de régression logistique dans à distance.

1.  Conservez le contexte de calcul tel quel. Vous continuez aussi à utiliser la même source de données.

2.  Appelez la fonction **rxLogit** , puis passez la formule nécessaire pour définir le modèle.

    ```R
    logitObj <- rxLogit(fraudRisk ~ state + gender + cardholder + balance + numTrans + numIntlTrans + creditLine, data = sqlFraudDS, dropFirst = TRUE)
    ```
  
    Comme il s’agit d’un modèle de grande taille, contenant 60 variables indépendantes, y compris les trois variables factices qui sont supprimées, vous devrez peut-être attendre que le contexte de calcul retourne l’objet.
    
    La raison pour laquelle la taille du modèle est si importante est que, dans R (et dans le package **RevoScaleR** ), chaque niveau d’une variable de facteur de catégorie est automatiquement traité comme une variable factice distincte.
  
3.  Pour afficher un résumé du modèle retourné, appelez R `summary` (fonction).
  
    ```R
    summary(logitObj)
    ```
  
**Résultats partiels**

```
Logistic Regression Results for: fraudRisk ~ state + gender + cardholder + balance + numTrans + numIntlTrans + creditLine
Data: sqlFraudDS (RxSqlServerData Data Source)
Dependent variable(s): fraudRisk
Total independent variables: 60 (Including number dropped: 3)
Number of valid observations: 10000 -2

LogLikelihood: 2032.8699 (Residual deviance on 9943 degrees of freedom)

Coefficients:
Estimate Std. Error z value Pr(>|z|)     (Intercept)
-8.627e+00  1.319e+00  -6.538 6.22e-11
state=AK                Dropped    Dropped Dropped  Dropped
state=AL             -1.043e+00  1.383e+00  -0.754   0.4511

(other states omitted)

gender=Male             Dropped    Dropped Dropped  Dropped
gender=Female         7.226e-01  1.217e-01   5.936 2.92e-09
cardholder=Principal    Dropped    Dropped Dropped  Dropped
cardholder=Secondary  5.635e-01  3.403e-01   1.656   0.0977
balance               3.962e-04  1.564e-05  25.335 2.22e-16
numTrans              4.950e-02  2.202e-03  22.477 2.22e-16
numIntlTrans          3.414e-02  5.318e-03   6.420 1.36e-10
creditLine            1.042e-01  4.705e-03  22.153 2.22e-16

Signif. codes:  0 ‘\*\*\*’ 0.001 ‘\*\*’ 0.01 ‘\*’ 0.05 ‘.’ 0.1 ‘ ’ 1
Condition number of final variance-covariance matrix: 3997.308
Number of iterations: 15
```

## <a name="next-step"></a>Étape suivante

[Score de nouvelles données](../../advanced-analytics/tutorials/deepdive-score-new-data.md)

## <a name="previous-step"></a>Étape précédente

[Visualiser des données SQL Server à l’aide de R](../../advanced-analytics/tutorials/deepdive-visualize-sql-server-data-using-r.md)
