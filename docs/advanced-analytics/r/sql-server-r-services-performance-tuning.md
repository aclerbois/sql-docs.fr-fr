---
title: Réglage des performances de SQL Server R Services | Microsoft Docs
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 974e4ab2a33e7cc218c22246f7279a131893f119
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="performance-tuning-for-r-in-sql-server"></a>Réglage des performances pour R dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article est le premier d’une série de quatre articles qui décrivent l’optimisation des performances pour R Services, en fonction de deux études de cas :

- Tests de performances effectués par l’équipe de développement de produit pour valider le profil de performance des solutions R

- Optimisation des performances de l’équipe de science des données de Microsoft pour un scénario d’apprentissage spécifique souvent demandé par les clients.

L’objectif de cette série est de fournir des conseils sur les types de techniques qui sont particulièrement utiles pour l’exécution de travaux R dans SQL Server de réglage des performances.

+ Cette rubrique (première) fournit une vue d’ensemble de l’architecture et des problèmes courants lors de l’optimisation pour les tâches courantes relatives aux données.
+ Le deuxième article couvre le matériel et les optimisations de SQL Server.
+ Le troisième article traite des optimisations de code R et des ressources pour une Opérationnalisation rapides.
+ Le quatrième décrit en détail et les résultats de rapports et les conclusions des méthodes de test.

**S’applique à :** SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services

## <a name="performance-goals-and-targeted-scenarios"></a>Les objectifs de performances et scénarios

La fonctionnalité de R Services a été introduite dans SQL Server 2016 pour prendre en charge l’exécution de scripts R par SQL Server. Lorsque vous utilisez cette fonctionnalité, le runtime R fonctionne dans un processus séparé à partir du moteur de base de données, mais il échange des données en toute sécurité avec le moteur de base de données, à l’aide des ressources du serveur et des services qui interagissent avec SQL Server, telles que le Launchpad approuvé.

Dans SQL Server 2017, prise en charge a été annoncé pour l’exécution de scripts Python à l’aide de la même architecture, avec des langues supplémentaires à suivre dans les futures.

À mesure que le nombre de versions prises en charge et la langue augmente, il est important que l’administrateur de base de données et l’architecte de base de données connaissent les risques de contention des ressources en raison de travaux de machine learning, et qu’ils sont en mesure de configurer le serveur pour prendre en charge les nouvelles charges de travail. Bien que la conservation d’analytique proches des données élimine les mouvements de données non sécurisée, il déplace également les charges de travail analytiques hors tension de l’ordinateur portable de spécialistes de données et sur le serveur qui héberge les données.

Optimisation des performances pour l’apprentissage n’est pas une solution globale. Les tâches courantes suivantes peuvent avoir des profils de performances très différentes :

- Tâches d’apprentissage : création d’apprentissage et un modèle de régression par rapport à l’apprentissage d’un réseau neuronal
- Ingénierie de fonctionnalité à l’aide de R et extraction de fonctionnalité à l’aide de T-SQL
- Calcul de score sur les lignes uniques ou les lots de petite taille, et en bloc à l’aide des entrées sous forme de tableau de calcul de score
- Calcul de score dans R et déploiement de modèles en production sur SQL Server dans les procédures stockées en cours d’exécution
- Modifier le code R pour réduire le transfert de données ou de supprimer des transformations de données coûteux
- Permettre les tests automatisés et le recyclage des modèles

Étant donné que le choix des techniques d’optimisation dépend de la tâche qui est essentiel pour votre application ou d’un cas d’usage, les études de cas couvrent les conseils de performances générales et obtenir des conseils sur l’optimisation pour un scénario spécifique, l’optimisation pour le calcul du score du lot.

+ **Options d’optimisation individuels dans SQL Server**

    Dans la première étude de cas de performances, plusieurs tests ont été exécutées sur un seul dataset à l’aide d’un seul type de modèle. L’algorithme rxLinMod dans RevoscaleR a été utilisé pour générer un modèle et créer des scores à partir de celui-ci, mais le code, ainsi que les tables de données sous-jacentes ont été modifiées systématiquement pour tester l’impact de chaque modification.

    Par exemple, dans une série de tests, le code R a été modifié afin qu’une comparaison a pu être établie entre l’ingénierie de fonctionnalité à l’aide d’une fonction de transformation et le calcul préliminaire des fonctionnalités et fonctions puis de chargement d’une table. Dans une autre série de tests, les performances d’apprentissage du modèle a été comparée entre l’utilisation d’une table indexée standard par rapport aux données dans une table avec différents types de compression ou de nouveaux types d’index.

+ **Optimisation pour un scénario de calcul de score de haut volume spécifique**

    Les tâches d’apprentissage automatique dans le deuxième étude de cas implique le traitement reprend nombreux soumise à plusieurs postes et recherche le meilleur candidat pour chaque poste de travail. Le modèle d’apprentissage lui-même est formulé comme un problème de classification binaire : il prend une description de reprise et de travail en tant qu’entrée et génère la probabilité pour chaque paire de reprendre le travail. Étant donné que l’objectif est de trouver la meilleure correspondance, un seuil de probabilité de défini par l’utilisateur est utilisé pour filtrer davantage et d’obtenir uniquement les correspondances de bonne.

    Pour cette solution, le principal objectif a été atteindre de faible latence pendant le calcul de score. Toutefois, cette tâche est sollicite même pendant le processus de calcul de score, chaque nouvelle tâche doit être mis en correspondance par rapport à des millions de reprises dans un délai raisonnable. En outre, l’étape d’ingénierie de fonctionnalité produit des fonctionnalités de 2000 par reprise ou de tâche et est un goulot d’étranglement de performances significatifs.

Nous vous suggérons de consulter tous les résultats de la première étude de cas pour déterminer les techniques sont applicables à votre solution et de mesurer leur impact potentiel.

Puis, examinez les résultats de l’étude de cas optimisation score pour savoir comment l’auteur appliqué différentes techniques et optimisé du serveur pour prendre en charge une charge de travail particulier.

## <a name="performance-optimization-process"></a>Processus d’optimisation des performances

Configuration et du paramétrage des performances requiert la création d’une base solide, sur lequel les optimisations de couche conçues pour les charges de travail spécifiques :

- Choisir un serveur approprié pour l’analytique de l’hôte. En règle générale, un rapport secondaire, l’entrepôt de données ou autre serveur est déjà utilisé par d’autres rapports ou l’analytique est préféré. Toutefois, dans une solution de traitement transactionnel analytiques (HTAP) hybride, les données opérationnelles peuvent être utilisées comme entrée à R pour calculer les scores rapide.

- Configurer l’instance de SQL Server pour équilibrer les opérations de moteur de base de données et de R ou Python script d’exécution aux niveaux appropriés. Cela peut inclure la modification des valeurs par défaut de SQL Server pour la mémoire et l’utilisation du processeur, NUMA et les paramètres d’affinité du processeur et la création de groupes de ressources.

- Optimiser l’ordinateur du serveur pour prendre en charge l’utilisation efficace de scripts externes. Cela peut inclure l’augmentation du nombre de comptes utilisés pour l’exécution du script externe, l’activation de gestion des packages, et affectation d’utilisateurs à liées à des rôles de sécurité.

- Application optimisations spécifiques au stockage de données et de transfert dans SQL Server, y compris l’indexation et les stratégies de statistiques, création de requête et l’optimisation des requêtes et la structure des tables qui sont utilisés pour l’apprentissage. Vous pouvez également analyser les sources de données et données modes de transport, ou modifier les processus ETL pour optimiser l’extraction de la fonctionnalité.

- Paramétrer le modèle analytique pour éviter le manque d’efficacité. L’étendue des optimisations possibles dépendent de la complexité de votre code R et les packages et les données que vous utilisez. Tâches clés incluent en éliminant les transformations de données onéreuse ou déchargeant des données de préparation ou de fonctionnalité ingénieries tâches de R ou Python pour les processus ETL ou SQL. Vous pourrez également refactoriser les scripts, éliminer en ligne package installe, divisez le code R dans des procédures distinctes pour l’apprentissage, de calcul de score et d’évaluation ou simplifier le code de travailler plus efficacement comme une procédure stockée.

## <a name="articles-in-this-series"></a>Articles de cette série

+ [Réglage des performances pour R dans SQL Server - matériel](..\r\sql-server-configuration-r-services.md)

    Fournit des conseils pour la configuration du matériel qui [!INCLUDE [ssNoVersion_md](..\..\includes\ssnoversion-md.md)] est installé et pour la configuration de l’instance de SQL Server pour mieux prendre en charge de scripts externes. Il est particulièrement utile pour **les administrateurs de base de données**.

+ [Réglage des performances pour R dans SQL Server - code et les données optimisation](..\r\r-and-data-optimization-r-services.md)

    Fournit des conseils spécifiques sur la façon d’optimiser le script externe pour éviter les problèmes connus. Il est plus utile de **chercheurs de données**.

    [!NOTE]
    > Alors que la plupart des informations de cette section s’applique à R en général, certaines informations sont spécifiques aux fonctions d’analyse RevoScaleR. Guide des performances détaillé ne sont pas disponible pour **revoscalepy** et d’autres prises en charge des bibliothèques de Python.

+ [Réglage des performances pour R dans SQL Server - méthodes et les résultats](..\r\performance-case-study-r-services.md)

    Résume les données qui a été utilisée deux études de cas, comment les performances ont été testées, et l’impact des résultats dans les optimisations.
