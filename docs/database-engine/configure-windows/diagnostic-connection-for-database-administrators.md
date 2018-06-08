---
title: Connexion de diagnostic pour les administrateurs de base de données | Microsoft Docs
ms.custom: ''
ms.date: 10/16/2015
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- server management [SQL Server], connections
- administrator connections [SQL Server]
- ports [SQL Server], DAC
- DAC
- network connections [SQL Server], dedicated administrator
- diagnostic connections [SQL Server]
- connections [SQL Server], dedicated administrator
- ports [SQL Server]
- dedicated administrator connections [SQL Server]
ms.assetid: 993e0820-17f2-4c43-880c-d38290bf7abc
caps.latest.revision: 65
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: a26a318a0675c0c0fe54e31ac8ea33376c1b2e6a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="diagnostic-connection-for-database-administrators"></a>Connexion de diagnostic pour les administrateurs de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit aux administrateurs une connexion de diagnostic spéciale lorsque des connexions standard au serveur sont impossibles. Cette connexion de diagnostic permet aux administrateurs d'accéder à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter des requêtes de diagnostic et résoudre des problèmes, même lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne répond pas à des demandes de connexion standard.  
  
 Cette connexion administrateur dédiée (DAC) prend en charge le chiffrement et d'autres fonctions de sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elle permet uniquement de changer de contexte utilisateur pour un autre administrateur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue chaque tentative pour établir la connexion DAC, mais cela peut échouer dans des situations extrêmes.  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] via [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
## <a name="connecting-with-dac"></a>Connexion avec DAC  
 Par défaut, la connexion est uniquement autorisée à partir d'un client s'exécutant sur le serveur. Les connexions réseau sont autorisées uniquement si elles sont configurées en utilisant la procédure stockée sp_configure avec [l’option remote admin connections](../../database-engine/configure-windows/remote-admin-connections-server-configuration-option.md).  
  
 Seuls les membres du rôle administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent se connecter à l'aide de la connexion DAC.  
  
 La connexion DAC est disponible et prise en charge par le biais de l’utilitaire d’invite de commandes **sqlcmd** , au moyen d’un commutateur d’administrateur spécial (**-A**). Pour plus d’informations sur l’utilisation de **sqlcmd**, consultez [Utiliser sqlcmd avec des variables de script](../../relational-databases/scripting/sqlcmd-use-with-scripting-variables.md). Vous pouvez également vous connecter en ajoutant le préfixe **admin:** au nom de l’instance, selon le format **sqlcmd -S admin:<*nom_instance*>**. Vous pouvez également lancer une session DAC à partir d’un éditeur de requête [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] via une connexion à **admin:\<*nom_instance*>**.  
  
## <a name="restrictions"></a>Restrictions  
 Comme la connexion DAC n'est prévue que pour diagnostiquer des problèmes de serveur dans de rares circonstances, certaines restrictions sont imposées sur la connexion :  
  
-   Pour garantir que des ressources sont disponibles pour la connexion, une seule connexion DAC est autorisée par instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si une connexion DAC est déjà active, toute nouvelle demande de connexion établie par son intermédiaire est refusée avec l'erreur 17810.  
  
-   Pour préserver les ressources, [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] n'écoute pas le port DAC à moins qu'il soit démarré avec l'indicateur de trace 7806.  
  
-   Au départ, la connexion DAC tente une connexion à la base de données par défaut associée à la connexion. Une fois la connexion établie, vous pouvez vous connecter à la base de données master. Si la base de données par défaut est hors connexion ou non disponible pour une raison quelconque, la connexion retourne l'erreur 4060. Cependant, elle réussit si vous remplacez la base de données par défaut pour vous connecter à la base de données master au lieu d'utiliser la commande suivante :  
  
     **sqlcmd –A –d master**  
  
     Nous vous conseillons de vous connecter à la base de données master avec la connexion DAC, car la disponibilité de la base de données master est garantie en cas de démarrage de l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interdit l'exécution de requêtes ou de commandes parallèles avec la connexion DAC. Par exemple, l'erreur 3637 est générée si vous exécutez l'une des instructions ci-dessous avec la connexion DAC :  
  
    -   RESTORE  
  
    -   BACKUP  
  
-   Seules des ressources limitées sont garanties disponibles avec la connexion DAC. N'utilisez pas la connexion DAC pour exécuter des requêtes à utilisation intensive des ressources (par exemple, une jointure complexe sur une grande table) ou des requêtes pouvant créer des blocages. Cela permet d'éviter que la connexion DAC n'amplifie d'éventuels problèmes de serveur existants. Pour éviter les scénarios de blocages potentiels, si vous devez exécuter des requêtes pouvant créer un blocage, exécutez la requête sous des niveaux d'isolement basés si possible sur un instantané ; sinon, réglez le niveau d'isolement des transactions à READ UNCOMMITTED et attribuez à LOCK_TIMEOUT une valeur faible, par exemple 2 000 millisecondes, ou les deux. Vous éviterez ainsi le blocage de la session DAC. Cependant, selon l'état de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la session DAC pourrait se bloquer sur un verrou. La session DAC peut éventuellement être terminée avec Ctrl+C, mais ceci n’est pas garanti. Dans ce cas, la seule option consiste à redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Pour garantir la connectivité et le dépannage avec la connexion DAC, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réserve des ressources limitées pour traiter les commandes exécutées sur la connexion DAC. Ces ressources ne permettent généralement que de simples fonctions de diagnostic et de dépannage, telles que celles répertoriées ci-dessous.  
  
 Même si vous pouvez théoriquement exécuter toute instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui ne doit pas nécessairement être exécutée en parallèle sur la connexion DAC, nous vous recommandons instamment de n'utiliser que les commandes de diagnostic et de dépannage suivantes :  
  
-   Interrogation de vues de gestion dynamique (DMV) pour des diagnostics de base, comme [sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md) pour l’état de verrouillage, [sys.dm_os_memory_cache_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md) pour vérifier l’état des caches, [et sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) et [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) pour les sessions et les demandes actives. Évitez les vues de gestion dynamiques qui consomment beaucoup de ressources (par exemple, [sys.dm_tran_version_store](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md) analyse le magasin de versions complet et peut provoquer des E/S intensives) ou qui utilisent des jointures complexes. Pour plus d'informations sur les implications sur les performances, consultez la documentation relative à la [vue de gestion dynamique](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)spécifique.  
  
-   Interrogation d'affichages catalogue.  
  
-   Les commandes DBCC de base, comme [DBCC FREEPROCCACHE](../..//t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md), [DBCC FREESYSTEMCACHE](../../t-sql/database-console-commands/dbcc-freesystemcache-transact-sql.md), [DBCC DROPCLEANBUFFERS](../../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md) et [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md). Évitez d’exécuter des commandes consommant beaucoup de ressources, comme [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md), [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md) ou [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).  
  
-   Commande [!INCLUDE[tsql](../../includes/tsql-md.md)] KILL*\<spid>*. Selon l'état de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la commande KILL risque de ne pas toujours aboutir ; dans ce cas, la seule option consiste à redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Voici quelques directives générales :  
  
    -   Vérifiez que le SPID a été supprimé en interrogeant `SELECT * FROM sys.dm_exec_sessions WHERE session_id = <spid>`. S'il ne retourne aucune ligne, la session a été supprimée.  
  
    -   Si la session est toujours active, vérifiez si des tâches sont affectées à cette session en exécutant la requête `SELECT * FROM sys.dm_os_tasks WHERE session_id = <spid>`. Si vous y voyez la tâche, votre session est très probablement en cours de fermeture. Notez que cette opération peut prendre beaucoup de temps et risque même d'échouer.  
  
    -   En l'absence de tâches dans le sys.dm_os_tasks associé à cette session, mais si la session est maintenue dans sys.dm_exec_sessions après l'exécution de la commande KILL, vous ne disposez d'aucun thread de travail. Sélectionnez l'une des tâches en cours d'exécution (une tâche répertoriée dans la vue sys.dm_os_tasks avec un `sessions_id <> NULL`), puis supprimez la session qui lui est associée pour libérer le thread de travail. Notez que la suppression d'une session unique ne sera peut-être pas suffisante : vous devrez éventuellement en supprimer plusieurs.  
  
## <a name="dac-port"></a>Port DAC  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est à l'écoute de la connexion DAC sur le port TCP 1434 s'il est disponible ou un port TCP attribué dynamiquement lors du démarrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Le journal des erreurs contient le numéro de port sur lequel la connexion DAC écoute. Par défaut, l'écouteur DAC n'accepte une connexion que sur le port local. Pour voir un exemple de code qui active des connexions d’administration à distance, consultez [remote admin connections (option de configuration de serveur)](../../database-engine/configure-windows/remote-admin-connections-server-configuration-option.md).  
  
 Une fois que la connexion d'administration distante est configurée, l'écouteur DAC est activé sans nécessiter un redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; un client peut maintenant se connecter à la DAC à distance. Vous pouvez permettre à l'écouteur DAC d'accepter des connexions à distance même si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne répond pas en vous connectant d'abord à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant la DAC localement, et en exécutant ensuite la procédure stockée sp_configure pour accepter la connexion à partir de connexions distantes.  
  
 Sur les configurations cluster, la connexion DAC est désactivée par défaut. Les utilisateurs peuvent exécuter l'option remote admin connection de sp_configure pour permettre à l'écouteur DAC d'accéder à une connexion distante. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne répond pas et si l'écouteur DAC n'est pas activé, vous risquez de devoir redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour établir une connexion avec la DAC. Nous recommandons par conséquent d'activer l'option de configuration remote admin connections sur les systèmes en cluster.  
  
 Le port DAC est affecté dynamiquement par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pendant le démarrage. Lors d’une connexion à l’instance par défaut, la DAC évite d’utiliser une demande SSRP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol) à SQL Server Browser Service lors de la connexion. Elle se connecte d'abord sur le port TCP 1434. Si cette tentative échoue, elle effectue un appel SSRP pour obtenir le port. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser n'écoute pas les demandes SSRP, la demande de connexion retourne une erreur. Reportez-vous au journal des erreurs pour trouver le numéro de port que la DAC écoute. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré pour accepter les connexions d'administration distante, la DAC doit être initialisée avec un numéro de port explicite :  
  
 **sqlcmd –S tcp:***\<serveur>,\<port>*  
  
 Le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indique le numéro de port de la connexion DAC, qui est 1434 par défaut. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configuré pour accepter uniquement des connexions DAC locales, connectez-vous au moyen de l'adaptateur de bouclage en utilisant la commande suivante :  
  
 **sqlcmd –S 127.0.0.1,1434**  
  
> [!TIP]  
>  Lors de la connexion à [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] via une connexion DAC, vous devez également spécifier le nom de la base de données dans la chaîne de connexion, à l’aide de l’option -d.  
  
## <a name="example"></a> Exemple  
 Dans cet exemple, un administrateur note que le serveur `URAN123` ne répond pas et souhaite diagnostiquer le problème. Pour ce faire, l'utilisateur active l'utilitaire de ligne de commande `sqlcmd` et se connecte au serveur `URAN123` en utilisant `-A` pour indiquer la connexion DAC.  
  
 `sqlcmd -S URAN123 -U sa -P <xxx> –A`  
  
 L'administrateur peut maintenant exécuter des requêtes pour diagnostiquer le problème et éventuellement terminer les sessions sans réponse.  
  
 Un exemple similaire de connexion à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] repose sur l’utilisation de la commande suivante, qui inclut le paramètre -d pour spécifier la base de données :  
  
 `sqlcmd -S serverName.database.windows.net,1434 -U sa -P <xxx> -d AdventureWorks`  
  
## <a name="related-content"></a>Contenu associé  
 [Utiliser sqlcmd avec des variables de script](../../relational-databases/scripting/sqlcmd-use-with-scripting-variables.md)  
 [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md)  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)  
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)  
 [DBCC CHECKALLOC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md)  
 [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
 [DBCC OPENTRAN &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-opentran-transact-sql.md)  
 [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
 [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
  
  

