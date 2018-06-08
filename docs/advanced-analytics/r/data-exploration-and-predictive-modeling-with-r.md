---
title: Exploration de données et modélisation prédictive avec R dans l’apprentissage de SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 9f808c2fffe0b008590ae1eaac51124471c02e5d
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="data-exploration-and-predictive-modeling-with-r-in-sql-server"></a>Exploration de données et modélisation prédictive avec R dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article décrit les améliorations apportées au processus de science des données qui sont possibles via l’intégration avec SQL Server.

S’applique à : SQL Server 2016 R Services, SQL Server 2017 Machine Learnign Services

## <a name="the-data-science-process"></a>Le processus de science des données

Les spécialistes de données adoptent le langage R pour explorer les données et créer des modèles prédictifs. C’est généralement un processus itératif d’essais et d’erreurs qui s’exécute jusqu’à obtenir un bon modèle prédictif. En tant que spécialiste des données, vous pouvez vous connecter à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et extraire les données vers votre poste de travail local à l’aide du package RODBC, explorer vos données et créer un modèle prédictif à l’aide des packages R standard.

Toutefois, cette approche présente plusieurs inconvénients, qui possèdent rigoureuse l’adoption plus large de R dans l’entreprise. 

+ Le déplacement des données peuvent être lent, inefficace ou non sécurisé
+ R lui-même présente des limitations de performances et évolutivité

Ces inconvénients deviennent plus évidents lorsque vous avez besoin de déplacer et d’analyser de grandes quantités de données ou d’utiliser des jeux de données plus grands que la mémoire disponible de votre ordinateur.

La nouvelle, évolutives et les fonctions R incluses avec [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] vous aider à résoudre la plupart de ces défis. 

## <a name="whats-different-about-revoscaler"></a>Quelle est la différence sur RevoScaleR ?

Le package **RevoScaleR** contient des implémentations de certaines fonctions R plus populaires, qui ont été repensées pour fournir les fonctionnalités de parallélisme et mise à l’échelle. Pour plus d’informations, consultez [Distributed Computing à l’aide de RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing).

Le package RevoScaleR prend également en charge la modification du *contexte d’exécution*. Cela signifie que, pour une solution entière ou simplement une fonction, vous pouvez indiquer que les calculs doivent être effectués à l’aide des ressources de l’ordinateur qui héberge l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et non pas celles de votre poste de travail local. Cela présente plusieurs avantages : vous n’avez pas à déplacer des données inutiles et vous pouvez tirer parti des ressources de calcul supérieures sur l’ordinateur serveur.

## <a name="r-environment-and-packages"></a>Packages et environnement R

L’environnement R pris en charge dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] se compose d’un runtime, du langage open source et d’un moteur graphique pris en charge et étendu par plusieurs packages. Le langage autorise l’utilisation d’extensions qui sont implémentées à l’aide de packages.  

### <a name="using-other-r-packages"></a>À l’aide d’autres Packages R

En plus des bibliothèques R propriétaires inclus avec Microsoft Machine Learning, vous pouvez utiliser presque toutes les packages R dans votre solution, y compris :

+ Packages R génériques de référentiels publics. Vous pouvez obtenir les packages R open source les plus populaires auprès de référentiels publics, tels que le CRAN, lequel héberge plus de 6 000 packages utilisables par les scientifiques des données.
  
  Pour la plateforme Windows, les packages R sont fournis sous forme de fichiers .zip, qui peuvent être téléchargés et installés avec la licence GPL.  
  
  Pour plus d’informations sur l’installation des packages tiers compatibles avec [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], consultez [Install Additional R Packages on SQL Server](../../advanced-analytics/r/install-additional-r-packages-on-sql-server.md)  
  
+ Autres packages et bibliothèques fournis par [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)].   
  
     Le package **RevoScaleR** comprend des fonctions avancées d’analytique de Big Data, versions améliorées des fonctions qui prennent en charge les tâches courantes de la science des données, les algorithmes d’apprentissage optimisés de Naive Bayes, la régression linéaire, les modèles de série chronologique et les réseaux neuronaux, ainsi que les bibliothèques de mathématiques avancées.  
  
     Le package **RevoPemaR** vous permet de développer vos propres algorithmes de mémoire externe parallèle dans R.  
  
     Pour plus d’informations sur ces packages et leur utilisation, consultez [What ' s RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler-user-guide-introduction) et [prise en main RevoPemaR](https://msdn.microsoft.com/microsoft-r/pemar-getting-started). 

+ **MicrosoftML** contient une collection d’algorithmes d’apprentissage automatique hautement optimisée et les transformations de données à partir de l’équipe de science des données de Microsoft. Un grand nombre des algorithmes sont également utilisés dans Azure Machine Learning. Pour plus d’informations, consultez [à l’aide du MicrosoftML Package](../../advanced-analytics/using-the-microsoftml-package.md).

### <a name="r-development-tools"></a>Outils de développement R

Lorsque vous développez votre solution R, veillez à télécharger le Client Microsoft R. Ce téléchargement gratuit comprend les bibliothèques nécessaires pour prendre en charge des contextes de calcul à distance et évolutive alorithms :

+ **[!INCLUDE[rsql_rro-noversion](../../includes/rsql-rro-noversion-md.md)] :** distribution du runtime R et ensemble de packages, tels que la bibliothèque Intel Math Kernel Library, qui améliore les performances des opérations standard de R.  
  
+ **RevoScaleR :** package R qui permet de transférer (pushing) des calculs à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[rsql_rre-noversion](../../includes/rsql-rre-noversion-md.md)]. Il inclut également un ensemble de fonctions R courantes qui ont été repensées pour offrir de meilleures performances et une plus grande scalabilité. Le préfixe **rx** identifie ces fonctions améliorées. Il inclut aussi des fournisseurs de données améliorées pour diverses sources. Ces fonctions ont le préfixe **Rx**.

Vous pouvez utiliser n’importe quel éditeur de code basé sur Windows qui prend en charge de R, tels que [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)] ou RStudio. Le téléchargement de [!INCLUDE[rsql_rro-noversion](../../includes/rsql-rro-noversion-md.md)] inclut également des outils en ligne de commande courants pour R, comme RGui.exe.

## <a name="use-new-data-sources-and-compute-contexts"></a>Utilisation des Sources de données et les contextes de calcul

Lorsque vous utilisez le package RevoScaleR pour vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], recherchez ces fonctions à utiliser dans votre code R :

+ **RxSqlServerData** est fournie dans le package RevoScaleR pour la prise en charge de la connectivité de données améliorée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
     Utilisez cette fonction dans votre code R pour définir la *source de données*. L’objet de source de données spécifie le serveur et les tables où les données résident et gère la tâche de lecture et d’écriture de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
-   Le package **RxInSqlServer** permet de spécifier le *contexte de calcul*.  En d’autres termes, vous pouvez indiquer où le code R doit être exécuté : sur votre station de travail locale ou sur l’ordinateur qui héberge l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  Pour plus d’informations, consultez [fonctions RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler).
  
     Quand vous définissez le contexte de calcul, cela s’applique uniquement aux calculs qui prennent en charge le contexte d’exécution à distance, c’est-à-dire les opérations R fournies par le package RevoScaleR et les fonctions associées. En règle générale, les solutions R basées sur les packages CRAN standard ne peuvent pas s’exécuter dans un contexte de calcul à distance, mais elles peuvent être exécutées sur l’ordinateur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] si elles sont démarrées par T-SQL. Vous pouvez toutefois utiliser la fonction `rxExec` pour appeler des fonctions R de façon individuelle et les exécuter à distance dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

Pour obtenir des exemples montrant comment créer et utiliser des sources de données et les contextes d’exécution, consultez les didacticiels :

+ [Immersion dans la science des données](../../advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)  
+  [Analyse des données à l’aide de Microsoft R](https://msdn.microsoft.com/en-us/microsoft-r/data-analysis-in-microsoft-r)

## <a name="deploy-r-code-to-production"></a>Déployer le Code R en Production

La science des données consiste principalement à échanger des analyses entre spécialistes ou à utiliser des modèles prédictifs pour les résultats ou les processus métier. Dans [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], il est facile de passer en production lorsque votre script ou modèle R est prêt.

Pour plus d’informations sur la migration de votre code vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Operationalizing Your R Code](../../advanced-analytics/r/operationalizing-your-r-code.md).

En général, le processus de déploiement commence par nettoyer votre script afin d’éliminer le code qui n’est pas nécessaire en production. Lorsque vous déplacez des calculs plus proche pour les données, vous constaterez peut-être façons de déplacer plus efficacement, résumer ou présenter des données que tous les éléments de R.  Nous recommandons que les spécialistes des données, consultez avec un développeur de base de données sur les façons d’améliorer les performances, surtout si la solution de nettoyage de données ou une fonctionnalité d’ingénierie qui peut être plus efficace dans SQL. Les processus ETL peuvent nécessiter des modifications qui vous assurent que les flux de travail de construction ou d’évaluation d’un modèle n’échouent pas et que les données d’entrée sont disponibles dans le format approprié.

## <a name="see-also"></a>Voir aussi

[Comparaison des fonctions Base R et ScaleR](https://msdn.microsoft.com/microsoft-r/scaler/compare-base-r-scaler-functions)

[Fonctions ScaleR pour travailler avec des données SQL Server](../../advanced-analytics/r/scaler-functions-for-working-with-sql-server-data.md)
