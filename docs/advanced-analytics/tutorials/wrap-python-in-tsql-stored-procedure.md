---
title: Retour à la ligne de code Python dans une procédure stockée | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 3b7ffeac0dfe1e441f188aae67e28004e294fc3e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wrap-python-code-in-a-stored-procedure"></a>Retour à la ligne de code Python dans une procédure stockée
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans un [leçon précédente](run-python-using-t-sql.md), vous avez appris comment rendre Python de communiquer avec SQL Server. Dans cette leçon, vous allez apprendre à incorporer le code Python dans une procédure stockée, pour obtenir des données à partir de jeux de données exemple Python et écrire ces données dans une table SQL Server.

La procédure stockée système [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) fournit le wrapper qui transfère des variables SQL et les jeux de données SQL dans Python. Aussi, il gère la sortie des résultats par Python et les transmet au serveur SQL Server dans un format compatible avec les types de données SQL.

Nous allons voir comment cela fonctionne.

## <a name="prepare-the-database-and-tables"></a>Préparer la base de données et tables

Bien qu’il soit possible de configurer un client distant et exécuter le code Python à l’aide de Code Visual Studio, Visual Studio, PyCharm ou autres outils, afin de simplifier le scénario, tout le code dans cette leçon doit être exécuté dans le cadre d’une procédure stockée.

1. Démarrez SQL Server Management Studio, puis ouvrez une nouvelle **requête** fenêtre.  

2. Créer une base de données pour ce projet et de modifier le contexte de votre **requête** fenêtre à utiliser la nouvelle base de données.

    ```sql
    CREATE DATABASE sqlpy
    GO
    USE sqlpy
    GO
    ```

    > [!TIP] 
    > Si vous débutez avec SQL Server, ou que vous travaillez sur un serveur que vous êtes propriétaire, une erreur courante consiste à vous connecter et commencer à travailler sans remarquer que vous êtes dans le **master** base de données. Pour vous assurer que vous utilisez la base de données, spécifiez toujours le contexte à l’aide de la `USE <database name>` instruction.

3. Ajouter des tables vides : un pour stocker les données et l’autre pour stocker les modèles que vous effectuez l’apprentissage. Ultérieurement, vous devez remplir les tables à l’aide de Python.

    Le code suivant crée la table pour les données d’apprentissage.

    ```sql
    DROP TABLE IF EXISTS iris_data;
    GO
    CREATE TABLE iris_data (
      id INT NOT NULL IDENTITY PRIMARY KEY
      , "Sepal.Length" FLOAT NOT NULL, "Sepal.Width" FLOAT NOT NULL
      , "Petal.Length" FLOAT NOT NULL, "Petal.Width" FLOAT NOT NULL
      , "Species" VARCHAR(100) NOT NULL, "SpeciesId" INT NOT NULL
    );
    ```

    Si vous ne connaissez pas T-SQL, il est utile de mémoriser la `DROP...IF` instruction. Lorsque vous essayez de créer une table, et il en existe déjà, SQL Server retourne une erreur : « Il existe déjà un objet nommé 'iris_data' dans la base de données. » Un pour éviter de telles erreurs consiste à supprimer des tables existantes ou autres objets dans le cadre de votre code.

4. Exécutez le code suivant pour créer la table utilisée pour stocker le modèle formé. Pour enregistrer les modèles de Python (ou R) dans SQL Server, ils doivent être sérialisées et stockées dans une colonne de type **varbinary (max)**. 

    ```sql
    DROP TABLE IF EXISTS iris_models;
    GO
    
    CREATE TABLE iris_models (
      model_name VARCHAR(50) NOT NULL DEFAULT('default model') PRIMARY KEY,
      model VARBINARY(MAX) NOT NULL
    );
    GO
    ```

    En plus du contenu du modèle, en règle générale, vous devez également ajouter des colonnes pour les autres métadonnées utiles, telles que le nom du modèle, la date, qu'il a été formé, l’algorithme de la source et des paramètres, sources de données et ainsi de suite. Pour l’instant, nous allons plus de simplicité et utiliser simplement le nom du modèle.

## <a name="populate-the-table"></a>Remplissez la table

Pour déplacer les données d’apprentissage à partir de Python dans un serveur SQL Server table est un processus en plusieurs étapes :

+ Vous concevez une procédure stockée qui obtient les données souhaitées.
+ Vous exécutez la procédure stockée pour accéder aux données.
+ Vous utilisez une instruction INSERT pour spécifier où les données récupérées doivent être enregistrées.

1. Créez la procédure stockée suivante qui inclut le code Python. 

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    from sklearn import datasets
    iris = datasets.load_iris()
    iris_data = pandas.DataFrame(iris.data)
    iris_data["Species"] = pandas.Categorical.from_codes(iris.target, iris.target_names)
    iris_data["SpeciesId"] = iris.target
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

    Lorsque vous exécutez ce code, vous devez obtenir le message « Commandes exécutée avec succès. » Tous les moyens de cette sont que la procédure stockée a été créée en fonction de vos spécifications.

2. Pour remplir la table, exécuter la procédure stockée et spécifier la table où les données doivent être écrites. Lors de l’exécution, la procédure stockée s’exécute le code Python, qui charge le jeu de données Iris à partir de l’exemple de données Python intégrés.

    ```sql
    INSERT INTO iris_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "SpeciesId")
    EXEC dbo.get_iris_dataset;
    ```

    Si vous utilisez T-SQL, gardez à l’esprit que l’instruction INSERT ajoute uniquement les nouvelles données ; Il ne sera pas vérifier les données existantes, ou supprimez et reconstruisez la table. Pour éviter plusieurs copies des mêmes données dans une table, vous pouvez exécuter cette instruction en premier : `TRUNCATE TABLE iris_data`. Le code T-SQL [TRUNCATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/truncate-table-transact-sql) instruction supprime les données existantes, mais laisse intactes les la structure de la table.

    > [!TIP]
    > Pour modifier la procédure stockée une version ultérieure, vous n’avez pas besoin de supprimer et recréer. Utilisez le [ALTER PROCEDURE](https://docs.microsoft.com/sql/t-sql/statements/alter-procedure-transact-sql) instruction. 

3. Pour vérifier que les données ont été chargées correctement, vous pouvez exécuter des requêtes simples :

    ```sql
    SELECT TOP(10) * FROM iris_data;
    SELECT COUNT(*) FROM iris_data;
    ```

Dans le [leçon suivante](../tutorials/train-score-using-python-in-tsql.md), vous créez un modèle d’apprentissage automatique et l’enregistrer dans une table.

### <a name="further-reading-about-stored-procedures"></a>Obtenir des informations supplémentaires sur les procédures stockées

Si vous ne connaissez pas SQL Server, vous constaterez des procédures stockées compliqués dans un premier temps. Mais une procédure stockée est une interface puissante et flexible pour passer des données entre les applications et le serveur. En utilisant une procédure stockée, vous pouvez définir dynamiquement les entrées, ce qui le rend facile à passer dans les nouveaux noms de modèle, les nouveaux paramètres et les nouvelles données, sans modifier votre code Python.

Pour une vue d’ensemble du travail de procédures stockées comment, consultez [de procédures stockées (moteur de base de données)](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine), ou ce didacticiel : [écriture d’instructions Transact-SQL](https://docs.microsoft.com/sql/t-sql/tutorial-writing-transact-sql-statements).

Il existe également quelques bons didacticiels sur les sites de Communauté comme [SQL Server Central](http://www.sqlservercentral.com/) ou [équipe SQL](http://www.sqlteam.com/).

Lorsque vous réfléchissez à la façon dont vous pouvez encapsuler mieux code Python dans T-SQL, également envisager d’utiliser ces fonctionnalités :

+ Définition des valeurs par défaut pour la procédure stockée
+ À l’aide du mot clé OUTPUT pour passer les variables d’entrée
+ Création de définitions de schéma à l’aide avec les résultats pour vous assurer que les données utilisées par les applications possède les types de données appropriée et les noms de colonnes
+ En fournissant des indications pour améliorer le traitement par lots
+ Emprunter l’identité d’un autre utilisateur pour tester votre code, à l’aide de la clause EXECUTE AS clause

## <a name="next-lesson"></a>Leçon suivante

[Former un modèle de Python et générer des scores dans SQL Server](../tutorials/train-score-using-python-in-tsql.md)
