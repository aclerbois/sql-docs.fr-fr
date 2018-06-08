---
title: Configurer les outils clients de Python pour une utilisation avec SQL Server Machine Learning | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: ace5ea536c020d2713f1aa07bd1b26c41065a587
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="set-up-python-client-tools-for-use-with-sql-server-machine-learning"></a>Définir les outils clients pour une utilisation avec SQL Server Machine Learning Python
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit comment configurer un environnement de Python sur un ordinateur Windows pour prendre en charge d’exécution du code Python dans SQL Server. Cela inclut les scénarios suivants :

+ Vous tester et développez des solutions sur une station de travail distante Python et envoyez le code d’exécution dans un contexte de calcul de SQL Server vers SQL Server. 

    En général, vous souhaitez installer un environnement de développement complet de Python. 
    
    Votre environnement de Python local doit être compatible avec l’environnement de Python sur l’instance de SQL Server, et vous devez installer les bibliothèques qui prennent en charge des contextes de calcul à distance.

+ Vous développerez et testez votre code à l’aide des outils Python dédiés et migrez le code pour SQL Server pour s’exécuter dans une procédure stockée.

    Vous utilisez un environnement IDE Python complet pour le développement, le serveur. Toutefois, avant de déployer votre code, vous le tester dans un environnement qui émule l’environnement SQL Server.

+ Vous exécutez le code Python principalement dans une procédure stockée dans SQL Server et vous ne développez pas le code Python. Vous vous attendez que le code a été testé et débogué avant que vous l’encapsuler dans une procédure stockée. Toutefois, il peut arriver que vous devrez peut-être exécuter le code en dehors de SQL Server, pour déterminer la source d’un problème.

    Vous ne souhaitez pas installer les outils Python sur le serveur et utiliserez uniquement les outils par défaut installés avec la distribution. Vous pouvez décider de configurer un environnement de Python qui utilise la bibliothèque de l’instance, pour vérifier que le code fonctionne en dehors d’une procédure stockée.

## <a name="requirements"></a>Spécifications

Quel que soit les outils que vous utilisez pour développer votre code Python, les conditions suivantes s’appliquent à chaque fois que vous exécutez le code Python dans SQL Server ou utilisez des données SQL Server :

+ Pour utiliser Python, SQL Server 2017 ou version ultérieure est requis. Vous devez également installer la fonctionnalité qui prend en charge de la Machine Learning Services (de-de base de données) et explicitement activer la fonctionnalité. Pour plus d’informations, consultez [configurer SQL Server 2017](../r/set-up-sql-server-r-services-in-database.md).

    Si vous avez installé une version préliminaire de SQL Server 2017, vous obtiendrez des erreurs si vous essayez d’exécuter des commandes de Python à partir de cet utilitaire de ligne de commande. Il est recommandé que vous [mise à niveau vers la dernière version](#bkmk_update) si possible. 

+ Vous devez vous assurer que le compte que vous utilisez pour exécuter le code a l’autorisation de se connecter à la base de données et à exécuter le code Python. L’autorisation spéciale `EXECUTE ANY EXTERNAL SCRIPT` est requis pour tous les comptes qui utilisent des scripts R ou Python. 

+ Vous devez vous assurer que le compte possède les autorisations de base de données qui peuvent être nécessaires pour lire les données ou créer de nouveaux objets. 

+ Si votre code requiert les packages qui ne sont pas installés par défaut avec SQL Server, organiser avec l’administrateur de base de données pour les packages installés dans l’environnement de Python qui est utilisé par l’instance. SQL Server est un environnement sécurisé et il existe des restrictions sur lequel les packages peuvent être installés. 

    Installation ad hoc des packages dans le cadre de votre code n’est pas recommandée, même si vous disposez des droits. En outre, toujours soigneusement les implications de sécurité avant d’installer de nouveaux packages dans la bibliothèque du serveur.

> [!NOTE]
> Si vous avez l’intention d’utiliser Python avec Machine Learning serveur, qui prend en charge des contextes de calcul supplémentaires, tels que les clusters Linux ou Spark, voir les articles suivants :
> 
> + [Comment installer interpréteur et des packages personnalisés Python localement sur Windows](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter)
> + [Lier des outils Python et IDE à l’interpréteur Python installé avec le serveur de Machine Learning](https://docs.microsoft.com/machine-learning-server/python/quickstart-python-tools)

## <a name="default-tools-included-with-standard-install"></a>Outils par défaut inclus avec l’installation standard

Une installation par défaut de 2017 du serveur SQL avec Machine Learning Services (de-de base de données) et Python ajoute les ressources des outils Python standards suivants :

+ Exemples de données Python
+ Distribution anaconda 4.2 
+ Pythonw.exe et Python exécutables python.exe

Par défaut, installation est dans ce dossier : `~\Program Files\Microsoft SQL Server\<instance_name>\PYTHON_SERVICES`. 

## <a name="open-python-from-the-command-line"></a>Ouvrir les Python à partir de la ligne de commande 

Pour utiliser le runtime Python qui est installé avec l’instance, vous pouvez démarrer l’exécutable de Python à partir du chemin d’installation. Des instructions de base sont disponibles sur le [Python pour Windows FAQ](https://docs.python.org/3/faq/windows.html).

> [!IMPORTANT]
> En règle générale, pour éviter les conflits de ressources, il est recommandé que vous **pas** exécuter Python à partir de la bibliothèque de l’instance sur le serveur, si vous pensez qu’il est possible de l’instance SQL Server est en cours d’exécution de code Python. Toutefois, l’aide des outils dans la bibliothèque de l’instance peut être utile si vous tentez de déboguer un problème qui se produit uniquement lors de l’exécution dans SQL Server et à afficher des messages d’erreur plus détaillés, ou vous assurer que tous les packages requis sont installés.

1. Accédez au dossier dans lequel la bibliothèque de l’instance est installée. Par exemple, dans une installation par défaut, le dossier est `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`.

2. Recherchez Python.exe. 

3. Avec le bouton droit et sélectionnez **exécuter en tant qu’administrateur** pour ouvrir une fenêtre de ligne de commande interactive.

## <a name="bkmk_update"></a> Mettre à jour des composants de Python

Vous pouvez mettre à jour les composants de Python télécharger et installer une version plus récente, en utilisant le script décrit ici : [client installer les Python sur Windows](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter#install-on-windows)
> 
> Le programme d’installation téléchargé par le script est [SPO_9.3.0.0_1033.cab](https://go.microsoft.com/fwlink/?LinkId=859054&clcid=1033). Si vous avez besoin d’une version différente, consultez [l’installation des composants d’apprentissage machine sans accès à internet](http://bing.com)

## <a name="set-up-a-python-development-environment"></a>Configurer un environnement de développement Python

Si vous déboguez simplement des scripts à partir de la ligne de commande, vous pouvez obtenir des résultats avec les outils Python standards installés avec les Services de Machine Learning, ou utiliser un éditeur de texte. Toutefois, si vous développez des nouvelles solutions, ou utilisation d’un client distant, nous vous recommandons d’utiliser un environnement IDE Python complet. Les options sont :

+ [Visual Studio 2017 Community Edition](https://www.visualstudio.com/vs/features/python/) avec Python
+ [Outils AI pour Visual Studio](https://docs.microsoft.com/visualstudio/ai/installation)
+ [Python dans le Code de Visual Studio](https://code.visualstudio.com/docs/languages/python)
+ Populaires des outils tiers tels que PyCharm, Spyder et Eclipse

Nous vous recommandons de Visual Studio, car il prend en charge les projets de base de données, ainsi que les projets de machine learning. Pour configurer un environnement de Python, consultez [environnements Python de gestion dans Visual Studio](https://docs.microsoft.com/visualstudio/python/managing-python-environments-in-visual-studio)

## <a name="install-revoscalepy"></a>Installer revoscalepy

Même si vous avez correctement installé les Services de Machine Learning, vous pouvez obtenir une erreur lors de la tentative de chargement **revoscalepy** fonctions à partir d’une ligne de commande Python. Les étapes suivantes décrivent comment vous pouvez installer une mise à jour pour activer l’utilisation de **revoscalepy**.

1. Télécharger le script de shell d’installation à partir de https://aka.ms/mls93-py (ou utilisez https://aka.ms/mls-py pour le 9.2. mise en production). Le script installe Anaconda 4.2.0, qui inclut Python 3.5.2, ainsi que tous les packages répertoriés précédemment.

2. Ouvrez une nouvelle fenêtre PowerShell avec des autorisations élevées (en tant qu’administrateur).

3. Ouvrez le dossier dans lequel vous avez téléchargé le programme d’installation et exécutez le script :

```ps1
cd {{download-directory}}
.\Install-PyForMLS.ps1
```

Vous pouvez également exécuter la `-InstallFolder` argument de ligne de commande et spécifiez le nouveau chemin d’accès dans le cadre de la commande. Par exemple : 

```ps1
.\Install-PyForMLS.ps1 -InstallFolder "<installation_path>")
```

Si vous obtenez une erreur, vous devrez peut-être suspendre des stratégies d’exécution pour un fichier spécifique, la durée de la session, comme suit : 

```ps1
powershell -ExecutionPolicy Bypass -File "C:\<installation_path>\Install-PyForMLS.ps1"
```

Vous pouvez également interrompre les stratégies d’exécution pendant la durée de la session. Avec cette instruction, la stratégie d’exécution a la valeur `Unrestricted` pour la durée de la session et ne pas modifier la configuration ou la modification d’écriture sur le disque.

```ps1
Set-ExecutionPolicy Bypass -Scope Process
```

## <a name="verify-that-python-and-revoscalepy-are-working"></a>Vérifier que les Python et revoscalepy fonctionnent

Après l’installation de tous les outils et bibliothèques, vous devez vous connecter au serveur et vérifiez que vous pouvez créer un contexte de calcul, ou que les Python peut communiquer avec SQL Server.

### <a name="verify-that-revoscalepy-works-from-the-python-command-line"></a>Vérifiez que revoscalepy fonctionne à partir de la ligne de commande Python

Pour vérifier que le **revoscalepy** module peut être chargé, exécutez l’exemple de code suivant à partir d’une invite de commandes interactive Python. Le code génère un résumé des données à l’aide de l’exemple de données Python et [rx_summary](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-summary). 

```Python
import os
from revoscalepy import rx_summary
from revoscalepy import RxXdfData
from revoscalepy import RxOptions
sample_data_path = RxOptions.get_option("sampleDataDir")
print(sample_data_path)
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
summary = rx_summary("ArrDelay+DayOfWeek", ds)
print(summary)
```

Le chemin d’accès de données exemple est imprimé afin que vous pouvez déterminer quelle instance de Python est appelée.

### <a name="verify-that-python-can-be-called-from-sql-server"></a>Vérifiez que les Python peut être appelée à partir de SQL Server

Pour vérifier que Python communique avec SQL Server, ouvrez SQL Server Management Studio. (Vous pouvez utiliser un autre outil similaire, telles que [SQL opérations Studio](https://docs.microsoft.com/sql/sql-operations-studio/what-is).) Ouvrez une nouvelle **requête** fenêtre et les exécuter n’importe quel Python simple des commandes dans le contexte d’une procédure stockée :

```SQL
EXEC sp_execute_external_script @language = N'Python', 
@script = N'print(3+4)'
```

Il peut prendre un certain temps pour lancer le runtime Python pour la première fois, mais s’il n’y a aucune erreur, vous savez que l’utilisation de SQL Server Launchpad et Python peut être démarré à partir de SQL Server.

Pour vérifier que **revoscalepy** est disponible dans la bibliothèque d’instance de SQL Server, exécutez le script basé sur [rx_summary](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-summary), avec de légères modifications à générer des résultats compatible avec SQL Server. 

```SQL
EXEC sp_execute_external_script @language = N'Python', 
@script = N'
import os
from pandas import DataFrame
from revoscalepy import rx_summary
from revoscalepy import RxXdfData
from revoscalepy import RxOptions

sample_data_path = RxOptions.get_option("sampleDataDir")
print(sample_data_path)

ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
summary = rx_summary("ArrDelay + DayOfWeek", ds)
print(summary)
dfsummary = summary.summary_data_frame
OutputDataSet = dfsummary
'
WITH RESULT SETS  ((ColName nvarchar(25) , ColMean float, ColStdDev  float, ColMin  float,   ColMax  float, Col_ValidObs  float, Col_MissingObs int))
```

Étant donné que rx_summary retourne un objet de type `class revoscalepy.functions.RxSummary.RxSummaryResults`, qui contient plusieurs éléments pour traiter les résultats dans SQL Server, vous pouvez extraire uniquement la trame de données dans un format tabulaire.

### <a name="verify-python-execution-in-sql-server-as-remote-compute-context"></a>Vérifiez que l’exécution de Python dans SQL Server en tant que contexte de calcul à distance

Si vous avez installé **revoscalepy** dans votre environnement de développement Python local, vous devez être en mesure de se connecter à une instance de SQL Server 2017 est où Python a été activé et exécuter un exemple de code similaire à l’aide du serveur en tant que le calcul contexte. 


```Python
import os
from revoscalepy import rx_summary, RxOptions, RxXdfData, RxSqlServerData, RxInSqlServer

# define connection string and compute context
sql_conn_string="Driver=SQL Server;Server=;Database=TestDB;Trusted_Connection=True"
sqlcc = RxInSqlServer(
    connection_string = sql_conn_string,
    num_tasks = 1,
    auto_cleanup = False,
    console_output = True,
    execution_timeout_seconds = 0,
    wait = True
    )
rx_set_compute_context(sqlcc)

# get sample data and path
sample_data_path = RxOptions.get_option("sampleDataDir")
print(sample_data_path)

# generate summary
ds = RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
summary = rx_summary("ArrDelay+DayOfWeek", ds)
print(summary)
```

Dans cet exemple, le résumé est retourné dans la console, plutôt que le retour des données tabulaires bien formées à SQL Server. 

En outre, étant donné que [rx_set_compute_context](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-set-compute-context) a été appelé, les exemples de données sont chargés à partir du dossier d’exemples sur l’ordinateur SQL Server et non à partir de votre dossier d’exemples local.
