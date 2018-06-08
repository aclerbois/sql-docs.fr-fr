---
title: Opérationaliser le code R dans SQL Server Machine Learning Services | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: f5fa7806ad70c37c7d51c5ae2cc9606191560e58
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="operationalize-r-code-machine-learning-services"></a>Opérationaliser le code de R (Machine Learning Services)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Les développeurs de base de données sont chargés d’intégrer plusieurs technologies et de les faire fonctionner ensemble afin que toute l’entreprise puisse en bénéficier. Le développeur de base de données fonctionne avec les développeurs d’applications, les développeurs SQL et des chercheurs de données pour concevoir et déployer des solutions.

Cet article résume les points clés pour le développeur de base de données prendre en compte lors de l’Opérationnalisation du code R à l’aide de SQL Server.

## <a name="get-started-with-r-code-in-sql-server"></a>Prise en main du code R dans SQL Server

En règle générale, l’intégration de machine learning solutions est destiné le recodage des étendues pour prendre en charge des performances et l’intégration. Toutefois, déplacement de code R et Python dans un environnement de production est beaucoup plus facile dans Machine Learning Services SQL Server, car le code peut être exécuté dans SQL Server et appelé à l’aide de procédures. Vous pouvez continuer à utiliser des outils familiers et que vous n’avez pas besoin d’installer un environnement de développement R. 

Pour plus d’informations sur la syntaxe de base, consultez :

+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [À l’aide de code R dans SQL](../../advanced-analytics/tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md).

Pour obtenir un exemple de la façon dont vous pouvez déployer du code R en production à l’aide de procédures stockées, consultez :

+ [Dans base de données Analytique pour les développeurs SQL](../../advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers.md).

## <a name="optimize-your-r-code"></a>Optimiser votre code R

Bien entendu, il est plus facile si certaines optimisations sont effectuées au préalable dans le code R ou Python de convertir votre code R dans SQL. Notamment, en évitant les types de données qui entraînent des problèmes, ce qui évite les conversions de données inutiles et réécrire le code R en tant qu’un seul appel de fonction qui peut être facilement paramétrable. Pour plus d'informations, consultez :

+ [Bibliothèques et types de données R](r-libraries-and-data-types.md)

+ [Conversion de code R pour une utilisation dans R Services](converting-r-code-for-use-in-sql-server.md)

+ [Génération d’un R de procédure stockée à l’aide de sqlrutils](generating-an-r-stored-procedure-for-r-code-using-the-sqlrutils-package.md)

## <a name="integrate-r-and-python-with-applications"></a>Intégrer R et Python des applications

Vous pouvez démarrer les Services de Machine Learning à partir d’une procédure stockée, vous pouvez exécuter des scripts R ou Python à partir de n’importe quelle application qui peut envoyer une instruction T-SQL et traiter les résultats.

Par exemple, vous pouvez recycler un modèle selon une planification à l’aide de la tâche d’exécution de T-SQL dans Integration Services, ou à l’aide d’un autre planificateur de travaux qui peut exécuter une procédure stockée.

Calcul de score est une tâche importante qui peut facilement être automatisée ou démarrée à partir d’applications externes. Vous l’apprentissage du modèle au préalable, à l’aide de R ou Python ou une procédure stockée et que vous enregistrez le modèle dans un format binaire à une table. Ensuite, le modèle peut être chargé dans une variable en tant que partie d’un appel de procédure stockée, à l’aide d’une des options suivantes pour calculer les scores de T-SQL :

+ [En temps réel](../real-time-scoring.md) de calcul de score, optimisé pour les lots de petite taille
+ Calcul de score, appelé à partir d’une application de ligne unique
+ [Calcul de score native](../sql-native-scoring.md), pour la prédiction de traitement rapide de SQL Server sans appeler R

Cette procédure pas à pas fournit des exemples de calcul de score à l’aide d’une procédure stockée dans le lot et les modes de ligne unique :

+ [Bout en bout de procédure pas à pas de données scientifiques de R dans SQL Server](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)

Consultez ces modèles de solution pour obtenir des exemples montrant comment intégrer le calcul de score dans une application :

+ [Prévisions de vente au détail](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/RetailForecasting/Introduction.md)
+ [Détection des fraudes](https://github.com/Microsoft/SQL-Server-R-Services-Samples/blob/master/FraudDetection/Introduction.md)
+ [Client de gestion de clusters](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/r-services/getting-started/customer-clustering)

## <a name="boost-performance-and-scale"></a>Améliorer les performances et l’échelle

Bien que le langage R open source connu pour avoir des limitations, le package RevoScaleR API peut opérer sur les jeux de données volumineux et tirer parti des calculs dans la base de données multicœurs, multithreads et multiprocessus.

Si votre solution R utilise des agrégations complexes ou implique des jeux de données volumineux, vous pouvez tirer parti des agrégations en mémoire très efficaces de SQL Server et les index columnstore et laisser le code R les calculs statistiques et de calcul de score.

Pour plus d’informations sur la façon d’améliorer les performances dans l’apprentissage de SQL Server, consultez :

+ [Réglage des performances pour SQL Server R Services](../../advanced-analytics/r/sql-server-r-services-performance-tuning.md)
+ [Conseils et astuces de l’optimisation des performances](https://gallery.cortanaintelligence.com/Tutorial/SQL-Server-Optimization-Tips-and-Tricks-for-Analytics-Services)

## <a name="adapt-r-code-for-other-platforms-or-compute-contexts"></a>Adapter le code R pour d’autres plateformes ou contextes de calcul

Le code R que vous exécutez sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] données peut être utilisées sur d’autres sources de données, telles que Hadoop et dans d’autres contextes de calcul.

Pour plus d’informations sur les plateformes prises en charge par Microsoft R, consultez :

+ [Présentation de Microsoft R](https://docs.microsoft.com/r-server/)

+ [Explorer RevoScaleR](https://docs.microsoft.com/r-server/r/tutorial-r-to-revoscaler)

Pour plus d’informations sur la façon dont vous pouvez optimiser vos solutions de Microsoft R à exécuter sur les données de grande taille ou de plusieurs plateformes, consultez :

+ [Écrire des algorithmes de segmentation](https://docs.microsoft.com/r-server/r/how-to-developer-write-chunking-algorithms)

+ [Calcul des données volumineuses dans R](https://docs.microsoft.com/r-server/r/tutorial-large-data-tips)

+ [Développer votre propre algorithme parallèle](https://docs.microsoft.com/r-server/r-reference/revopemar/pemar)

