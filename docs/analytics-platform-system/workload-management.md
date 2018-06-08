---
title: Gestion de la charge de travail dans le système de plateforme Analytique | Documents Microsoft
description: Gestion de la charge de travail dans le système de plateforme d’Analytique.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: a4f748ed39705f865a303f1b59ae352068f93431
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707097"
---
# <a name="workload-management-in-analytics-platform-system"></a>Gestion de la charge de travail dans le système de plateforme Analytique

Fonctionnalités de gestion de la charge de travail de SQL Server PDW permettent aux utilisateurs et aux administrateurs d’affecter des demandes au préalable définir des configurations de mémoire et d’accès concurrentiel. Utiliser la gestion de la charge de travail pour améliorer les performances de votre charge de travail cohérent ou mixte, en permettant aux demandes pour que les ressources appropriées sans priver les demandes indéfiniment.  
  
Par exemple, avec les techniques de gestion de la charge de travail dans SQL Server PDW, vous pouvez :  
  
-   Allouer un grand nombre de ressources à une tâche de chargement.  
  
-   Spécifier plus de ressources pour la création d’un index columnstore.  
  
-   Résoudre les problèmes d’une jointure de hachage exécution lente pour voir si elle a besoin de davantage de mémoire et lui donner plus de mémoire.  
  
## <a name="Basics"></a>Principes fondamentaux de gestion de la charge de travail  
  
### <a name="key-terms"></a>Termes clés  
Gestion de la charge de travail  
*Gestion de la charge de travail* est la capacité à comprendre et ajuster l’utilisation des ressources système pour obtenir de meilleures performances pour les demandes simultanées.  
  
Classe de ressource  
Dans SQL Server PDW, un *classe de ressource* est un rôle de serveur intégré qui a préalablement assigné des limites de mémoire et d’accès concurrentiel. SQL Server PDW alloue des ressources pour les demandes en fonction de l’appartenance du rôle serveur ressources classe de la connexion qui soumet les requêtes.  
  
Sur les nœuds de calcul, l’implémentation de classes de ressources utilise la fonctionnalité gouverneur de ressources dans SQL Server. Pour plus d’informations sur le gouverneur de ressources, consultez [du gouverneur de ressources](http://msdn.microsoft.com/library/bb933866(v=sql.11).aspx) sur MSDN.  
  
### <a name="understand-current-resource-utilization"></a>Comprendre l’utilisation actuelle des ressources  
Pour comprendre l’utilisation des ressources système pour les demandes en cours d’exécution, utilisez les vues de gestion dynamique SQL Server PDW. Par exemple, vous pouvez utiliser des vues de gestion dynamique à comprendre si une jointure de hachage de grande taille à exécution lente peut tirer parti de davantage de mémoire.  
  
<!-- MISSING LINKS
For examples, see [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md).  
-->
  
### <a name="adjust-resource-allocations"></a>Ajuster les Allocations de ressources  
Pour ajuster l’utilisation des ressources, modifier l’appartenance de classe de ressource de la connexion qui soumet la demande. Les rôles de serveur de classe de ressource sont nommées **mediumrc**, **largerc**, et **xlargerc**. Elles représentent respectivement les allocations de ressources, moyenne et très grandes.  
  
Par exemple, pour allouer une grande quantité de ressources système à une demande, ajoutez la connexion qui soumet la demande à la **largerc** rôle de serveur. L’instruction ALTER SERVER ROLE suivante ajoute la connexion Anna au rôle de serveur largerc.  
  
```sql  
ALTER SERVER ROLE largerc ADD MEMBER Anna;  
```  
  
## <a name="RC"></a>Description de classe de ressource  
Le tableau suivant décrit les classes de ressources et leurs affectations de ressources système.  
  
|Classe de ressource|Importance de la demande|L’utilisation de mémoire maximale *|Les emplacements d’accès concurrentiel (maximale = 32)|Description|  
|------------------|----------------------|--------------------------|---------------------------------------|---------------|  
|par défaut|Moyenne|400 MO| 1|Par défaut, chaque connexion est autorisée une petite quantité de mémoire et des ressources d’accès concurrentiel pour ses demandes.<br /><br />Lorsqu’une connexion est ajoutée à une classe de ressource, la nouvelle classe est prioritaire. Lorsqu’une connexion est supprimée à partir de toutes les classes de ressources, la connexion revient à l’allocation de ressources par défaut.|  
|MediumRC|Moyenne|1 200 MO|3|Exemples de requêtes nécessitant de la classe de ressources de support :<br /><br />Les opérations SACT ayant une grande des jointures de hachage.<br /><br />Sélectionnez les opérations que vous avez besoin de davantage de mémoire pour éviter la mise en cache sur le disque.<br /><br />Chargement des données dans les index columnstore en cluster.<br /><br />Génération, la reconstruction et la réorganisation des index columnstore en cluster pour les tables plus petites qui possèdent des colonnes de 10 à 15.|  
|largerc|Élevée|2,8 GO|7|Exemples de requêtes nécessitant de la classe de ressources volumineux :<br /><br />Opérations SACT très volumineuses qui ont des jointures de hachage volumineux, ou contient les agrégations importantes, telles que les clauses ORDER BY ou GROUP BY volumineux.<br /><br />Sélectionnez les opérations qui nécessitent de très grandes quantités de mémoire pour des opérations telles que les jointures de hachage ou des agrégations telles que les clauses ORDER BY ou GROUP BY<br /><br />Chargement des données dans les index columnstore en cluster.<br /><br />Génération, la reconstruction et la réorganisation des index columnstore en cluster pour les tables plus petites qui possèdent des colonnes de 10 à 15.|  
|xlargerc|Élevée|8.4 GO|22|La classe de ressource de très grande taille est pour les demandes qui peuvent nécessiter la consommation des ressources de très grande taille au moment de l’exécution.|  
  
<sup>*</sup>Utilisation de la mémoire maximale est une approximation.  
  
### <a name="request-importance"></a>Importance de la demande  
L’importance de la demande correspond à la quantité de temps processeur qui donne aux requêtes de SQL Server, en cours d’exécution sur les nœuds de calcul.  Demandes à priorité élevée reçoivent plus de temps processeur.  
  
### <a name="maximum-memory-usage"></a>Utilisation de la mémoire maximale  
Utilisation de la mémoire maximale est la quantité maximale de mémoire disponible, qu'une demande peut utiliser au sein de chaque espace de traitement. Par exemple une demande mediumrc peut utiliser jusqu'à 1 200 Mo pour le traitement au sein de chaque point de distribution. Il est toujours important de vérifier les données ne sont pas déformées afin d’éviter d’avoir quelques distributions effectue la plupart du travail.  
  
### <a name="concurrency-slots"></a>Emplacements d’accès concurrentiel  
L’allocation de 1, 3, 7 et les emplacements d’accès concurrentiel 22 vise à permettent aux petites et grandes processus à exécuter en même temps, sans bloquer les petites processus lors de l’exécution d’un processus de grande taille.  Par exemple, SQL Server PDW peut allouer au maximum 32 emplacements d’accès concurrentiel pour exécuter 1 demande de très grande taille (22 emplacements), 1 demande de grande taille (7 connecteurs) et 1 demande de support (3 emplacements) en même temps.  
  
Exemples d’allouer des emplacements d’accès concurrentiel jusqu'à 32 à plusieurs demandes simultanées :  
  
-   28 emplacements = 4 volumineux  
  
-   30 emplacements = 10 moyen  
  
-   les 32 emplacements = 32 par défaut  
  
-   les 32 emplacements = 1 de très nombreuses + 1 moyen volumineux + 1  
  
-   les 32 emplacements = 2 moyen volumineux + 4 + 6 par défaut  
  
Supposons que 6 grandes requêtes sont soumises à SQL Server PDW, puis les 10 requêtes par défaut sont envoyées. SQL Server PDW traitera les demandes dans l’ordre de priorité comme suit :  
  
-   Allouer des emplacements d’accès concurrentiel 28 pour commencer le traitement 4 grandes requêtes comme mémoire devient disponible et maintenir les 2 grandes requêtes dans la file d’attente.  
  
-   Allouer 4 emplacements d’accès concurrentiel pour lancer le traitement des requêtes par défaut 4 et conserver des requêtes par défaut 6 dans la file d’attente.  
  
Comme la fin des demandes et les emplacements d’accès concurrentiel sont disponibles, SQL Server PDW alloue les demandes restantes, en fonction des ressources disponibles et la priorité. Par exemple, lorsqu’il existe 7 concurrence emplacements ouvrir, en attente de grandes requêtes aura une priorité plus élevée pour les emplacements de 7 à en attente de demandes de support.  Si les emplacements de 6 à ouvrir, SQL Server PDW alloue 6 plus de requêtes dimensionné par défaut. Toutefois, les emplacements de mémoire et d’accès concurrentiel doivent toutes être disponibles avant que SQL Server PDW autorise une demande d’exécution.  
  
Dans chaque classe de ressource, les requêtes s’exécutent en premier dans l’ordre premier sorti (FIFO).  
  
## <a name="GeneralRemarks"></a>Remarques d’ordre général  
Si une connexion est membre de plusieurs classes de ressources, la classe avec le plus de ressources est prioritaire.  
  
Lorsqu’une connexion est ajoutée ou supprimée à partir d’une classe de ressource, la modification prend effet immédiatement pour toutes les demandes futures ; demandes en cours qui sont en cours d’exécution ou en attente ne sont pas affectées. La connexion n’a pas besoin de vous déconnecter et vous reconnecter afin que la modification se produise.  
  
Pour chaque connexion, les paramètres de classe de ressource sont appliqués aux opérations et les instructions individuelles et non à la session.  
  
Avant SQL Server PDW exécute une instruction, il essaie d’acquérir les emplacements d’accès concurrentiel nécessaires pour la demande. Si elle ne peut pas acquérir suffisamment emplacements d’accès concurrentiel, SQL Server PDW déplace la demande dans un état d’attente pour exécution. Tous les systèmes de ressources qui étaient déjà allouées à la demande sont retournés dans le système.  
  
La plupart des instructions SQL ont toujours besoin d’allocations de ressources par défaut et par conséquent, n’est pas contrôlée par les classes de ressources. Par exemple, CREATE LOGIN uniquement a besoin d’une petite quantité de ressources et bénéficie des ressources par défaut même si la connexion appelant CREATE LOGIN est un membre d’un une classe de ressource.  Par exemple, si Anna est un membre de la classe de ressource largerc et elle soumet une instruction CREATE LOGIN, l’instruction CREATE LOGIN exécuteront avec le nombre par défaut de ressources.  
  
Instructions SQL et opérations de classes de ressources :  
  
-   ALTER INDEX REBUILD  
  
-   ALTER INDEX REORGANIZE  
  
-   ALTER TABLE REBUILD  
  
-   CRÉER DES INDEX CLUSTER  
  
-   CREATE CLUSTERED COLUMNSTORE INDEX  
  
-   CREATE TABLE AS SELECT  
  
-   CRÉATION À DISTANCE TABLE AS SELECT  
  
-   Chargement des données avec **dwloader**.  
  
-   INSERT-SELECT  
  
-   UPDATE  
  
-   Suppression  
  
-   RESTORE DATABASE lors de la restauration à un dispositif avec plusieurs nœuds de calcul.  
  
-   Sélectionnez, à l’exclusion de la vue de gestion dynamique, seules les requêtes  
  
## <a name="Limits"></a>Limitations et restrictions  
Les classes de ressources régissent les allocations de mémoire et d’accès concurrentiel.  Elles ne déterminent pas les opérations d’entrée/sortie.  
  
## <a name="Metadata"></a>Métadonnées  
Vues de gestion dynamique qui contiennent des informations sur les classes de ressources et les membres de classe de ressource.  
  
-   [sys.server_role_members](../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
-   [sys.server_principals](../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
Vues de gestion dynamique qui contiennent des informations sur l’état des demandes et les ressources que dont ils ont besoin :  
  
-   [sys.dm_pdw_lock_waits](../relational-databases/system-dynamic-management-views/sys-dm-pdw-lock-waits-transact-sql.md)  
  
-   [sys.dm_pdw_resource_waits](../relational-databases/system-dynamic-management-views/sys-dm-pdw-resource-waits-transact-sql.md)  
  
Vues système connexes exposées dans les vues de gestion dynamique SQL Server sur les nœuds de calcul. Consultez [vues de gestion dynamique SQL Server](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) pour des liens vers ces DMV sur MSDN.  
  
-   sys.dm_pdw_nodes_resource_governor_resource_pools  
  
-   sys.dm_pdw_nodes_resource_governor_workload_groups  
  
-   sys.dm_pdw_nodes_resource_governor_resource_pools  
  
-   sys.dm_pdw_nodws_resource_governor_workload_groups  
  
-   sys.dm_pdw_nodes_exec_sessions  
  
-   sys.dm_pdw_nodes_exec_requests  
  
-   sys.dm_pdw_nodes_exec_query_memory_grants  
  
-   sys.dm_pdw_nodes_exec_query_resource_semaphores  
  
-   Sys.dm_pdw_nodes_os_memory_brokers  
  
-   sys.dm_pdw_nodes_os_memory_cache_entries  
  
-   sys.dm_pdw_nodes_exec_cached_plans  
  
## <a name="RelatedTasks"></a>Tâches associées  
[Tâches de gestion de la charge de travail](workload-management-tasks.md)  
  
<!-- MISSING LINKS
See the Workload Management section of [Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md) for the following tasks:  
  
1.  Find the number of active requests for each resource group.  
  
2.  Determine if my requests need more memory  
  
3.  Determine if there are a high number of query optimizations or suboptimal plan generations.  
  
4.  Determine Average CPU time per request in each resource pool to date  
  
## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  
-->
  
