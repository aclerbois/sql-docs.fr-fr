---
title: Didacticiels pour SQL Server R | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e1a6329acbc4d05faa073196b1e5e8d54d78442a
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="sql-server-r-tutorials"></a>Didacticiels de SQL Server R
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fournit une liste des didacticiels et des exemples qui illustrent l’utilisation de R avec SQL Server 2016 ou SQL Server 2017. Grâce à ces exemples et les démonstrations, vous allez apprendre :

+ L’exécution de R à partir de T-SQL
+ Quelles sont les contextes de calcul locaux et distants, et comment vous pouvez exécuter du code R à l’aide de l’ordinateur SQL Server
+ Comment faire pour encapsuler le code R dans une procédure stockée
+ Optimisation du code R pour un environnement de production SQL
+ Des scénarios concrets pour l’incorporation d’apprentissage dans les applications

Pour plus d’informations sur le programme d’installation, consultez [conditions préalables](#bkmk_Prerequisites).

## <a name="bkmk_sqltutorials"></a>Didacticiels de R

Sauf indication contraire, les didacticiels ont été développés pour SQL Server 2016 R Services et doivent fonctionner dans SQL Server 2017 Machine Learning Services sans apporter de modifications importantes.

Tous les didacticiels utilisent beaucoup de fonctionnalités dans le package RevoScaleR pour SQL Server contextes de calcul.

+ [Présentation approfondie de science des données avec R et SQL Server](../tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)

  Découvrez comment utiliser les fonctions dans les packages de RevoScaleR. Déplacement des données entre R et SQL Server et passer des contextes pour l’adapter à une tâche particulière de calcul. Créer des modèles et des graphiques et les déplacer entre votre environnement de développement et le serveur de base de données.

  **Public :** pour chercheurs de données ou les développeurs qui connaissent déjà le langage R, et qui souhaitent en savoir plus sur les packages R améliorés et les fonctions dans Microsoft R par Revolution Analytique.

  **Configuration requise :** des connaissances de base R. Accès à un serveur avec SQL Server R Services ou Machine Learning Services avec R. Pour le programme d’installation, consultez [conditions préalables](#bkmk_Prerequisites).

+ [Analytique de R dans base de données pour les développeurs SQL](../tutorials/sqldev-in-database-r-for-sql-developers.md)

  Générer et déployer une solution complète de R, à l’aide uniquement [!INCLUDE[tsql](../../includes/tsql-md.md)] outils.

  Se concentre sur le déplacement d’une solution en production. Vous découvrirez comment encapsuler du code R dans une procédure stockée, enregistrer un modèle R dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et effectuer des appels paramétrables au modèle R pour la prédiction.

  **Public :** pour les développeurs SQL, les développeurs d’applications ou les professionnels SQL qui prennent en charge des solutions R et souhaitez apprendre à déployer des modèles de R dans SQL Server.

  **Configuration requise :** aucun environnement R n’est nécessaire. Tout le code R est fourni, et vous pouvez générer la solution complète à l’aide uniquement [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et décisionnel familiers et les outils de développement SQL. Toutefois, des connaissances de base de R sont utile.

  Vous devez avoir accès à un serveur SQL Server avec le langage R installé et activé. Pour le programme d’installation, consultez [conditions préalables](#bkmk_Prerequisites).

+ [Démarrage rapide : Utilisation de R dans T-SQL](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md)

  Ce démarrage rapide couvre la syntaxe de base pour l’utilisation de R dans [!INCLUDE[tsql](../../includes/tsql-md.md)].

  Découvrez comment appeler le runtime R à partir de T-SQL, encapsuler des fonctions R dans le code SQL et exécuter une procédure stockée qui enregistre la sortie de R et des modèles R à une table SQL.

  **Public :** pour les personnes qui sont nouveaux pour la fonctionnalité et souhaitez connaître les principes fondamentaux de l’appel de R à partir d’une procédure stockée.

  **Configuration requise :** aucune connaissance de R ou SQL nécessaire. Toutefois, vous avez besoin de SQL Server Management Studio ou sur un autre client qui peut se connecter à une base de données et exécutez le T-SQL. Nous vous recommandons de la version gratuite [extension MSSQL pour le Code de Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql) si vous ne connaissez pas les requêtes T-SQL.

  Vous devez également pouvoir accéder à un serveur avec SQL Server R Services ou Machine Learning Services avec R déjà activé. Pour le programme d’installation, consultez [conditions préalables](#bkmk_Prerequisites).

+ [Procédure pas à pas pour une solution complète de science des données](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)

  Illustre le processus de science des données à partir du début à la fin, l’acquisition des données et les enregistrer dans SQL Server, analyser les données avec R et générer des graphiques.

  Vous allez apprendre comment déplacer des graphiques entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et R et l’ingénierie de fonctionnalité comparaison dans T-SQL avec des fonctions R. Enfin, vous allez apprendre à utiliser le modèle prédictif en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour à la fois calcul du score du lot et le score de ligne unique.

  **Public :** pour les personnes qui sont familiarisés avec R et les outils de développement tels que SQL Server Management Studio.

  **Configuration requise :** vous devez avoir accès à un environnement de développement R et savoir comment exécuter des commandes de R. Utilisation de PowerShell est nécessaire pour télécharger le jeu de données taxi New York City. Vous devez avoir accès à un serveur avec SQL Server R Services ou Machine Learning Services avec R déjà activé. Pour le programme d’installation, consultez [conditions préalables](#bkmk_Prerequisites).

## <a name ="bkmk_samples"></a>Exemples de produits

Ces exemples et démonstrations sont fournies par l’équipe de développement SQL Server pour mettre en évidence les nombreuses méthodes que vous pouvez utiliser l’analytique incorporées dans les applications du monde réel.

+ [Créer un modèle prédictif à l’aide de R et SQL Server](https://microsoft.github.io/sql-ml-tutorials/R/rentalprediction)

  Découvrez comment une entreprise de location ski peut utiliser d’apprentissage prévoir les futures Locations, ce qui vous permet du plan d’entreprise et le personnel pour répondre à la demande.

+ [Exécuter le client clusters à l’aide de R et SQL Server](https://microsoft.github.io/sql-ml-tutorials/R/customerclustering/)

  Utiliser l’apprentissage aux clients de segment basé sur les données de ventes.

## <a name="bkmk_Prerequisites"></a>Conditions préalables

Pour utiliser ces didacticiels et des exemples, vous devez installer un des produits serveur suivants :

+ SQL Server 2016 R Services (de-de base de données)
  
  Prend en charge R. Veillez à installer les fonctionnalités d’apprentissage automatique, puis activer les scripts externes.

+ SQL Server 2017 Machine Learning Services (de-de base de données)
  
  Prend en charge de R ou Python. Vous devez sélectionner l’apprentissage de fonctionnalité et la langue à installer et puis activer les scripts externes.

Après avoir exécuté le programme d’installation de SQL Server, n’oubliez pas ces étapes importantes :

+ Activez la fonctionnalité d’exécution de script externe en exécutant `sp_configure 'external scripts enabled', 1`
+ Redémarrer le serveur
+ Assurez-vous que le service qui appelle le runtime externe dispose des autorisations nécessaires
+ Assurez-vous que votre compte de connexion SQL ou un compte d’utilisateur Windows dispose des autorisations nécessaires pour se connecter au serveur, pour lire des données et à créer des objets de base de données requis par l’exemple

Si vous rencontrez des problèmes, consultez cet article pour certains problèmes courants : [dépannage Machine Learning Services](../machine-learning-troubleshooting-faq.md)
