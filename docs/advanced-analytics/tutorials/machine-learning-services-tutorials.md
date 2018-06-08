---
title: Didacticiels de Services de formation l’ordinateur SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 604bbe9eb2a28c32b862174ce5da9cc5092e0c64
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="tutorials-for-sql-server-machine-learning-services"></a>Didacticiels pour SQL Server Machine Learning Services
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fournit une liste complète des didacticiels, des démonstrations et des exemples d’applications qui utilisent les fonctionnalités d’apprentissage machine dans SQL Server 2016 ou SQL Server 2017. Démarrez ici pour savoir comment exécuter R ou Python à partir de T-SQL, comment utiliser les contextes de calcul locaux et distants et comment optimiser votre code R et Python pour un environnement de production SQL.

## <a name="start-here"></a>Démarrez ici

+ [Didacticiels Python](../tutorials/sql-server-python-tutorials.md)

+ [Didacticiels de R](../tutorials/sql-server-r-tutorials.md)

Pour plus d’informations sur les exigences et obtenir la configuration, consultez [conditions préalables](#bkmk_prerequisites).

## <a name="samples-and-solutions"></a>Exemples et des solutions

+ [Exemples](#bkmk_samples) 

    Ces scénarios réels de l’équipe de développement SQL Server montrent comment incorporer un apprentissage automatique dans les applications. Tous les exemples incluent le code que vous pouvez télécharger, modifier et utiliser en production.

+ [Solutions](#bkmk_solutions) 

    Modèles à partir de l’équipe de science des données de Microsoft sont personnalisables, de démarrage rapide avec un apprentissage. Chaque solution est adaptée à un problème de tâche ou le secteur d’activité spécifique. La plupart des solutions est conçue pour s’exécuter dans SQL Server ou dans un environnement de cloud comme Azure Machine Learning. Autres solutions peuvent exécuter sur Linux ou dans les clusters Spark ou Hadoop, à l’aide de Microsoft R Server ou serveur de Machine Learning.

### <a name ="bkmk_samples"></a>Exemples de produit de SQL Server

Ces exemples et les démonstrations fournies par l’équipe de développement SQL Server et serveur R mettez en surbrillance les façons dont vous pouvez utiliser analytique incorporées dans les applications réelles.

+ [Exécuter le client clusters à l’aide de R et SQL Server](https://microsoft.github.io/sql-ml-tutorials/R/customerclustering/)

  Utiliser l’apprentissage aux clients de segment basé sur les données de ventes. Cet exemple utilise l’algorithme rxKmeans évolutives de Microsoft R pour générer le modèle de clustering. 
  
  S’applique à : SQL Server 2016 ou 2017

+ [NOUVEAU ! Exécuter le client clusters à l’aide de Python et SQL Server](https://microsoft.github.io/sql-ml-tutorials/python/customerclustering/)

    Découvrez comment utiliser l’algorithme de Kmeans pour effectuer de clustering non supervisé de clients. Cet exemple utilise la Python langue de base de données.
    
    S’applique à : SQL Server 2017

+ [Créer un modèle prédictif à l’aide de R et SQL Server](https://microsoft.github.io/sql-ml-tutorials/R/rentalprediction)

  Découvrez comment une entreprise de location ski peut utiliser d’apprentissage prévoir les futures Locations, ce qui vous permet du plan d’entreprise et le personnel pour répondre à la demande. Cet exemple utilise les algorithmes Microsoft pour générer des modèles d’arbres de décision et de régression logistique. 
  
  S’applique à : SQL Server 2016 ou 2017

+ [Créer un modèle prédictif à l’aide de Python et SQL Server](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/)

   Générez l’application d’analyse de location ski à l’aide de Python, pour faciliter la planification de la demande future. Cet exemple utilise la nouvelle bibliothèque de Python, **revoscalepy**, pour créer un modèle de régression linéaire.
   
   S’applique à : SQL Server 2017

+ [Comment utiliser un Tableau avec les Services de SQL Server Machine Learning](https://blogs.msdn.microsoft.com/mlserver/2017/12/14/how-to-use-tableau-with-sql-server-machine-learning-services-with-r-and-python/)

    Analyser des médias sociaux et créer des graphiques de Tableau, à l’aide de SQL Server et R.

### <a name="bkmk_solutions"></a>Modèles de solution

L’équipe de science des données Microsoft a fourni des modèles de solution qui peuvent être utilisés pour lancer des solutions pour les scénarios courants. Tout le code est fourni, ainsi que des instructions sur la façon d’effectuer l’apprentissage et de déployer un modèle pour calculer les scores à l’aide de procédures stockées SQL Server.

+ [Détection des fraudes](https://gallery.cortanaanalytics.com/Tutorial/Online-Fraud-Detection-Template-with-SQL-Server-R-Services-1)
+ [Prédiction d’évolution personnalisée](https://gallery.cortanaanalytics.com/Tutorial/Customer-Churn-Prediction-Template-with-SQL-Server-R-Services-1)
+ [Maintenance prédictive](https://gallery.cortanaanalytics.com/Tutorial/Predictive-Maintenance-Template-with-SQL-Server-R-Services-1)
+ [Prédire la durée hôpital de](https://gallery.cortanaintelligence.com/Solution/Predicting-Length-of-Stay-in-Hospitals-1)

Pour plus d’informations, consultez [Machine Learning Templates with SQL Server 2016 R Services](https://blogs.technet.microsoft.com/machinelearning/2016/03/23/machine-learning-templates-with-sql-server-2016-r-services/) (Modèles d’apprentissage automatique avec SQL Server 2016 R Services).

## <a name="more-resources-and-reading"></a>Plus de ressources et de la lecture

+ [Pourquoi nous générez-le ?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/01/10/sql-server-r-services-why-did-we-build-it/)

    Vous voulez savoir ce qui se cache derrière R Services ? Lisez cet article à partir de l’équipe de développement et PM qui explique l’origine et les objectifs de SQL Server R Services.

+ [Didacticiels et exemples de données pour Microsoft R](https://docs.microsoft.com/machine-learning-server/r/tutorial-introduction)

    En savoir plus sur Microsoft R et les fonctions proposées par le package RevoScaleR dans cette collection de didacticiels rapides. Apprenez à écrire du code R une seule fois et de déployer n’importe où, à l’aide de RevoScaleR des sources de données et les contextes de calcul à distance.

+ [Prise en main MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package)

  Découvrez comment utiliser les nouveaux algorithmes dans le package MicrosoftML pour avancées de modélisation et de transformations de données évolutives, optimisées pour plusieurs contextes de calcul.

## <a name="bkmk_Prerequisites"></a>Conditions préalables

Pour exécuter ces didacticiels, vous devez télécharger et installer l’apprentissage automatique, les composants SQL Server comme décrit ici :

+ [Installer SQL Server 2017 Machine Learning Services (de-de base de données)](../install/sql-machine-learning-services-windows-install.md)
+ [Installer SQL Server 2016 R Services (de-de base de données)](../install/sql-r-services-windows-install.md)

Avec SQL Server 2017, vous pouvez installer R ou Python ou les deux. Sinon, le processus d’installation de globale, l’architecture et les exigences sont les mêmes.

Après avoir exécuté le programme d’installation de SQL Server, n’oubliez pas ces étapes importantes :

1. Activez la fonctionnalité d’exécution de script externe en exécutant `sp_configure 'external scripts enabled', 1`. Suivez les instructions pour reconfigurer et redémarrer SQL Server.
2. Assurez-vous que le service Launchpad s’exécute et que les comptes de travail Launchpad peuvent se connecter à l’instance de SQL Server.
3. Passez en revue les autorisations associées aux utilisateurs qui doivent exécuter des scripts R ou Python. Que vous utilisiez les connexions SQL ou des comptes d’utilisateur Windows, l’utilisateur doit avoir l’autorisation d’exécuter des scripts R ou Python et doit être en mesure de se connecter à l’instance. En fonction de ce didacticiel, l’utilisateur peut également nécessiter des autorisations pour écrire des données, de créer des objets de base de données ou de faire un bloc importation de données.

Pour plus d’informations, consultez cet article pour certains problèmes courants de configuration et le programme d’installation : [dépannage Machine Learning Services](../machine-learning-troubleshooting-faq.md)

> [!NOTE]
> Impossible d’exécuter ces didacticiels à l’aide d’un autre outil R ou Python en open source. Votre environnement de développement et de l’ordinateur SQL Server avec un apprentissage doivent avoir les bibliothèques R ou Python fournis par Microsoft, qui prennent en charge l’intégration avec SQL Server et l’utilisation de contextes de calcul à distance.
