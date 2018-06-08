---
title: Créer des flux de travail BI avec R | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: b1c16ac069942af02c2b2d337e887c43d4dff468
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="creating-bi-workflows-with-r"></a>Création de Workflows de BI avec R
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Une base de données est une technologie hautement optimisée permettant de fournir des solutions évolutives de traitement transactionnel, de stockage et d’interrogation des données.

En revanche, généralement des solutions R généralement reposent sur l’importation de données à partir de diverses sources, souvent au format CSV, pour effectuer la modélisation et exploration de données supplémentaire. Ces pratiques sont non seulement inefficaces, mais aussi non sécurisées.

Cet article décrit les scénarios d’intégration de R avec SQL Server qui éviter les risques de sécurité qui peuvent se produire si les solutions d’apprentissage machine sont développées à l’extérieur de la base de données et des pièges courants.

Il fournit également des exemples illustrant comment les applications business intelligence, notamment Integration Services et Reporting Services, peuvent interagir avec le code R et consommer des données ou des graphiques générés par R.

S’applique à : SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services

## <a name="bring-compute-power-to-the-data"></a>Mettre la puissance de calcul pour les données

Un objectif majeur de l’intégration d’apprentissage avec SQL Server a été mettre analytique proches des données. Cela présente plusieurs avantages :

+ Sécurité des données. Placement de R à la source de données permet d’éviter le déplacement des données superflues ou non sécurisé.

+ Vitesse. Les bases de données sont optimisées pour les opérations basées sur un jeu. Les innovations récentes dans les bases de données tels que des tables en mémoire rendent des résumés et les agrégations lightning et sont complètent parfaitement science des données.

+ Facilité de déploiement et l’intégration. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est le point central des opérations de nombreuses autres tâches de gestion des données et applications. À l’aide de données qui résident dans la base de données ou d’un entrepôt de création de rapports, vous assurer que les données utilisées par les solutions d’apprentissage sont cohérentes et à jour. 

+ Efficacité dans le cloud et locales. Au lieu de traiter les données dans R, vous pouvez utiliser les pipelines de données d’entreprise, notamment [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et Azure Data Factory. La création de rapports de résultats ou d’analyses est simple dans Power BI ou [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

En utilisant le bonne combinaison entre SQL et R pour différentes tâches de traitement et d’analyse des données, tant les scientifiques des données que les développeurs peuvent gagner en productivité.

## <a name="use-integration-services-for-data-transformation-and-automation"></a>Utiliser les Services d’intégration pour les données de Transformation et automatisation

Les workflows de science des données sont hautement itératifs et impliquent beaucoup de transformations de données, notamment la mise à l'échelle, les agrégations, le calcul des probabilités, le renommage et la fusion des attributs. Les scientifiques des données sont habitués à effectuer une grande partie de ces tâches dans R, Python ou un autre langage, mais l’exécution de ces flux de travail sur des données d’entreprise nécessite une intégration transparente avec les outils et processus ETL.

[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] vous permet d’effectuer des opérations complexes dans R avec Transact-SQL et les procédures stockées. Vous pouvez donc intégrer des tâches propres à R avec des processus ETL existants quasiment sans nouveau développement. Plutôt que d’effectuer une chaîne de tâches de nécessitant beaucoup de mémoire dans R, la préparation des données peut être optimisée à l’aide des outils les plus efficaces, y compris [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et [!INCLUDE[tsql](../../includes/tsql-md.md)]. 

Voici quelques idées pour comment vous pouvez automatiser le traitement des données et modélisation de pipelines à l’aide de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]:

+ Utilisez [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] tâches pour créer des fonctionnalités de données nécessaires dans la base de données SQL
+ Utiliser le branchement conditionnel afin de changer de contexte de calcul pour les travaux R
+ Exécuter des travaux R qui génèrent leurs propres données dans la base de données et de partager ces données avec des applications
+ Lorsque vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], charger le script R enregistré dans une variable de texte et l’exécuter dans SQL Server

### <a name="examples"></a>Exemples

[Tiens votre projet d’apprentissage machine à l’aide de SQL Server 2016 SSIS et R Services](https://blogs.msdn.microsoft.com/ssis/2016/01/11/operationalize-your-machine-learning-project-using-sql-server-2016-ssis-and-r-services/)  

Ce billet de blog montre des techniques de base pour la manipulation à l’aide du code R [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]: 

+ Appeler le code R à l’aide de la tâche d’exécution SQL, pour générer des données et l’enregistrer dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

+ Utiliser une procédure stockée pour former un modèle R et le stocker dans la base de données

+ Effectuer un calcul de score sur le modèle à l’aide de la tâche de script et de la tâche d’exécution SQL

##  <a name="bkmk_ssrs"></a> Utiliser Reporting Services pour la visualisation

Si R permet de créer des graphiques et offre une visualisation intéressante, il n'est pas bien intégré avec des sources de données externes, ce qui signifie que chaque diagramme ou graphique doit être produit individuellement. Le partage peut également être difficile.

Avec [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], vous pouvez effectuer des opérations complexes dans R avec des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)], qui peuvent facilement être utilisées par divers outils de rapports d’entreprise, y compris [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et Power BI.

+ Visualiser les objets graphiques retournés à partir d'un script R avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
+ Utiliser la table dans Power BI

### <a name="examples"></a>Exemples

[R Graphics Device de Microsoft Reporting Services (SSRS)](https://rgraphicsdevice.codeplex.com/)

Ce projet CodePlex fournit du code pour vous aider à créer un élément de rapport personnalisé qui restitue la sortie graphique de R sous la forme d’une image utilisable dans les rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  Avec l’élément de rapport personnalisé, vous pouvez :

+ Publier des graphiques et tracés créés à l’aide du périphérique graphique de R dans les tableaux de bord [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]

+ Passer des paramètres [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] aux tracés R

> [!NOTE]
> Pour cet exemple, le code qui prend en charge le périphérique d’affichage R pour Reporting Services doit être installé sur le serveur Reporting Services, ainsi que dans Visual Studio. Vous devez également effectuer manuellement la configuration et la compilation.
