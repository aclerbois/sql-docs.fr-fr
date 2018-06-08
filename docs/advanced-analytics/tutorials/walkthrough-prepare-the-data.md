---
title: Préparer les données à l’aide de PowerShell (procédure pas à pas) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: ccdccaf4a3624bef365cec85e452a88526b9fd6b
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585931"
---
# <a name="prepare-the-data-using-powershell-walkthrough"></a>Préparer les données à l’aide de PowerShell (procédure pas à pas)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

À ce stade, vous devez avoir un des éléments suivants installés :

+ SQL Server 2016 R Services
+ Services SQL Server 2017 Machine Learning, avec le langage R activé

Dans cette leçon, vous téléchargez les données, les packages R et les scripts R utilisés dans la procédure pas à pas à partir d’un référentiel Github. Vous pouvez télécharger tout à l’aide d’un script PowerShell pour des raisons pratiques.

Vous devez également installer des packages R supplémentaires, à la fois sur le serveur et sur votre station de travail R. Les étapes sont décrites.

Ensuite, vous utilisez un deuxième script PowerShell, RunSQL_R_Walkthrough.ps1, pour configurer la base de données qui est utilisé pour la modélisation et de calcul de score. Que le script effectue un chargement en masse des données dans la base de données vous spécifiez et crée ensuite certaines fonctions SQL et les procédures stockées qui simplifient les tâches courantes relatives aux données.

C’est parti !

## <a name="1-download-the-data-and-scripts"></a>1. Télécharger les données et les scripts

Tout le code nécessaire a été fourni dans un référentiel GitHub. Vous pouvez utiliser un script PowerShell pour créer une copie locale des fichiers.

1.  Sur votre ordinateur client de science des données, ouvrez une invite de commandes Windows PowerShell en tant qu’administrateur.

2.  Pour vérifier que vous pouvez exécuter le script de téléchargement sans erreur, exécutez cette commande. Celle-ci autorise temporairement les scripts sans changer les valeurs système par défaut.

    ```
    Set-ExecutionPolicy Unrestricted -Scope Process -Force
    ```
      
3.  Exécutez la commande Powershell suivante pour télécharger les fichiers de script dans un répertoire local. Si vous ne spécifiez pas un répertoire différent, par défaut, le dossier `C:\tempR` est créé et tous les fichiers enregistrés.
  
    ```
    $source = 'https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/RSQL/Download_Scripts_R_Walkthrough.ps1'  
    $ps1_dest = "$pwd\Download_Scripts_R_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_R_Walkthrough.ps1 –DestDir 'C:\tempR'
    ```
  
    Si vous souhaitez enregistrer les fichiers dans un répertoire différent, modifiez les valeurs du paramètre *DestDir* et spécifiez un dossier différent sur votre ordinateur. Si vous tapez un nom de dossier n’existe pas, le script PowerShell crée le dossier pour vous.
  
4.  Le téléchargement peut prendre du temps. Une fois le téléchargement terminé, la console de commandes Windows PowerShell doit ressembler à ceci :
  
    ![Après complétion du script PowerShell](media/rsql-e2e-psscriptresults.PNG "Après complétion du script PowerShell")
  
5.  Dans la console PowerShell, vous pouvez exécuter la commande `ls` pour afficher la liste des fichiers qui ont été téléchargés dans *DestDir*.  Pour obtenir une description des fichiers, consultez [ce qui est inclus](#whats-included-in-the-sample).

## <a name="2-install-required-r-packages"></a>2. Installer des packages R requis

Cette procédure pas à pas nécessite certaines bibliothèques R qui ne sont pas installées par défaut dans le cadre de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]. Vous devez installer les packages sur le client lorsque vous développez la solution et sur la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordinateur où vous déployez la solution.

### <a name="install-required-packages-on-the-client"></a>Installer les packages nécessaires sur le client

Le script R que vous avez téléchargé contient les commandes pour télécharger et installer ces packages.

1. Dans votre environnement R, ouvrez le fichier de script RSQL_R_Walkthrough.R.

2. Mettez en surbrillance et exécutez ces lignes.
    
    ```
    # Install required R libraries, if they are not already installed.
    if (!('ggmap' %in% rownames(installed.packages()))){install.packages('ggmap')}
    if (!('mapproj' %in% rownames(installed.packages()))){install.packages('mapproj')}
    if (!('ROCR' %in% rownames(installed.packages()))){install.packages('ROCR')}
    if (!('RODBC' %in% rownames(installed.packages()))){install.packages('RODBC')}
    ```
    
    Certains packages également installent les packages requis. En tout, environ 32 packages sont nécessaires.

### <a name="install-required-packages-on-the-server"></a>Installer les packages nécessaires sur le serveur

Il existe différentes manières que vous pouvez installer des packages sur SQL Server. Par exemple, SQL Server fournit [gestion des packages R](../r/install-additional-r-packages-on-sql-server.md) fonctionnalité qui permet aux administrateurs de base de données de créer un référentiel de packages et d’affecter des droits pour installer leurs propres packages utilisateur. Toutefois, si vous êtes un administrateur sur l’ordinateur, vous pouvez installer les nouveaux packages à l’aide de R, tant que vous installez pour la bibliothèque appropriée.

> [!NOTE]
> Sur le serveur, **pas** installer dans une bibliothèque de l’utilisateur, même si vous y êtes invité. Si vous installez dans une bibliothèque de l’utilisateur, l’instance de SQL Server ne peut pas trouver ou exécuter les packages. Pour plus d’informations, consultez [Installer des packages R supplémentaires sur SQL Server](../r/install-additional-r-packages-on-sql-server.md).

1. Sur l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ouvrez RGui.exe **en tant qu’administrateur**.  Si vous avez installé SQL Server R Services à l’aide des paramètres par défaut, vous trouverez RGui.exe dans C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES\bin\x64).

2.  À l’invite R, exécutez les commandes R suivantes :
  
    ```
    install.packages("ggmap", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
    install.packages("mapproj", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
    install.packages("ROCR", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
    install.packages("RODBC", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
    ```

    - Cet exemple utilise la fonction grep R permet de rechercher le vecteur de chemins d’accès disponibles et de trouver le chemin d’accès qui inclut « Program Files ». Pour plus d’informations, consultez [ http://www.rdocumentation.org/packages/base/functions/grep ](http://www.rdocumentation.org/packages/base/functions/grep).

    - Si vous pensez que les packages sont déjà installés, vérifiez la liste des packages installés en exécutant `installed.packages()`.

## <a name="3-prepare-the-environment-using-runsqlrwalkthroughps1"></a>3. Préparer l’environnement à l’aide de RunSQL_R_Walkthrough.ps1

En même temps que les fichiers de données, des scripts R et des scripts T-SQL, le téléchargement inclut le script PowerShell `RunSQL_R_Walkthrough.ps1`. Le script effectue les actions suivantes :

- Il vérifie si SQL Native Client et les utilitaires de ligne de commande pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installés. Les outils en ligne de commande sont nécessaires pour exécuter [l’utilitaire bcp](../../tools/bcp-utility.md), qui est utilisé pour le chargement en bloc rapide des données dans les tables SQL.

- Il se connecte à l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et exécute certains scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] qui configurent la base de données et créent les tables pour le modèle et les données.

- Il exécute un script SQL pour créer plusieurs procédures stockées.

- Il charge les données que vous avez téléchargées précédemment dans une table nommée `nyctaxi_sample`.

- Il réécrit les arguments dans le fichier de script R pour utiliser le nom de base de données que vous spécifiez.

Exécutez ce script sur l’ordinateur où vous générez la solution : par exemple, l’ordinateur portable sur lequel vous développez et testez votre code R. Cet ordinateur, que nous allons appeler le client de science des données, doit pouvoir se connecter à l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide du protocole Canaux nommés.

1. Ouvrez une ligne de commande PowerShell **en tant qu’administrateur**.
  
2.  Accédez au dossier où vous avez téléchargé les scripts et tapez le nom du script comme indiqué. Appuyez sur Entrée.

    ```
    .\RunSQL_R_Walkthrough.ps1
    ```
  
3.  Vous êtes invité à chacun des paramètres suivants :
  
    **Nom du serveur de base de données**: le nom de l’instance de SQL Server sur lequel la Machine learning Services ou R Services est installé.

    En fonction des spécifications de votre réseau, le nom d’instance peut nécessiter une qualification avec un ou plusieurs noms de sous-réseaux.  Par exemple, si MYSERVER ne fonctionne pas, essayez myserver.subnet.mycompany.com.
    
    **Nom de la base de données que vous souhaitez créer** : par exemple, vous pouvez taper **Didacticiel** ou **Taxi**

    **Nom d’utilisateur** : indiquez un compte disposant de privilèges d’accès aux bases de données. Deux options s’offrent à vous :
    
    + Saisissez le nom d’une session SQL qui possède des privilèges CREATE DATABASE et fournissez le mot de passe SQL sur une invite suivante.
    + Appuyez sur Entrée sans taper de nom pour utiliser votre propre identité Windows, puis, à l’invite sécurisée, tapez votre mot de passe Windows. PowerShell n’autorise pas la saisie d’un nom d’utilisateur Windows différent.
    + Si vous ne parvenez pas à spécifier un utilisateur valide, le script par défaut à l’aide de l’authentification Windows intégrée.
    
      > [!WARNING]
      > Lorsque vous utilisez l’invite de commandes dans le script PowerShell pour fournir vos informations d’identification, le mot de passe est écrit dans le fichier de script mis à jour en texte brut. Immédiatement après avoir créé les objets R nécessaires, modifiez le fichier pour supprimer les informations d’identification.
      
    **Chemin au fichier csv** : indiquez le chemin complet au fichier de données. Le chemin et le nom de fichier par défaut sont `C:\tempR\nyctaxi1pct.csv`.
  
4.  Appuyez sur Entrée pour exécuter le script.

    Le script doit télécharger le fichier et charger les données dans la base de données automatiquement. Cette opération peut prendre du temps. Observez les messages d’état dans la fenêtre PowerShell.
      
    Si l’importation en bloc ou n’importe quel autre échoue, vous pouvez charger les données manuellement comme décrit dans la [dépannage](#bkmk_Troubleshooting) section.

**Résultats (réussite)**

```
Execution successful
Completed registering all stored procedures used in this walkthrough.
This step (registering all stored procedures) takes 0.39 seconds.
Plug in the database server name, database name, user name and password into the R script file
This step (plugging in database information) takes 0.48 seconds.
```

Cliquez sur ce lien pour accéder à la leçon suivante : [afficher et Explorer les données à l’aide de SQL](walkthrough-view-and-explore-the-data.md)

## <a name="bkmk_Troubleshooting"></a>Dépannage

Si vous avez des problèmes avec le script PowerShell, vous pouvez exécuter tout ou partie des étapes manuellement, en utilisant les lignes du script PowerShell comme exemples. Cette section répertorie certains problèmes courants et des solutions de contournement.

### <a name="powershell-script-didnt-download-the-data"></a>Le script PowerShell n’a pas téléchargé les données

Pour télécharger les données manuellement, cliquez avec le bouton droit sur le lien suivant et sélectionnez **Enregistrer la cible sous**.

[http://getgoing.blob.core.windows.net/public/nyctaxi1pct.csv](http://getgoing.blob.core.windows.net/public/nyctaxi1pct.csv)

Prenez note du chemin du fichier de données téléchargé et du nom du fichier où les données ont été enregistrées. Vous devez le chemin d’accès complet pour charger les données de la table à l’aide **bcp**.

### <a name="unable-to-download-the-data"></a>Impossible de télécharger les données

Le fichier de données est volumineux. Vous devez utiliser un ordinateur qui dispose d’une connexion Internet relativement bonne, ou le téléchargement peut expirer.

### <a name="could-not-connect-or-script-failed"></a>Connexion impossible ou échec du script

Vous pouvez obtenir cette erreur durant l’exécution de l’un des scripts suivants : *Une erreur liée au réseau ou spécifique à l’instance s’est produite lors de l’établissement d’une connexion à SQL Server.*

+ Vérifiez l’orthographe du nom de l’instance.
+ Vérifiez la chaîne de connexion complète.
+ En fonction des spécifications de votre réseau, le nom d’instance peut nécessiter une qualification avec un ou plusieurs noms de sous-réseaux.  Par exemple, si MYSERVER ne fonctionne pas, essayez myserver.subnet.mycompany.com.
+ Vérifiez si le Pare-feu Windows autorise les connexions par SQL Server.
+ Essayez d’inscrire votre serveur et vérifiez qu’il autorise les connexions à distance.
+ Si vous utilisez une instance nommée, activez SQL Browser pour faciliter les connexions.

### <a name="network-error-or-protocol-not-found"></a>Erreur réseau ou protocole introuvable

+ Vérifiez que l’instance prend en charge les connexions à distance.
+ Vérifiez que l’utilisateur SQL spécifié peut se connecter à distance à la base de données.
+ Activez les canaux nommés sur l’instance.
+ Vérifiez les autorisations du compte. Le compte que vous avez spécifié n’est peut-être pas autorisé à créer une base de données et à charger des données.

### <a name="bcp-did-not-run"></a>bcp ne s’est pas exécuté

+ Vérifiez que [l’utilitaire bcp](../../tools/bcp-utility.md) est disponible sur votre ordinateur. Vous pouvez exécuter **bcp** à partir d’une fenêtre PowerShell ou d’une invite de commandes Windows.
+ En cas d’erreur, ajoutez l’emplacement de l’utilitaire **bcp** à la variable d’environnement système PATH et réessayez.

### <a name="the-table-schema-was-created-but-the-table-has-no-data"></a>Le schéma de table a été créé, mais la table ne contient pas de données

Si le reste du script s’est exécuté sans problème, vous pouvez charger les données dans la table manuellement en appelant **bcp** à partir de la ligne de commande, comme suit :

#### <a name="using-a-sql-login"></a>Avec une session SQL

~~~~
bcp TutorialDB.dbo.nyctaxi_sample in c:\tempR\nyctaxi1pct.csv -t ',' -S rtestserver.contoso.com -f C:\tempR\taxiimportfmt.xml -F 2 -C "RAW" -b 200000 -U <SQL login> -P <password>
~~~~

#### <a name="using-windows-authentication"></a>Avec l’authentification Windows

~~~~
bcp TutorialDB.dbo.nyctaxi_sample in c:\tempR\nyctaxi1pct.csv -t ',' -S rtestserver.contoso.com -f C:\tempR\taxiimportfmt.xml -F 2 -C "RAW" -b 200000 -T
~~~~

+ Le `in` mot clé spécifie la direction du déplacement des données.
+ L’argument  **-f** requiert que vous spécifiiez le chemin d’accès complet d’un fichier de format. Un fichier de format est nécessaire si vous utilisez l’option **in** .
+ Utilisez les arguments **-U** et **-P** si vous exécutez bcp avec une session SQL.
+ Utilisez l’argument **-T** si vous utilisez l’authentification intégrée Windows.

Si le script ne charge pas les données, inspectez la syntaxe et vérifiez que le nom de votre serveur est spécifié correctement pour votre réseau. Par exemple, veillez à inclure tous les sous-réseaux ainsi que le nom de l’ordinateur si vous vous connectez à une instance nommée. Vérifiez que la connexion a la possibilité d’effectuer des téléchargements de bloc.

### <a name="i-want-to-run-the-script-without-prompts"></a>Je souhaite exécuter le script sans invite

Vous pouvez spécifier, à l’aide de ce modèle, tous les paramètres sur une seule ligne de commande en utilisant des valeurs propres à votre environnement.

```
.\RunSQL_R_Walkthrough.ps1 -server <server address> -dbname <new db name> -u <user name> -p <password> -csvfilepath <path to csv file>
```

L’exemple suivant exécute le script à l’aide d’une connexion SQL :

```
.\RunSQL_R_Walkthrough.ps1 -server MyServer.subnet.domain.com -dbname MyDB –u SqlUserName –p SqlUsersPassword -csvfilepath C:\temp\nyctaxi1pct.csv
```

-   Il se connecte à l’instance et à la base de données spécifiées avec les informations d’identification de *SqlUserName*.
-   Il obtient les données à partir du fichier *C:\temp\nyctaxi1pct.csv*.
-   Il charge les données dans la table *dbo.nyctaxi_sample*, dans la base de données *MyDB* sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommée *MyServer*.

### <a name="the-data-loaded-but-it-contains-duplicates"></a>Les données sont chargées, mais il y a des doublons

Si votre base de données contient une table existante du même nom et le même schéma, **bcp** insère une nouvelle copie de ces données plutôt que de remplacement des données existantes.

Pour éviter la duplication des données, tronquer des tables existantes avant de réexécuter le script.

## <a name="whats-included-in-the-sample"></a>Ce qui est inclus dans l’exemple

Lorsque vous téléchargez les fichiers à partir du référentiel GitHub, vous obtenez :

+ Données au format CSV ; consultez [apprentissage et le score des données](#bkmk_data) pour plus d’informations
+ Un script PowerShell pour la préparation de l’environnement
+ Un fichier de format XML pour l’importation des données vers SQL Server à l’aide de bcp
+ Plusieurs scripts T-SQL
+ Tout le code R dont vous avez besoin pour cette procédure pas à pas

### <a name="bkmk_data"></a>Apprentissage et le score des données

Les données sont un échantillon représentatif du jeu de données sur les taxis de la ville de New York, qui contient les enregistrements de plus de 173 millions de trajets en 2013, y compris les tarifs et le montant des pourboires versés pour chaque trajet. Pour rendre les données plus faciles à manipuler, l’équipe de science des données Microsoft a effectué un sous-échantillonnage pour obtenir seulement 1 % des données.  Ces données a été partagées dans un conteneur de stockage d’objets blob public dans Azure, au format .CSV. La source de données est un fichier non compressé, juste sous 350 Mo.

+ Jeu de données public : () [NYC Taxi et briefings Commission]http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml)

+ [Création de modèles Azure ML sur le Dataset NYC Taxi] (https://blogs.technet.microsoft.com/machinelearning/2015/04/02/building-azure-ml-models-on-the-nyc-taxi-dataset/.

### <a name="powershell-and-r-script-files"></a>Fichiers de script PowerShell et R

+ **RunSQL_R_Walkthrough.ps1** vous exécutez ce script en premier lieu, à l’aide de PowerShell. Il appelle les scripts SQL pour charger les données dans la base de données.

+ **taxiimportfmt.xml** Un fichier de définition de format utilisé par l’utilitaire bcp pour charger des données dans la base de données.

+ **RSQL_R_Walkthrough.R** Voici le script R de base qui est utilisé dans le reste des leçons pour votre analyse de données et de modélisation. Il fournit tout le code R dont vous avez besoin pour explorer les données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , générer le modèle de classification et de créer des tracés.

### <a name="t-sql-script-files"></a>Fichiers de script T-SQL

Le script PowerShell s’exécute plusieurs [!INCLUDE[tsql](../../includes/tsql-md.md)] des scripts sur l’instance de SQL Server. Le tableau suivant répertorie les [!INCLUDE[tsql](../../includes/tsql-md.md)] des scripts et leur signification.

|Nom du fichier de script SQL|Description|
|------------------------|----------------|
|create-db-tb-upload-data.sql|Crée une base de données et deux tables :<br /><br /> *nyctaxi_sample*: table qui contient les données de formation, un échantillon de 1 % du jeu de données sur les taxis de New York. Un index cluster columnstore est ajouté à la table pour améliorer les performances de stockage et des requêtes.<br /><br /> *nyc_taxi_models*: une table utilisée pour stocker les modèles formés au format binaire.|
|PredictTipBatchMode.sql|Crée une procédure stockée qui appelle un modèle formé pour prédire les étiquettes des nouvelles observations. Il accepte une requête comme paramètre d’entrée.|
|PredictTipSingleMode.sql|Crée une procédure stockée qui appelle un modèle de classification formé pour prédire les étiquettes des nouvelles observations. Les variables des nouvelles observations sont passées comme paramètres inline.|
|PersistModel.sql|Crée une procédure stockée qui permet de stocker la représentation binaire du modèle de classification dans une table de la base de données.|
|fnCalculateDistance.sql|Crée une fonction scalaire SQL qui calcule la distance directe entre les lieux de prise en charge et de dépose.|
|fnEngineerFeatures.sql|Crée une fonction table SQL qui crée les caractéristiques d’apprentissage du modèle de classification.|

Les requêtes T-SQL utilisées dans cette procédure pas à pas ont été testées et peut être exécutés en tant que-se trouve dans votre code R. Toutefois, si vous souhaitez faire des expérimentations supplémentaires ou développer votre propre solution, nous vous recommandons d’utiliser un environnement de développement SQL dédié pour tester et optimiser vos requêtes avant de les ajouter à votre code R.

+ L’[extension mssql](https://code.visualstudio.com/docs/languages/tsql) pour [Visual Studio Code](https://code.visualstudio.com/) est un environnement d’exécution de requêtes gratuit et léger qui prend également en charge la plupart des tâches de développement de base de données.
+ [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) est un outil performant et gratuit destiné au développement et à la gestion des bases de données SQL Server.

## <a name="next-lesson"></a>Leçon suivante

[Afficher et Explorer les données à l’aide de R et SQL](walkthrough-view-and-explore-the-data.md)

## <a name="previous-lesson"></a>Leçon précédente

[Procédure pas à pas de données de bout en bout science pour R et SQL Server](walkthrough-data-science-end-to-end-walkthrough.md)

[Prérequis pour les procédures pas à pas de science des données](walkthrough-prerequisites-for-data-science-walkthroughs.md)
