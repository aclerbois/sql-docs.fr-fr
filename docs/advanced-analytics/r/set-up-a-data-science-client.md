---
title: Configurer un client de science des données pour le développement de R sur SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: dd0b420630846382b9d7cf456352bb606a4f0040
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="set-up-a-data-science-client-for-r-development-on-sql-server"></a>Configurer un client de science des données pour le développement de R sur SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Après avoir configuré une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] pour prendre en charge d’apprentissage, vous devez configurer un environnement de développement qui est capable de se connecter au serveur pour le déploiement et l’exécution à distance.

Cet article décrit certains scénarios de client standard, y compris la configuration de l’édition de Visual Studio Community gratuite pour exécuter le code R dans SQL Server.

## <a name="install-r-libraries-on-the-client"></a>Installer les bibliothèques R sur le client

Votre environnement client doit inclure Microsoft R Open, ainsi que les packages RevoScaleR supplémentaires qui prennent en charge l’exécution distribuée de R sur SQL Server. Distributions standards de R n’ont pas les packages qui prennent en charge des contextes de calcul à distance ou de l’exécution parallèle de tâches R.

Pour obtenir ces bibliothèques, installez les éléments suivants :
  
+ [Microsoft R Client](http://aka.ms/rclient/download)

+ Microsoft R Server (pour SQL Server 2016)

    - Pour installer à partir de l’installation de SQL Server, consultez [installer SQL Server 2016 R Server (autonome)](../install/sql-r-standalone-windows-install.md)

    - Pour utiliser le programme d’installation distinct basé sur Windows, consultez [installer Machine Learning pour Windows Server](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install)

+ Apprentissage de serveur (pour SQL Server 2017)

    - Pour installer à partir de l’installation de SQL Server, consultez [installer SQL Server 2017 Machine Learning Server (autonome)](../install/sql-machine-learning-standalone-windows-install.md)

    - Pour utiliser le programme d’installation distinct basé sur Windows, consultez [installer R Server 9.1 pour Windows](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows)

## <a name="r-tools"></a>Outils R

Lorsque vous installez R avec SQL Server, vous obtenez les mêmes outils R qui sont installés avec les **base** installation de R, tels que RGui, Rterm et ainsi de suite. Par conséquent techniquement, vous devez tous les outils dont vous avez besoin pour développer et tester le code R.

Les outils R standards suivants sont inclus dans un *installation de base* de R et par conséquent, sont installés par défaut.

+ **RTerm**: un terminal de ligne de commande pour l’exécution des scripts R

+ **RGui.exe** : éditeur interactif simple pour R. Les arguments de ligne de commande sont les mêmes pour RGui.exe et RTerm.

+ **RScript** : outil en ligne de commande pour l’exécution de scripts R en mode batch.

Pour rechercher ces outils, déterminez la bibliothèque R qui a été installée lorsque vous installez SQL Server ou la fonctionnalité d’apprentissage autonome. Par exemple, dans une installation par défaut, les outils R sont situés dans ces dossiers :

+ SQL Server 2016 R Services : `~\Program Files\Microsoft SQL Server\MSSQL13.<instancename>\R_SERVICES\bin\x64`
+ Microsoft R Server autonome : `~\Program Files\Microsoft R\R_SERVER\bin\x64`
+ SQL Server 2017 apprentissage Services : `~\Program Files\Microsoft SQL Server\MSSQL14.<instancename>\R_SERVICES\bin\x64`
+ Apprentissage Server (autonome) : `~\Program Files\Microsoft\ML Server\R_SERVER\bin\x64`

Si vous avez besoin d’aide avec les outils R, ouvrez simplement **RGui**, cliquez sur **aide**, puis sélectionnez une des options

## <a name="microsoft-r-client"></a>Microsoft R Client

Microsoft R Client est un téléchargement gratuit qui vous permet d’accéder aux packages RevoScaleR à des fins de développement. En installant le Client de R, vous pouvez créer des solutions R qui peuvent être exécutées dans tous les contextes de calcul pris en charge, y compris analytique dans base de données de SQL Server et traitements distribués R sur Hadoop, Spark ou Linux à l’aide de la Machine Learning Server.

Si vous avez déjà installé un autre environnement de développement R, tels que RStudio, veillez à reconfigurer l’environnement pour utiliser les bibliothèques et les exécutables fournis par le Client de Microsoft R. En procédant comme vous pouvez donc utiliser toutes les fonctionnalités du package RevoScaleR, bien que les performances seront en trouvent limitées.

Pour plus d’informations, consultez [qu’est Microsoft R Client ?](https://docs.microsoft.com/machine-learning-server/r-client/what-is-microsoft-r-client)

## <a name="install-a-development-environment"></a>Installez un environnement de développement

Si vous n’avez pas encore un environnement de développement R par défaut, nous vous recommandons une des opérations suivantes :

+ Outils R pour Visual Studio

    Fonctionne avec Visual Studio 2015.

    Pour plus d’informations d’installation, consultez [comment installer les outils R pour Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation).
 
    Pour configurer RTV pour utiliser vos bibliothèques de client de Microsoft R, consultez [sur le Client Microsoft R](https://docs.microsoft.com/machine-learning-server/r-client/what-is-microsoft-r-client)

+ Visual Studio 2017

    Même l’édition Community gratuite inclut la charge de science des données, qui installe des modèles de projet pour R, Python et F #.

    Télécharger Visual Studio à partir de [ce site](https://www.visualstudio.com/vs/). 

+ RStudio

    Si vous préférez RStudio, vous devez effectuer des étapes supplémentaires pour pouvoir utiliser les bibliothèques RevoScaleR :

    - Installer le Client Microsoft R pour obtenir les packages requis et les bibliothèques.
    - Mettre à jour votre chemin d’accès de R pour utiliser le runtime de Microsoft R.

    Pour plus d’informations, consultez [R Client - configurer votre IDE](https://docs.microsoft.com/machine-learning-server/r-client/what-is-microsoft-r-client#step-2-configure-your-ide).

## <a name="configure-your-ide"></a>Configurez votre interface IDE

+ Outils R pour Visual Studio

    Consultez [ce site](https://docs.microsoft.com/visualstudio/rtvs/getting-started-with-r) pour obtenir des exemples montrant comment générer et déboguer R projets à l’aide des outils R pour Visual Studio. 

+ Visual Studio 2017

    Si vous installez le Client Microsoft R ou R Server **avant** vous installez Visual Studio, les bibliothèques R Server sont automatiquement détectés et utilisés pour votre chemin d’accès de la bibliothèque. Si vous n’avez pas installé les bibliothèques RevoScaleR, à partir de la **outils R** menu, sélectionnez **installer le Client R**.

## <a name="run-r-in-sql-server"></a>Exécuter R dans SQL Server

Cet exemple utilise Visual Studio 2017 Community Edition, avec la charge de travail de science des données installé.

1. À partir de la **fichier** menu, sélectionnez **nouveau** , puis sélectionnez **projet**.

2. -Main volet contient une liste de modèles préinstallés. Cliquez sur **R**, puis sélectionnez **projet R**. Dans le **nom** , tapez `dbtest` et cliquez sur **OK**.

3. Visual Studio crée un nouveau dossier de projet et un fichier de script par défaut, `Script.R`. 

4. Type `.libPaths()` sur la première ligne du script de fichier, puis appuyez sur CTRL + ENTRÉE.

5. Le chemin d’accès de bibliothèque R en cours doit être affiché dans le **Interactive R** fenêtre. 

6. Cliquez sur le **outils R** menu et sélectionnez **Windows** pour afficher une liste des autres fenêtres R spécifiques que vous pouvez afficher dans votre espace de travail.
 
    + Afficher l’aide sur les packages dans la bibliothèque en cours en appuyant sur CTRL + 3.
    + Voir les variables de R dans le **Explorer Variable**, en appuyant sur CTRL + 8.

7. Créer une chaîne de connexion à une instance de SQL Server et utilisez la chaîne de connexion dans le constructeur RxInSqlServer pour créer un objet de source de données SQL Server. 

    ```r
    connStr <- "Driver=SQL Server;Server=MyServer;Database=MyTestDB;Uid=;Pwd="
    sqlShareDir <- paste("C:\\AllShare\\", Sys.getenv("USERNAME"), sep = "")
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    cc <- RxInSqlServer(connectionString = connStr, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    sampleDataQuery <- "SELECT TOP 100 * from [dbo].[MyTestTable]"
    inDataSource <- RxSqlServerData(sqlQuery = sampleDataQuery, connectionString = connStr, rowsPerRead = 500)
    ```

    > [!TIP]
    > Pour exécuter un lot, sélectionnez les lignes que vous souhaitez exécuter, puis appuyez sur CTRL + ENTRÉE.

8. Définir le contexte de calcul sur le serveur, puis exécutez le code R simple sur les données.

    ```r
    rxSetComputeContext(cc)
    rxGetVarInfo(data = inDataSource)
    ```

    Résultats sont retournés dans le **Interactive R** fenêtre.
    
    Si vous souhaitez vous assurer que le code est en cours d’exécution sur l’instance de SQL Server, vous pouvez utiliser le Générateur de profils pour créer une trace.