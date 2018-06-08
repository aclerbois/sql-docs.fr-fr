---
title: Présentation de l’architecture de SQL Server Machine Learning Services | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 4abe5e508f1e183e1e6cb5012b9541fe3723b77a
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="architecture-overview-for-sql-server-machine-learning-services"></a>Présentation de l’architecture de SQL Server Machine Learning Services 
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les objectifs de l’infrastructure d’extensibilité qui prend en charge l’exécution du script Python et R dans SQL Server.

Il fournit également une vue d’ensemble de la façon dont l’architecture est conçue pour répondre à ces objectifs, comment R Python pris en charge et est exécutées par SQL Server et les avantages de l’intégration.

En général, l’infrastructure d’extensibilité est presque identique pour R et Python, avec quelques différences mineures dans les détails des lanceurs sont appelées, options de configuration et ainsi de suite. Pour plus d’informations sur l’implémentation pour une langue spécifique, consultez les articles suivants :

- [Présentation de l’architecture de SQL Server R Services](r/architecture-overview-sql-server-r.md)
- [Présentation de l’architecture pour Python dans SQL Server](python/architecture-overview-sql-server-python.md)


## <a name="background"></a>Arrière-plan

Dans SQL Server 2016, de nombreuses modifications ont été introduites pour le moteur de base de données pour prendre en charge l’exécution de scripts R à l’aide de SQL Server. Dans SQL Server 2017, cette infrastructure sous-jacente a été améliorée pour prendre en charge le langage Python.

L’objectif de l’infrastructure d’extensibilité était pour créer une interface mieux entre SQL Server et les langues de science des données tels que R et Python, à la fois pour réduire la friction qui se produit lorsque les solutions de science des données sont déplacées en production et pour protéger les données qui peuvent être exposées au cours du processus de développement de science des données.

En exécutant un langage de script fiable au sein d’une infrastructure sécurisée géré par SQL Server, le développeur de base de données peut maintenir la sécurité tout en permettant aux chercheurs de données à utiliser les données d’entreprise.

  ![Objectifs de l’intégration avec SQL Server](media/ml-service-value-add.png "Machine Learning Services à valeur ajoutée")

- Les utilisateurs actuels de R ou Python doivent être en mesure de leur code de port et de l’exécuter dans SQL Server avec des modifications mineures.
- Le modèle de sécurité des données dans SQL Server est étendu aux données utilisées par les langages de script externe. En d’autres termes, quelqu'un qui exécute le script R ou Python ne doit pas être en mesure d’utiliser toutes les données qui ne sont pas accessible par l’utilisateur dans une requête SQL.
- L’administrateur de base de données doit être en mesure de gérer les ressources utilisées par les scripts externes, gérer les utilisateurs, gérer et surveiller des bibliothèques de code externe.
- Le système doit prendre en charge les solutions entièrement basées sur des distributions d’ouvrir la source de R et Python, mais utilisez des composants développés par Microsoft pour fournir la sécurité et des performances supérieures.

## <a name="architecture-core-concepts"></a>Concepts d’architecture de base

Pour atteindre ces objectifs, l’architecture de SQL Server 2016 R Services et les Services SQL Server 2017 Machine Learning pour R et Python est basée sur les concepts principaux :

+ **Architecture de plusieurs processus**

  R et Python est des langages d’open source avec prise en charge de la Communauté riche et enthousiaste. Par conséquent, il est important de conserver une interopérabilité complète avec R open source et Python.

  Distributions d’ouvrir la source de R et Python sont installées avec SQL Server sous licence et peuvent fonctionner de façon indépendante à partir de SQL Server si nécessaire.

   En outre, Microsoft fournit un ensemble de bibliothèques propriétaires qui offrent une intégration avec SQL Server, notamment la conversion de données, la compression et son optimisation visant à chaque langue prise en charge.

+ **Sécurité**

   Une meilleure sécurité signifie prend en charge pour l’authentification Windows intégrée et les connexions SQL basée sur un mot de passe, gestion et sécurité des informations d’identification, vis-à-vis de SQL Server pour la protection des données et l’utilisation de SQL Server approuvée Launchpad pour gérer l’exécution du script externe et sécuriser les données utilisées dans les scripts.

+ **Performances et évolutivité**

  L’intégration avec SQL Server est essentielle pour améliorer l’utilité de R et Python dans l’entreprise. N’importe quel script R ou Python peut être exécuté en appelant une procédure stockée, et les résultats sont retournés en tant que résultats tabulaires directement à SQL Server, ce qui permet de générer ou consommer machine learning à partir de n’importe quelle application qui peut envoyer une requête SQL et traiter les résultats.

  Optimisation des performances s’appuie sur deux aspects tout aussi puissantes de la plateforme : la gouvernance des ressources et parallèle de traitement à l’aide de SQL Server et distributed computing fourni par les algorithmes dans **RevoScaleR** et **revoscalepy**.

## <a name="solution-development-and-deployment"></a>Développement de solutions et de déploiement

En plus de ces objectifs fondamentaux pour la plateforme d’extensibilité, les services d’apprentissage machine dans SQL Server sont conçus pour fournir une parfaite intégration avec le moteur de base de données et la pile de BI de ces avantages :

+ Gestion des ressources et de performances par le biais des outils de surveillance traditionnels
+ Facile à utiliser les données de Python et R en suites de tests BI ou de toute application qui peut consommer des résultats de la requête SQL
+ Quantité barrière inférieure pour le développement d’entreprise des solutions d’apprentissage machine

Nous allons voir comment il fonctionne dans la pratique.

  ![Processus de développement de solution ML](media/ml-solution-development-process.png "développer et déployer à l’aide des Services de Machine Learning")

1. Données sont conservées dans les limites de conformité et de l’utilisation des données permettre être gérée et surveillée par SQL Server. Pendant ce temps, l’administrateur possède un contrôle total sur les personnes autorisées à installer des packages ou exécuter des scripts sur le serveur. Si vous le souhaitez, l’administrateur peut également déléguer des autorisations sur un niveau de base de données à des chercheurs de données ou des gestionnaires.
2. Les chercheurs de données peuvent générer et tester des solutions dans leurs environnements R ou Python préférés, déconnectés du serveur.
3. Le développeur SQL peut utiliser des outils familiers tels que Management Studio ou Visual Studio pour intégrer le code R ou Python avec SQL Server. L’intégration étroite signifie que le développeur expérimenté peut choisir le meilleur outil pour optimiser chaque tâche. Par exemple, vous pouvez utiliser SQL pour certaines tâches ingénierie de fonctionnalité et de R pour d’autres. Vous pouvez incorporer le script Python dans une tâche d’Integration Services pour effectuer le texte sophistiquées analytique.
4. Testé et prêt à déployer des solutions peuvent être optimisées à l’aide des technologies SQL Server, tels que des index columnstore, pour de meilleures performances. Nouvelles fonctionnalités vous permettent de lot-train nombreux modèles petite en parallèle sur un jeu de données ou de score des millions de lignes à l’aide de code SQL natif optimisé pour les tâches d’apprentissage automatique.
5. Prêt à soulevez ? Vous pouvez exposer facilement vos solutions prédictives de la pile de BI ou des applications externes à l’aide de procédures stockées.

## <a name="related-products"></a>Produits connexes

Ne savez pas quelle solution d’apprentissage répond à vos besoins ? En plus de l’analytique incorporé dans SQL Server 2016 et SQL Server 2017, Microsoft fournit suivantes d’apprentissage plates-formes et des services :

+ [Microsoft R Server et serveur de Machine Learning](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)

  Un environnement multi-plateforme de développement, de distribution et de gérer des tâches d’apprentissage machine
+ [Machine virtuelle pour la science des données](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview)

  Tous les outils que vous avez besoin pour l’apprentissage automatique, préinstallé. Utiliser des ordinateurs portables Notebook, Python ou R.
  
  Essayer le nouveau [version d’évaluation Windows 2016](http://aka.ms/dsvm/win2016), notamment les versions GPU des infrastructures de formation approfondie populaires tels que CNTK et mxNet, ainsi que la prise en charge pour les conteneurs Windows !

+ [Services cognitifs Azure](https://azure.microsoft.com/services/cognitive-services/)

  Une variété de services de cloud computing pour l’ajout d’AI et ML dans vos applications, y compris l’indexation de langage naturel de reconnaissance faciale vidéo, détection émotion, analytique de texte, de l’ordinateur traduction et bien plus
+ [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/)

  Une interface de glisser-déplacer sur le cloud pour concevoir le flux de travail machine learning, associé à la capacité à automatiser et intégrer des applications via les services web et de PowerShell

## <a name="see-also"></a>Voir aussi

[Comparer des produits Machine Learning Server et Microsoft R](https://docs.microsoft.com/machine-learning-server/what-is-r-server-interoperability)
