---
title: Machine learning du cycle de vie et le processus d’équipe | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: ca7d0acf3097ec031cba5f3a7245d5594b7927bf
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="machine-learning-lifecycle-and-personas"></a>Personnages et cycle de vie de machine learning
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Projets d’apprentissage machine peuvent être complexes, car ils requièrent les compétences et la collaboration d’un jeu disparates de professionnels. Cet article décrit les principales tâches dans machine learning du cycle de vie, le type de professionnels des données qui sont engagés dans l’apprentissage automatique, et comment SQL Server prend en charge les besoins.

> [!TIP]
> 
> Avant de vous lancer dans un projet d’apprentissage, nous vous recommandons de consulter les outils et les meilleures pratiques fournies par le [processus de science des données de Microsoft Team](https://blogs.technet.microsoft.com/machinelearning/2017/10/09/the-microsoft-team-data-science-process-tdsp-recent-updates/), ou TDSP. Ce processus a été créé par apprentissage consultants chez Microsoft pour consolider les meilleures pratiques pour la planification et l’itération sur les projets d’apprentissage. Le TDSP a ses racines dans les normes industrielles telles que CRISP-DM, mais intègre des pratiques récentes telles que DevOps et la visualisation.

## <a name="machine-learning-life-cycle"></a>Cycle de vie de machine learning

Apprentissage automatique est un processus complexe qui touche tous les aspects des données de l’entreprise, et de nombreux projets d’apprentissage machine finissent prend plus de temps et qui est plus complexe que prévu. Voici quelques-unes des manières qu’apprentissage requiert la prise en charge de professionnels des données de l’entreprise :

+ Apprentissage commence avec l’identification des objectifs et des règles d’entreprise.
+ Les professionnels de l’apprentissage machine doivent être conscients de stratégies pour le stockage, l’extraction et l’audit des données.
+ Collecte des données potentiellement applicables est le suivant.  Sources de données doivent être identifiés et les données appropriées extraites des capteurs et applications d’entreprise. 
+ La qualité des efforts d’apprentissage machine est dépendent non seulement le type de données qui n’est disponibles, mais les processus très utilisés pour l’extraction, le traitement et le stockage des données. 
+ Aucun projet d’apprentissage machine ne serait complète sans une stratégie pour le rapport d’analyse et éventuellement engagement du client et des commentaires.

SQL Server permet de combler la plupart des écarts entre les professionnels des données et les experts machine learning :

+ Les données peuvent être stockés en local ou dans le cloud
+ SQL Server est intégré à chaque étape du traitement des données entreprise, y compris la création de rapports et ETL
+ SQL Server prend en charge la sécurité des données, de la redondance des données et de l’audit
+ Fournit la gouvernance des ressources

## <a name="data-scientists"></a>Chercheurs de données

Les chercheurs de données utilisent divers outils pour l’analyse de données et l’apprentissage automatique, allant d’Excel ou les plateformes open source gratuits, aux coûteuses suites statistiques qui requièrent des connaissances techniques approfondies. Toutefois, à l’aide de R ou Python avec SQL Server fournit des avantages uniques par rapport à ces outils traditionnels :

+ Vous pouvez développer et tester une solution à l’aide de l’environnement de développement de votre choix, puis déployer votre code R ou Python en tant que partie du code T-SQL.
+ Déplacer des calculs complexes hors tension d’un ordinateur portable du chercheur de données et sur le serveur, en évitant le déplacement de données pour se conformer aux stratégies de sécurité d’entreprise.
+ Performances et évolutivité sont améliorées via les API et les packages R spéciale. Vous n’êtes plus limité par l’architecture monothread, liée à la mémoire de R et pouvez utiliser les jeux de données volumineux et de calculs multicœurs, multithreads et multiprocessus.
+ Portabilité du code : Solutions peuvent être exécutées dans SQL Server ou dans Hadoop ou Linux, à l’aide de [Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server). Une fois le code, déployer en tout lieu.

## <a name="application-and-database-developers"></a>Développeurs d’applications et de la base de données

Les développeurs de base de données sont chargés d’intégrer plusieurs technologies et de les faire fonctionner ensemble afin que toute l’entreprise puisse en bénéficier. Le développeur de base de données fonctionne avec les développeurs d’applications, les développeurs SQL et des chercheurs de données pour concevoir des solutions, recommander des méthodes de gestion de données et créer ou déployer des solutions.

Intégration avec SQL Server offre de nombreux avantages aux développeurs de données :

+ Le chercheur de données peut travailler dans RStudio, tandis que le développeur de données déploie la solution à l’aide de SQL Server Management Studio. Réécriture du code n’y a plus de solutions de R ou Python.
+ Optimisez vos solutions à l’aide de la meilleure de T-SQL, R et Python. Des opérations complexes sur les jeux de données volumineux peuvent être exécutées beaucoup plus efficacement à l’aide de SQL Server que de tirer parti de R. la base de connaissances des professionnels de votre base de données pour améliorer les performances des solutions d’apprentissage machine, en utilisant les index columnstore en mémoire, et agrégations à l’aide des opérations reposant sur des ensembles SQL. 
+ Facilement automatiser les tâches qui doivent s’exécuter à plusieurs reprises sur de grandes quantités de données, comme la génération de scores de prédiction sur les données de production. 
+ Accès paramétrables script R ou Python à partir de toute application qui utilise [!INCLUDE[tsql](../../includes/tsql-md.md)]. Simplement appeler une procédure stockée pour former un modèle, générer un graphique ou de sortie des prédictions.
+ Les API peuvent diffuser des jeux de données volumineux et tirer parti des calculs dans la base de données multicœurs, multithreads et multiprocessus.

Pour plus d’informations sur les tâches associées, consultez :
+ [Opérationnalisation de votre code R](../../advanced-analytics/r/operationalizing-your-r-code.md)

### <a name="database-administrators"></a>Administrateurs de base de données

Les administrateurs de base de données doivent intégrer des projets et priorités concurrents dans un point de contact unique : le serveur de base de données. Il doivent accorder l’accès aux données, non seulement aux chercheurs de données mais aussi aux développeurs de rapports, analystes d’entreprise et autres utilisateurs de données d’entreprise, tout en préservant l’intégrité des magasins de données opérationnelles et de rapport. En entreprise, l’administrateur de base de données est un intervenant essentiel dans la conception et le déploiement d’une infrastructure efficace pour la recherche de données. 

SQL Server fournit des fonctionnalités uniques pour l’administrateur de base de données qui doit prendre en charge les données chargé de :

+ Sécurité par SQL Server : l’architecture de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] protège vos bases de données et isole l’exécution des sessions de script externe à partir de l’opération de l’instance de base de données. Vous pouvez spécifier qui est autorisé à exécuter des scripts de machine learning et utiliser des rôles de base de données pour gérer les packages.

+ Les sessions R et Python sont exécutées dans un processus séparé pour garantir que votre serveur continue à exécuter comme d’habitude, même si le script externe rencontre des problèmes.

+ La gouvernance de ressources à l’aide de SQL Server vous permet de contrôler la mémoire allouées aux runtimes externes, pour empêcher que des calculs massifs mettent en péril les performances globales du serveur de processus.

Pour plus d’informations sur les tâches associées, consultez :
+ [Gestion et surveillance des solutions Machine Learning](../../advanced-analytics/r/managing-and-monitoring-r-solutions.md)

## <a name="architects-and-data-engineers"></a>Ingénieurs architectes et des données

Des architectes conçoivent des flux de travail intégrés qui s’étendent sur tous les aspects du cycle de vie machine learning. Les ingénieurs de données concevoir et créer des solutions ETL et déterminent comment optimiser pour l’apprentissage des tâches ingénierie de fonctionnalité. La plateforme de données globales doit être conçue pour équilibrer les besoins métier concurrents.

Étant donné que [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] est profondément intégré à d’autres outils Microsoft, tels que l’aide à la décision et la pile de l’entrepôt de données, les outils d’entreprise cloud et de mobilité, et Hadoop, il offre divers avantages à l’ingénieur chargé du traitement des données ou à l’architecte système désireux de promouvoir des analyses avancées.

+ Appeler n’importe quel script Python ou R à l’aide de procédures stockées système, pour remplir les jeux de données, de générer des graphiques ou d’obtenir des prédictions. Aucun plus conception flux de travail parallèles dans la science des données et des outils ETL. Prise en charge d’Azure Data Factory et de la base de données SQL Azure facilite le cloud des sources de données dans le flux de travail d’apprentissage.

+ Pour la planification et la gestion des tâches d’apprentissage automatique, utiliser des flux de travail automation standard dans SQL Server, selon les Services d’intégration, l’Agent SQL ou Azure Data Factory. Sinon, utilisez le [les fonctionnalités à l’Opérationnalisation](https://docs.microsoft.com/machine-learning-server/operationalize/how-to-deploy-web-service-publish-manage-in-r) dans Machine Learning Server.

Pour plus d’informations sur les tâches associées, consultez :

+ [Création de flux de travail machine learning dans SQL Server](../../advanced-analytics/r/creating-workflows-that-use-r-in-sql-server.md)

