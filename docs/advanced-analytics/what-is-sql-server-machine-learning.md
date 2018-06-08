---
title: Nouveautés de SQL Server Machine Learning Services ? | Microsoft Docs
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: overview
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: ecd58ee9670724a2732ce8aabc5d9f2c62042995
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585451"
---
# <a name="what-is-sql-server-machine-learning-services"></a>Nouveautés de SQL Server Machine Learning Services ?
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Machine Learning Services SQL Server est un incorporé, prédictif analytique et les données scientifiques moteur qui peut exécuter du code R et Python au sein d’une base de données SQL Server en tant que procédures stockées, en tant que script T-SQL contenant des instructions de R ou Python ou R ou Python code contenant T-SQL. 

La proposition de valeur de clé des Services de Machine Learning est la puissance de ses propriétaires packages pour remettre d’analytique avancée à l’échelle et la possibilité de mettre des calculs et traitement à l’emplacement des données, en éliminant la nécessité pour extraire des données sur le réseau.

Il existe deux options pour l’utilisation des fonctionnalités de machine learning dans SQL Server : 

+ [**SQL Server Machine Learning Services (de-de base de données)** ](r/sql-server-r-services.md) opère au sein de l’instance du moteur de base de données, où le moteur de calcul est entièrement intégré avec le moteur de base de données. La plupart des installations sont cette option.
+ [**SQL Server Machine Learning Server (autonome)** ](r/r-server-standalone.md) est Machine Learning pour Windows Server qui s’exécute indépendamment du moteur de base de données. Bien que vous utilisez le programme d’installation de SQL Server pour installer le serveur, la fonctionnalité n’est pas dépendant des instances. Il équivaut fonctionnellement, non-SQL-Server [Microsoft Machine Learning pour Windows Server](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-windows-install).

## <a name="r-and-python-packages"></a>Packages R et Python

Prise en charge pour chaque langue est par le biais des packages Microsoft propriétaires utilisés pour la création et l’apprentissage de modèles de différents types de données et le traitement parallèle utilise les ressources de système sous-jacent de calcul de score.

Étant donné que les packages propriétaires reposent sur les distributions de R et Python open source, script ou code que vous exécutez dans SQL Server peut également appeler des fonctions de base et utiliser des packages tiers compatibles avec la version de langage fournie dans SQL Server (Python 3.5 et versions récentes de R, 3.3.3 actuellement).

| R  | Python | Description |
|-----------|----------------|-------------|
| [RevoScaleR](r/revoscaler-overview.md) | [revoscalepy](python/what-is-revoscalepy.md)   | Fonctions de ces bibliothèques sont parmi les plus couramment utilisées. Transformations de données et la manipulation, synthèse des statistique, la visualisation et de nombreux formulaires d’analyses et de modélisation sont trouvent dans ces bibliothèques. En outre, les fonctions de ces bibliothèques distribue automatiquement les charges de travail entre les cœurs disponibles pour un traitement parallèle, avec la possibilité de fonctionner sur les segments de données qui sont coordonnées et gérées par le moteur de calcul. |
| [MicrosoftML](using-the-microsoftml-package.md) | [microsoftml](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) | Pointe algorithmes d’apprentissage pour la personnalisation de l’image, problèmes de classification et bien plus encore. |
| [olapR](r/how-to-create-mdx-queries-using-olapr.md) | none | Générer ou exécuter une requête MDX dans le script R.
| [sqlRUtils](r/generating-an-r-stored-procedure-for-r-code-using-the-sqlrutils-package.md) | none | Fonctions de la mise des scripts R dans T-SQL des procédures stockées, l’inscription d’une procédure stockée avec une base de données et la procédure stockée en cours d’exécution à partir d’un environnement de développement R.
| [mrsdeploy](operationalization-with-mrsdeploy.md) | none | Principalement utilisé sur une installation non SQL Server d’apprentissage Machine, telles que la [(autonome) version](r/r-server-standalone.md). Utilisez ce package pour déployer et d’héberger des services web, de générer des topologies de montée en puissance parallèle avec web dédié et de nœuds de calcul, de basculer entre les sessions locales et distantes, exécutez les diagnostics et bien plus encore. Pour une installation (de-de base de données), utilisez ce package dans une capacité d’un client : par exemple, pour accéder à un service web sur un serveur distant dédié simplement les charges de travail Machine Learning Services en cours d’exécution. |

La portabilité de votre code R et Python personnalisé est adressée par l’intermédiaire de la distribution de packages et les interpréteurs de plusieurs produits. Les packages qui sont fournis dans SQL Server sont également disponibles dans plusieurs autres produits et services Microsoft, y compris une version non SQL appelée [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/). Les clients libres qui incluent des interpréteurs de notre R et Python incluent [Microsoft R Client](https://docs.microsoft.com/machine-learning-server/r-client/what-is-microsoft-r-client) et le [bibliothèques de Python](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter).

Les packages et les interpréteurs de sont également disponibles sur plusieurs [machines virtuelles](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-azure-vm-on-linux), Azure Machine Learning et des services Azure tels que [HDInsight](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-on-azure-hdinsight). 


## <a name="use-cases"></a>Cas d’utilisation

**Dans la base de données analytique**

Analystes et les développeurs ont souvent le code qui s’exécute sur une instance locale de SQL Server. Si vous avez tels que SQL Server et un bus IDE [Visual Studio avec R](https://docs.microsoft.com/visualstudio/rtvs/) ou [Visual Studio avec Python](https://docs.microsoft.com/visualstudio/python/installing-python-support-in-visual-studio) sur le même ordinateur, vous pouvez générer, former et tester localement des modèles dans les deux langages. Accès local simplifie la gestion des packages : en tant qu’administrateur, vous pouvez charger des packages tiers supplémentaires à l’aide des autorisations intégrées pour ce rôle.

L’approche la plus courante pour la base de données analytique consiste à utiliser [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour exécuter le script R ou Python. Les didacticiels répertoriés dans [étapes](#next-steps) vous aideront à démarrer.

**Configurations client-serveur**

À partir de toute station de travail cliente qui a un bus IDE, installez [Microsoft R Client](https://docs.microsoft.com/machine-learning-server/r-client/what-is-microsoft-r-client) ou [bibliothèques de Python](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter)et puis écrire du code qui exécute un push de l’exécution (appelé un *le contexte de calcul à distance*) données et aux opérations à un serveur SQL distant. 

De même, si vous utilisez l’édition développeur, vous pouvez créer des solutions sur une station de travail cliente en utilisant les bibliothèques et les interpréteurs de même et ensuite déployer le code de production sur SQL Server Machine Learning Services (de-de base de données). 

## <a name="version-history"></a>Historique des versions

SQL Server 2017 Machine Learning Services est la génération suivante de SQL Server 2016 R Services améliorées pour inclure les Python. Le tableau suivant est une liste complète de toutes les versions de la création à la version actuelle. 

| Nom de produit | Version du moteur | Date de publication |
|--------------|---------|--------------|
| SQL Server 2017 Machine Learning Services (de-de base de données) | R Server 9.2.1 <br/> Python Server 9.2 | Octobre 2017 |
| SQL Server 2017 Machine Learning Server (autonome) | R Server 9.2.1 <br/> Python Server 9.2 | Octobre 2017 |
| SQL Server 2016 R Services (de-de base de données) | R Server 9.1  | Juillet 2017  |
| SQL Server 2016 R Server (autonome)  |  R Server 9.1 | Juillet 2017 |



## <a name="documentation-for-each-version"></a>Documentation de chaque version.

Les versions récentes de la documentation de SQL Server sont indépendant de la version. Pour SQL Server Machine Learning Services, Python est disponible uniquement en 2017 et versions ultérieures, alors que la prise en charge R est dans toutes les versions. Sauf indication contraire, vous pouvez supposer que documentation de R s’applique aux versions 2016 et 2017.


## <a name="related-machine-learning-products"></a>Produits apprentissage connexe

 +  [Configurer une Machine virtuelle Azure](r/provision-the-r-server-only-sql-server-2016-enterprise-vm-on-azure.md)
  
  Azure marketplace inclut plusieurs images de machine virtuelle qui incluent la Machine Learning serveur ou R. Création d’un ordinateur virtuel dans Microsoft Azure est le moyen le plus rapide pour obtenir pour le développement et le déploiement de modèles prédictifs. Les images sont fournis avec les fonctionnalités de mise à l’échelle et de partage déjà configuré, ce qui le rend plus facile à incorporer analytique à l’intérieur d’applications et d’intégrer des systèmes principaux.

+ [Machine virtuelle pour la science des données](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)

  La dernière version de la machine virtuelle de science des données comprend des Machine Learning Server, SQL Server, plus un tableau des outils les plus populaires pour l’apprentissage automatique, toutes les préinstallé et testé. Créer les blocs-notes Notebook, développer des solutions dans Julia et utiliser des bibliothèques de GPU compatible de formation approfondie, MXNet, CNTK et TensorFlow.

<a name="next-steps"></a>

## <a name="next-steps"></a>Étapes suivantes

**Étape 1 :** installer et configurer le logiciel. 

+ [Installer SQL Server 2017 Machine Learning Services (de-de base de données)](install/sql-machine-learning-services-windows-install.md)

**Étape 2 :** commencer avec le code à l’aide de l’une de ces didacticiels :

+ [Didacticiel : Exécutez Python dans T-SQL](tutorials/run-python-using-t-sql.md)
+ [Didacticiel : Exécuter R dans T-SQL](tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md)

**Étape 3 :** ajouter vos packages R et Python favoris et les utiliser avec les packages fournis par Microsoft

+ [Gestion des packages de R pour SQL Server](r/install-additional-r-packages-on-sql-server.md)
