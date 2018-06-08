---
title: Présentation approfondie du science des données avec SQL Server à l’aide de RevoScaleR packages | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 5c2596c881d48d2f2629b4363749e7d1c45b3489
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="data-science-deep-dive-using-the-revoscaler-packages-with-sql-server"></a>Présentation approfondie de science des données : utilisation de packages RevoScaleR avec SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Ce didacticiel montre comment utiliser les packages R améliorés fournis dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] pour travailler avec des données SQL Server et créer des solutions R évolutives, en utilisant le serveur en tant qu’un contexte de calcul pour l’analytique des données hautes performances.

Vous apprenez à créer un contexte de calcul à distance, de déplacer des données entre les contextes de calcul locaux et distants et exécutez du code R sur un serveur SQL distant. Vous apprendrez également comment analyser et tracer des données à la fois localement et sur le serveur distant et comment créer et déployer des modèles.

> [!NOTE]
> 
> Ce didacticiel fonctionne plus précisément les données SQL Server sur Windows et utilise des contextes de calcul de la base de données. Si vous souhaitez utiliser R dans d’autres contextes, tels que Teradata, Linux ou Hadoop, consultez les didacticiels de Microsoft R Server : 
> + [Utiliser un serveur R avec sparklyr](https://docs.microsoft.com/machine-learning-server/r/tutorial-sparklyr-revoscaler)
> + [Explore R and ScaleR in 25 functions](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler) (Découvrir 25 fonctions de R et ScaleR)
> + [Prise en main ScaleR sur Hadoop MapReduce](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-hadoop)

## <a name="overview"></a>Vue d'ensemble

Pour bénéficier de la puissance de traitement et de flexibilité du package RevoScaleR, dans ce didacticiel vous déplacer des données et échange de contextes de calcul fréquemment. Pour illustrer cela, voici quelques-unes des tâches dans ce didacticiel :

+ Les données obtenues initialement proviennent de fichiers CSV ou XDF. Vous importez les données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’utilisation des fonctions dans le package RevoScaleR.
+ Modèle d’apprentissage et de calcul de score sont effectuée à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contexte de calcul. 
+ Utiliser les fonctions RevoScaleR pour créer de nouveaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables pour enregistrer les résultats du calcul de score.
+ Créer des graphiques à la fois sur le serveur et le contexte de calcul locaux.
+ Former un modèle de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données, en cours d’exécution R le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.
+ Extraire un sous-ensemble de données et les enregistrer dans un fichier XDF pour une réutilisation dans l’analyse sur votre station de travail local.
+ Obtenir de nouvelles données pour calculer les scores, en ouvrant une connexion ODBC à le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données. Calcul de score est effectuée sur la station de travail.
+ Créer une fonction R personnalisée et l’exécuter sur le serveur pour effectuer une simulation de contexte de calcul.

### <a name="article-list-and-time-required"></a>Liste d’articles et le temps requis

Ce didacticiel prend environ 75 minutes, ne pas les paramètres de configuration.

1. [Travailler avec des données de SQL Server à l’aide de R](../../advanced-analytics/tutorials/deepdive-work-with-sql-server-data-using-r.md)
2. [Créer des objets de données SQL Server à l’aide de RxSqlServerData](../../advanced-analytics/tutorials/deepdive-create-sql-server-data-objects-using-rxsqlserverdata.md)
3. [Interroger et modifier les données SQL Server](../../advanced-analytics/tutorials/deepdive-query-and-modify-the-sql-server-data.md)
4. [Définir et utiliser des contextes de calcul](../../advanced-analytics/tutorials/deepdive-define-and-use-compute-contexts.md)
5. [Créer et exécuter des Scripts R](../../advanced-analytics/tutorials/deepdive-create-and-run-r-scripts.md)
6. [Visualiser les données de SQL Server à l’aide de R](../../advanced-analytics/tutorials/deepdive-visualize-sql-server-data-using-r.md)
7. [Créez des modèles R](../../advanced-analytics/tutorials/deepdive-create-models.md)
8. [Score de nouvelles données](../../advanced-analytics/tutorials/deepdive-score-new-data.md)
9. [Transformer des données à l’aide de R](../../advanced-analytics/tutorials/deepdive-transform-data-using-r.md)
10. [Charger des données dans la mémoire à l’aide de rxImport](../../advanced-analytics/tutorials/deepdive-load-data-into-memory-using-rximport.md)
11. [Créer des tables SQL Server à l’aide de rxDataStep](../../advanced-analytics/tutorials/deepdive-create-new-sql-server-table-using-rxdatastep.md)
12. [Effectuer une analyse de segmentation à l’aide de rxDataStep](../../advanced-analytics/tutorials/deepdive-perform-chunking-analysis-using-rxdatastep.md)
13. [Analyser des données dans un contexte de calcul local](../../advanced-analytics/tutorials/deepdive-analyze-data-in-local-compute-context.md)
14. [Déplacer les données à partir de SQL Server à l’aide de fichiers XDF](../../advanced-analytics/tutorials/deepdive-move-data-between-sql-server-and-xdf-file.md)
15. [Créer une simulation simple](../../advanced-analytics/tutorials/deepdive-create-a-simple-simulation.md)

### <a name="target-audience"></a>Public visé

Ce didacticiel est destiné à des chercheurs de données ou pour les personnes qui sont déjà familiarisé avec R et tâches courantes relatives aux données telles que des résumés et la création du modèle.  Toutefois, tout le code est fourni, donc même si vous ne connaissez pas R, vous pouvez exécuter le code et suivre la procédure, en supposant que vous avez les environnements de client et le serveur requis.

Vous devez également être familiarisé avec [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe et savoir comment accéder à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de données à l’aide des outils tels que ceux-ci :

+ [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 
+ Outils de base de données dans Visual Studio 
+ La version gratuite [extension mssql pour le Code de Visual Studio](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode).
  
> [!TIP]
> Enregistrez votre espace de travail R entre les leçons pour pouvoir facilement reprendre là où vous en étiez.

### <a name="prerequisites"></a>Configuration requise

- **SQL Server avec prise en charge pour R**
  
    Installer SQL Server 2016 et activer R Services (de-de base de données). Ou, installez SQL Server 2017 et activer les Services de Machine Learning et choisissez le langage R.
  
-  **Autorisations de base de données**
  
    Pour exécuter les requêtes utilisées pour former le modèle, vous devez disposer des privilèges de **db_datareader** sur la base de données où sont stockées les données. Pour exécuter R, l’utilisateur doit disposer de l’autorisation EXECUTE ANY EXTERNAL SCRIPT.

-   **Ordinateur de développement de science des données**
  
    Vous devez installer les packages de RevoScaleR et les fournisseurs associés sur l’ordinateur utilisé en tant que l’environnement de développement R. La façon la plus simple consiste à installer le Client Microsoft R, Microsoft R Server (autonome) ou Machine Learning Server (autonome). 
      
    > [!NOTE] 
    > Les autres versions de Revolution R Enterprise ou Revolution R Open ne sont pas prises en charge.
    > 
    > Une distribution open source r ne peut pas être utilisée dans ce didacticiel, car seules les fonctions RevoScaleR peuvent utiliser les contextes de calcul à distance.
  
-   **Packages R supplémentaires**
  
    Dans ce didacticiel, vous installez les packages suivants : **dplyr**, **ggplot2**, **ggthemes**, **reshape2**, et **e1071** . Des instructions sont fournies dans le didacticiel.
  
    Tous les packages doivent être installés dans deux emplacements : sur l’ordinateur que vous utilisez pour le développement de solutions de R et sur l’ordinateur SQL Server où les scripts R sont exécutées. Si vous n’avez pas l’autorisation d’installer des packages sur l’ordinateur serveur, demandez à un administrateur. 
    
    **N’installez pas les packages dans une bibliothèque d’utilisateur.** Les packages doivent être installés dans la bibliothèque de package de R est utilisée par l’instance de SQL Server.

Pour plus d’informations, consultez [conditions préalables pour les procédures pas à pas de données scientifiques](../../advanced-analytics/tutorials/walkthrough-prerequisites-for-data-science-walkthroughs.md).

## <a name="next-step"></a>Étape suivante

[Travailler avec des données de SQL Server à l’aide de R](../../advanced-analytics/tutorials/deepdive-work-with-sql-server-data-using-r.md)

