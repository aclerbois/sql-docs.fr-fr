---
title: Créer des objets de données SQL Server à l’aide de RxSqlServerData (SQL et R approfondie) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e15c3e7c13c1be4f524c25bb1cec6da3c8e20c7e
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="create-sql-server-data-objects-using-rxsqlserverdata-sql-and-r-deep-dive"></a>Créer des objets de données SQL Server à l’aide de RxSqlServerData (SQL et R approfondie)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie du didacticiel de présentation approfondie de science des données, sur l’utilisation de [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) avec SQL Server.

Maintenant que vous avez créé le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de données et disposer des autorisations nécessaires pour travailler avec les données. Dans cette étape, vous créez certains objets dans R qui vous permettent de travailler avec les données.

## <a name="create-the-sql-server-data-objects"></a>Créer les objets de données SQL Server

Dans cette étape, vous utilisez des fonctions à partir de la **RevoScaleR** package pour créer et remplir les deux tables. Une table est utilisée pour former les modèles et l’autre pour le calcul de score. Les deux tables contiennent des données simulées de fraude de carte de crédit.

Pour créer des tables sur l’instance distante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordinateur, appelez le **RxSqlServerData** (fonction).

> [!TIP]
> Si vous utilisez les outils R pour Visual Studio, sélectionnez **Outils R** dans la barre d’outils, puis cliquez sur **Windows** pour afficher les options pour le débogage et l’affichage des variables R.

### <a name="create-the-training-data-table"></a>Créer la table de données d’apprentissage

1. Enregistrer la chaîne de connexion de base de données dans une variable de R. Voici deux exemples de chaînes de connexion ODBC valides pour SQL Server : une à l’aide d’une connexion SQL et une pour l’authentification intégrée de Windows.

    **Connexion SQL**
    ```R
    sqlConnString <- "Driver=SQL Server;Server=instance_name; Database=DeepDive;Uid=user_name;Pwd=password"
    ```

    **Authentification Windows**
    ```R
    sqlConnString <- "Driver=SQL Server;Server=instance_name;Database=DeepDive;Trusted_Connection=True"
    ```

    Veillez à modifier le nom d’instance, le nom de base de données, le nom d’utilisateur et le mot de passe comme il convient.
  
2. Spécifiez le nom de la table que vous voulez créer et enregistrez-le dans une variable R.
  
    ```R
    sqlFraudTable <- "ccFraudSmall"
    ```
  
    Étant donné que les noms d’instances et de bases de données sont déjà spécifiés dans la chaîne de connexion, quand vous combinez les deux variables, le nom *complet* de la nouvelle table devient _instance.database.schema.ccFraudSmall_.
  
3.  Avant d’instancier l’objet de source de données, ajoutez une ligne spécifiant un paramètre supplémentaire, *rowsPerRead*.  Le paramètre *rowsPerRead* contrôle le nombre de lignes de données lues dans chaque lot.
  
    ```R
    sqlRowsPerRead = 5000
    ```
  
    Bien que ce paramètre soit facultatif, il est important pour gérer l’utilisation de la mémoire et pour l’efficacité des calculs.  La plupart des fonctions analytiques améliorées dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] traitent les données dans des segments et de stocker des résultats intermédiaires, en retournant les calculs finales une fois toutes les données a été lu.
  
    Si la valeur de ce paramètre est trop grande, l’accès aux données peut être lent, car vous n’avez pas suffisamment de mémoire pour traiter efficacement un si grand segment de données.  Sur certains systèmes, si la valeur de *rowsPerRead* est trop basse, les performances peuvent être ralenties. Par conséquent, nous vous recommandons d’expérimenter avec ce paramètre sur votre système lorsque vous travaillez avec un jeu de données volumineux.
  
    Pour cette procédure pas à pas, utilisez la taille de processus de traitement par défaut définie par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance pour contrôler le nombre de lignes dans chaque segment. Enregistrer cette valeur dans la variable `sqlRowsPerRead`.
  
4.  Enfin, définissez une variable pour le nouvel objet de source de données et passez les arguments définis précédemment au constructeur RxSqlServerData. Notez que cette opération ne fait que créer l’objet de source de données, elle ne le remplit pas.
  
    ```R
    sqlFraudDS <- RxSqlServerData(connectionString = sqlConnString,
       table = sqlFraudTable,
       rowsPerRead = sqlRowsPerRead)
    ```

#### <a name="to-create-the-scoring-data-table"></a>Pour créer la table de données de calcul de score

À l’aide de la même procédure, créez la table qui contient les données de calcul de score à l’aide du même processus.

1. Créez une variable R, *sqlScoreTable*, pour stocker le nom de la table utilisée pour le calcul de score.
  
    ```R
    sqlScoreTable <- "ccFraudScoreSmall"
    ```
  
2. Fournir cette variable en tant qu’argument à la fonction RxSqlServerData pour définir un second objet de source de données, *sqlScoreDS*.
  
    ```R
    sqlScoreDS \<- RxSqlServerData(connectionString = sqlConnString,
       table = sqlScoreTable, rowsPerRead = sqlRowsPerRead)
    ```

Étant donné que vous avez déjà défini la chaîne de connexion et d’autres paramètres en tant que variables dans l’espace de travail R, il est facile de créer des sources de données pour les différentes tables, des vues ou des requêtes.

> [!NOTE]
> La fonction utilise des arguments différents pour la définition d’une source de données basée sur une table entière que pour une source de données basée sur une requête. Il s’agit, car le moteur de base de données SQL Server doit préparer les requêtes différemment. Plus loin dans ce didacticiel, vous allez apprendre à créer un objet de source de données basé sur une requête SQL.

## <a name="load-data-into-sql-tables-using-r"></a>Charger des données dans des tables SQL à l’aide de R

Maintenant que vous avez créé les tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez y charger des données à l’aide de la fonction **Rx** appropriée.

Le **RevoScaleR** package contient des fonctions qui prennent en charge de différentes sources de données : pour les données de texte, utilisez [RxTextData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxtextdata) pour générer l’objet de source de données. Il existe des fonctions supplémentaires pour créer des objets de source de données à partir des données Hadoop, des données ODBC et ainsi de suite.

> [!NOTE]
> Dans cette section, vous devez avoir **DDL d’exécution** autorisations sur la base de données.

### <a name="load-data-into-the-training-table"></a>Charger des données dans la table de formation

1. Créez une variable R, *ccFraudCsv*et affectez à la variable, le chemin d’accès de fichier pour le fichier CSV contenant les exemples de données.
  
    ```R
    ccFraudCsv <- file.path(rxGetOption("sampleDataDir"), "ccFraudSmall.csv")
    ```
  
    Notez l’appel à **rxGetOption**, qui est la méthode GET associée [rxOptions](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxoptions) dans **RevoScaleR**. Cet utilitaire permet de définir et répertorient les options relatives aux contextes de calcul locaux et distants, tels que le répertoire partagé par défaut, ou le nombre de processeurs (cœurs) à utiliser dans les calculs.
    
    Cet appel particulier Obtient les exemples à partir de la bibliothèque appropriée, quelle que soit l’où vous exécutez votre code. Par exemple, essayez d’exécuter la fonction sur SQL Server et sur votre ordinateur de développement, et voyez comment les chemins diffèrent.
  
2. Définissez une variable pour stocker les nouvelles données et utilisez la fonction **RxTextData** pour spécifier la source de données texte.
  
    ```R
    inTextData <- RxTextData(file = ccFraudCsv,      colClasses = c(
        "custID" = "integer", "gender" = "integer", "state" = "integer",
        "cardholder" = "integer", "balance" = "integer",
        "numTrans" = "integer",
        "numIntlTrans" = "integer", "creditLine" = "integer",
        "fraudRisk" = "integer"))
    ```
  
    L’argument *colClasses* est important. Vous l’utilisez pour indiquer le type de données à affecter à chaque colonne de données chargées à partir du fichier texte. Dans cet exemple, toutes les colonnes sont gérées comme du texte, sauf les colonnes nommées qui sont gérées comme des entiers.
  
3. À ce stade, vous souhaiterez peut-être suspendre un moment et d’afficher votre base de données dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  Actualisez la liste de tables dans la base de données.
  
    Vous pouvez voir que, bien que les objets de données R ont été créés dans votre espace de travail local, les tables n'ont pas été créées dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données. En outre, aucune donnée n’a été chargée à partir du fichier texte dans la variable de R.
  
4. À présent, appelez la fonction [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) pour insérer les données dans la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
    ```R
    rxDataStep(inData = inTextData, outFile = sqlFraudDS, overwrite = TRUE)
    ```
  
    En supposant que votre chaîne de connexion n’ait rencontré aucun problème, après une courte pause, vous devez voir les résultats suivants :
  
      *Nombre total de lignes écrites : 10000, Durée totale : 0,466*

      *Lignes lues : 10000, Total des lignes traitées : 10000, Durée totale du segment : 0,577 seconde*
  
5. À l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], actualisez la liste de tables. Pour vérifier que chaque variable possède les types de données correct et qu’il a été importé avec succès, vous pouvez cliquer également sur la table dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et sélectionnez **sélectionnez 1000 lignes du haut**.

### <a name="load-data-into-the-scoring-table"></a>Charger des données dans la table calcul de score

1. Répétez les étapes pour charger le jeu de données utilisé pour calculer les scores dans la base de données.
  
    Commencez par fournir le chemin du fichier source.
  
    ```R
    ccScoreCsv <- file.path(rxGetOption("sampleDataDir"), "ccFraudScoreSmall.csv")
    ```
  
2. Utilisez la fonction **RxTextData** pour obtenir les données et les enregistrer dans la variable *inTextData*.
  
    ```R
    inTextData <- RxTextData(file = ccScoreCsv,      colClasses = c(
        "custID" = "integer", "gender" = "integer", "state" = "integer",
        "cardholder" = "integer", "balance" = "integer",
        "numTrans" = "integer",
        "numIntlTrans" = "integer", "creditLine" = "integer"))
    ```
  
3.  Appelez la fonction **rxDataStep** pour remplacer la table actuelle par le nouveau schéma et les nouvelles données.
  
    ```R
    rxDataStep(inData = inTextData, sqlScoreDS, overwrite = TRUE)
    ```
  
    - L’argument *inData* définit la source de données à utiliser.
  
    - L’argument *outFile* spécifie la table de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous voulez enregistrer les données.
  
    - Si la table existe déjà et que vous n’utilisez pas le *remplacer* option, résultats sont insérés sans les tronquer.
  
Là encore, si la connexion a réussi, vous devez voir un message indiquant l’achèvement et le temps qui a été nécessaire à l’écriture des données dans la table :

*Nombre total de lignes écrites : 10000, Durée totale : 0,384*

*Lignes lues : 10000, Total des lignes traitées : 10000, Durée totale du segment : 0,456 seconde*

## <a name="more-about-rxdatastep"></a>En savoir plus sur rxDataStep

[rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep) est une fonction puissante qui peut effectuer plusieurs transformations sur une trame de données R. Vous pouvez également utiliser rxDataStep pour convertir la représentation sous forme de requis par la destination des données : dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Si vous le souhaitez, vous pouvez spécifier des transformations sur les données, à l’aide des fonctions R dans les arguments de **rxDataStep**. Exemples de ces opérations sont fournis plus loin dans ce didacticiel.

## <a name="next-step"></a>Étape suivante

[Interroger et modifier les données SQL Server](../../advanced-analytics/tutorials/deepdive-query-and-modify-the-sql-server-data.md)

## <a name="previous-step"></a>Étape précédente

[Travailler avec des données de SQL Server à l’aide de R](../../advanced-analytics/tutorials/deepdive-work-with-sql-server-data-using-r.md)
