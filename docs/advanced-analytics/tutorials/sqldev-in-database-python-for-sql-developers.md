---
title: Dans base de données analytique de Python pour les développeurs SQL | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: a8b41b1c0c48cb0cf5c9be5db8b7d59007bc22c1
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="in-database-python-analytics-for-sql-developers"></a>Analytique Python de la base de données pour les développeurs SQL
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L’objectif de cette procédure pas à pas est de fournir des programmeurs SQL avec une expérience pratique de créer une solution à l’aide de Python qui s’exécute dans SQL Server d’apprentissage. Dans cette procédure pas à pas, vous allez apprendre à ajouter le code Python aux procédures stockées et exécuter des procédures stockées pour générer et de prédire à partir de modèles.

> [!NOTE]
> Préférez R ? Consultez [ce didacticiel](sqldev-in-database-r-for-sql-developers.md), qui fournit une solution similaire, mais utilise R et peut être exécuté dans SQL Server 2016 ou SQL Server 2017.

## <a name="overview"></a>Vue d'ensemble

Le processus de création d’une solution d’apprentissage est complexe qui peut impliquer plusieurs outils et la coordination des experts du sujet via plusieurs phases :

+ obtention et nettoyage des données
+ Explorer les données et création de fonctionnalités utiles pour la modélisation
+ apprentissage et le modèle de paramétrage
+ déploiement en production

**L’objectif de cette procédure pas à pas est de créer et déployer une solution à l’aide de SQL Server.**

Les données sont dans le jeu de données NYC Taxi bien connu. Pour effectuer cette procédure pas à pas simple et rapide, les données sont échantillonnées. Vous allez créer un modèle de classification binaire qui prédit si un voyage particulier est susceptible d’obtenir un Conseil ou non, en fonction des colonnes, telles que l’heure du jour, distance et l’emplacement d’extraction.

Toutes les tâches peuvent être effectuées à l’aide [!INCLUDE[tsql](../../includes/tsql-md.md)] des procédures stockées dans l’environnement familier de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]

- [Étape 1 : Télécharger les exemples de données](sqldev-py1-download-the-sample-data.md)

    Téléchargez l’exemple de dataset et tous les fichiers de script sur un ordinateur local.

- [Étape 2 : Importer des données dans SQL Server à l’aide de PowerShell](sqldev-py2-import-data-to-sql-server-using-powershell.md)

    Exécuter un script PowerShell qui crée une base de données et une table sur l’instance spécifiée et charge les données d’exemple à la table.

- [Étape 3 : Explorer et visualiser les données à l’aide de Python](sqldev-py3-explore-and-visualize-the-data.md)

    En appelant Python à partir de la visualisation et l’exploration de données de base [!INCLUDE[tsql](../../includes/tsql-md.md)] des procédures stockées.

- [Étape 4 : Créer des fonctionnalités de données à l’aide de Python dans T-SQL](sqldev-py5-train-and-save-a-model-using-t-sql.md)

    Créez des caractéristiques de données à l’aide de fonctions personnalisées.
  
- [Étape 5 : L’apprentissage et enregistrer un modèle de Python à l’aide de T-SQL](sqldev-py5-train-and-save-a-model-using-t-sql.md)

    Générez et enregistrez le modèle d’apprentissage automatique, à l’aide de Python dans les procédures stockées.
  
    Cette procédure pas à pas montre comment effectuer une tâche de classification binaire ; Vous pouvez également utiliser les données pour générer des modèles de régression ou de classification multiclasse.

  
-  [Étape 6 : Mettre le modèle de Python](sqldev-py6-operationalize-the-model.md)

    Une fois que le modèle a été enregistré dans la base de données, appelez le modèle pour la prédiction à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="requirements"></a>Spécifications

### <a name="prerequisites"></a>Configuration requise

+ Installez une instance de SQL Server 2017 avec Machine Learning Services et Python activée. Pour plus d’informations, consultez [installer SQL Server 2017 Machine Learning Services (de-de base de données)](../install/sql-machine-learning-services-windows-install.md).
+ La connexion que vous utilisez pour cette procédure pas à pas doit avoir les autorisations pour créer des bases de données et autres objets, pour charger des données, sélectionner des données et exécuter des procédures stockées.

### <a name="experience-level"></a>Niveau d’expérience

Vous devez être familiarisé avec les opérations fondamentales de la base de données, telles que la création de bases de données et tables, l’importation de données dans des tables et créer des requêtes SQL.

Un programmeur SQL expérimenté doit être en mesure d’effectuer cette procédure pas à pas à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou en exécutant les scripts PowerShell fournis.

Python : Connaissances de base sont utile, mais n’est pas obligatoire. Tout le code Python est fourni.

Connaissance de PowerShell est utile.

### <a name="tools"></a>Outils

Pour ce didacticiel, nous supposons que vous avez atteint la phase de déploiement. Vous disposent de nettoyer les données, terminez le code T-SQL pour fonction d’ingénierie et l’utilisation de code Python. Par conséquent, vous pouvez effectuer ce didacticiel à l’aide de SQL Server Management Studio, ou tout autre outil qui prend en charge les instructions SQL en cours d’exécution.

+ [Vue d’ensemble des outils SQL Server](https://docs.microsoft.com/sql/tools/overview-sql-tools) 

Si vous souhaitez développer et tester votre propre code Python ou que vous déboguez une solution de Python, nous recommandons d’utiliser un environnement de développement dédié :

+ **Visual Studio 2017** prend en charge les deux R et [Python](https://blogs.msdn.microsoft.com/visualstudio/2017/05/12/a-lap-around-python-in-visual-studio-2017/). Nous vous recommandons du [charge de travail de science des données](https://blogs.msdn.microsoft.com/visualstudio/2016/11/18/data-science-workloads-in-visual-studio-2017-rc/), qui prend également en charge R et F #.
+ Si vous disposez d’une version antérieure de Visual Studio, [Extensions Python pour Visual Studio](https://docs.microsoft.com/visualstudio/python/python-in-visual-studio) , ce qui la rend facile à gérer plusieurs environnements Python.
+ PyCharm est un IDE populaires parmi les développeurs Python.

    > [!NOTE]
    > En règle générale, évitez d’écrire ou de tester le nouveau code Python dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Si le code que vous incorporez dans une procédure stockée a des problèmes, les informations retournées par la procédure stockée sont généralement insuffisant pour comprendre la cause de l’erreur.

Utilisez les ressources suivantes pour vous aider à planifier et exécuter un projet d’apprentissage réussie :

+ [Processus de science des données équipe](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview)

### <a name="estimated-time-required"></a>Durée estimée

|Étape| Temps (h)|
|----|----|
|Télécharger les exemples de données| 0:15|
|Importer des données vers SQL Server à l’aide de PowerShell|0:15|
|Explorer et visualiser les données|0:20|
|Créer des fonctionnalités de données à l’aide de T-SQL|0:30|
|L’apprentissage et enregistrez un modèle à l’aide de T-SQL|0:15|
|Rendez le modèle opérationnel|0:40|

## <a name="get-started"></a>Prise en main

  [Étape 1 : Télécharger les exemples de données](sqldev-py1-download-the-sample-data.md)
