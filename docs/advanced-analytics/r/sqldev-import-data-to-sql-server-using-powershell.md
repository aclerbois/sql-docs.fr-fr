---
title: Leçon 2 importer des données dans SQL Server à l’aide de PowerShell | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 6dbd1af581865d44c1636a6bac5acc02ae68532b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="lesson-2-import-data-to-sql-server-using-powershell"></a>Leçon 2 : Importer des données vers SQL Server à l’aide de PowerShell
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie d’un didacticiel pour les développeurs SQL sur la façon d’utiliser R dans SQL Server.

Dans cette étape, vous allez exécuter un des scripts téléchargés pour créer les objets de base de données nécessaires à la procédure pas à pas. Le script crée également la plupart des procédures stockées que vous allez utiliser et charge les données d’exemple sur une table de la base de données que vous avez spécifiée.

## <a name="run-the-scripts-to-create-sql-objects"></a>Exécutez les scripts pour créer des objets SQL

Parmi les fichiers téléchargés, vous devez voir un script PowerShell que vous pouvez exécuter pour préparer l’environnement pour la procédure pas à pas. Les actions effectuées par le script sont les suivantes :

- Installation des utilitaires en ligne de commande SQL et SQL Native Client, si ce n’est déjà fait. Ces utilitaires sont nécessaires pour le chargement en bloc des données sur la base de données à l’aide de **bcp**.

- Création d’une base de données et d’une table sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et insertion en bloc des données dans la table.

- Création de plusieurs procédures stockées et fonctions SQL.

### <a name="run-the-script"></a>Exécutez le script

1.  Ouvrez une invite de commandes PowerShell comme administrateur et exécutez la commande suivante :
  
    ```ps
    .\RunSQL_SQL_Walkthrough.ps1
    ```
  
    Vous serez invité à entrer les informations suivantes :
  
    - Le nom ou l’adresse d’une instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] où [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] a été installé.
  
    - Les nom d’utilisateur et mot de passe d’un compte sur l’instance. Le compte doit disposer des autorisations nécessaires pour créer des bases de données, créer des tables et procédures stockées et charger des données dans les tables. Si vous ne fournissez pas de nom d’utilisateur et mot de passe, votre identité Windows est utilisée pour se connecter à SQL Server.
  
    - Le chemin et le nom du fichier de données exemple que vous venez de télécharger. Par exemple :
  
        `C:\tempRSQL\nyctaxi1pct.csv`
  
2.  Dans le cadre de cette étape, tous les scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été modifiés pour remplacer les espaces réservés par le nom de la base de données et le nom d’utilisateur que vous fournissez comme entrées de script.
  
    Prenez une minute pour passer en revue les procédures stockées et les fonctions créées par le script.
  
    |**Nom de fichier de script SQL**|**Fonction**|
    |-|-|
    |create-db-tb-upload-data.sql|Crée une base de données et deux tables :<br /><br />nyctaxi_sample : contient le dataset sur les taxis de New York (NYC Taxi). Un index cluster columnstore est ajouté à la table pour améliorer les performances du stockage et des requêtes. L’échantillon de 1 % du dataset NYC Taxi sera inséré dans cette table.<br /><br />nyc_taxi_models : permet de rendre persistant le modèle d’analytique avancée formé.|
    |fnCalculateDistance.sql|Crée une fonction scalaire qui calcule la distance directe entre les lieux de prise en charge et de dépose.|
    |fnEngineerFeatures.sql|Crée une fonction table qui crée des caractéristiques de données pour la formation du modèle.|
    |PersistModel.sql|Crée une procédure stockée qui peut être appelée pour enregistrer un modèle. La procédure stockée prend un modèle qui a été sérialisé dans un type de données varbinary et l’écrit dans la table spécifiée.|
    |PredictTipBatchMode.sql|Crée une procédure stockée qui appelle le modèle formé pour créer des prédictions à l’aide du modèle. La procédure stockée accepte une requête comme paramètre d’entrée et retourne une colonne de valeurs numériques qui contient les scores pour les lignes d’entrée.|
    |PredictTipSingleMode.sql|Crée une procédure stockée qui appelle le modèle formé pour créer des prédictions à l’aide du modèle. Cette procédure stockée accepte une nouvelle observation comme entrée, avec des valeurs de caractéristiques passées comme paramètres inline, et retourne une valeur qui prédit l’issue de la nouvelle observation.|
  
    Vous allez créer des procédures stockées supplémentaires dans la dernière partie de cette procédure pas à pas :
  
    |**Nom de fichier de script SQL**|**Fonction**|
    |------|------|
    |PlotHistogram.sql|Crée une procédure stockée pour l’exploration des données. Cette procédure stockée appelle une fonction R pour tracer l’histogramme d’une variable, puis retourne le traçage sous forme d’objet binaire.|
    |PlotInOutputFiles.sql|Crée une procédure stockée pour l’exploration des données. Cette procédure stockée crée un graphique à l’aide d’une fonction R, puis enregistre la sortie dans un fichier PDF local.|
    |TrainTipPredictionModel.sql|Crée une procédure stockée qui forme un modèle de régression logistique en appelant un package R. Le modèle prédit la valeur de la colonne tipped et est formé à l’aide d’un échantillon de 70 % des données sélectionné de façon aléatoire. La sortie de la procédure stockée représente le modèle formé, qui est enregistré dans la table nyc_taxi_models.|
  
3.  Connectez-vous à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et du compte de connexion que vous avez spécifié, pour vérifier que vous pouvez voir la base de données, les tables, les fonctions et les procédures stockées qui ont été créées.
  
    ![rsql_devtut_BrowseTables](media/rsql-devtut-browsetables.png "rsql_devtut_BrowseTables")
  
    > [!NOTE]
    > Si les objets de base de données existent déjà, ils ne peuvent pas être recréés.
    >   
    > Si la table existe déjà, les données sont ajoutées, pas remplacées. Ainsi, pensez à supprimer tout objet avant d’exécuter le script.

## <a name="next-lesson"></a>Leçon suivante

[Leçon 3 : Explorer et visualiser les données](../tutorials/sqldev-explore-and-visualize-the-data.md)

## <a name="previous-lesson"></a>Leçon précédente

[Leçon 1 : Télécharger les exemples de données](../tutorials/sqldev-download-the-sample-data.md)
