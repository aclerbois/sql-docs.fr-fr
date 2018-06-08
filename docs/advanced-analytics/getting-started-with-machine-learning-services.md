---
title: Prise en main d’apprentissage dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 94f95b1a2ddaee32896d3a370338e53aded3ab99
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="getting-started-with-machine-learning-in-sql-server"></a>Bien démarrer avec Machine Learning dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Microsoft fournit un ensemble intégré et évolutif des solutions d’apprentissage pour à la fois localement et le cloud :

+ **Intégré**, car vous pouvez exécuter R ou Python dans SQL Server. Cela vous permet de facilement processus d’entreprise de fusion pour ETL et de création de rapports avec les données des tâches de science telles que la fonctionnalité d’ingénierie, de la création du modèle et le score.
+ **Évolutive**, car le chercheur de données permettre développer et tester une solution sur un ordinateur portable et puis le déployer sur plusieurs plateformes pour distribuées ou le traitement parallèle des opérations de clé comme modèle d’apprentissage et de calcul de score. Plateformes prises en charge incluent SQL Server sur Windows, Hadoop et Spark.

Cet article fournit des liens vers des ressources pour chaque produit dans la plateforme Microsoft Machine Learning.

## <a name="machine-learning-in-sql-server"></a>Apprentissage automatique dans SQL Server

+ SQL Server 2017

  À partir de SQL Server 2017, vous pouvez maintenant utiliser du code Python dans SQL Server. Afin de refléter la prise en charge élargie des solutions dans plusieurs langues (et d’autres viendront !) et le nom a été remplacé par [!INCLUDE[rsql-productnamenew-md](../includes/rsql-productnamenew-md.md)]. Maintenant vous pouvez automatiser les tâches d’apprentissage automatique à l’aide des outils SQL pour exécuter le code R ou Python. Sinon, utilisez l’ordinateur SQL Server en tant que le _contexte de calcul_ pour les travaux lancés à partir d’un environnement de développement à distance.

    + [Présentation de l’architecture pour Python dans SQL Server](../advanced-analytics/python/architecture-overview-sql-server-python.md)
    + [Installer les Services SQL Server 2017 Machine Learning](install/sql-machine-learning-services-windows-install.md)

+ SQL Server 2016

  SQL Server 2016 prend en charge l’exécution du code R dans SQL Server à l’aide de procédures stockées. Cela facilite l’automatisation des tâches d’apprentissage automatique à l’aide des outils SQL. Ou bien, vous pouvez exécuter le code R à partir d’un ordinateur portable à distance ou d’un environnement de développement R, lors de l’utilisation de l’ordinateur SQL Server en tant que le _contexte de calcul_.

  Cette intégration offre une sécurité pour vos données et vous permet de gérer et d’équilibrer les ressources utilisées par R.

    + [Prise en main en utilisant SQL Server R Services](r/getting-started-with-sql-server-r-services.md)
    + [Installer SQL Server 2016 R Services](install/sql-r-services-windows-install.md)

## <a name="microsoft-machine-learning-server-microsoft-r-server"></a>Microsoft Machine Learning Microsoft R (serveur)

L’option d’installation [!INCLUDE[rsql-platformnew-md](../includes/rsql-platformnew-md.md)] est fourni dans 2017 du serveur SQL pour prendre en charge les clients d’entreprise qui souhaitent exécuter distribuée et évolutive d’apprentissage travaux, mais qui n’ont besoin de l’intégration avec le moteur de base de données SQL Server, telles que l’utilisation de compute SQL contextes.

Dans SQL Server 2016, utilisez l’option pour installer [!INCLUDE[rsql-platform-md](../includes/rsql-platformnew-md.md)].
  
  + [Bienvenue sur le serveur d’apprentissage](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
  
Vous pouvez également installer [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)] ou [!INCLUDE[rsql-platformnew-md](../includes/rsql-platformnew-md.md)] via les programmes d’installation spécifique à la plateforme :

  + [Installer sur Windows](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install)
  + [Installer sur Linux](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-linux-install)
  + [Installer sur Hadoop](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-hadoop-install)

> [!IMPORTANT]
> Si vous souhaitez exécuter Python à l’aide de R Server, veillez à installer la version la plus récente, [!INCLUDE[rsql-platformnew-md](../includes/rsql-platformnew-md.md)], qui est disponible uniquement via [!INCLUDE[sscurrent-md](../includes/sscurrent-md.md)] le programme d’installation :
> 
>    + [Installer SQL Server 2017 Machine Learning Server (autonome)](install/sql-machine-learning-standalone-windows-install.md) ou [installer R Server (autonome) de SQL Server 2016](install/sql-r-standalone-windows-install.md).

## <a name="related-products"></a>Produits connexes

+ [Configurer un client de science des données](../advanced-analytics/r/set-up-a-data-science-client.md)

  Si vous avez déjà installé un des produits serveur d’apprentissage, cet article fournit des informations sur la façon de configurer un ordinateur distinct pour le développement et le test de solutions, notamment des outils et bibliothèques requises.

+ [Machine virtuelle pour la science des données](../advanced-analytics/r/provision-the-r-server-only-sql-server-2016-enterprise-vm-on-azure.md)

  Lancer votre entrée en apprentissage automatique en obtenant cette solution à partir d’Azure Marketplace d’apprentissage complete. La machine virtuelle de science des données (souvent abrégée « DSVM ») inclut SQL Server, Microsoft Machine Learning Server et tous les outils de développement.
  
  La version la plus récente de la Machine virtuelle de science des données (DSVM) s’exécute sur la version d’évaluation Windows 2016, pour fournir l’aspect personnalisable de nettoyage de Windows 10. Elle est préconfigurée avec les pilotes, CUDA Toolkit 8.0 et la bibliothèque de cuDNN NVIDIA pour les charges de travail GPU NVIDIA.

## <a name="resources-for-learning"></a>Ressources d’apprentissage

+ [Didacticiels d’apprentissage machine](../advanced-analytics/tutorials/machine-learning-services-tutorials.md)

  Démarrez ici pour rechercher une liste de toutes les ressources pour en savoir plus sur les solutions d’apprentissage machine à l’aide de SQL Server 2016 et SQL Server 2017.

### <a name="r-tutorials"></a>Didacticiels de R

+ [Didacticiels de SQL Server R](../advanced-analytics/tutorials/sql-server-r-tutorials.md)

   Découvrez comment exécuter R dans SQL Server, créer et utiliser les contextes de calcul à distance ou effectuer des simulations dans R à l’aide de SQL Server.
   
   Inclut des didacticiels de « tout le code fourni » afin que vous pouvez exécuter une solution R à partir de SQL Server Management Studio, sans jamais ouvrir un IDE R !

+ [Explorer R et ScaleR 25 fonctions courtes](https://docs.microsoft.com/r-server/r/tutorial-r-to-revoscaler)

   Vous débutez avec R ? Vous vous demandez la manière dont Microsoft R (ou RevoScaleR) compare à R standard ? Pour R Server et serveur de Machine Learning, consultez ces rapide-démarre.

### <a name="python-tutorials"></a>Didacticiels de Python

+ [Didacticiels de SQL Server Python](../advanced-analytics/tutorials/sql-server-r-tutorials.md)

  En savoir plus sur l’exécution de Python [!INCLUDE[ssnoversion](../includes/ssnoversion.md)]. Générer un modèle à l’aide de Python et l’utiliser pour évaluer des données de SQL Server.

   Une solution de bout en bout pour les développeurs SQL fournit tout le code que vous devez exécuter Python à partir de SQL Server Management Studio.


### <a name="product-samples-with-code"></a>Exemples de produit avec le code

Ces solutions à partir de l’équipe de développement de SQL Server exécutent dans Python ou R et illustrent des scénarios courants pour l’intégration d’apprentissage automatique avec les applications métiers.

+ [Créer une application intelligente avec SQL Server et R](https://microsoft.github.io/sql-ml-tutorials/R/rentalprediction)

+ [Créer une application intelligente avec SQL Server et Python](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/)

### <a name="data-science-solution-templates"></a>Modèles de solution de science des données

Les modèles de solution à partir de l’équipe Microsoft pour la science des données représentent des solutions exemple complet adaptées aux scénarios ou des secteurs spécifiques. Elles sont destinées à servir comme blocs de construction pour vous aider à implémenter une solution rapide, ou pour illustrer les meilleures pratiques. Chaque modèle inclut des exemples de données et de code personnalisables.

+ [Modèles de solution](../advanced-analytics/tutorials/data-science-scenarios-and-solution-templates.md)

## <a name="next-steps"></a>Étapes suivantes

[Prise en main Machine Learning Services SQL Server](../advanced-analytics/r/getting-started-with-sql-server-r-services.md)

[Prise en main Server d’apprentissage Microsoft](../advanced-analytics/r/getting-started-with-microsoft-r-server-standalone.md)
