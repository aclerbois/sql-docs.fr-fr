---
title: Transformer des données à l’aide de R (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 80472b3328c392d886733aad97adf1aa6eeae4ba
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="transform-data-using-r-sql-and-r-deep-dive"></a>Transformer des données à l’aide de R (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Le package **RevoScaleR** fournit plusieurs fonctions pour transformer des données à différents stades de votre analyse :

- **rxDataStep** peut être utilisé pour créer et transformer des sous-ensembles de données.

- **rxImport** prend en charge la transformation des données à mesure qu’elles sont importées vers ou depuis un fichier XDF ou une trame de données en mémoire.

- Bien qu’elles ne soient pas spécifiquement conçues pour le déplacement des données, les fonctions **rxSummary**, **rxCube**, **rxLinMod**et **rxLogit** prennent toutes en charge les transformations de données à la volée.

Dans cette section, vous allez apprendre à utiliser ces fonctions. Commençons par [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep).

## <a name="use-rxdatastep-to-transform-variables"></a>Utiliser rxDataStep pour transformer des variables

La fonction **rxDataStep** traite les données segment par segment, en lisant à partir d’une source de données et en écrivant dans une autre. Vous pouvez spécifier les colonnes à transformer, les transformations à charger, etc.

Pour que cet exemple intéressant, nous allons utiliser une fonction à partir d’un autre package R pour transformer les données.  Le package **boot** est un des packages « recommandés » ; en d’autres termes, **boot** est inclus avec chaque distribution de R, mais n’est pas chargé automatiquement au démarrage. Ainsi, le package doit être déjà disponible sur l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous utilisez avec [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)].

À partir de la **démarrage** du package, utilisez la fonction `inv.logit`, qui calcule l’inverse d’un logit. Autrement dit, la fonction `inv.logit` convertit un logit en une probabilité sur l’échelle [0,1].

> [!TIP] 
> Une autre méthode pour obtenir des prédictions dans cette échelle consisterait à définir le *type* paramètre **réponse** dans l’appel d’origine à rxPredict.

1. Commencez par créer une source de données pour contenir les données destinées à la table, `ccScoreOutput`.
  
    ```R
    sqlOutScoreDS <- RxSqlServerData( table =  "ccScoreOutput",  connectionString = sqlConnString, rowsPerRead = sqlRowsPerRead )
    ```
  
2. Ajouter une autre source de données pour contenir les données de la table `ccScoreOutput2`.
  
    ```R
    sqlOutScoreDS2 <- RxSqlServerData( table =  "ccScoreOutput2",  connectionString = sqlConnString, rowsPerRead = sqlRowsPerRead )
    ```
  
    Dans la nouvelle table, stockez toutes les variables du précédent `ccScoreOutput` table, ainsi que la variable nouvellement créée.
  
3. Définissez le contexte de calcul sur l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
4. Utilisez la fonction **rxSqlServerTableExists** pour vérifier si la table de sortie `ccScoreOutput2` déjà existe ; et si tel est le cas, utilisez la fonction **rxSqlServerDropTable** pour supprimer la table.
  
    ```R
    if (rxSqlServerTableExists("ccScoreOutput2"))     rxSqlServerDropTable("ccScoreOutput2")
    ```
  
5. Appelez la fonction **rxDataStep** et spécifiez les transformations de votre choix dans une liste.
  
    ```R
    rxDataStep(inData = sqlOutScoreDS,
        outFile = sqlOutScoreDS2,
        transforms = list(ccFraudProb = inv.logit(ccFraudLogitScore)),
        transformPackages = "boot",
        overwrite = TRUE)
    ```

    Quand vous définissez les transformations qui sont appliquées à chaque colonne, vous pouvez également spécifier les packages R supplémentaires qui sont nécessaires pour effectuer les transformations.  Pour plus d’informations sur les types de transformations que vous pouvez effectuer, consultez [transformation et le sous-ensemble des données à l’aide de RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-data-transform).
  
6. Appelez **rxGetVarInfo** pour afficher un récapitulatif des variables dans le nouveau dataset.
  
    ```R
    rxGetVarInfo(sqlOutScoreDS2)
    ```

    **Résultats**
    
    *Var 1 : ccFraudLogitScore, Type : numérique*
    
    *Var 2 : state, Type : caractère*
    
    *Var 3 : gender, Type : caractère*
    
    *Var 4 : cardholder, Type : caractère*
    
    *Var 5 : balance, Type : entier*
    
    *Var 6 : numTrans, Type : entier*
    
    *Var 7 : numIntlTrans, Type : entier*
    
    *Var 8 : creditLine, Type : entier*
    
    *Var 9 : ccFraudProb, Type : numérique*

Les scores de logit d’origine sont conservés, mais une nouvelle colonne, *ccFraudProb*, a été ajoutée. Les scores logit y sont représentés sous forme de valeurs comprises entre 0 et 1.

Notez que les variables de facteur ont été écrits dans la table `ccScoreOutput2` en tant que données de type caractère. Pour les utiliser comme facteurs dans les analyses ultérieures, utilisez le paramètre *colInfo* pour spécifier les niveaux.

## <a name="next-step"></a>Étape suivante

[Charger des données en mémoire à l’aide de rxImport](../../advanced-analytics/tutorials/deepdive-load-data-into-memory-using-rximport.md)

## <a name="previous-step"></a>Étape précédente

[Créer et exécuter des scripts R](../../advanced-analytics/tutorials/deepdive-create-and-run-r-scripts.md)
