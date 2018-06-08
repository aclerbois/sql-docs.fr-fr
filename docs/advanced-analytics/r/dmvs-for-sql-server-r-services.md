---
title: Vues de gestion de données (DMV) pour SQL Server d’apprentissage Services | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: e2180794ca96fc6387105745e346802725afe1dd
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="dmvs-for-sql-server-machine-learning-services"></a>Vues de gestion dynamique pour les Services SQL Server Machine Learning
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

L’article répertorie les affichages catalogue système et les vues de gestion dynamique liées à l’apprentissage automatique dans SQL Server.

Pour plus d’informations sur les événements étendus, consultez [événements étendus pour apprentissage](../../advanced-analytics/r/extended-events-for-sql-server-r-services.md).

> [!TIP]
> Utiliser des rapports intégrés pour les sessions du moniteur apprentissage et l’utilisation du package. Pour plus d’informations, consultez [surveiller apprentissage à l’aide des rapports personnalisés dans Management Studio](../../advanced-analytics/r/monitor-r-services-using-custom-reports-in-management-studio.md).

## <a name="system-configuration-and-system-resources"></a>Configuration du système et des ressources système

Vous pouvez surveiller et analyser les ressources utilisées par les scripts externes à l’aide de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] DMV et affichages catalogue système.

+ [ sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)

  Retourne des informations pour les connexions utilisateur et pour les sessions système. Vous pouvez identifier les sessions système en examinant la colonne *session_id* : les valeurs supérieures ou égales à 51 sont des connexions utilisateur ; les valeurs inférieures à 51 sont des processus système.

+ [sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)

  Retourne une ligne pour chaque compteur de performance système utilisé par le serveur.  Vous pouvez utiliser ces informations pour savoir combien de scripts ont été exécutés, quels scripts ont été exécutés avec quel mode d’authentification ou combien d’appels R ont été émis sur l’instance dans son ensemble.

  Cet exemple récupère uniquement les compteurs associés au script R :

  ```SQL
  SELECT * from sys.dm_os_performance_counters WHERE object_name LIKE '%External Scripts%'
  ```

  Cette DMV présente les compteurs suivants pour les scripts externes par instance :

  + **Nombre total d’exécutions**: nombre de processus externes démarrées par des appels locaux ou distants
  + **Exécutions en parallèle**: nombre de fois où un script inclus le _@parallel_ spécification et que [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] a été en mesure de générer et utiliser un plan de requête parallèle
  + **Diffusion en continu des exécutions**: nombre de fois que la fonctionnalité de diffusion en continu a été appelée.
  + **Exécutions de CC SQL**: nombre d’externes scripts d’exécution où l’appel a été instancié à distance et de SQL Server a été utilisé comme contexte de calcul
  + **Connexions par auth. Connexions**: nombre de fois où un appel de bouclage ODBC a été effectué à l’aide d’implicite de l’authentification ; autrement dit, le [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] exécuté l’appel de la part de l’utilisateur envoie la demande de script
  + **Nombre total de temps d’exécution (ms)**: temps écoulé entre l’appel et l’achèvement de l’appel
  + **Erreurs d’exécution** : nombre de fois où des scripts ont signalé des erreurs. Ce nombre n’inclut pas les erreurs R.


+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)

  Cette DMV présente chaque compte de travail qui exécute actuellement un script externe sur une ligne unique. Notez que le compte de travail est différent des informations d’identification de la personne qui envoie le script. Si un seul utilisateur Windows envoie plusieurs demandes de script, un seul compte de travail est affecté au traitement de toutes les demandes de cet utilisateur. Si un autre utilisateur Windows se connecte pour exécuter un script externe, la demande est traitée par un compte de travail distinct.

  Cette DMV ne retourne pas de résultat si aucun script n’est en cours d’exécution. Elle est donc très utile pour l’analyse des scripts de longue durée. Elle retourne les valeurs suivantes :

  + **external_script_request_id**: un GUID, qui est également utilisé comme le nom temporaire du répertoire de travail utilisé pour stocker les scripts et les résultats intermédiaires
  + **langage**: une valeur telle que `R` qui désigne la langue du script externe
  + **degree_of_parallelism**: un entier qui indique le nombre de parallèle les processus qui ont été utilisés.
  + **external_user_name**: compte Launchpad d’un travail, tel que **SQLRUser01**

+ [sys.dm_external_script_execution_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md)

  Cette DMV est fournie pour la surveillance interne (télémétrie) effectuer le suivi du nombre d’appels script externe est effectué sur une instance. Le service de télémétrie démarre lorsque [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] et incrémente un compteur de basée sur disque chaque appel à un fonction d’apprentissage spécifique.

  Le compteur est incrémenté par un appel à une fonction de suivi spécifique. Par exemple, si `rxLinMod` est appelé et s’exécute en parallèle, le compteur est incrémenté d’une unité.
  
  En règle générale, les compteurs de performance demeurent valides tant que le processus qui les a générés reste actif. Par conséquent, une demande sur une vue de gestion dynamique ne peut pas faire état des données des services qui ne sont plus en cours d’exécution. Par exemple, si le Launchpad crée plusieurs travaux R en parallèle et qu’ils sont exécutés très rapidement puis nettoyés par l’objet de traitement Windows, il est possible qu’une DMV n’affiche aucune donnée.
 
  Toutefois, les compteurs suivis par cette DMV continuent de s’exécuter, et l’état du compteur sys.dm_external_script_execution est préservé par l’utilisation d’écritures sur le disque, même si l’instance est arrêtée.
 
 Pour plus d’informations sur les compteurs de performance système utilisés par [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], consultez [Utiliser des objets SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).

## <a name="resource-governor-views"></a>Vues du gouverneur de ressources

Dans les éditions qui prennent en charge le gouverneur de ressources, la création de pools de ressources externes pour les charges de travail R ou Python peut être efficacement la fr pour suivre et gérer des ressources.

+ [sys.resource_governor_resource_pools](../../relational-databases/system-catalog-views/sys-resource-governor-resource-pools-transact-sql.md)

  Retourne des informations sur l'état et la configuration actuels des pools de ressources, ainsi que sur leurs statistiques.

  > [!IMPORTANT]
  > 
  > Vous devez modifier les pools de ressources qui s’appliquent à d’autres services serveur avant de pouvoir allouer des ressources supplémentaires à R Services.

+ [sys.resource_governor_external_resource_pools](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)

  Nouvelle vue de catalogue qui affiche les valeurs de configuration actuelles des pools de ressources externes.
  Dans l’édition Enterprise, vous pouvez configurer des pools de ressources externes supplémentaires. Vous pouvez par exemple décider de traiter les ressources des travaux R exécutés dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] séparément de celles qui proviennent d’un client distant.

  > [!NOTE]
  > 
  > Dans l’Édition Standard, toutes les tâches de script externe s’exécuter dans le même pool de ressources externes par défaut.

+ [sys.resource_governor_workload_groups](../../relational-databases/system-catalog-views/sys-resource-governor-workload-groups-transact-sql.md)

  Retourne les statistiques de groupe de charges de travail et la configuration actuelle du groupe de charges de travail. Cette vue peut être jointe avec `sys.dm_resource_governor_resource_pools` pour obtenir le nom de pool de ressources.
  Pour les scripts externes, une nouvelle colonne a été ajoutée qui affiche l’ID du pool externe associé au groupe de charges de travail.

+ [sys.dm_resource_governor_external_resource_pool_affinity](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)

  Une nouvelle vue de catalogue système qui vous permet de voir les processeurs et les ressources ayant des affinités avec un pool de ressources particulier.

  Retourne une ligne par planificateur dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] où chaque planificateur est associé à un processeur. Vous pouvez utiliser cette vue pour surveiller la condition d'un planificateur ou pour identifier des tâches d'échappement.

  Dans la configuration par défaut, les pools de charges de travail sont automatiquement affectés à des processeurs ; par conséquent, il n’existe aucune valeur d’affinité à retourner.

  Le planificateur d’affinité mappe le pool de ressources aux planifications [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] identifiées par les ID spécifiés. Ces ID mappage aux valeurs dans le `scheduler_id` colonne `sys.dm_os_schedulers`.


> [!NOTE] 
> 
> Bien que la configuration et la personnalisation des pools de ressources soient disponibles uniquement dans les éditions Enterprise et Developer, les pools par défaut et les DMV sont disponibles dans toutes les éditions. Par conséquent, vous pouvez utiliser ces vues de gestion dynamique dans l’Édition Standard pour déterminer les ressources pour vos tâches de script externe.

Pour obtenir des informations générales sur l’analyse des instances [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], consultez [Affichages catalogue](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md) et [Vues de gestion dynamiques pour Resource Governor](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md).

## <a name="monitoring-script-execution"></a>Surveillance de l’exécution du script

Les scripts R et Python qui s’exécutent dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] sont démarrés par le [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] interface. Toutefois, les ressources du Launchpad ne sont pas régies ou analysées séparément dans la mesure où le Launchpad est supposé être un service sécurisé fourni par Microsoft qui gère les ressources de manière appropriée.

Des scripts qui s’exécutent sous le service Launchpad sont gérés à l’aide de la [objet de traitement Windows](https://msdn.microsoft.com/library/windows/desktop/ms684161.aspx). Avec un objet de traitement, des groupes de processus peuvent être gérés en tant qu’unité. Chaque objet de traitement est hiérarchique et contrôle les attributs de tous les processus qui lui sont associés. Les opérations effectuées sur un objet de traitement affectent tous les processus associés à cet objet.

Par conséquent, si vous avez besoin de mettre fin à une tâche associée à un objet, sachez que tous les processus connexes seront également arrêtés. Si vous exécutez un script R qui est affecté à un objet de traitement Windows et que ce script exécute un travail ODBC qui doit être arrêté, le processus de script R parent est également arrêté.

Si vous démarrez un script externe qui utilise le traitement parallèle, un seul objet de tâche Windows gère tous les processus enfants parallèles.

Pour déterminer si un processus s’exécute dans un travail, utilisez la fonction `IsProcessInJob`.

## <a name="next-steps"></a>Étapes suivantes

[Gestion et surveillance des solutions Machine Learning](../../advanced-analytics/r/managing-and-monitoring-r-solutions.md)
