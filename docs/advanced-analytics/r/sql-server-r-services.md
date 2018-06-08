---
title: Apprentissage et R Services (de-de base de données) de l’ordinateur SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 559309a29944f20f8c006ccc92769f0b2824e3b3
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34585961"
---
# <a name="sql-server-machine-learning-and-r-services-in-database"></a>Apprentissage et R Services (de-de base de données) de l’ordinateur SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Une installation de base de données de l’apprentissage intervient dans le contexte d’une instance de moteur de base de données SQL Server, les données résidentes dans votre instance de SQL Server prend en charge de script externe R et Python. Étant donné que l’apprentissage est intégré à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez conserver analytique proches des données et éliminer les coûts et les risques de sécurité liés au déplacement des données.

Le moteur de base de données étant à plusieurs instances, vous pouvez installer plusieurs instances de la base de données analytique, ou encore plus anciens et plus récentes versions côte à côte. Les choix incluent le [Machine Learning Services (de-de base de données) de SQL Server 2017](../install/sql-machine-learning-standalone-windows-install.md) avec R et Python, ou [R Services (de-de base de données) de SQL Server 2016](../install/sql-r-standalone-windows-install.md) avec simplement R. 

Composants de machine learning peuvent également être installés en tant qu’instance indépendant [serveurs autonomes](r-server-standalone.md). En règle générale, nous vous recommandons traiter (autonome) et (dans-base de données) installations mutuellement exclusif afin d’éviter les conflits de ressources, mais si vous disposez de suffisamment de ressources, il n’y aucun interdictions contre les installer à la fois sur le même ordinateur physique.

## <a name="choosing-between-in-database-and-standalone-analytics"></a>Choix entre analytique dans base de données et autonome

Comprendre vos besoins de développement peut vous aider à choisir entre (de-de base de données) et les approches de (autonome). Un serveur autonome est plus simple à configurer et gérer une souplesse maximale pour son utilisation, ou si vous souhaitez également vous connecter à une variété de sources de données en dehors de SQL Server. 

Dans la base de données analytique est conçus pour une intégration étroite avec les données dans SQL Server. Vous pouvez écrire des requêtes T-SQL qui appellent les fonctions R ou Python et exécutent le script dans SQL Server Management Studio ou tout outil ou l’application utilisée pour T-SQL externe ou incorporée. Si vous avez besoin exécuter du code R ou Python [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], soit à l’aide de procédures stockées à l’aide de SQL Server de l’instance en tant que le [contexte de calcul](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-compute-context), vous devez installer la base de données analytique. Cette option fournit la sécurité des données et l’intégration avec les outils SQL Server.

À la fois dans la base de données et les serveurs autonomes peuvent réduire les contraintes de mémoire et de traitement de l’open source R et Python. Les deux options incluent les mêmes packages et outils, avec la possibilité de charger et traiter de grandes quantités de données sur plusieurs cœurs et agréger les résultats en une seule sortie consolidée. Les algorithmes et les fonctions sont conçues pour la montée en puissance et l’utilitaire : livraison prédictive analytique, la modélisation statistique, des visualisations de données et algorithmes dans un produit serveur commerciale d’apprentissage pointe conçue et la prise en charge par Microsoft. 

## <a name="components-of-an-in-database-installation"></a>Composants d’une installation de base de données

SQL Server 2016 est R uniquement. SQL Server 2017 prend en charge R et Python. Le tableau suivant décrit les fonctionnalités de chaque version. À l’exception du service SQL Server Launchpad, cette table est identique à celle fournie dans le [article de serveur autonome](r-server-standalone.md).

| Composant | Description |
|-----------|-------------|
| Service SQL Server Launchpad | Un service qui gère les communications entre les exécutions de R et Python externes et le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance. |
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

+ [SQL Server 2017 Machine Learning Services (de-de base de données)](../install/sql-machine-learning-services-windows-install.md)

+ [SQL Server 2016 R Services (dans-base de données) - R uniquement](../install/sql-r-services-windows-install.md)
 
### <a name="step-2-configure-a-development-tool"></a>Étape 2 : Configurer un outil de développement

Configurez vos outils de développement pour utiliser les fichiers binaires du serveur de Machine Learning. Pour plus d’informations sur les Python, consultez [binaires Python de lien](https://docs.microsoft.com/machine-learning-server/python/quickstart-python-tools). Pour obtenir des instructions sur la façon de se connecter dans R Studio, consultez [à l’aide de différentes Versions de R](https://support.rstudio.com/hc/en-us/articles/200486138-Using-Different-Versions-of-R) et pointez l’outil C:\Program Files\Microsoft SQL Server\140\R_SERVER\bin\x64. Vous pouvez également essayer [R Tools pour Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation). 

Chercheurs de données utilisent généralement R ou Python sur leur propre station de travail d’un ordinateur portable ou de développement, pour Explorer les données et de créer et de paramétrer des modèles prédictifs jusqu'à ce qu’un bon modèle prédictif. 

Avec analytique dans base de données dans SQL Server, il est inutile de modifier ce processus. Une fois l’installation terminée, vous pouvez exécuter le code R ou Python sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] localement ou à distance :

![rsql_keyscenario2](media/rsql-keyscenario2.png) 

+ **Utiliser l’IDE vous préférez**. Les composants du client[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] offrent aux spécialistes de la science des données tous les outils dont ils ont besoin pour tester et développer. Ces outils incluent le runtime R, la « Intel Math Kernel Library » pour améliorer les performances des opérations R standard, et un ensemble de packages R améliorés qui prennent en charge l’exécution du code R dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

+ **Travailler à distance ou localement**. Les chercheurs de données peuvent se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et apporter les données au client pour une analyse locale, comme d’habitude. Toutefois, une meilleure solution consiste à utiliser le **RevoScaleR** ou **revoscalepy** API permettant d’envoyer des calculs à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ordinateur, en évitant le déplacement de données coûteux et non sécurisé.

+ **Incorporer des scripts R ou Python dans [!INCLUDE[tsql](../../includes/tsql-md.md)] des procédures stockées**. Lorsque votre code est entièrement optimisé, placez-le dans une procédure stockée pour éviter le déplacement des données inutiles et optimiser les tâches de traitement des données.

### <a name="step-3-write-your-first-script"></a>Étape 3 : Écrire votre script premier

Appel de R ou fonctions Python à partir de script T-SQL :
  
  + [R : Utilisation de code R dans Transact-SQL](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md) 
  + [R : Analytique en base de données pour les développeurs SQL](../tutorials/sqldev-in-database-r-for-sql-developers.md)
  + [Python : Exécuter Python avec T-SQL](../tutorials/run-python-using-t-sql.md)
  + [Python : Analytique en base de données pour les développeurs SQL](../tutorials/sqldev-in-database-python-for-sql-developers.md)

Choisir le meilleur langage pour la tâche. R est idéal pour effectuer des calculs statistiques qui sont difficiles à implémenter à l’aide de SQL. Pour les opérations basées sur les données, exploiter la puissance du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour optimiser les performances. Utilisez le moteur de base de données en mémoire pour effectuer des calculs très rapides sur les colonnes.

### <a name="step-4-optimize-your-solution"></a>Étape 4 : Optimiser votre solution

Lorsque le modèle est prêt à l’échelle sur les données d’entreprise, le chercheur de données fonctionne souvent le développeur de base de données ou SQL pour optimiser les processus tels que :

+ Ingénierie des caractéristiques
+ Intégrer les données et la transformation des données
+ Calcul de score

En règle générale, les chercheurs de données à l’aide de R ont des problèmes de performances et d’évolutivité, surtout lorsque vous utilisez le jeu de données volumineux. C’est parce que l’implémentation common runtime est monothread et peut accepter uniquement les jeux de données qui entrent dans la mémoire disponible sur l’ordinateur local. L’intégration avec les Services de SQl Server Machine Learning offre plusieurs fonctionnalités pour de meilleures performances, davantage de données :

+ **RevoScaleR**: package R ce contient des implémentations de certaines fonctions R plus populaires, repensées pour fournir un parallélisme et mise à l’échelle. Le package comprend également des fonctions qui améliorent encore davantage les performances et la mise à l’échelle en envoyant des calculs à l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , lequel dispose généralement de bien plus de mémoire et de puissance de calcul.

+ **revoscalepy**. Cette bibliothèque Python, disponible dans SQL Server 2017, implémente les fonctions les plus populaires dans RevoScaleR, telles que les contextes de calcul à distance et de nombreux algorithmes qui prennent en charge de traitement distribué.

**Ressources**

+ [Étude de cas sur les performances](../../advanced-analytics/r/performance-case-study-r-services.md)
+ [R et optimisation des données](../../advanced-analytics/r/r-and-data-optimization-r-services.md)

### <a name="step-5-deploy-and-consume"></a>Étape 5 : Déployer et utiliser

Une fois le script ou le modèle est prêt pour la production, un développeur de base de données peut-être incorporer le code ou le modèle dans une procédure stockée, d’afin que le code R ou Python enregistré peut être appelé à partir d’une application. Le stockage et l’exécution de code R à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offrent de nombreux avantages : ils permettent d’utiliser l’interface [!INCLUDE[tsql](../../includes/tsql-md.md)] , et tous les calculs ont lieu dans la base de données, ce qui évite les déplacements inutiles des données.

![rsql_keyscenario1](media/rsql-keyscenario1.png)

+ **Sécurisé et extensible**. [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] utilise une nouvelle architecture d’extensibilité qui protège votre moteur de base de données et isole les sessions R et Python. Vous pouvez contrôler les utilisateurs autorisés à exécuter des scripts, et vous pouvez spécifier les bases de données est accessible par le code. Vous pouvez contrôler la quantité de ressources allouées à l’exécution, afin d’éviter que des calculs massifs mettent en péril les performances globales du serveur.

+ **Planification et d’audit**. Lors de l’exécution des tâches de script externe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez contrôler et auditer les données utilisées par les chercheurs de données. Vous pouvez également planifier des travaux et créer des workflows contenant des scripts R ou Python externes, tout comme vous planifiez tout autre travail T-SQL ou procédure stockée.

Pour tirer parti de la gestion des ressources et les fonctionnalités de sécurité ne dans SQL Server, le processus de déploiement peut inclure ces tâches :

+ Conversion yourcode à une fonction qui peut exécuter de façon optimale dans une procédure stockée
+ Configuration de la sécurité et en verrouillant les packages utilisés par une tâche particulière
+ L’activation de la gouvernance de ressources (nécessite l’édition Enterprise)

**Ressources**

+ [Gouvernance des ressources pour R](resource-governance-for-r-services.md)
+ [Gestion des packages R pour SQL Server](install-additional-r-packages-on-sql-server.md)

## <a name="see-also"></a>Voir aussi

 [SQL Server Machine Learning et R Server (autonome)](sql-server-r-services.md)
