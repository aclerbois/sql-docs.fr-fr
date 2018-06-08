---
title: Configurer l’option de configuration de serveur max worker threads | Microsoft Docs
ms.custom: ''
ms.date: 11/23/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- worker threads [SQL Server]
- max worker threads option
ms.assetid: abeadfa4-a14d-469a-bacf-75812e48fac1
caps.latest.revision: 36
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 1a6744af68d27716f63a54b93bb83653a0bf3db1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-the-max-worker-threads-server-configuration-option"></a>Configurer l'option de configuration de serveur max worker threads
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **max worker threads** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **max worker threads** configure le nombre de threads de travail disponibles pour les processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les services de thread natifs des systèmes d'exploitation pour qu'un ou plusieurs threads prennent en charge chaque réseau que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge simultanément, qu'un autre thread prenne en charge les points de contrôle de base de données et qu'un pool de threads gère tous les utilisateurs. La valeur par défaut de **Nombre maximum de threads de travail** est 0. Cela permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de configurer automatiquement le nombre de threads de travail au démarrage. Ce paramètre par défaut convient à la plupart des systèmes. Cependant, selon votre configuration système, l'attribution d'une valeur spécifique à l'option **Nombre maximum de threads de travail** permet parfois d'accroître les performances.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option max worker threads, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l'option max worker threads](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Lorsque le nombre de demandes de requêtes est inférieur au nombre défini dans l'option **Nombre maximum de threads de travail**, un thread traite chaque demande de requête. En revanche, si le nombre de demandes de requête dépasse la valeur définie pour l'option **Nombre maximum de threads de travail**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] regroupe les threads de travail afin que le prochain thread de travail disponible puisse traiter la demande.  
  
###  <a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous suspectez un problème de performance, celui ne vient probablement pas de la disponibilité des threads. Le problème est plus vraisemblablement causé par des opérations d’E/S qui contraignent les threads de worker à attendre. Nous vous conseillons d’identifier la cause racine d’un problème de performance avant de changer le paramètre max worker threads.  
  
-   Le regroupement de threads permet d'optimiser les performances lorsque de nombreux clients sont connectés au serveur. Habituellement, un thread de système d'exploitation séparé est créé pour chaque demande de requête. Cependant, s'il existe des centaines de connexions au serveur, l'utilisation d'un thread par demande de requête peut consommer de grandes quantités de ressources système. L'option **Nombre maximum de threads de travail** permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de créer un pool de threads de travail afin de servir un grand nombre de demandes de requête, ce qui améliore les performances.  
  
-   Le tableau ci-dessous montre le nombre maximal de threads de travail automatiquement configuré pour différentes combinaisons d'UC et de versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
    |Nombre d'unités centrales|ordinateur 32 bits|Ordinateur 64 bits|  
    |------------|------------|------------|  
    |\<= 4 processeurs|256|512|  
    |8 processeurs|288|576|  
    |16 processeurs|352|704|  
    |32 processeurs|480|960|  
    |64 processeurs|736|1472|  
    |128 processeurs|4224|4480|  
    |256 processeurs|8320|8576| 
    
    À l’aide de la formule suivante :
    
    |Nombre d'unités centrales|ordinateur 32 bits|Ordinateur 64 bits|  
    |------------|------------|------------| 
    |\<= 4 processeurs|256|512|
    |\>4 processeurs|256 + ((UC logiques - 4) * 8)|512 + ((UC logiques - 4) * 16)| 
  
    > [!NOTE]  
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut plus être installé sur un système d’exploitation 32 bits. Les valeurs d’ordinateur 32 bits sont répertoriées pour aider les clients exécutant [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions antérieures.   Nous vous recommandons d'utiliser 1 024 comme nombre maximal de threads de travail pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée sur un ordinateur 32 bits.  
  
    > [!NOTE]  
    >  Pour obtenir des recommandations concernant l’utilisation de plus de 64 unités centrales, consultez [Recommandations pour l’exécution de SQL Server sur des ordinateurs comportant plus de 64 unités centrales](../../relational-databases/thread-and-task-architecture-guide.md#best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus).  
  
-   Lorsque tous les threads de travail traitent de longues requêtes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut sembler ne plus répondre jusqu'à ce qu'un thread de travail soit terminé et devienne disponible. Même s'il ne s'agit pas d'une défaillance, ce comportement peut parfois être indésirable. Si un processus semble ne pas répondre et si aucune nouvelle requête n'est traitée, connectez-vous à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de la connexion administrateur dédiée (DAC) et terminez le processus. Pour éviter cette situation, augmentez la valeur de l'option max worker threads.  
  
 L'option de configuration du serveur **max worker threads** (nombre maximal de threads de travail) ne prend pas en compte les threads requis pour toutes les tâches système, telles que les groupes de disponibilité, Service Broker, le gestionnaire de verrous et autres. Si le nombre de threads configurés est dépassé, la requête suivante fournit des informations sur les tâches système qui ont engendré les threads supplémentaires.  
  
 ```sql  
 SELECT  s.session_id, r.command, r.status,  
    r.wait_type, r.scheduler_id, w.worker_address,  
    w.is_preemptive, w.state, t.task_state,  
    t.session_id, t.exec_context_id, t.request_id  
 FROM sys.dm_exec_sessions AS s  
 INNER JOIN sys.dm_exec_requests AS r  
    ON s.session_id = r.session_id  
 INNER JOIN sys.dm_os_tasks AS t  
    ON r.task_address = t.task_address  
 INNER JOIN sys.dm_os_workers AS w  
    ON t.worker_address = w.worker_address  
 WHERE s.is_user_process = 0;  
 ```  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="SSMSProcedure"></a> Utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Pour configurer l'option max worker threads  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Processeurs** .  
  
3.  Dans la zone du **nombre maximum de threads de worker**, tapez ou sélectionnez une valeur comprise entre 128 et 32 767.  
  
> [!TIP]
> Utilisez l'option de définition du **nombre maximal de threads de travail** pour définir le nombre de threads de travail disponibles pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le paramètre par défaut de **max worker threads** est adapté à la plupart des systèmes. Cependant, selon votre configuration système, l'attribution d'une valeur plus faible à l'option **max worker threads** (Nombre maximum de threads de travail) permet parfois d'accroître les performances.
> Pour plus d’informations, consultez [Recommandations](#Recommendations) dans cette page.
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Pour configurer l'option max worker threads  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `max worker threads` la valeur `900`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'max worker threads', 900 ;  
GO  
RECONFIGURE;  
GO  
```  
  
##  <a name="FollowUp"></a> Suivi : Après avoir configuré l'option max worker threads  
 Le changement prend effet immédiatement après l’exécution de [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md), sans nécessiter le redémarrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Connexion de diagnostic pour les administrateurs de base de données](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)  
  
  
