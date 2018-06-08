---
title: Interopérabilité de R dans SQL Server R Services | Microsoft Docs
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 59196e0569ac9cc683b3affa68fc17f068e74994
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="r-interoperability-in-sql-server"></a>Interopérabilité de R dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cette rubrique se concentre sur le mécanisme d’exécution r dans SQL Server et décrit les différences entre Microsoft R et open source R.

S’applique à : SQL Server 2016 R Services, SQL Server 2017 d’apprentissage automatique Services

Pour plus d’informations sur les composants supplémentaires, consultez [nouveaux composants de SQL Server](../../advanced-analytics/r-services/new-components-in-sql-server-to-support-r.md).

### <a name="open-source-r-components"></a>Composants R Open source

[!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] inclut une distribution complète des outils et packages R de base. Pour plus d’informations sur le contenu de la distribution de base, consultez la documentation installée par le programme d’installation à l’emplacement par défaut suivant : `C:\Program Files\Microsoft SQL Server\<instance_name>\R_SERVICES\doc\manual`

Dans le cadre de l’installation de [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)], vous devez accepter les termes du contrat de la licence publique GNU. Cela vous donne le droit d’exécuter ensuite les packages R standard sans autre modification, comme dans n’importe quelle autre distribution open source de R.

[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] n’apporte aucune modification au runtime R. Le runtime R est exécuté hors du processus [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et peut être exécuté indépendamment de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. Toutefois, nous vous recommandons fortement de ne pas exécuter ces outils pendant que [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] utilise R, afin d’éviter les contentions de ressources.

La distribution de packages R de base qui est associée à une instance [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] spécifique se trouve dans le dossier associé à l’instance. Par exemple, si vous avez installé les Services de R sur l’instance par défaut, les bibliothèques R se trouvent dans ce dossier par défaut :

    C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library

De même, les outils R associés à l’instance par défaut se trouvent dans le dossier de thi par défaut :

    C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin

Pour plus d’informations sur la façon dont Microsoft R est différent à partir d’une distribution de base de R que vous pouvez obtenir à partir de CRAN, voir [l’interopérabilité avec le langage R et des produits de Microsoft R et des fonctionnalités](https://docs.microsoft.com/en-us/r-server/what-is-r-server-interoperability)

### <a name="additional-r-packages-from-microsoft-r"></a>Packages R supplémentaires à partir de Microsoft R

En plus de la distribution de R de base, [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] inclut certains packages R propriétaires, ainsi que d’une infrastructure pour l’exécution parallèle de R qui prend également en charge l’exécution de R dans des contextes de calcul à distance.

**Microsoft R** désigne l’ensemble de ces fonctionnalités R, qui combine la distribution R de base et les fonctionnalités et packages R avancés. Si vous installez Microsoft R Server (autonome), vous obtenez exactement le même ensemble de packages qui sont installés avec SQL Server R Services (dans la base de données). La seule différence est le dossier d’installation.

Microsoft R inclut une distribution de la bibliothèque Intel Math Kernel Library, qui est utilisée chaque fois que possible pour accélérer le traitement mathématique. Par exemple, la bibliothèque BLAS (Basic Linear Algebra) est utilisée pour la plupart des packages de composants additionnels, mais aussi pour R lui-même. Pour plus d’informations, consultez ces articles :

+ [Comment le noyau de mathématiques Intel accélère les R](http://blog.revolutionanalytics.com/2014/10/revolution-r-open-mkl.html)
+ [Gains de performance de la liaison de R à des bibliothèques de mathématiques multithread](http://blog.revolutionanalytics.com/2010/06/performance-benefits-of-multithreaded-r.html)

Les packages **RevoScaleR** et **RevoPemaR** sont deux des principales nouveautés dans Microsoft R. Il s’agit de packages R écrits en grande partie en C ou C++ pour optimiser les performances.

+ **RevoScaleR.** Fournit une variété d’API conçues pour la manipulation et l’analyse des données. Les API ont été optimisées pour analyser les jeux de données qui sont trop volumineux pour être stockés en mémoire et pour effectuer des calculs distribués entre plusieurs cœurs ou processeurs.

   Les API RevoScaleR prennent également en charge l’utilisation des sous-ensembles de données pour offrir une plus grande extensibilité. En d’autres termes, la plupart des fonctions RevoScaleR peuvent être exécutées sur des segments de données et utiliser des algorithmes de mise à jour pour agréger les résultats. Cela explique pourquoi les solutions R basées sur les fonctions RevoScaleR prennent en charge les jeux de données de très grande taille, sans dépendre de la mémoire locale.

  Le package RevoScaleR prend également en charge le format de fichier .XDF pour accélérer le transfert et le stockage des données nécessaires pour l’analyse. Le format XDF est un format portable qui utilise le stockage en colonnes. Il permet de charger et manipuler des données de différentes sources, y compris une connexion texte, SPSS ou ODBC. 

+ **RevoPemaR.** PEMA est l’acronyme de « Parallel External Memory Algorithm ». Le package **RevoPemaR** fournit des API utiles pour développer vos propres algorithmes parallèles. Pour plus d’informations, consultez [RevoPemaR Getting Started Guide](https://docs.microsoft.com/r-server/r/how-to-developer-pemar) (Bien démarrer avec RevoPemaR).

Nous vous recommandons également de que vous essayez de [MicrosoftML](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package), un nouveau package à partir de Microsoft R qui prend en charge de l’exécution à distance du code R et évolutifs, de traitement, distribué à l’aide d’algorithmes d’apprentissage améliorée développée par Microsoft Research.

## <a name="next-steps"></a>Étapes suivantes

[Vue d’ensemble de l’architecture](../../advanced-analytics/r/architecture-overview-sql-server-r.md)

[Composants de SQL Server pour prendre en charge R](../../advanced-analytics/r/new-components-in-sql-server-to-support-r.md)

[Vue d’ensemble de la sécurité](../../advanced-analytics/r/security-overview-sql-server-r.md)

