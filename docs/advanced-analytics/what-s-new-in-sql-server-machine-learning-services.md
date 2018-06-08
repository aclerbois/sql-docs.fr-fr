---
title: Ce que&#39;s nouvelles dans SQL Server Machine Learning Services | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/02/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 092216f7bc1142125156b3658f035154d809c2e9
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34586071"
---
# <a name="whats-new-in-sql-server-machine-learning-services"></a>Nouveautés de SQL Server Machine Learning Services 
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Fonctionnalités de formation d’ordinateur sont ajoutées à SQL Server dans chaque version car nous continuons à développer, étendre et approfondir l’intégration entre la plateforme de données et de la science des données, analytique et supervisé learning que vous souhaitez implémenter sur vos données. 

## <a name="new-in-sql-server-2017"></a>Nouveautés de SQL Server 2017

Cette version ajouté la prise en charge de Python et les algorithmes d’apprentissage pointe. Renommé pour refléter la nouvelle étendue, SQL Server 2017 marqué l’introduction de **Machine Learning Services (de-de base de données) de SQL Server**, avec prise en charge linguistique pour Python et R. 

Cette version introduite également **Machine Learning Server (autonome) de SQL Server**, entièrement indépendants de SQL Server, pour les charges de travail R et Python que vous souhaitez exécuter sur un système dédié. Avec le serveur autonome, vous pouvez distribuer et l’échelle des solutions de R ou Python sans l’aide de SQL Server.

| Version | Mise à jour de la fonctionnalité |
|---------|----------------|
| CU 6 | Correctifs de bogues et actualisation du package, mais pas des fonctionnalités nouvelles annonces. Correctifs incluent la prise en charge pour les types de données DateTime requête SPEES pour Python et des messages d’erreur améliorés dans microsoftml lorsque dont l’apprentissage des modèles sont manquants. |
| CU 5 | Correctifs de bogues et actualisation du package, mais pas des fonctionnalités nouvelles annonces. Correctifs incluent des améliorations pour transformer les fonctions et variables dans revoscalepy, la correction des erreurs de long chemin d’accès relatif dans rxInstallPackages, résolution des connexions dans une boucle pour RxExec rx_exec fonctions et les révisions des messages d’avertissement. |
| CU 4 | Correctifs de bogues et actualisation du package, mais pas des fonctionnalités nouvelles annonces. |
| CU 3 | Sérialisation dans revoscalepy, de modèle de Python à l’aide de la [rx_serialize_model fonction](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-serialize-model).<br/><br/>[Calcul de score native](sql-native-scoring.md) ainsi que des améliorations apportées aux [en temps réel de calcul de score](real-time-scoring.md). Calcul de score dans base de données, le débit est un million de lignes par seconde à l’aide des modèles R. Cette mise à jour, calculer les scores en temps réel et le score natif offrent les meilleures performances dans une ligne et le calcul du score du lot. Natif score utilise une fonction T-SQL pour le score rapide qui peut être exécutée sur n’importe quelle édition de SQL Server, y compris sur Linux. La fonction ne requiert aucune installation de R ou une configuration supplémentaire. Cela signifie que vous pouvez effectuer l’apprentissage d’un modèle à un autre emplacement, enregistrez-le dans SQL Server et puis effectuer le calcul de score sans jamais appeler R. Pour plus d’informations sur les méthodes de calcul de score, consultez [l’exécution de calculer les scores en temps réel ou calculer les scores natif](r/how-to-do-realtime-scoring.md). |
| CU 2 | Correctifs de bogues et actualisation du package, mais pas des fonctionnalités nouvelles annonces. |
| CU 1 | Dans revoscalepy, ajoute rx_create_col_info pour retourner des informations de schéma à partir d’une source de données SQL Server, qui est similaire à [rxCreateColInfo](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxcreatecolinfo) pour R. <br/><br/>Améliorations apportées aux [rx_exec](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-exec) pour prendre en charge les scénarios parallèles à l’aide de la `RxLocalParallel` contexte de calcul.|
| Version initiale |[**Intégration de Python pour la base de données analytique**](https://blogs.technet.microsoft.com/dataplatforminsider/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/) <br/><br/>Le [revoscalepy](python/what-is-revoscalepy.md) package est l’équivalent de Python de RevoScaleR. Vous pouvez créer des modèles de Python pour des régressions linéaires et logistiques, les arbres de décision, les arbres augmentés et les forêts aléatoires, tout parallèles et capables de les exécuter dans des contextes de calcul à distance. Ce package prend en charge l’utilisation de plusieurs sources de données et les contextes de calcul à distance. Le chercheur de données ou le développeur peut exécuter du code Python sur un serveur SQL distant, pour Explorer les données ou de créer des modèles sans déplacement de données. <br/><br/>Le [microsoftml](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) package est l’équivalent de Python du package MicrosoftML R.<br/><br/>Intégration de T-SQL et Python via [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql). Vous pouvez appeler n’importe quel code Python à l’aide de cette procédure stockée. Cette infrastructure de sécurité permet le déploiement d’entreprise de modèles de Python et des scripts qui peuvent être appelées à partir d’une application à l’aide d’une procédure stockée simple. Gains de performances supplémentaires sont possibles par diffusion en continu des données à partir de SQL pour les processus de Python et la parallélisation anneau MPI. <br/><br/>Vous pouvez utiliser le code T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) fonction à exécuter [score native](sql-native-scoring.md) sur un modèle dont l’apprentissage a déjà été enregistré au format binaire.|
| Version initiale | [**MicrosoftML (R)** ](using-the-microsoftml-package.md) contient la transformation de données et des algorithmes qui peut être des contextes de calcul à distance de mise à l’échelle ou s’exécuter en apprentissage automatique de pointe. Les algorithmes sont des réseaux neuronaux profonds personnalisables, rapide de MDT et forêts décisionnelles, régression linéaire et la régression logistique. |
| Version initiale | [**Modèles préentraînés** ](r/install-pretrained-models-sql-server.md) pour la reconnaissance d’image et d’analyse des sentiments de négatif positif. Utilisez ces modèles pour générer des prédictions sur vos propres données. |
| Version initiale | [**Gestion des packages R**](r/install-additional-r-packages-on-sql-server.md), y compris les caractéristiques suivantes : la base de données pour aider à l’administrateur de gérer les packages et affecter des autorisations pour installer des packages, [créer une bibliothèque externe](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) instruction T-SQL pour bases de données d’aide gérer les packages sans avoir à connaître R et des fonctions dans un ensemble complet de R [RevoScaleR](r/use-revoscaler-to-manage-r-packages.md) pour aider à installer, supprimer ou répertorier les packages appartenant aux utilisateurs. |
| Version initiale | [**À l’Opérationnalisation via mrsdeploy** ](https://docs.microsoft.com/machine-learning-server/r-reference/mrsdeploy/mrsdeploy-package) pour le déploiement et l’hébergement de script R comme un service web. S’applique à un script R uniquement (pas d’équivalent Python). Destiné à l’option de serveur (autonome) éviter la concurrence de ressources avec d’autres opérations de SQL Server. |


## <a name="new-in-sql-server-2016"></a>Nouveautés de SQL Server 2016

Cette version introduite apprentissage automatique capacités dans SQL Server via **SQL Server 2016 R Services**, un moteur de base de données analytique pour le script de traitement R sur les données résidentes dans une instance du moteur de base de données.

En outre, **(autonome) du serveur SQL Server 2016 R** a été publié comme un moyen d’installer R Server sur un serveur Windows. Le programme d’installation de SQL Server fournies au départ, la seule façon d’installer R Server pour Windows. Dans les versions ultérieures, les développeurs et les chercheurs de données qui voulaient R Server sur Windows peuvent utiliser un autre programme d’installation d’autonome pour atteindre le même objectif. Le serveur autonome dans SQL Server est fonctionnellement équivalent au produit serveur autonome, [Microsoft R Server pour Windows](https://docs.microsoft.com/machine-learning-server/install/r-server-install-windows).

| Version |Mise à jour de la fonctionnalité |
|---------|----------------|
| Ajouts CU | [**Calculer les scores en temps réel** ](real-time-scoring.md) s’appuie sur les bibliothèques natives C++ pour lire un modèle stocké dans un format binaire optimisé, puis générer des prédictions sans avoir à appeler le runtime R. Cela rend les opérations de calcul de score beaucoup plus rapides. Avec le calcul de score en temps réel, vous pouvez exécuter une procédure stockée ou effectuer en temps réel à partir du code R de calcul de score. Calculer les scores en temps réel sont également disponible pour SQL Server 2016, si l’instance est mis à niveau vers la dernière version de [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)]. |
| Version initiale | [**Intégration de R pour la base de données analytique**](r/sql-server-r-services.md). <br/><br/> Packages R pour R appelant des fonctions dans T-SQL et vice versa. Les fonctions RevoScaleR fournissent des analytique R à l’échelle par segmentation des données dans des composants, la coordination et la gestion distribuée de traitement et l’agrégation des résultats. Dans SQL Server 2016 R Services (de-de base de données), le moteur de RevoScaleR est intégré à une instance du moteur de base de données, chambres analytique dans le même contexte de traitement et données. <br/><br/>Intégration de T-SQL et R via [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql). Vous pouvez appeler n’importe quel code R à l’aide de cette procédure stockée. Cette infrastructure de sécurité permet le déploiement d’entreprise de modèles de Rn et les scripts qui peuvent être appelées à partir d’une application à l’aide d’une procédure stockée simple. Gains de performances supplémentaires sont possibles par diffusion en continu des données à partir de SQL pour les processus R et la parallélisation anneau MPI. <br/><br/>Vous pouvez utiliser le code T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) fonction à exécuter [score native](sql-native-scoring.md) sur un modèle dont l’apprentissage a déjà été enregistré au format binaire.|

## <a name="linux-support-roadmap"></a>Guide de prise en charge de Linux

Apprentissage à l’aide de R ou Python dans-base de données n’est pas pris en charge dans SQL Server sur Linux. Recherchez des annonces dans une version ultérieure.

Toutefois, sur Linux, vous pouvez effectuer [score native](sql-native-scoring.md) à l’aide de la fonction de prédiction de T-SQL. Calculer les scores native vous permet de score à partir d’un modèle préformé très rapide, sans appeler ou même nécessitant un runtime R. Cela signifie que vous pouvez utiliser SQL Server sur Linux pour générer des prédictions très rapides, à répondre à des applications clientes.

<a name="azure-sql-database-roadmap"></a>

## <a name="azure-sql-database-roadmap"></a>Feuille de route de base de données SQL Azure

Il existe une prise en charge limitée pour R dans la base de données SQL Azure : disponible uniquement dans l’ouest des États-Unis, dans les services créés au niveau Premium. Couverture de protection, y compris la prise en charge de Python, est susceptible de suivre dans une version ultérieure. Toutefois, il n’existe aucune date de publication prévue pour l’instant.  

## <a name="next-steps"></a>Étapes suivantes

+ [Installer SQL Server 2017 Machine Learning Services (de-de base de données)](install/sql-machine-learning-services-windows-install.md)
+ [Exemples et didacticiels de machine learning](tutorials/machine-learning-services-tutorials.md)
