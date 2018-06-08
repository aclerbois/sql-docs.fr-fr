---
title: Gouvernance des ressources pour Python dans l’apprentissage de SQL Server | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 88116b7db71e1b9a33815686f2f1ade7561264e9
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="resource-governance-for-python"></a>Gouvernance des ressources pour Python
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Car Python est activée via la même architecture d’extensibilité qui a été implémentée dans le langage R dans SQL Server 2016, vous pouvez utiliser les outils existants dans SQL Server, tels que le gouverneur de ressources, vues de gestion dynamique et les événements étendus, pour surveiller l’exécution de scripts Python dans SQL Server.

Gouvernance des ressources en particulier sont importante, car l’analyse de grandes quantités de données de production peut taxe avancée même matériel.  Pour empêcher les données d’être déplacé en dehors de la base de données sur des ordinateurs qui ne peuvent pas être gérés ou audités, il est important que l’administrateur de base de données alloue suffisamment de ressources pour les opérations d’analytique avancée.

Cette section fournit des informations sur la façon de gérer les ressources utilisées par le runtime Python et contrôler l’utilisation des ressources par Python de scripts de travaux exécutés dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.

> [!NOTE]
> Prise en charge de Python est une nouvelle fonctionnalité de SQL Server 2017 et dans la version préliminaire. Rechercher des informations plus rapidement.
> En règle générale, vous pouvez analyser n’importe quel script externe, y compris un qui s’exécute de Python, à l’aide de la même infrastructure que celle qui a été fournie pour la gouvernance de ressources de scripts R dans SQL Server 2016.

## <a name="what-is-resource-governance"></a>Qu’est-ce que la gouvernance des ressources ?

La gouvernance des ressources vise à identifier et à prévenir les problèmes courants au sein d’un environnement de serveur de base de données, où coexistent souvent plusieurs applications dépendantes et plusieurs services à prendre en charge et à équilibrer. Pour [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], la gouvernance des ressources implique les tâches suivantes :  

+ **Identification des scripts qui utilisent des ressources serveur excessive**. L’administrateur doit pouvoir arrêter ou limiter les travaux qui consomment trop de ressources.

+ **Atténuation des charges de travail imprévisibles**. Si plusieurs travaux Python sont en cours d’exécution simultanément sur le serveur et les travaux ne sont pas isolées des autres à l’aide de pools de ressources, le conflit de ressources qui en résulte peut entraîner des performances imprévisibles ou menacent la fin de la charge de travail.

+ **Hiérarchisation des charges de travail**. L’administrateur ou l’architecte doit pouvoir spécifier les charges de travail prioritaires ou garantir l’achèvement de certaines charges de travail en cas de conflit de ressources.

Vous pouvez utiliser [du gouverneur de ressources](../../relational-databases/resource-governor/resource-governor.md) pour gérer les ressources utilisées par les runtimes externes. Cela inclut les scripts Python démarrées à partir d’une distance terminal mais exécutée à l’aide de l’ordinateur SQL Server en tant que le contexte de calcul.

> [!NOTE] 
> Le gouverneur de ressources est disponible dans l’édition Enterprise.

## <a name="how-to-use-resource-governor-to-manage-python-execution"></a>L’utilisation du gouverneur de ressources pour gérer l’exécution de Python

En général, vous gérez les ressources allouées à une tâche de script externe, en créant un *pool de ressources externes* et l’affectation des charges de travail au pool. Un pool de ressources externe est un nouveau type du pool de ressources introduit dans SQL Server 2016, pour aider à gérer le runtime R et les autres processus externe au moteur de base de données. Il peut être utilisé pour surveiller les travaux de Python à partir de SQL Server 2017.

Il existe trois types de pools de ressources par défaut :

+ Le *pool interne* représente les ressources utilisées par le serveur SQL Server lui-même et ne peut ni être modifié ni être limité.
+ Le *pool par défaut* est un pool d’utilisateurs prédéfini que vous pouvez utiliser pour modifier l’utilisation des ressources pour le serveur dans son ensemble. Vous pouvez aussi définir les groupes d’utilisateurs qui appartiennent à ce pool de façon à gérer l’accès aux ressources.
+ Le *pool externe par défaut* est un pool d’utilisateurs prédéfini pour les ressources externes. Par ailleurs, vous pouvez créer de nouveaux pools de ressources externes et définir les groupes d’utilisateurs qui appartiennent à ces pools.

Vous pouvez même créer des *pools de ressources définis par l’utilisateur* pour allouer des ressources au moteur de base de données ou à d’autres applications, et créer des *pools de ressources externes définis par l’utilisateur* pour gérer des processus R et d’autres processus externes.

Vous trouverez une bonne introduction à la terminologie et aux concepts généraux dans la rubrique [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md).

## <a name="resource-management-using-resource-governor"></a>Gestion de ressources avec Resource Governor

Si vous débutez au gouverneur de ressources, consultez l’article pour une présentation rapide de modifier les ressources de l’instance par défaut et créer un nouveau pool de ressources externes : [comment faire : créer un Pool de ressources pour R](../../advanced-analytics/r-services/how-to-create-a-resource-pool-for-r.md)

Vous pouvez utiliser la *pool de ressources externes* mécanisme permettant de gérer les ressources utilisées par les exécutables pris en charge suivants :

+ Python35.exe lorsqu’appelée à partir de SQL Server ou appelée à distance avec SQL Server en tant que le contexte de calcul à distance
+ BxlServer.exe et les processus satellites
+ Lanceurs démarrés par le Launchpad, tels que PythonLauncher.dll

> [!NOTE]
> La gestion directe du service Launchpad à l’aide du gouverneur de ressources n’est pas pris en charge. Cela s’explique par le fait que [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] est un service approuvé qui peut par sa conception héberger uniquement les lanceurs fournis par Microsoft. Les lanceurs approuvés sont aussi configurés pour éviter de consommer trop de ressources.

Nous vous recommandons de gérer les processus satellites à l’aide de Resource Governor et de les paramétrer en fonction des besoins de la configuration de base de données individuelle et de la charge de travail.  Par exemple, n’importe quel processus satellite individuel peut être créé ou détruit à la demande pendant l’exécution.

## <a name="disable-or-enable-external-script-execution"></a>Désactiver ou activer l’exécution du Script externe

Prise en charge des scripts externes est facultatif dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le programme d’installation et même une fois que le programme d’installation l’exécution du script terminée, externe est définie sur OFF par défaut pour des raisons de sécurité. Par conséquent, après avoir terminé l’installation de l’un de l’apprentissage de langues et les services associés, vous devez reconfigurer explicitement l’instance et puis redémarrez le service de moteur de base de données.

En cas de perte de contrôle scripts, vous pouvez désactiver l’exécution de tous les scripts. Simplement inverse ce processus, puis définissez la propriété `external scripts enabled` FALSE ou 0, sur l’instance. Cela désactive immédiatement toute exécution de script externe. Vous devez réserver cette option pour les problèmes de sécurité ou les situations dans lesquelles un administrateur doit atténuer les problèmes de ressource immédiatement.

## <a name="see-also"></a>Voir aussi

[Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)

