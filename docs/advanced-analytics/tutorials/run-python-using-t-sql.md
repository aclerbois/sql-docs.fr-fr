---
title: Exécuter à l’aide de T-SQL de Python | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: db959c9d8802ad3d3d2f7e8bdb64e194130dfeba
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="run-python-using-t-sql"></a>Exécutez Python à l’aide de T-SQL
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Ce didacticiel explique comment vous pouvez exécuter le code Python dans SQL Server 2017. Il vous guide tout au long du processus de déplacement des données entre SQL Server et Python et explique comment inclure dans un wrapper de code Python correcte dans une procédure stockée [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour générer, former et utiliser des modèles d’apprentissage automatique dans SQL Serveur.

## <a name="prerequisites"></a>Configuration requise

Pour effectuer ce didacticiel, vous devez tout d’abord installer SQL Server 2017 et activer les Services de Machine Learning sur l’instance, comme décrit dans [installer SQL Server 2017 Machine Learning Services (de-de base de données)](../install/sql-machine-learning-services-windows-install.md). 

Vous devez également installer [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Ou bien, vous pouvez utiliser outil de base de données une autre requête ou de gestion, tant qu’il peut se connecter à un serveur et la base de données et exécuter une requête T-SQL ou une procédure stockée.

Après avoir terminé le programme d’installation, revenir à ce didacticiel pour apprendre à exécuter du code Python dans le contexte d’une procédure stockée. 

## <a name="overview"></a>Vue d'ensemble

Ce didacticiel inclut les quatre leçons :

+ Les principes fondamentaux de déplacement de données entre SQL Server et Python : Découvrez les conditions de base, les structures de données, les entrées et les sorties.
+ Pratiques à l’aide de procédures stockées pour les tâches de Python simples, comme le chargement des données d’exemple.
+ Utiliser des procédures stockées pour créer un modèle d’apprentissage Python et générer des scores à partir du modèle.
+ Une leçon facultatif pour les utilisateurs souhaitant exécuter Python à partir d’un client distant, à l’aide de SQL Server en tant que le _contexte de calcul_. Inclut le code de création d’un modèle ; Toutefois, requiert que vous êtes déjà familiarisé avec les environnements Python et les outils Python.

Exemples de Python supplémentaires spécifiques à SQL Server 2017 fournis ici : [didacticiels de SQL Server Python](../tutorials/sql-server-python-tutorials.md)

## <a name="verify-that-python-is-enabled-and-the-launchpad-is-running"></a>Vérifiez que Python est activé et que le Launchpad est en cours d’exécution.

1. Dans Management Studio, exécutez cette instruction pour vous assurer que le service a été activé.

    ```sql
    sp_configure 'external scripts enabled'
    ```

    Si **run_value** est 1, la fonctionnalité d’apprentissage machine est installée et prête à fonctionner.

    Une cause courante d’erreurs est que le Launchpad, qui gère la communication entre SQL Server et Python, s’est arrêté. Vous pouvez afficher l’état de Launchpad à l’aide de Windows **Services** panneau, ou en ouvrant le Gestionnaire de Configuration SQL Server. Si le service est arrêté, redémarrez-le.

2. Ensuite, vérifiez que le runtime Python travaille et communique avec SQL Server. Pour ce faire, ouvrez une nouvelle **requête** fenêtre dans SQL Server Management Studio et connectez-vous à l’instance où Python a été installé.

    ```sql
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'print(3+4)'
    ```

    Si tout fonctionne correctement, vous devez voir un message similaire à celle du résultat

    ```text
    STDOUT message(s) from external script: 
    7
    ```

3. Si vous obtenez des erreurs, il existe diverses choses à que faire pour vous assurer que le serveur et les Python permettre communiquer. 

    Vous devez ajouter le groupe d’utilisateurs Windows `SQLRUserGroup` en tant que connexion sur l’instance, pour vous assurer que Launchpad peut permettre une communication entre Python et SQL Server. (Le même groupe est utilisé pour les deux R et l’exécution du code Python). Pour plus d’informations, consultez [activé l’authentification implicite](../r/add-sqlrusergroup-to-database.md).
    
    En outre, vous devrez peut-être activer les protocoles réseau qui ont été désactivées, ou d’ouvrir le pare-feu afin que SQL Server peut communiquer avec les clients externes. Pour plus d’informations, consultez [dépannage de l’installation](../common-issues-external-script-execution.md).

## <a name="basic-python-interaction"></a>Interaction de base Python

Il existe deux façons d’exécuter le code Python dans SQL Server :

+ Ajouter un script Python en tant qu’argument de la procédure stockée système, **sp_execute_external_script**
+ À partir d’un client distant de Python, se connecter à SQL Server et d’exécuter du code à l’aide de SQL Server en tant que le contexte de calcul. Cela nécessite [revoscalepy](../python/what-is-revoscalepy.md).

L’objectif principal de ce didacticiel est pour vous assurer que vous pouvez utiliser Python dans une procédure stockée.

1. Exécuter du code simple pour visualiser la façon dont les données sont transmises dans les deux sens entre SQL Server et Python.

    ```sql
    execute sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

2. En supposant que tous les éléments sont correctement configuré et Python et SQL Server sont adressent à l’autre, le résultat correct est calculé et les Python `print` fonction retourne le résultat à la **Messages** windows.

    **Résultats**

    ```text
    STDOUT message(s) from external script: 
    0.5 2
    ```
    
    Lors de l’obtention **stdout** messages est pratique lorsque vous testez votre code, plus vous devez souvent retourner les résultats dans un format tabulaire, afin que vous pouvez l’utiliser dans une application ou écrire dans une table. 

Pour l’instant, n’oubliez pas ces règles :

+ Tous les éléments à l’intérieur de la `@script` l’argument doit être le code Python valide. 
+ Le code doit respecter toutes les règles Pythonic en matière de mise en retrait, les noms de variable et ainsi de suite. Lorsque vous obtenez une erreur, vérifiez votre espace blanc et la casse.
+ Si vous utilisez toutes les bibliothèques qui ne sont pas chargés par défaut, vous devez utiliser une instruction d’importation au début de votre script de les charger. 
+ Si la bibliothèque n’est pas déjà installée, désactivez puis installer le package Python en dehors de SQL Server, comme décrit ici : [installer de nouveaux packages Python sur SQL Server](../python/install-additional-python-packages-on-sql-server.md)

## <a name="inputs-and-outputs"></a>Entrées et sorties

Par défaut, [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) accepte un seul dataset d’entrée, vous fournissez en général sous la forme d’une requête SQL valide. Autres types d’entrée peuvent être passés en tant que variables SQL : par exemple, vous pouvez passer un modèle formé en tant que variable, à l’aide d’une fonction de sérialisation comme [marinade](https://docs.python.org/3.0/library/pickle.html) ou [rx_serialize_model](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) pour écrire le modèle un format binaire.

La procédure stockée retourne une seule Python [pandas](http://pandas.pydata.org/pandas-docs/stable/index.html) trame de données en tant que sortie. Toutefois, vous pouvez produire scalaires et les modèles en tant que variables. Vous pouvez par exemple, un modèle formé en tant que binaire variable de sortie et le transmettre à une instruction T-SQL INSERT pour écrire ce modèle dans une table. Vous pouvez également générer des graphiques (au format binaire) ou des valeurs scalaires (valeurs individuelles, telles que la date et l’heure, la durée pour l’apprentissage du modèle et ainsi de suite).

Pour l’instant, examinons simplement la valeur par défaut des variables d’entrée et de sortie, `InputDataSet` et `OutputDataSet`. 

1. Exécutez le code suivant pour certains Maths et génère les résultats.

        ```sql
        execute sp_execute_external_script 
        @language = N'Python', 
        @script = N'
        a = 1
        b = 2
        c = a/b
        print(c)
        OutputDataSet = c
        '
        WITH RESULT SETS ((ResultValue float))
        ```

2. Vous devez obtenir une erreur, car le code Python génère une valeur scalaire, pas une trame de données.

        **Results**

        ```text
        line 43, in transform
            raise TypeError('OutputDataSet should be of type pandas.DataFrame')
        ```

3. Maintenant voir que se passe-t-il lorsque vous passez un jeu de données tabulaire à Python, à l’aide de la variable d’entrée par défaut `InputDataSet`. 

    ```sql
    EXECUTE sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    OutputDataSet = InputDataSet
    ',
    @input_data_1 = N'SELECT 1 as Col1'
    ```

    La procédure stockée retourne un data.frame automatiquement, sans avoir à faire quelque chose supplémentaire dans votre code Python.

    **Résultats**

    | Aucun nom de colonne|
    |------|
    | 1|

    Par défaut, le jeu de données d’entrée sous forme de tableau unique portant le nom, `InputDataSet`. Toutefois, vous pouvez modifier le nom en ajoutant une ligne comme suit : `@input_data_1_name = N'myResultName'`.

    Noms de colonnes utilisés par Python ne sont jamais conservées dans la sortie. Bien que la requête d’entrée spécifié le nom de colonne `Col1`, ce nom n’est pas retourné, pas plus que tous les en-têtes de colonne utilisées par votre script Python. Pour spécifier un type de données et le nom de colonne lorsque vous affichez les données de SQL Server, utilisez le code T-SQL `WITH RESULT SETS` clause.

4. Cet exemple fournit de nouveaux noms pour les variables d’entrée et de sortie.

    ```sql
    execute sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    MyOutput = MyInput
    ',
    @input_data_1_name = N'MyInput',
    @input_data_1 = N'SELECT 1 as Col1',
    @output_data_1_name = N'MyOutput'
    WITH RESULT SETS ((ResultValue int))
    ```

    La clause WITH RESULT SET définit le schéma pour la sortie, étant donné que les noms de colonne Python ne sont jamais retournés avec le data.frame.

    **Résultats**

    | ResultValue|
    |------|
    | 1|

5. Maintenant examinons erreur Python typique. Remplacez la ligne dans l’exemple précédent, à partir de `@input_data_1_name = N'MyInput'` à `@input_data_1_name = N'myinput'`.

    Erreurs de Python sont passés à vous en tant que messages, par le service de satellite utilisé par SQL Server. Les messages peuvent être longs et incluent les erreurs liées à SQL Server ou Launchpad en plus des erreurs de Python, soyez patient dans plonger dans le texte. Message de la clé est dans cette ligne :

    ```text
    MyOutput = MyInput
    NameError: name 'MyInput' is not defined
    ```

    Rappelez-vous que Python, tels que R, respecte la casse. Par conséquent, lorsque vous obtenez tout type d’erreur, veillez à vérifier vos noms de variables et rechercher des problèmes avec les types de données, mise en retrait et l’espacement.

## <a name="python-data-structures"></a>Structures de données Python

SQL Server s’appuie sur les Python **pandas** package, ce qui est très pratique pour l’utilisation des données tabulaires. Toutefois, vous avez déjà vu que vous ne pouvez pas transmettre une valeur scalaire à partir de Python à SQL Server et attendre qu’il « tout simplement ». Dans cette section, nous allons examiner des définitions de type de base de données, pour vous préparer pour les autres problèmes que vous pouvez exécuter sur plusieurs lors du passage des données tabulaires entre Python et SQL Server.

+ Une trame de données est une table avec _plusieurs_ colonnes.
+ Une seule colonne d’une trame de données est un objet de liste appelé une série.
+ Une valeur unique est une cellule d’une trame de données et doit être appelée par l’index.

Comment exposerait l’unique résultat d’un calcul en tant qu’une trame de données, si un data.frame requiert une structure tabulaire ? Une solution consiste à représenter la valeur scalaire unique sous la forme d’une série, qui est facilement convertie pour une trame de données. 

1. Cet exemple effectue les quelques calculs simple et convertit une valeur scalaire en une série. Une série requiert un index, vous pouvez affecter manuellement, comme illustré ici, ou par programmation.

    ```sql
    execute sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    a = 1
    b = 2
    c = a/b
    print(c)
    s = pandas.Series(c, index =["simple math example 1"])
    print(s)
    '
    ```

2. Étant donné que la série n’a pas été convertie en un data.frame, les valeurs sont retournées dans la fenêtre des Messages, mais vous pouvez voir que les résultats sont dans un format tabulaire plus.

    **Résultats**

    ```text
    STDOUT message(s) from external script: 
    0.5
    simple math example 1    0.5
    dtype: float64
    ```

3. Pour augmenter la longueur de la série, vous pouvez ajouter de nouvelles valeurs, à l’aide d’un tableau. 

    ```sql
    execute sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    s = pandas.Series([c,d])
    print(s)
    '
    ```

    Si vous ne spécifiez pas un index, qui a des valeurs en commençant par 0 et se terminant par la longueur du tableau est généré par un index.

    **Résultats**

    ```text
    STDOUT message(s) from external script: 
    0    0.5
    1    2.0
    dtype: float64
    ```

4. Si vous augmentez le nombre de **index** valeurs, mais ne pas ajouter de nouveaux **données** valeurs, les valeurs de données sont répétées pour remplir la série.

    ```sql
    execute sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    a = 1
    b = 2
    c = a/b
    s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
    print(s)
    '
    ```

    **Résultats**

    ```text
    STDOUT message(s) from external script: 
    0.5
    simple math example 1    0.5
    simple math example 2    0.5
    dtype: float64
    ```

### <a name="convert-series-to-data-frame"></a>Convertir en série trame de données

Avoir converti les nos résultats scalaires mathématiques à une structure tabulaire, nous devons encore pour les convertir en un format SQL Server peut gérer. 

1. Pour convertir une série à un data.frame, appelez les pandas [trame de données](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe) (méthode).

    ```sql
    execute sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    import pandas as pd
    a = 1
    b = 2
    c = a/b
    d = a*b
    s = pandas.Series([c,d])
    print(s)
    df = pd.DataFrame(s)
    OutputDataSet = df
    '
    WITH RESULT SETS (( ResultValue float ))
    ```

2. Notez que les valeurs d’index ne sont pas de sortie, même si vous utilisez l’index pour obtenir des valeurs spécifiques à partir de la data.frame.

    **Résultats**

    |ResultValue|
    |------|
    |0.5|
    |2|

### <a name="output-values-into-dataframe-using-an-index"></a>Valeurs de sortie en data.frame à l’aide d’un index

Nous allons voir comment la conversion en un data.frame fonctionne avec nos deux séries qui contient les résultats d’opérations mathématiques simples. La première a un index de valeurs séquentielles générées par Python. La seconde utilise un index arbitraire de valeurs de chaîne.

1. Cet exemple obtient une valeur de la série qui utilise un index d’entiers.

    ```sql
    EXECUTE sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    import pandas as pd
    a = 1
    b = 2
    c = a/b
    d = a*b
    s = pandas.Series([c,d])
    print(s)
    df = pd.DataFrame(s, index=[1])
    OutputDataSet = df
    '
    WITH RESULT SETS (( ResultValue float ))
    ```

    N’oubliez pas que l’index généré automatiquement commence à 0. Essayez d’utiliser une valeur hors limites index et observez le résultat.

2. Maintenant, nous allons apprendre une valeur unique à partir d’autres trame de données qui a un index de chaîne. 

    ```sql
    EXECUTE sp_execute_external_script 
    @language = N'Python', 
    @script = N'
    import pandas as pd
    a = 1
    b = 2
    c = a/b
    s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
    print(s)
    df = pd.DataFrame(s, index=["simple math example 1"])
    OutputDataSet = df
    '
    WITH RESULT SETS (( ResultValue float ))
    ```

    **Résultats**

    |ResultValue|
    |------|
    |0.5|

    Si vous essayez d’utiliser un index numérique pour obtenir une valeur à partir de cette série, vous obtenez une erreur.

Cet exercice a été conçu pour vous donner une idée de la façon de travailler avec des structures de données différentes de Python et vous assurer de qu'obtenir le résultat correct en tant qu’une trame de données. Vous pouvez ont conclu cette sortie une valeur unique comme une trame de données est plus difficile à sa valeur ! Heureusement, vous pouvez facilement passer tous les types de valeurs dans et hors de la procédure stockée en tant que variables. Qui est abordée dans la leçon suivante.

## <a name="tips"></a>Conseils

+ Parmi les langages de programmation Python est un des plus souple en matière de guillemets simples ou des guillemets doubles ; ils sont quasiment interchangeables. 

    Toutefois, T-SQL utilise des guillemets simples pour uniquement certaines choses et le `@script` argument utilise les guillemets simples pour encadrer le code Python sous forme de chaîne Unicode. Par conséquent, vous devrez peut-être examiner votre code Python et modifier des guillemets simples à des guillemets doubles.

+ Impossible de trouver la procédure stockée, `sp_execute_external_script`? Cela signifie que vous n’avez pas probablement terminé la configuration de l’instance pour prendre en charge l’exécution du script externe. Une fois le programme d’installation de SQL Server 2017 en cours d’exécution et en sélectionnant les Python en tant que langue d’apprentissage, vous devez explicitement activer la fonctionnalité à l’aide de `sp_configure`, puis redémarrez l’instance. 

    Pour plus d’informations, consultez [installer SQL Server 2017 Machine Learning Services (de-de base de données)](../install/sql-machine-learning-services-windows-install.md).

## <a name="next-steps"></a>Étapes suivantes

[Retour à la ligne de code Python dans une procédure stockée SQL](wrap-python-in-tsql-stored-procedure.md)