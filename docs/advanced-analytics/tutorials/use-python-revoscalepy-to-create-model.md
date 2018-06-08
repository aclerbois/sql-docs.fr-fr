---
title: Utiliser Python avec revoscalepy pour créer un modèle | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: d886466d7bf4f0c86c1cd9505480a3fadb6e66ef
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="use-python-with-revoscalepy-to-create-a-model"></a>Utiliser Python avec revoscalepy pour créer un modèle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Dans cette leçon, vous allez apprendre à exécuter le code Python à partir d’un client de développement à distance, pour créer un modèle de régression linéaire dans SQL Server. 

## <a name="prerequisites"></a>Configuration requise

+ Cette leçon utilise les leçons précédentes des données différentes. Vous n’avez pas besoin effectuer les leçons précédentes en premier. Toutefois, si vous avez terminé les leçons précédentes et disposez d’un serveur déjà configuré pour s’exécuter Python, utiliser ce serveur et la base de données en tant qu’un contexte de calcul.
+ Pour exécuter le code Python à l’aide de SQL Server comme un calcul contexte nécessite SQL Server 2017 ou version ultérieure. En outre, vous devez installer explicitement et puis activez la fonctionnalité, **Machine Learning Services**, en choisissant l’option de langage Python.

    Si vous avez installé une version préliminaire de SQL Server 2017, vous devez mettre à jour au moins la version RTM. Les versions ultérieures de service ont continué à développer et à améliorer les fonctionnalités de Python. Certaines fonctionnalités de ce didacticiel peut ne pas fonctionnent dans les premières versions préliminaires.

+ Cet exemple utilise un environnement Python prédéfini, nommé `PYTEST_SQL_SERVER`. L’environnement a été configuré pour qu’il contienne **revoscalepy** et d’autres bibliothèques requises. 

    Si vous n’avez pas d’un environnement configuré pour s’exécuter Python, vous devez le faire séparément. En savoir plus sur la création ou de modifier des environnements Python est hors de portée pour ce didacticiel. Pour plus d’informations sur la façon de configurer un client Python qui contient les bibliothèques corrects, consultez [client de l’installation de Python](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter) et [Python lien outils](https://docs.microsoft.com/machine-learning-server/python/quickstart-python-tools).

## <a name="remote-compute-contexts-and-revoscalepy"></a>Revoscalepy et contextes de calcul à distance

Cet exemple illustre le processus de création d’un modèle de Python dans une distance _contexte de calcul_, qui vous permet de vous travaillez à partir d’un client, mais que vous choisissez un environnement distant, telles que SQL Server, Spark ou Machine Learning Server, où le les opérations sont effectuées. L’utilisation de contextes de calcul rend plus faciles à écrire du code une fois et déployez-le sur n’importe quel environnement pris en charge.

Pour exécuter le code Python dans SQL Server requiert le **revoscalepy** package. C’est un package Python spécial fourni par Microsoft, similaire à la **RevoScaleR** package pour le langage R. Le **revoscalepy** package prend en charge la création de contextes de calcul et fournit l’infrastructure pour le transfert des données et des modèles entre une station de travail locale et un serveur distant. Le **revoscalepy** de fonction qui prend en charge l’exécution de code de la base de données est [RxInSqlServer](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rxinsqlserver).

Dans cette leçon, vous utilisez données dans SQL Server pour former un modèle linéaire basé sur [rx_lin_mod](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-lin-mod), une fonction dans **revoscalepy** qui prend en charge la régression sur les jeux de données très volumineux. 

Cette leçon explique également comment configurer et utiliser ensuite un **contexte de calcul de SQL Server** dans Python. Pour plus d’informations de contextes de calcul fonctionnent avec d’autres plateformes et contextes de calcul qui sont pris en charge, consultez [contexte de calcul pour l’exécution du script dans Machine Learning Server](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-compute-context)

## <a name="prepare-the-database-and-sample-data"></a>Préparer les données de base de données et exemple

1. Cette leçon est basée sur la base de données `sqlpy`. Si vous n’avez pas effectué aucune des leçons précédentes, vous pouvez créer la base de données en exécutant le code suivant :

    ```sql
    CREATE DATABASE sqlpy;
    GO
    USE sqlpy;
    GO
    ```

    > [!IMPORTANT]
    > Si vous souhaitez utiliser une autre base de données, veillez à modifier l’exemple de code et de modifier le nom de la base de données dans la chaîne de connexion.

2. Cet exemple utilise le jeu de données compagnie aérienne, qui est disponible dans R et Python. Une fois que vous avez créé une base de données pour vos exemples de Python, remplir une table avec les données comme décrit ici : [les exemples de données dans RevoScaleR](https://docs.microsoft.com/machine-learning-server/r/sample-built-in-data).

3. Modifiez le nom de cet environnement pour utiliser un environnement disponible sur votre client.

## <a name="run-the-sample-code"></a>Exécutez l’exemple de code

Une fois que vous avez préparé la base de données et les données pour l’apprentissage, stocké dans une table, ouvrez un environnement de développement Python et exécuter l’exemple de code.

Le code exécute les étapes suivantes :

1. Importe les bibliothèques requises et les fonctions.
2. Crée une connexion à SQL Server. Crée **source de données** objets pour travailler avec les données.
3. Modifie les données à l’aide de **transformations** afin qu’il peut être utilisé par l’algorithme de régression logistique.
4. Appels `rx_lin_mod` et définit la formule utilisée pour ajuster le modèle.
5. Génère un ensemble de prédictions basées sur les données d’origine.
6. Crée un résumé basé sur les valeurs prédites.

Toutes les opérations sont effectuées à l’aide d’une instance de SQL Server en tant que le contexte de calcul.

> [!NOTE]
> Pour une démonstration de cet exemple en cours d’exécution à partir de la ligne de commande, consultez cette vidéo : [Analytique SQL Server 2017 avancé Python](https://www.youtube.com/watch?v=FcoY795jTcc)

### <a name="sample-code"></a>Exemple de code

```python
from revoscalepy import RxComputeContext, RxInSqlServer, RxSqlServerData
from revoscalepy import rx_lin_mod, rx_predict, rx_summary
from revoscalepy import RxOptions, rx_import

from pandas import Categorical
import os

def test_linmod_sql():
    sql_server = os.getenv('PYTEST_SQL_SERVER', '.')
    
    sql_connection_string = 'Driver=SQL Server;Server=' + sqlServer + ';Database=sqlpy;Trusted_Connection=True;'
    print("connectionString={0!s}".format(sql_connection_string))

    data_source = RxSqlServerData(
        sql_query = "select top 10 * from airlinedemosmall",
        connection_string = sql_connection_string,

        column_info = {
            "ArrDelay" : { "type" : "integer" },
            "DayOfWeek" : {
                "type" : "factor",
                "levels" : [ "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" ]
            }
        })

    sql_compute_context = RxInSqlServer(
        connection_string = sql_connection_string,
        num_tasks = 4,
        auto_cleanup = False
        )

    #
    # Run linmod locally
    #
    linmod_local = rx_lin_mod("ArrDelay ~ DayOfWeek", data = data_source)
    #
    # Run linmod remotely
    #
    linmod = rx_lin_mod("ArrDelay ~ DayOfWeek", data = data_source, compute_context = sql_compute_context)

    # Predict results
    # 
    predict = rx_predict(linmod, data = rx_import(input_data = data_source))
    summary = rx_summary("ArrDelay ~ DayOfWeek", data = data_source, compute_context = sql_compute_context)
```

### <a name="defining-a-data-source-vs-defining-a-compute-context"></a>Définition d’une source de données par rapport à la définition d’un contexte de calcul

Une source de données est différente à partir d’un contexte de calcul. Le _source de données_ définit les données utilisées dans votre code. Le _contexte de calcul_ définit où le code sera exécuté. Cependant, ils utilisent certains des mêmes informations :

+ Variables de Python, tel que `sql_query` et `sql_connection_string`, définissez la source de données. 

    Passer ces variables à la [RxSqlServerData](https://docs.microsoft.com/r-server/python-reference/revoscalepy/rxsqlserverdata) constructeur pour implémenter le **objet source de données** nommé `data_source`.

+ Vous créez un **objet de contexte de calcul** à l’aide de la [RxInSqlServer](https://docs.microsoft.com/r-server/python-reference/revoscalepy/rxinsqlserverdata) constructeur. Résultant **objet de contexte de calcul** est nommé `sql_cc`.

    Cet exemple utilise de nouveau la même chaîne de connexion que vous avez utilisé dans la source de données, en supposant que les données sont sur la même instance de SQL Server que vous utiliserez comme contexte de calcul. 
    
    Toutefois, la source de données et le contexte de calcul peut être sur des serveurs différents.
 
### <a name="changing-compute-contexts"></a>Modification des contextes de calcul

Après avoir défini un contexte de calcul, vous devez définir le **contexte de calcul active**. 

Par défaut, la plupart des opérations sont exécutées localement, ce qui signifie que si vous ne spécifiez pas un contexte de calcul différentes, les données seront extraites à partir de la source de données, et le code s’exécute dans votre environnement actuel de Python.

Il existe deux façons de définir le contexte de calcul active :
+ En tant qu’argument d’une méthode ou une fonction
+ En appelant `rx_set_computecontext`

#### <a name="set-compute-context-as-an-argument-of-a-method-or-function"></a>Définir le contexte de calcul en tant qu’argument d’une méthode ou une fonction

Dans cet exemple, vous définissez le contexte de calcul à l’aide d’un argument de la personne **rx** (fonction).
    
`linmod = rx_lin_mod_ex("ArrDelay ~ DayOfWeek", data = data, compute_context = sql_compute_context)`

Ce contexte de calcul est réutilisé dans l’appel à [rxsummary](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-summary):.

`summary = rx_summary("ArrDelay ~ DayOfWeek", data = data_source, compute_context = sql_compute_context)`

#### <a name="set-a-compute-context-explicitly-using-rxsetcomputecontext"></a>Définir un contexte de calcul explicitement à l’aide de rx_set_compute_context

La fonction [rx_set_compute_context](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-set-compute-context) vous permet de basculer entre les contextes qui ont déjà été définies de calcul.

Après avoir défini contexte de calcul active, elle reste active jusqu'à ce que vous la changiez.

### <a name="using-parallel-processing-and-streaming"></a>À l’aide d’un traitement parallèle et diffusion en continu

Lorsque vous définissez le contexte de calcul, vous pouvez également définir les paramètres qui contrôlent comment les données sont gérées par le contexte de calcul. Ces paramètres diffèrent selon le type de source de données.

Pour les contextes de calcul de SQL Server, vous pouvez définir la taille de lot, ou fournir des conseils sur le degré de parallélisme à utiliser dans les tâches en cours d’exécution.

+ L’exemple a été exécuté sur un ordinateur équipé de quatre processeurs, donc la `num_tasks` paramètre est défini à 4 pour autoriser l’utilisation maximale des ressources. 
+ Si vous définissez cette valeur sur 0, SQL Server utilise la valeur par défaut, qui consiste à s’exécuter autant de tâches en parallèle que possible, sous les paramètres MAXDOP actuels pour le serveur. Toutefois, le nombre exact de tâches qui peuvent être alloués dépend de nombreux autres facteurs, tels que les paramètres du serveur et les autres tâches qui s’exécutent.

## <a name="related-samples"></a>Exemples associés

Ces exemples de Python supplémentaires et des didacticiels illustrent des scénarios de bout en bout à l’aide de sources de données plus complexes, ainsi que l’utilisation de contextes de calcul à distance.

+ [Python dans base de données pour les développeurs SQL](sqldev-in-database-python-for-sql-developers.md)
+ [Créer un modèle prédictif à l’aide de Python et SQL Server](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/)
+ [Créer un modèle prédictif à l’aide de Python et SQL Server](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/)
