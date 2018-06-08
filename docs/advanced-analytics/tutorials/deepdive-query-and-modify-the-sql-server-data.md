---
title: Interroger et modifier les données de SQL Server (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 90b836cd09fd0c6f130ff65c531f6077a28c2014
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="query-and-modify-the-sql-server-data-sql-and-r-deep-dive"></a>Interroger et modifier les données de SQL Server (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Maintenant que vous avez chargé les données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez utiliser les sources de données que vous avez créées comme arguments de fonctions R dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], pour obtenir des informations de base sur les variables, et générer des résumés et des histogrammes.

Dans cette étape, vous réutiliser les sources de données pour effectuer des analyses rapides et d’améliorer les données.

## <a name="query-the-data"></a>Interroger les données

Obtenez d’abord la liste des colonnes et leurs types de données.

1.  Utilisez la fonction [rxGetVarInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf) et spécifiez la source de données que vous souhaitez analyser.

    Selon votre version de RevoScaleR, vous pouvez également utiliser [rxGetVarNames](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxgetvarnames). 
  
    ```R
    rxGetVarInfo(data = sqlFraudDS)
    ```

    **Résultats**
    
    *Var 1 : custID, Type : entier*
    
    *Var 2 : gender, Type : entier*
    
    *Var 3 : state, Type : entier*
    
    *Var 4 : cardholder, Type : entier*
    
    *Var 5 : balance, Type : entier*
    
    *Var 6 : numTrans, Type : entier*
    
    *Var 7 : numIntlTrans, Type : entier*
    
    *Var 8 : creditLine, Type : entier*
    
    *Var 9 : fraudRisk, Type : entier*


## <a name="modify-metadata"></a>Modifier les métadonnées

Toutes les variables sont stockées sous forme d’entiers, mais certaines variables représentent des données de catégorie appelées *variables de facteur* dans R. Par exemple, la colonne *state* contient des nombres qui représentent des identificateurs pour les 50 états, plus le District de Columbia.  Pour mieux comprendre les données, vous pouvez remplacer les numéros par une liste d’abréviations des noms des états.

Dans cette étape, vous créez un vecteur de chaîne qui contient les abréviations et ensuite mappez ces valeurs catégoriques à un identificateur entier d’origine. Puis vous utilisez la nouvelle variable dans le *colInfo* argument, pour spécifier que cette colonne est gérée comme un facteur. Chaque fois que vous analysez les données ou le déplacez, les abréviations sont utilisées, et la colonne est traitée comme un facteur.

Le fait de mapper la colonne vers les abréviations avant de l’utiliser comme facteur améliore également les performances. Pour plus d’informations, consultez [R et les données de l’optimisation](..\r\r-and-data-optimization-r-services.md).

1. Commencez par créer la variable R *stateAbb*et par définir le vecteur des chaînes que vous lui ajoutez, comme suit :
  
    ```R
    stateAbb <- c("AK", "AL", "AR", "AZ", "CA", "CO", "CT", "DC",
        "DE", "FL", "GA", "HI","IA", "ID", "IL", "IN", "KS", "KY", "LA",
        "MA", "MD", "ME", "MI", "MN", "MO", "MS", "MT", "NB", "NC", "ND",
        "NH", "NJ", "NM", "NV", "NY", "OH", "OK", "OR", "PA", "RI","SC",
        "SD", "TN", "TX", "UT", "VA", "VT", "WA", "WI", "WV", "WY")
    ```

2. Ensuite, créez un objet d’informations de colonne, nommé *ccColInfo*, qui spécifie le mappage des valeurs entières existantes aux niveaux des catégories (les abréviations du nom des états).
  
    Cette instruction crée également des variables de facteur pour gender et cardholder.
  
    ```R
    ccColInfo <- list(
    gender = list(
              type = "factor",
              levels = c("1", "2"),
              newLevels = c("Male", "Female")
              ),
    cardholder = list(
                  type = "factor",
                  levels = c("1", "2"),
                  newLevels = c("Principal", "Secondary")
                   ),
    state = list(
             type = "factor",
             levels = as.character(1:51),
             newLevels = stateAbb
             ),
    balance = list(type = "numeric")
    )
    ```
  
3. Pour créer la source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui utilise les données mises à jour, appelez la fonction **RxSqlServerData** comme précédemment, mais ajoutez l’argument *colInfo* .
  
    ```R
    sqlFraudDS <- RxSqlServerData(connectionString = sqlConnString,
    table = sqlFraudTable, colInfo = ccColInfo,
    rowsPerRead = sqlRowsPerRead)
    ```
  
    - Pour le paramètre *table* , passez la variable *sqlFraudTable*qui contient la source de données que vous avez créée.
    - Pour le paramètre *colInfo* , passez la variable *ccColInfo* , qui contient les types de données de colonne et les niveaux de facteur.

4.  Vous pouvez maintenant utiliser la fonction **rxGetVarInfo** pour demander des informations sur les variables dans la nouvelle source de données.
  
    ```R
    rxGetVarInfo(data = sqlFraudDS)
    ```

    **Résultats**
    
    *Var 1 : custID, Type : entier*
    
    *Var 2 : sexe 2 niveaux facteur : Male féminin*
    
    *Var 3 : 51 niveaux des facteurs d’état : autorité de certification AZ AK AL AR... VT WA WI WV WY*
    
    *Var 4 : niveaux de facteur titulaires de cartes 2 : Principal de base de données secondaire*
    
    *Var 5 : balance, Type : entier*
    
    *Var 6 : numTrans, Type : entier*
    
    *Var 7 : numIntlTrans, Type : entier*
    
    *Var 8 : creditLine, Type : entier*
    
    *Var 9 : fraudRisk, Type : entier*

Les trois variables que vous avez spécifiées (_gender_, _state_et _cardholder_) sont maintenant traitées comme des facteurs.

## <a name="next-step"></a>Étape suivante

[Définir et utiliser des contextes de calcul](../../advanced-analytics/tutorials/deepdive-define-and-use-compute-contexts.md)

## <a name="previous-step"></a>Étape précédente

[Créer des objets de données SQL Server à l’aide de RxSqlServerData](../../advanced-analytics/tutorials/deepdive-create-sql-server-data-objects-using-rxsqlserverdata.md)
