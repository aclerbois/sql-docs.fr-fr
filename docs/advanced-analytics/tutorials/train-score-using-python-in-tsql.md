---
title: Utiliser le modèle dans SQL pour l’apprentissage et de calcul de score Python | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: b7f5883356ff6878f869ee10f915bcb93a2dba17
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="use-python-model-in-sql-for-training-and-scoring"></a>Utiliser le modèle de Python dans SQL pour l’apprentissage et de calcul de score
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans le [leçon précédente](wrap-python-in-tsql-stored-procedure.md), vous avez appris le modèle commun pour l’utilisation de Python avec SQL. Vous avez appris que votre Python code doit sortie data.frame clairement un et peut éventuellement générer plusieurs variables scalaires ou binaire. Vous avez appris que la procédure stockée SQL doit être conçue pour passer du type de données dans les Python et traiter les résultats.

Dans cette section, vous utilisez ce même modèle pour former un modèle sur les données que vous avez ajouté à SQL Server, enregistrez le modèle dans une table SQL Server :

+ Concevoir une procédure stockée qui appelle une fonction d’apprentissage Python.
+ La procédure stockée nécessite des données à partir de SQL Server à utiliser dans l’apprentissage du modèle.
+ La procédure stockée génère un modèle formé en tant que binaire variable. 
+ Vous enregistrez le modèle formé en insérant le modèle de variable dans une table. 

## <a name="create-the-stored-procedure-and-train-a-python-model"></a>Créez la procédure stockée et l’apprentissage d’un modèle de Python

1. Exécutez le code suivant dans SQL Server Management Studio pour créer la procédure stockée qui crée un modèle.

    ```sql
    CREATE PROCEDURE generate_iris_model (@trained_model varbinary(max) OUTPUT)
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python',
    @script = N'
    import pickle
    from sklearn.naive_bayes import GaussianNB
    GNB = GaussianNB()
    trained_model = pickle.dumps(GNB.fit(iris_data[[0,1,2,3]], iris_data[[4]]))
    '
    , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
    , @input_data_1_name = N'iris_data'
    , @params = N'@trained_model varbinary(max) OUTPUT'
    , @trained_model = @trained_model OUTPUT;
    END;
    GO
    ```

2. Si cette commande s’exécute sans erreur, une procédure stockée est créée et ajoutée à la base de données. Vous trouverez des procédures stockées dans Management Studio **l’Explorateur d’objets**, sous **programmabilité**.

3. Maintenant, exécutez la procédure stockée.

    ```sql
    EXEC generate_iris_model
    ```

    Vous devez obtenir une erreur, car vous n’avez pas fourni de que l’entrée de la procédure stockée requiert.

    « Procédure ou fonction 'generate_iris_model' attend le paramètre '@trained_model', qui n’a pas été fourni. »

4. Pour générer le modèle avec les entrées requises et les enregistrer dans une table nécessite des instructions supplémentaires :

    ```sql
    DECLARE @model varbinary(max);
    EXEC generate_iris_model @model OUTPUT;
    INSERT INTO iris_models (model_name, model) values('Naive Bayes', @model);
    ```

5. Maintenant, exécutez le code de génération de modèle une fois de plus. 

    Vous devez obtenir l’erreur : « Violation de contrainte de clé primaire ne peut pas insérer une clé dupliquée dans l’objet 'dbo.iris_models'. La valeur de clé dupliquée est (Naive Bayes) ».

    C’est parce que le nom du modèle a été fourni en tapant manuellement dans « Naive Bayes » dans le cadre de l’instruction INSERT. En supposant que vous envisagez de créer un grand nombre de modèles, à l’aide des paramètres différents ou algorithmes différents à chaque exécution, vous devez envisager de définir un schéma de métadonnées afin que vous pouvez nommer automatiquement les modèles et plus facile de les identifient.

6. Pour contourner cette erreur, vous pourrez procéder à quelques modifications mineures au wrapper SQL. Cet exemple génère un nom de modèle unique en ajoutant la date et heure actuelles :

    ```sql
    DECLARE @model varbinary(max);
    DECLARE @new_model_name varchar(50)
    SET @new_model_name = 'Naive Bayes ' + CAST(GETDATE()as varchar)
    SELECT @new_model_name 
    EXEC generate_iris_model @model OUTPUT;
    INSERT INTO iris_models (model_name, model) values(@new_model_name, @model);
    ```

7. Pour afficher les modèles, exécutez une instruction SELECT simple.

    ```sql
    SELECT * FROM iris_models;
    GO
    ```

    **Résultats**

    |Model_Name | model |
    |------|------|
    | Naive Bayes | 0x800363736B6C656172... |
    | Naive Bayes 2018 de 01 Jan 9:39 AM | 0x800363736B6C656172... |
    | Naive Bayes 2018 01 de février 10:51 AM | 0x800363736B6C656172... |

## <a name="generate-scores-from-the-model"></a>Générer des scores à partir du modèle

Enfin, nous allons charger ce modèle à partir de la table dans une variable et le passer à Python pour générer des scores.

1. Exécutez le code suivant pour créer la procédure stockée qui effectue le calcul de score. 

    ```sql
    CREATE PROCEDURE predict_species (@model varchar(100))
    AS
    BEGIN
    DECLARE @nb_model varbinary(max) = (SELECT model FROM iris_models WHERE model_name = @model);
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    import pickle
    irismodel = pickle.loads(nb_model)
    species_pred = irismodel.predict(iris_data[[1,2,3,4]])
    iris_data["PredictedSpecies"] = species_pred
    OutputDataSet = iris_data.query( ''PredictedSpecies != SpeciesId'' )[[0, 5, 6]]
    print(OutputDataSet)
    '
    , @input_data_1 = N'select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
    , @input_data_1_name = N'iris_data'
    , @params = N'@nb_model varbinary(max)'
    , @nb_model = @nb_model
    WITH RESULT SETS ( ("id" int, "SpeciesId" int, "SpeciesId.Predicted" int));
    END;
    GO
    ```

    La procédure stockée Obtient le modèle Naïve Bayes à partir de la table et utilise les fonctions associées au modèle pour générer des scores. Dans cet exemple, la procédure stockée Obtient le modèle à partir de la table en utilisant le nom du modèle. Toutefois, selon le type de métadonnées que vous enregistrez avec le modèle, vous pouvez également obtenir le modèle plus récent, ou le modèle avec la précision la plus élevée.

2. Exécutez les lignes suivantes pour passer le nom du modèle « Naive Bayes » à la procédure stockée qui exécute le code de calcul de score. 

    ```sql
    EXEC predict_species 'Naive Bayes';
    GO
    ```

    Lorsque vous exécutez la procédure stockée, elle retourne un data.frame Python. Cette ligne de T-SQL spécifie le schéma pour les résultats renvoyés : `WITH RESULT SETS ( ("id" int, "SpeciesId" int, "SpeciesId.Predicted" int));`

    Vous pouvez insérer les résultats dans une nouvelle table ou les renvoyer à une application.

    Cet exemple a été effectué simple en utilisant les données du jeu de données iris Python pour calculer les scores. (Voir la ligne `iris_data[[1,2,3,4]])`.) Toutefois, plus généralement vous exécuter une requête SQL pour obtenir les nouvelles données et passer en Python comme `InputDataSet`. 

### <a name="remarks"></a>Notes

Si vous êtes habitué à utiliser dans les Python, vous pouvez être habitué à charger des données, créer des résumés et les graphiques, puis apprentissage d’un modèle et générer des scores dans les mêmes 250 lignes de code.

Toutefois, si votre objectif est de mettre le processus (création de modèles, de calcul de score, etc.) dans SQL Server, il est important de prendre en compte les méthodes que vous pouvez séparer le processus en étapes reproductibles qui peuvent être modifiées à l’aide des paramètres. Autant que possible, vous souhaitez que le code Python que vous exécutez une procédure stockée clairement définis entrées et sorties qui correspondent à la procédure stockée entrées et sorties.

En outre, vous pouvez généralement améliorer les performances en séparant le processus d’exploration de données à partir du processus d’apprentissage d’un modèle ou la génération de scores. 

Score et les processus d’apprentissage peuvent souvent être optimisés en tirant parti des fonctionnalités de SQL Server, telles que le traitement parallèle, ou à l’aide des algorithmes dans [revoscalepy](../python/what-is-revoscalepy.md) ou [MicrosoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) cette prise en charge de diffusion en continu et l’exécution en parallèle, au lieu d’utiliser des bibliothèques de Python standard. 

## <a name="next-lesson"></a>Leçon suivante

Dans la dernière leçon, vous exécutez le code Python à partir d’un client distant, à l’aide de SQL Server en tant que le contexte de calcul. Cette étape est facultative, si vous n’avez pas un client Python ou que vous ne voulez pas exécuter les Python en dehors d’une procédure stockée.

+ [Créer un modèle de revoscalepy à partir d’un client Python](use-python-revoscalepy-to-create-model.md)
