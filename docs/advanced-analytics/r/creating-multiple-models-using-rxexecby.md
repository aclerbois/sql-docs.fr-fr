---
title: Création de plusieurs modèles à l’aide de rxExecBy (SQL Server Machine Learning) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 752ab5fc883f8fd99309496abb771931a05afc6a
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="creating-multiple-models-using-rxexecby"></a>Création de plusieurs modèles à l’aide de rxExecBy
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

SQL Server 2017 CTP 2.0 comprend une nouvelle fonction, **rxExecBy**, qui prend en charge le traitement parallèle de plusieurs modèles associés. Plutôt que d’effectuer l’apprentissage un très grand modèle en fonction des données provenant de plusieurs entités similaires, le chercheur de données peut créer très rapidement de nombreux modèles connexes, chacune utilisant des données spécifiques à une entité unique.

Par exemple, vous êtes surveillance des échecs de l’appareil et la capture des données pour différents types de matériel. À l’aide de rxExecBy, vous pouvez fournir un seul dataset volumineux comme entrée, spécifier une colonne sur laquelle effectuer la stratification sur le jeu de données, telles que le type de périphérique et puis créer plusieurs modèles de modèles pour des périphériques.

Ce processus a reçu le nom du traitement de « pleasingly parallel », car il accepte une tâche qui a été quelque peu onéreuse pour les spécialistes des données, ou au mieux fastidieux et il effectue une opération rapide et simple.

Les applications classiques de cette approche incluent les prévisions pour les compteurs de smart ménage individuels, la création des projections de chiffre d’affaires pour les lignes de produits distincts ou la création de modèles pour les approbations de prêt qui sont adaptées à des succursales.

## <a name="how-rxexec-works"></a>Fonctionne de rxExec

La fonction rxExecBy dans RevoScaleR est conçue pour traitement sur un grand nombre de petits jeux de données en parallèle élevées.

1. Vous appelez la fonction rxExecBy dans le cadre de votre code R et que vous passez un jeu de données de données non ordonnées.
2. Spécifiez la partition à laquelle les données doivent être groupées ou triées.
3. Définir une transformation ou une fonction qui doit être appliquée à chaque partition de données de modélisation
4. Lorsque la fonction s’exécute, les requêtes de données sont traitées en parallèle si votre environnement prend en charge. En outre, les tâches de modélisation ou de transformation sont réparties entre les cœurs individuels et exécutées en parallèle. Contexte de calcul pris en charge pour les trois opérations incluent RxSpark et RxInSQLServer.
5. Plusieurs résultats sont retournés.

## <a name="rxexecby-syntax-and-examples"></a>rxExecBy syntaxe et exemples

**rxExecBy** accepte quatre entrées, une des entrées en cours d’un objet de source de données ou du jeu de données qui peut être partitionné sur un **clé** colonne. La fonction retourne une sortie pour chaque partition. La forme de la sortie dépend de la fonction qui est passée en tant qu’argument, par exemple, si vous passez une fonction de modélisation tels que rxLinMod, vous pouvez retourner un modèle formé distinct pour chaque partition du jeu de données.

### <a name="supported-functions"></a>Fonctions prises en charge

Modélisation : `rxLinMod`, `rxLogit`, `rxGlm`, `rxDtree`

Calcul de score : `rxPredict`,

Transformation ou l’analyse : `rxCovCor`

## <a name="example"></a>Exemple

L’exemple suivant montre comment créer plusieurs modèles à l’aide de la compagnie aérienne jeu de données, qui est partitionnée sur la colonne [DayOfWeek]. La fonction définie par l’utilisateur, `delayFunc`, est appliqué à chacune des partitions en appelant rxExecBy. La fonction crée des modèles distincts pour lundi, mardi, et ainsi de suite.

```SQL
EXEC sp_execute_external_script
@language = N'R'
, @script = N'
delayFunc <- function(key, data, params) { 
    df <- rxImport(inData = airlineData) 
    rxLinMod(ArrDelay ~ CRSDepTime, data = df) 
} 
OutputDataSet <- rxExecBy(airlineData, c("DayOfWeek"), delayFunc)
'
, @input_data_1 = N'select ArrDelay, DayOfWeek, CRSDepTime from AirlineDemoSmall]'
, @input_data_1_name = N'airlineData'

```

Si vous obtenez l’erreur, `varsToPartition is invalid`, vérifiez si le nom de l’ou les colonnes clés est correct. Le langage R respecte la casse.

Notez que cet exemple n’est pas optimisé pour SQL Server, et vous pouvez souvent obtenir de meilleures performances à l’aide de SQL pour regrouper les données. Toutefois, à l’aide de rxExecBy, vous pouvez créer des tâches parallèles à partir de R.

L’exemple suivant illustre le processus dans R, à l’aide de SQL Server en tant que le contexte de calcul :

```R
sqlServerConnString <- "SERVER=hostname;DATABASE=TestDB;UID=DBUser;PWD=Password;"
inTable <- paste("airlinedemosmall")
sqlServerDataDS <- RxSqlServerData(table = inTable, connectionString = sqlServerConnString)

# user function
".Count" <- function(keys, data, params)
{
  myDF <- rxImport(inData = data)
  return (nrow(myDF))
}

# Set SQL Server compute context with level of parallelism = 2
sqlServerCC <- RxInSqlServer(connectionString = sqlServerConnString, numTasks = 4)
rxSetComputeContext(sqlServerCC)

# Execute rxExecBy in SQL Server compute context
sqlServerCCResults <- rxExecBy(inData = sqlServerDataDS, keys = c("DayOfWeek"), func = .Count)
```


