---
title: Étape 2 importer des données dans SQL Server à l’aide de PowerShell | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: d85419c06915cc7d96c9713053239c27c70a9f0b
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="step-2-import-data-to-sql-server-using-powershell"></a>Étape 2 : Importer des données vers SQL Server à l’aide de PowerShell
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie d’un didacticiel, [analytique Python de la base de données pour les développeurs SQL](sqldev-in-database-python-for-sql-developers.md). 

Dans cette étape, vous exécutez un des scripts téléchargés, pour créer les objets de base de données requis pour la procédure pas à pas. Le script crée plusieurs procédures stockées et télécharge les exemples de données à une table dans la base de données que vous avez spécifié.

## <a name="create-database-objects-and-load-data"></a>Créer des objets de base de données et charger des données

Parmi les fichiers téléchargés, vous devez voir un script PowerShell, `RunSQL_SQL_Walkthrough.ps1`. L’objectif de ce script est pour préparer l’environnement pour la procédure pas à pas.

Le script effectue les actions suivantes :

- Installe les utilitaires de ligne de commande SQL, SQL Native Client si pas déjà installé. Ces utilitaires sont nécessaires pour le chargement en bloc des données sur la base de données à l’aide de **bcp**.

- Crée une base de données et une table sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance et -insertions en bloc des données dans la table.

- Crée plusieurs fonctions SQL et les procédures stockées.

Si vous rencontrez des problèmes, vous pouvez utiliser le script en tant que référence pour effectuer les étapes manuellement.

1. Ouvrez une invite de commandes PowerShell en tant qu’administrateur. Si vous n’êtes pas déjà dans le dossier créé à l’étape précédente, accédez au dossier, puis exécutez la commande suivante :
  
    ```ps
    .\RunSQL_SQL_Walkthrough.ps1
    ```

2. Le script vous invite à entrer les informations suivantes :

    - Le nom ou l’adresse d’un [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] instance où la Machine Learning Services avec Python a été installé.
    - Les nom d’utilisateur et mot de passe d’un compte sur l’instance. Le compte que vous utilisez doit avoir la possibilité de créer des bases de données, créer des tables et des procédures stockées et en bloc des données de charge pour les tables. 
    - Si vous ne fournissez pas de nom d’utilisateur et mot de passe, votre identité de Windows est utilisée pour se connecter à SQL Server, et vous sont promus pour entrer un mot de passe.
    - Le chemin et le nom du fichier de données exemple que vous venez de télécharger. Par exemple : `C:\temp\pysql\nyctaxi1pct.csv`

    > [!NOTE]
    > Pour charger les données correctement, le fichier xmlrw.dll bibliothèque doit être dans le même dossier que bcp.exe.

3. Le script modifie également la [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts que vous avez téléchargé précédemment, et des espaces réservés remplace avec le nom de la base de données et l’utilisateur nom que vous fournissent.
  
4. Lorsque le script terminé, connectez-vous à le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance à l’aide de la connexion spécifiée, pour vérifier que la base de données, tables, les fonctions et procédures stockées ont été créés. L’illustration suivante montre les objets dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

    ![Parcourir les tables dans SSMS](media/sqldev-python-browsetables1.png "afficher les tables dans SSMS")

    > [!NOTE]
    > Si les objets de base de données existaient déjà, ils ne peuvent pas être créés à nouveau.
    > 
    > Si une table existante du même nom et du schéma est trouvé, les données seront ajoutées, ne pas remplacés. Par conséquent, veillez à supprimer ou tronquer des tables existantes avant d’exécuter le script.

## <a name="list-of-stored-procedures-and-functions"></a>Liste des procédures stockées et fonctions

Les objets SQL Server suivants sont créés par le script :

|**Nom de fichier de script SQL**|**Fonction**|
|------|------|
|create-db-tb-upload-data.sql|Crée une base de données et deux tables :<br /><br />nyctaxi_sample : contient le dataset sur les taxis de New York (NYC Taxi). Un index cluster columnstore est ajouté à la table pour améliorer les performances du stockage et des requêtes. L’échantillon de 1 % du dataset NYC Taxi sera inséré dans cette table.<br /><br />nyc_taxi_models : permet de rendre persistant le modèle d’analytique avancée formé.|
|fnCalculateDistance.sql|Crée une fonction scalaire qui calcule la distance directe entre les lieux de prise en charge et de dépose.|
|fnEngineerFeatures.sql|Crée une fonction table qui crée des caractéristiques de données pour la formation du modèle.|
|TrainingTestingSplit.sql|Fractionner les données dans la table nyctaxi_sample en deux parties : nyctaxi_sample_training et nyctaxi_sample_testing.|
|PredictTipSciKitPy.sql|Crée une procédure stockée qui appelle le modèle formé (scikit-en savoir plus) pour créer des prédictions à l’aide du modèle. La procédure stockée accepte une requête comme paramètre d’entrée et retourne une colonne de valeurs numériques qui contient les scores pour les lignes d’entrée.|
|PredictTipRxPy.sql|Crée une procédure stockée qui appelle le modèle formé (revoscalepy) pour créer des prédictions à l’aide du modèle. La procédure stockée accepte une requête comme paramètre d’entrée et retourne une colonne de valeurs numériques qui contient les scores pour les lignes d’entrée.|
|PredictTipSingleModeSciKitPy.sql|Crée une procédure stockée qui appelle le modèle formé (scikit-en savoir plus) pour créer des prédictions à l’aide du modèle. Cette procédure stockée accepte une nouvelle observation comme entrée, avec des valeurs de caractéristiques passées comme paramètres inline, et retourne une valeur qui prédit l’issue de la nouvelle observation.|
|PredictTipSingleModeRxPy.sql|Crée une procédure stockée qui appelle le modèle formé (revoscalepy) pour créer des prédictions à l’aide du modèle. Cette procédure stockée accepte une nouvelle observation comme entrée, avec des valeurs de caractéristiques passées comme paramètres inline, et retourne une valeur qui prédit l’issue de la nouvelle observation.|

Dans la dernière partie de cette procédure pas à pas, vous créez ces procédures stockées supplémentaires :
    
|**Nom de fichier de script SQL**|**Fonction**|
|------|------|
|SerializePlots.sql|Crée une procédure stockée pour l’exploration des données. Cette procédure stockée crée un graphique à l’aide de Python et puis sérialiser les objets de graphique.|
|TrainTipPredictionModelSciKitPy.sql|Crée une procédure stockée qui effectue l’apprentissage d’un modèle de régression logistique (scikit-en savoir plus). Le modèle prédit la valeur de la colonne pourboires et est formé à l’aide d’un sélectionné de façon aléatoire de 60 % des données. La sortie de la procédure stockée représente le modèle formé, qui est enregistré dans la table nyc_taxi_models.|
|TrainTipPredictionModelRxPy.sql|Crée une procédure stockée qui effectue l’apprentissage d’un modèle de régression logistique (revoscalepy). Le modèle prédit la valeur de la colonne pourboires et est formé à l’aide d’un sélectionné de façon aléatoire de 60 % des données. La sortie de la procédure stockée représente le modèle formé, qui est enregistré dans la table nyc_taxi_models.|

## <a name="next-step"></a>Étape suivante

[Étape 3 : Explorer et visualiser les données](sqldev-py3-explore-and-visualize-the-data.md)

## <a name="previous-step"></a>Étape précédente

[Étape 1 : Télécharger les exemples de données](sqldev-py1-download-the-sample-data.md)

