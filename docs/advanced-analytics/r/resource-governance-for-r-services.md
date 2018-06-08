---
title: La gouvernance de ressources pour l’apprentissage dans SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: abe130ef7e465326999e0c71ce01e88dfa6269a3
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="resource-governance-for-machine-learning-in-sql-server"></a>Gouvernance de ressources pour l’apprentissage dans SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fournit une vue d’ensemble de la gouvernance de ressources dans SQL Server, les fonctionnalités qui aident à allouer et équilibrer les ressources utilisées par les scripts R et Python.

**S’applique à** : [!INCLUDE[sscurrent-md](../../includes/sscurrent-md.md)]
[!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] et [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)]

## <a name="goals-of-resource-governance-for-machine-learning"></a>Objectifs de la gouvernance de ressources pour l’apprentissage

Un point de problèmes connus avec les langues d’apprentissage machine tels que R et Python est que les données sont déplacées souvent à l’extérieur de la base de données sur des ordinateurs non contrôlées par informatique. Un autre est que R est monothread, ce qui signifie que vous pouvez uniquement utiliser les données disponibles dans la mémoire. 

SQL Server Machine Learning Services atténue ces deux problèmes, et vous aide à répondre aux exigences de conformité d’entreprise. Il conserve analytique avancée à l’intérieur de la base de données et prend en charge des performances sur grands jeux de données via des fonctionnalités telles que la diffusion en continu et opérations de segmentation. Toutefois, le déplacement de R et Python calculs dans les bases de données peut affecter les performances d’autres services qui utilisent la base de données, y compris les requêtes d’utilisateur standard, des applications externes et les travaux planifiés de la base de données.

Cette section fournit des informations sur la façon de gérer les ressources utilisées par les runtimes externes, tels que R et Python, pour atténuer l’impact sur d’autres services de base de données de base. En général, un environnement de serveur de base de données est le hub pour plusieurs applications dépendantes et des services.

Vous pouvez utiliser [du gouverneur de ressources](../../relational-databases/resource-governor/resource-governor.md) pour gérer les ressources utilisées par les runtimes externes pour R et Python.  Pour l’apprentissage, la gouvernance des ressources impliquent les tâches :

+ Identifier les scripts qui utilisent trop de ressources de serveur.
  
     L’administrateur doit pouvoir arrêter ou limiter les travaux qui consomment trop de ressources.
  
+ Atténuer les effets de charges de travail imprévisibles.
  
     Par exemple, si plusieurs tâches d’apprentissage machine sont en cours d’exécution simultanément sur le serveur, le conflit de ressources qui en résulte peut entraîner des performances imprévisibles ou menacent la fin de la charge de travail. Toutefois, si les pools de ressources sont utilisées, les travaux peuvent être isolées des autres.
  
-   Hiérarchiser les charges de travail.
  
     L’administrateur ou un architecte doit être en mesure de spécifier des charges de travail qui doivent sont prioritaires ou garantir certaines charges de travail lorsqu’il existe un conflit de ressources.

## <a name="how-to-use-resource-governor-to-manage-machine-learning"></a>L’utilisation du gouverneur de ressources pour gérer l’apprentissage automatique
 
Gérer les ressources allouées aux sessions R ou Python en créant un *pool de ressources externes*et l’affectation des charges de travail au pool ou des pools. Un pool de ressources externe est un nouveau type du pool de ressources introduit dans [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] pour aider à gérer le runtime R et autres processus externe au moteur de base de données.

SQL Server prend en charge trois types de pools de ressources par défaut : 
  
-   Le *pool interne* représente les ressources utilisées par le serveur SQL Server lui-même et ne peut ni être modifié ni être limité.
  
-   Le *pool par défaut* est un pool d’utilisateurs prédéfini que vous pouvez utiliser pour modifier l’utilisation des ressources pour le serveur dans son ensemble. Vous pouvez aussi définir les groupes d’utilisateurs qui appartiennent à ce pool de façon à gérer l’accès aux ressources.
  
-   Le *pool externe par défaut* est un pool d’utilisateurs prédéfini pour les ressources externes. Par ailleurs, vous pouvez créer de nouveaux pools de ressources externes et définir les groupes d’utilisateurs qui appartiennent à ces pools.
  
 Vous pouvez même créer des *pools de ressources définis par l’utilisateur* pour allouer des ressources au moteur de base de données ou à d’autres applications, et créer des *pools de ressources externes définis par l’utilisateur* pour gérer des processus R et d’autres processus externes.
  
 Vous trouverez une bonne introduction à la terminologie et aux concepts généraux dans la rubrique [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md).

  
## <a name="resource-management-walkthrough-with-resource-governor"></a>Procédure pas à pas gestion de ressources avec le gouverneur de ressources

Si vous débutez au gouverneur de ressources, consultez cette rubrique pour une présentation rapide de modifier les ressources de l’instance par défaut et créer un nouveau pool de ressources externes : [créer un pool de ressources de scripts externes](../../advanced-analytics/r/how-to-create-a-resource-pool-for-r.md)
  
 Vous pouvez utiliser la *pool de ressources externes* mécanisme permettant de gérer les ressources utilisées par les exécutables suivants sont utilisés dans l’apprentissage :

+ Rterm.exe et les processus satellites
+ Processus Python.exe et satellites
+ BxlServer.exe et les processus satellites
+ Processus satellites lancés par Launchpad
  
> [!NOTE]
> 
> La gestion directe du service Launchpad à l’aide du gouverneur de ressources n’est pas pris en charge. Cela s’explique par le fait que [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] est un service approuvé qui peut par sa conception héberger uniquement les lanceurs fournis par Microsoft. Lanceurs approuvés sont configurés pour éviter de consommer trop de ressources.
>   
> Nous vous recommandons de gérer les processus satellites à l’aide de Resource Governor et de les paramétrer en fonction des besoins de la configuration de base de données individuelle et de la charge de travail.  Par exemple, n’importe quel processus satellite individuel peut être créé ou détruit à la demande pendant l’exécution.
  
## <a name="disable-external-script-execution"></a>Désactiver l’exécution du script externe

La prise en charge des scripts externes est optionnelle dans le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Même après l’installation des fonctionnalités d’apprentissage, la possibilité d’exécuter des scripts externes est désactivé par défaut, et vous devez manuellement de reconfigurer la propriété et de redémarrer l’instance pour activer l’exécution du script.

Par conséquent, s’il existe un problème de ressource qui doit être atténuée immédiatement, ou d’un problème de sécurité, un administrateur peut immédiatement désactiver toute exécution de script externe à l’aide de [sp_configure &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) et en définissant la propriété `external scripts enabled` FALSE ou 0.
  
## <a name="see-also"></a>Voir aussi

[Gestion et surveillance des solutions Machine Learning](../../advanced-analytics/r/managing-and-monitoring-r-solutions.md)

[Créer un pool de ressources pour Machine Learning](../../advanced-analytics/r/how-to-create-a-resource-pool-for-r.md)

[Pools de ressources du gouverneur de ressources](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
