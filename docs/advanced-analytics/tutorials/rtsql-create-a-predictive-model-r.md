---
title: Créer un modèle de prévision (R dans démarrage rapide de SQL) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 3a56ddd95f0282550662cc559ff5a393d0bd236b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-predictive-model-r-in-sql-quickstart"></a>Créer un modèle de prévision (R dans démarrage rapide de SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Au cours de cette étape, vous allez apprendre à former un modèle utilisant R et l’enregistrer dans une table dans SQL Server. Il s’agit d’un modèle de régression simple qui prédit la distance d’arrêt d’une voiture en fonction de sa vitesse. Vous allez utiliser le `cars` dataset inclus avec R, car il est facile à comprendre et de petite taille.

## <a name="create-the-source-data"></a>Créer la source de données

Dans un premier temps, créez une table pour enregistrer les données d’apprentissage.

```sql
CREATE TABLE CarSpeed ([speed] int not null, [distance] int not null)
INSERT INTO CarSpeed
EXEC sp_execute_external_script
        @language = N'R'
        , @script = N'car_speed <- cars;'
        , @input_data_1 = N''
        , @output_data_1_name = N'car_speed'
```

+ Certains préfèrent utiliser les tables temporaires, mais gardez à l’esprit que certains clients R déconnectent des sessions entre les traitements.

+ Le runtime R contient de nombreux datasets de tailles diverses. Pour obtenir la liste des datasets installées avec R, tapez `library(help="datasets")` à partir d’une invite de commandes R.

## <a name="create-a-regression-model"></a>Créer un modèle de régression

Les données de vitesse de voiture contiennent deux colonnes, toutes deux numériques, `dist` et `speed`. Certaines vitesses sont observées plusieurs fois. À partir de ces données, vous allez créer un modèle de régression linéaire qui décrit une relation entre la vitesse de la voiture et la distance nécessaire à son arrêt.

Les exigences d’un modèle linéaire sont simples :

+ Définir une formule qui décrive la relation entre la variable dépendante `speed` et la variable indépendante `distance`.

+ Fournir les données d’entrée à utiliser pour former le modèle.

> [!TIP]
> Si vous avez besoin d’un rappel sur les modèles linéaires, nous vous recommandons de ce didacticiel, qui décrit le processus d’ajustement d’un modèle à l’aide de rxLinMod : [qui respecte les modèles linéaires](https://docs.microsoft.com/r-server/r/how-to-revoscaler-linear-model)

Pour créer effectivement le modèle, vous devez définir la formule à l’intérieur de votre code R et passer les données en tant que paramètre d’entrée.

```sql
DROP PROCEDURE IF EXISTS generate_linear_model;
GO
CREATE PROCEDURE generate_linear_model
AS
BEGIN
    EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'lrmodel <- rxLinMod(formula = distance ~ speed, data = CarsData);
        trained_model <- data.frame(payload = as.raw(serialize(lrmodel, connection=NULL)));'
    , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
    , @input_data_1_name = N'CarsData'
    , @output_data_1_name = N'trained_model'
    WITH RESULT SETS ((model varbinary(max)));
END;
GO
```

+ Le premier argument de rxLinMod est le paramètre *formula*, qui définit la distance comme étant dépendante de la vitesse (speed).
+ Les données d’entrée sont stockées dans la variable `CarsData`, qui est remplie par la requête SQL. Si vous n’attribuez pas de nom spécifique à vos données d’entrée, le nom par défaut de la variable est _InputDataSet_.

## <a name="create-a-table-for-storing-the-model"></a>Créer une table pour stocker le modèle

Ensuite, stockez le modèle afin de former à nouveau ou utiliser pour la prédiction. La sortie d’un package R qui crée un modèle est généralement un **objet binaire**. Par conséquent, la table dans laquelle vous stockez le modèle doit contenir une colonne de type **varbinary**.

```sql
CREATE TABLE stopping_distance_models (
    model_name varchar(30) not null default('default model') primary key,
    model varbinary(max) not null);
```

## <a name="save-the-model"></a>Enregistrer le modèle

Pour enregistrer le modèle, exécutez l’instruction Transact-SQL suivante de façon à appeler la procédure stockée, à générer le modèle, puis à l’enregistrer dans une table.

```sql
INSERT INTO stopping_distance_models (model)
EXEC generate_linear_model;
```

Notez que si vous exécutez ce code une deuxième fois, vous obtenez cette erreur :

```
Violation of PRIMARY KEY constraint...Cannot insert duplicate key in object dbo.stopping_distance_models
```

Vous pouvez éviter cette erreur en mettant à jour le nom de chaque nouveau modèle. Par exemple, vous pouvez opter pour un nom plus descriptif en indiquant par exemple le type du modèle, le jour de sa création, etc.

```sql
UPDATE stopping_distance_models
SET model_name = 'rxLinMod ' + format(getdate(), 'yyyy.MM.HH.mm', 'en-gb')
WHERE model_name = 'default model'
```

## <a name="output-additional-variables"></a>Sortir des variables supplémentaires

En règle générale, la sortie de R résultant de la procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) se limite à une trame de données unique. (Cette limitation sera peut-être supprimée ultérieurement.)

Cependant, vous pouvez retourner des sorties d’autres types (p. ex., scalaires), en plus de la trame de données.

Par exemple, supposons que vous voulez former un modèle, mais afficher immédiatement une table de coefficients à partir du modèle. Vous pouvez créer la table de coefficients en tant que jeu de résultats principal et sortir le modèle formé dans une variable SQL. Vous pouvez utiliser immédiatement nouveau le modèle en appelant la variable, ou vous a pu enregistrer le modèle dans une table comme indiqué ici.

```sql
DECLARE @model varbinary(max), @modelname varchar(30)
EXEC sp_execute_external_script
    @language = N'R'
    , @script = N'
        speedmodel <- rxLinMod(distance ~ speed, CarsData)
        modelbin <- serialize(speedmodel, NULL)
        OutputDataSet <- data.frame(coefficients(speedmodel));'
    , @input_data_1 = N'SELECT [speed], [distance] FROM CarSpeed'
    , @input_data_1_name = N'CarsData'
    , @params = N'@modelbin varbinary(max) OUTPUT'
    , @modelbin = @model OUTPUT
    WITH RESULT SETS (([Coefficient] float not null));

-- Save the generated model
INSERT INTO [dbo].[stopping_distance_models] (model_name, model)
VALUES ('latest model', @model)
```

**Résultats**

![rslq_basictut_coefficients](media/rslq-basictut-coefficients.PNG)

### <a name="summary"></a>Résumé

N’oubliez pas ces règles pour l’utilisation des paramètres SQL et les variables de R dans `sp_execute_external_script`:

+ Tous les paramètres SQL sont mappés à un script R doivent être répertoriés par nom dans la _@params_ argument.
+ Pour sortir l’un de ces paramètres, ajoutez le mot clé OUTPUT dans la liste _@params_.
+ Après avoir répertorié les paramètres mappés, fournissez le mappage, ligne par ligne, des paramètres SQL aux variables R, de suite après la liste _@params_.

## <a name="next-lesson"></a>Leçon suivante

Maintenant que vous disposez d’un modèle, à la dernière étape, vous allez apprendre à générer des prédictions à partir de celui-ci et représenter graphiquement les résultats.

[Prédire et tracer à partir du modèle](../tutorials/rtsql-predict-and-plot-from-model.md)


