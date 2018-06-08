---
title: Comment créer une procédure stockée à l’aide de sqlrutils | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 82af827d95def976a04ac69073b58e1420cc9130
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-stored-pprocedure-using-sqlrutils"></a>Créer une procédure stockée à l’aide de sqlrutils
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les étapes pour convertir votre code R à exécuter en tant qu’une procédure stockée T-SQL. Pour de meilleurs résultats, vous devrez peut-être modifier un peu votre code pour être sûr que toutes les entrées peuvent être paramétrées.

## <a name="bkmk_rewrite"></a>Étape 1. Réécrivez le Script R

Pour de meilleurs résultats, vous devez réécrire votre code R pour encapsuler comme une fonction unique.

Toutes les variables utilisées par la fonction doivent être définies à l’intérieur de la fonction, ou doivent être définies en tant que paramètres d’entrée. Consultez le [exemple de code](#samples) dans cet article.

En outre, étant donné que les paramètres d’entrée pour la fonction R deviendront les paramètres d’entrée de l’instruction SQL de procédure stockée, vous devez vous assurer que vos entrées et les sorties sont conformes aux exigences de type suivant :

### <a name="inputs"></a>Entrées

Parmi les paramètres d’entrée, il peut y avoir au plus une trame de données.

Les objets à l’intérieur de la trame de données, ainsi que tous les autres paramètres d’entrée de la fonction, doivent être des types de données R suivants :
- POSIXct
- numérique
- caractère
- entier
- logique
- brut

Si un type d’entrée n’est pas l’un des types ci-dessus, il doit être sérialisé et transmis à la fonction en tant que type *brut*. Dans ce cas, la fonction doit également inclure du code pour désérialiser l’entrée.

### <a name="outputs"></a>Sorties

La fonction peut générer l’une des sorties suivantes :

- Une trame de données contenant les types de données pris en charge. Tous les objets dans la trame de données doivent utiliser l’un des types de données pris en charge.
- Une liste nommée contenant au plus une trame de données. Tous les membres de la liste doivent utiliser l’un des types de données pris en charge.
- Une valeur NULL, si la fonction ne retourne pas de résultat.

## <a name="step-2-generate-required-objects"></a>Étape 2. Générer des objets requis

Une fois que votre code R a été nettoyé et peut être appelé comme une fonction unique, vous allez utiliser les fonctions dans le **sqlrutils** package pour préparer les entrées et sorties dans un formulaire qui peut être passé au constructeur qui génère réellement la procédure stockée.

**sqlrutils** fournit des fonctions qui définissent le schéma de données d’entrée et le type et définissent le schéma de données de sortie et le type. Il comprend également des fonctions qui peuvent convertir des objets R pour le type de sortie requis. Vous pouvez effectuer plusieurs appels de fonction pour créer les objets requis, en fonction de votre code utilise les types de données.

### <a name="inputs"></a>Entrées

Si votre fonction accepte des entrées, pour chaque entrée, appelez les fonctions suivantes :

- `setInputData` Si l’entrée est une trame de données
- `setInputParameter` pour tous les autres types d’entrée

Lorsque vous apportez chaque fonction à appeler, R création d’un objet que vous allez passer ultérieurement en tant qu’argument à `StoredProcedure`, pour créer la procédure stockée terminée.

### <a name="outputs"></a>Sorties

**sqlrutils** fournit plusieurs fonctions pour convertir le R des objets tels que des listes pour les data.frame requis par SQL Server.
Si votre fonction génère une trame de données directement, sans l’encapsuler au préalable dans une liste, vous pouvez ignorer cette étape.
Vous pouvez également ignorer de la conversion de cette étape si votre fonction retourne NULL.

Lorsque la conversion d’une liste ou d’obtention d’un élément particulier dans une liste, choisissez à partir de ces fonctions :

- `setOutputData` Si la variable à obtenir à partir de la liste est une trame de données
- `setOutputParameter` pour tous les autres membres de la liste

Lorsque vous apportez chaque fonction à appeler, R création d’un objet que vous allez passer ultérieurement en tant qu’argument à `StoredProcedure`, pour créer la procédure stockée terminée.

## <a name="step-3-generate-the-stored-procedure"></a>Étape 3. Générer la procédure stockée

Lorsque tous les paramètres d’entrée et de sortie sont prêtes, effectuez un appel à la `StoredProcedure` constructeur.

**Utilisation**

`StoredProcedure (func, spName, ..., filePath = NULL ,dbName = NULL, connectionString = NULL, batchSeparator = "GO")`

Pour illustrer cela, supposons que vous souhaitez créer une procédure stockée nommée **sp_rsample** avec ces paramètres :

- Utilise une fonction existante **foosql**. La fonction était basée sur du code existant dans la fonction R **foo**, mais chargée de la fonction pour les rendre conformes aux spécifications comme décrit dans [cette section](#bkmk_rewrite)et la fonction de mise à jour en tant que **foosql**.
- Utilise la trame de données **queryinput** en tant qu’entrée
- Génère en sortie une trame de données avec le nom de variable R **sqloutput**
- Vous souhaitez créer le code T-SQL dans un fichier dans le `C:\Temp` dossier, afin que vous puissiez exécuter ultérieurement à l’aide de SQL Server Management Studio

```R
StoredProcedure (foosql, sp_rsample, queryinput, sqloutput, filePath = "C:\\Temp")
```

> [!NOTE]
> Étant donné que vous écrivez le fichier dans le système de fichiers, vous pouvez omettre les arguments qui définissent la connexion de base de données.

La sortie de la fonction est une procédure stockée T-SQL qui peut être exécutée sur une instance de SQL Server 2016 (nécessite les Services R) ou 2017 de serveur SQL (nécessite la Machine Learning Services avec R). 

Pour obtenir des exemples supplémentaires, consultez l’aide du package, en appelant `help(StoredProcedure)` à partir d’un environnement de R.

## <a name="step-4-register-and-run-the-stored-procedure"></a>Étape 4. Enregistrer et exécuter la procédure stockée

Il existe deux méthodes que vous pouvez exécuter la procédure stockée :

- À l’aide de T-SQL, à partir de n’importe quel client qui prend en charge les connexions à l’instance SQL Server 2016 ou SQL Server 2017
- À partir d’un environnement R

Les deux méthodes requièrent que la procédure stockée est enregistré dans la base de données où vous prévoyez d’utiliser la procédure stockée.

### <a name="register-the-stored-procedure"></a>Inscrire la procédure stockée

Vous pouvez enregistrer la procédure stockée à l’aide de R, ou vous pouvez exécuter l’instruction CREATE PROCEDURE dans T-SQL.

- À l’aide de T-SQL.  Si vous êtes plus à l’aise avec T-SQL, ouvrez SQl Server Management Studio (ou tout autre client qui peut exécuter des commandes SQL DDL) et exécutez l’instruction CREATE PROCEDURE en utilisant le code préparé par le `StoredProcedure` (fonction).
- À l’aide de R. Lorsque vous vous trouvez encore dans votre environnement R, vous pouvez utiliser la `registerStoredProcedure` fonctionner dans **sqlrutils** pour inscrire la procédure stockée avec la base de données.

  Par exemple, vous pourriez inscrire la procédure stockée **sp_rsample** dans l’instance et de la base de données définie dans *sqlConnStr*, en effectuant cet appel R :

  ```R
  registerStoredProcedure(sp_rsample, sqlConnStr)
  ```


> [!IMPORTANT]
> Que vous utilisiez R ou SQL, vous devez exécuter l’instruction à l’aide d’un compte qui dispose des autorisations de créer des objets de base de données.

### <a name="run-using-sql"></a>Exécuter à l’aide de SQL

Une fois la procédure stockée a été créée, ouvrir une connexion à la base de données SQL à l’aide de n’importe quel client qui prend en charge T-SQL et passer des valeurs pour tous les paramètres requis par la procédure stockée.

### <a name="run-using-r"></a>Exécuter à l’aide de R

Une préparation supplémentaire est nécessaire si vous souhaitez exécuter la procédure stockée à partir du code R, au lieu de cela à partir de SQL Server. Par exemple, si la procédure stockée requiert des valeurs d’entrée, vous devez définir ces paramètres d’entrée avant de la fonction peut être exécutée, puis passer ces objets à la procédure stockée dans votre code R.

Le processus global de l’appel de la procédure stockée SQL préparée est comme suit :

1. Appelez `getInputParameters` pour obtenir une liste d’objets de paramètres d’entrée.
2. Définissez un `$query` ou un `$value` pour chaque paramètre d’entrée.
3. Utilisez `executeStoredProcedure` pour exécuter la procédure stockée à partir de l’environnement de développement R, en passant la liste des objets de paramètres d’entrée que vous avez définis.

## <a name = "samples"></a>Exemple

Cet exemple montre l’avant et après les versions d’un script R qui obtient des données à partir d’une base de données SQL Server, effectue quelques transformations sur les données et l’enregistre dans une autre base de données.

Cet exemple simple est utilisé uniquement pour illustrer comment vous pouvez réorganiser votre code R pour faciliter la conversion d’une procédure stockée.

### <a name="before-code-preparation"></a>Avant la préparation du code


```R
sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineSrc;Trusted_Connection=Yes;"
  
sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineTest;Trusted_Connection=Yes;"
  
sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"
  
dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
xFunc <- function(data) {
    data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
    return(data)
    }
  
xVars <- c("CRSDepTime")
  
sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
rxOpen(dsSqlFrom)
rxOpen(dsSqlTo)
  
if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo))   {
    rxSqlServerDropTable("cleanData")}
  
rxDataStep(inData = dsSqlFrom, 
     outFile = dsSqlTo,
     transformFunc = xFunc,
     transformVars = xVars,
     overwrite = TRUE)
```

> [!NOTE]
> 
> Lorsque vous utilisez une connexion ODBC au lieu d’appeler le *RxSqlServerData* (fonction), vous devez ouvrir la connexion à l’aide de *rxOpen* avant de pouvoir effectuer des opérations sur la base de données.


### <a name="after-code-preparation"></a>Après la préparation du code

Dans la version mise à jour, la première ligne définit le nom de fonction. Le reste du code à partir de la solution R d’origine devient une partie de cette fonction.

```R
myetl1function <- function() { 
   sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=Airline01;Trusted_Connection=Yes;"
   sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer02;Database=Airline02;Trusted_Connection=Yes;"
    
   sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"

   dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
   dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
   xFunc <- function(data) {
     data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
     return(data)}
  
   xVars <- c("CRSDepTime")
  
   sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
   if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo)) {rxSqlServerDropTable("cleanData")}
  
   rxDataStep(inData = dsSqlFrom, 
        outFile = dsSqlTo,
        transformFunc = xFunc,
        transformVars = xVars,
        overwrite = TRUE)
   return(NULL)
}
```

> [!NOTE]
> 
> Bien que vous n’ayez pas besoin d’ouvrir la connexion ODBC explicitement dans le cadre de votre code, une connexion ODBC est quand même nécessaire pour utiliser **sqlrutils**.

## <a name="see-also"></a>Voir aussi

[Génération d’une procédure stockée à l’aide de sqlrutils](../../advanced-analytics/r-services/generating-an-r-stored-procedure-for-r-code-using-the-sqlrutils-package.md)


