---
title: SQL Server Machine Learning Server (autonome) et R Server (autonome) | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 2c416049692f8860e4ba608e58f401ce527b135c
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="sql-server-machine-learning-server-standalone-and-r-server-standalone"></a>SQL Server Machine Learning Server (autonome) et R Server (autonome)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Un serveur autonome est une installation de composants d’apprentissage machine, exprimées en tant que fonctionnalités R et Python, qui s’exécutent indépendamment des instances du moteur de base de données SQL Server. Vous pouvez installer un serveur autonome, sans dépendances sur SQL Server. Car un serveur autonome est indépendant de SQL Server, configuration et administration des tâches et outils sont plus semblables à une version non SQL du serveur Machine Learning, ce qui vous pouvez en savoir plus sur dans [cet article](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server).

L’objectif de serveur autonome machine learning est de fournir un environnement de développement riche, avec le traitement distribué et parallèle de charges de travail R et Python sur petit de grands jeux de données à l’aide de packages propriétaires et les moteurs de calcul installé avec le serveur. Les packages R et Python sur un serveur autonome sont les mêmes que celles fournies dans une installation de SQL Server (de-de base de données), facilitant ainsi la portabilité du code et [changement de contexte de calcul](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-compute-context).

Les développeurs de la raison principale choisissent que serveur autonome machine learning est de dépasser les contraintes de mémoire et de traitement de l’open source R et Python. Les serveurs autonomes peuvent charger et traiter de grandes quantités de données sur plusieurs cœurs et agréger les résultats en une seule sortie consolidée. Les algorithmes et les fonctions sont conçues pour la montée en puissance et l’utilitaire : livraison prédictive analytique, la modélisation statistique, des visualisations de données et algorithmes dans un produit serveur commerciale d’apprentissage pointe conçue et la prise en charge par Microsoft.

En règle générale, nous vous recommandons traiter (autonome) et (dans-base de données) installations mutuellement exclusif afin d’éviter les conflits de ressources, mais si vous disposez de suffisamment de ressources, il n’existe aucune interdiction contre les installer à la fois sur le même ordinateur physique.

Vous ne pouvez avoir qu’un serveur autonome sur l’ordinateur : soit [Machine Learning Server (autonome) de SQL Server 2017](../install/sql-machine-learning-standalone-windows-install.md) ou [(autonome) du serveur SQL Server 2016 R](../install/sql-r-standalone-windows-install.md). Vous devez désinstaller manuellement une version avant d’installer une version différente.

## <a name="components-of-a-standalone-server"></a>Composants d’un serveur autonome

SQL Server 2016 est R uniquement. SQL Server 2017 prend en charge R et Python. Le tableau suivant décrit les fonctionnalités de chaque version.

| Composant |  Description |
|-----------|-------------|
| Packages R | [RevoScaleR](revoscaler-overview.md) est la bibliothèque principale pour R évolutive avec des fonctions de manipulation de données, de transformation, visualzation et analyse.  <br/>[MicrosoftML (R)](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/microsoftml-package) ajoute des algorithmes d’apprentissage automatique pour créer des modèles personnalisés pour l’analyse de texte, l’analyse de l’image et analyse des sentiments. <br/>[mrsdeploy](operationalization-with-mrsdeploy.md) offres web de déploiement du service (dans SQL Server 2017 uniquement). <br/>[olapR](how-to-create-mdx-queries-using-olapr.md) de spécifier des requêtes MDX dans R.|
| Microsoft R Open (MRO) | [MRO](https://mran.microsoft.com/open) est la répartition de Microsoft open source R. Le package et un interpréteur sont inclus. Utilisez toujours la version de MRO fournie dans le programme d’installation. |
| Outils R | Invites de commandes et les fenêtres de console R sont des outils standard dans une distribution de R. Les trouver à \Program files\Microsoft SQL Server\140\R_SERVER\bin\x64. |
| Exemples de R et de scripts |  Les packages R et RevoScaleR Open source incluent les jeux de données intégrées afin que vous pouvez créer et exécuter le script à l’aide de données préalablement installées. Examinez les \Program files\Microsoft SQL Server\140\R_SERVER\library\datasets et \library\RevoScaleR. |
| Packages de Python | [revoscalepy](../python/what-is-revoscalepy.md) est la bibliothèque principale pour Python évolutive avec des fonctions de manipulation de données, de transformation, visualzation et analyse. <br/>[microsoftml (Python)](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) ajoute des algorithmes d’apprentissage automatique pour créer des modèles personnalisés pour l’analyse de texte, l’analyse de l’image et analyse des sentiments.  |
| Outils Python | L’outil de ligne de commande Python intégré est utile pour les tests ad hoc et tâches. Trouvez l’outil à \Program files\Microsoft SQL Server\140\PYTHON_SERVER\python.exe. |
| Anaconda | Anaconda est une distribution open source de Python et packages essentielles. |
| Scripts et les exemples de Python | Comme avec R, Python inclut les jeux de données intégrés et des scripts. Rechercher les données revoscalepy à \Program files\Microsoft SQL Server\140\PYTHON_SERVER\lib\site-packages\revoscalepy\data\sample-data. |
| Dont l’apprentissage des modèles dans R et Python | Dont l’apprentissage des modèles sont pris en charge et utilisable sur un serveur autonome, mais vous ne pouvez pas les installer le programme d’installation de SQL Server. Le programme d’installation de Microsoft Machine Learning Server fournit les modèles, vous pouvez installer gratuitement. Pour plus d’informations, consultez [installation pretrained des modèles d’apprentissage automatique sur SQL Server](install-pretrained-models-sql-server.md). |

## <a name="get-started-step-by-step"></a>Prise en main de pas à pas

Démarrer avec le programme d’installation, attachez les fichiers binaires à votre outil de développement favori et écrire votre script premier.

### <a name="step-1-install-the-software"></a>Étape 1 : Installer le logiciel

Installer l’une de ces versions :

+ [SQL Server 2017 Machine Learning Server (autonome)](../install/sql-machine-learning-standalone-windows-install.md)
+ [SQL Server 2016 R Server (autonome) - R uniquement](../install/sql-r-standalone-windows-install.md)

### <a name="step-2-configure-a-development-tool"></a>Étape 2 : Configurer un outil de développement

Configurez vos outils de développement pour utiliser les fichiers binaires du serveur de Machine Learning. Pour plus d’informations sur les Python, consultez [binaires Python de lien](https://docs.microsoft.com/machine-learning-server/python/quickstart-python-tools). Pour obtenir des instructions sur la façon de se connecter dans R Studio, consultez [à l’aide de différentes Versions de R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R) et pointez l’outil C:\Program Files\Microsoft SQL Server\140\R_SERVER\bin\x64. Vous pouvez également essayer [R Tools pour Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation). 

### <a name="step-3-write-your-first-script"></a>Étape 3 : Écrire votre script premier

Écrire un script R ou Python à l’aide de fonctions à partir de RevoScaleR, revoscalepy et les algorithmes d’apprentissage.
  
  + [Explorer R et RevoScaleR dans les 25 fonctions](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler): démarrer avec les commandes de base R, puis de progression pour les fonctions analytiques distribuables RevoScaleR qui offrent des performances élevées et de mise à l’échelle pour des solutions R. Inclut des versions parallélisables des packages de modélisation R les plus populaires comme le clustering k-means, les arbres de décision, les forêts de décision et les outils de manipulation des données.

  + [Démarrage rapide : Un exemple de classification binaire avec le package de Python microsoftml](https://docs.microsoft.com/machine-learning-server/python/quickstart-binary-classification-with-microsoftml): créer un modèle de classification binaire en utilisant les fonctions de microsoftml et le jeu de données de cancer du sein bien connu.

Choisir le meilleur langage pour la tâche. R est idéal pour effectuer des calculs statistiques qui sont difficiles à implémenter à l’aide de SQL. Pour les opérations basées sur les données, exploiter la puissance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour optimiser les performances. Utilisez le moteur de base de données en mémoire pour effectuer des calculs très rapides sur les colonnes.

### <a name="step-4-operationalize-your-solution"></a>Étape 4 : Mettre votre solution

Les serveurs autonomes peuvent utiliser le [à l’Opérationnalisation](https://docs.microsoft.com//machine-learning-server/what-is-operationalization) fonctionnalités de la non SQL-marque [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server). Vous pouvez configurer un serveur autonome pour une Opérationnalisation rapides, ce qui vous offre les avantages : déployer et héberger votre code, comme les services web, exécutez les diagnostics, la capacité du service web de test.

## <a name="see-also"></a>Voir aussi

 [Ordinateur SQL Server Learning Services (de-de base de données)](sql-server-r-services.md)

