---
title: Résoudre les problèmes de mémoire insuffisante | Microsoft Docs
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f855e931-7502-44bd-8a8b-b8543645c7f4
caps.latest.revision: 18
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: e29658950a9a058214fea4cf9e7bdcfda9daa808
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="resolve-out-of-memory-issues"></a>Résoudre les problèmes de mémoire insuffisante
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[hek_1](../../includes/hek-1-md.md)] , l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il est possible que la quantité de mémoire que vous avez installée et allouée pour [!INCLUDE[hek_2](../../includes/hek-2-md.md)] ne convienne plus à vos besoins croissants. Si c'est le cas, vous risquez de manquer de mémoire. Cette rubrique explique les procédures à mettre en œuvre en cas d'insuffisance de mémoire. Consultez [Analyse et dépannage de l’utilisation de mémoire](../../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md) pour des conseils pouvant vous aider à éviter de nombreuses situations d’épuisement de la mémoire.  
  
## <a name="covered-in-this-topic"></a>Thèmes abordés dans cette rubrique  
  
|Rubrique|Vue d'ensemble|  
|-----------|--------------|  
|[Résoudre les problèmes de restauration de base de données en cas d'insuffisance de mémoire](#bkmk_resolveRecoveryFailures)|Explique la procédure à suivre si vous recevez le message d’erreur « Échec de l’opération de restauration pour la base de données « *\<nom_base_de_données>*  » en raison d’une mémoire insuffisante dans le pool de ressources « *\<nom_pool_de_ressources>*  » ».|  
|[Résoudre les problèmes d'insuffisance de mémoire ayant un impact sur la charge de travail](#bkmk_recoverFromOOM)|Explique la procédure à suivre si vous constatez que les problèmes d'insuffisance de mémoire ont un effet négatif sur les performances.|  
|[Résoudre les échecs d'allocation de pages dus à une mémoire insuffisante alors qu'il y a suffisamment de mémoire à disposition](#bkmk_PageAllocFailure)|Explique la procédure à suivre si vous recevez le message d’erreur « Interdiction des allocations de pages pour la base de données « *\<nom_base_de_données>*  » en raison d’une mémoire insuffisante dans le pool de ressources « *\<nom_pool_de_ressources>*  ». … » lorsque la mémoire disponible est suffisante pour l'opération.|
|[Bonnes pratiques concernant l’utilisation de l’OLTP en mémoire dans un environnement de machine virtuelle](#bkmk_VMs)|Ce qu’il faut savoir avant d’utiliser l’OLTP en mémoire dans un environnement virtualisé.|
  
##  <a name="bkmk_resolveRecoveryFailures"></a> Résoudre les problèmes de restauration de base de données en cas d'insuffisance de mémoire  
 Quand vous tentez de restaurer une base de données, il se peut que le message d’erreur suivant s’affiche : « Échec de l’opération de restauration pour la base de données « *\<nom_base_de_données>*  » en raison d’une mémoire insuffisante dans le pool de ressources « *\<nom_pool_de_ressources>*  » ». Cela indique que le serveur ne dispose pas de suffisamment de mémoire pour la restauration de la base de données. 
   
Le serveur sur lequel vous restaurez une base de données doit disposer de suffisamment de mémoire pour les tables à mémoire optimisée dans la sauvegarde de base de données. Dans le cas contraire, la base de données n’est pas mise en ligne et est marquée comme suspecte.  
  
Si le serveur n’a pas suffisamment de mémoire physique, et que cette erreur persiste, cela peut signifier que les autres processus utilisent trop de mémoire ou indiquer qu’un problème de configuration ne permet pas de disposer d’une quantité de mémoire suffisante pour procéder à la restauration. Pour cette classe de problèmes, prenez les mesures suivantes pour augmenter la quantité de mémoire disponible pour l’opération de restauration : 
  
-   Fermez temporairement les applications en cours d'exécution.   
    En fermant une ou plusieurs applications en cours d’exécution ou en interrompant les services inutiles pour le moment, vous libérez la mémoire utilisée pour la mettre à disposition de l’opération de sauvegarde. Vous pourrez les redémarrer à la fin de la restauration.  
  
-   Augmentez la valeur de MAX_MEMORY_PERCENT.   
    Si la base de données est [liée à un pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md), ce qui est recommandé, la mémoire disponible pour la restauration est régie par MAX_MEMORY_PERCENT. Si la valeur est trop faible, la restauration échouera. Cet extrait de code modifie la valeur de MAX_MEMORY_PERCENT pour le pool de ressources PoolHk à 70 % de la mémoire installée.  
  
    > [!IMPORTANT]  
    > Si le serveur s'exécute sur une machine virtuelle sans être dédié, attribuez à MIN_MEMORY_PERCENT et à MAX_MEMORY_PERCENT la même valeur.   
    > Pour plus d’informations, consultez la rubrique [Bonnes pratiques concernant l’utilisation de l’OLTP en mémoire dans un environnement de machine virtuelle](#bkmk_VMs).  
  
    ```sql  
    -- disable resource governor  
    ALTER RESOURCE GOVERNOR DISABLE  
  
    -- change the value of MAX_MEMORY_PERCENT  
    ALTER RESOURCE POOL PoolHk  
    WITH  
         ( MAX_MEMORY_PERCENT = 70 )  
    GO  
  
    -- reconfigure the Resource Governor  
    --    RECONFIGURE enables resource governor  
    ALTER RESOURCE GOVERNOR RECONFIGURE  
    GO  
  
    ```  
  
     Pour plus d’informations sur les valeurs maximales de MAX_MEMORY_PERCENT, consultez la section [Pourcentage de mémoire disponible pour les tables et index mémoire optimisés](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_PercentAvailable).  
  
-   Augmentez **max server memory**.  
    Pour plus d’informations sur la configuration de **max server memory**, consultez la rubrique [Mémoire du serveur (option de configuration de serveur)](../../database-engine/configure-windows/server-memory-server-configuration-options.md).  
  
##  <a name="bkmk_recoverFromOOM"></a> Résoudre les problèmes d'insuffisance de mémoire ayant un impact sur la charge de travail  
 Évidemment, il est préférable de ne pas se trouver dans une situation d'insuffisance de mémoire. Une planification et une surveillance appropriées permettent souvent d'éviter ce type de problème. Néanmoins, même la meilleure des planifications ne suffit pas toujours pour anticiper ce qui va réellement se produire. Vous risquez donc d'être confronté à un problème d'insuffisance de mémoire à un moment ou un autre. Vous pouvez mettre fin à une situation d'insuffisance de mémoire en procédant aux deux étapes ci-dessous :  
  
1.  [Ouvrir une connexion administrateur dédiée (DAC).](#bkmk_openDAC)  
  
2.  [Prendre une mesure corrective](#bkmk_takeCorrectiveAction)  
  
###  <a name="bkmk_openDAC"></a> Ouvrir une connexion administrateur dédiée (DAC).  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une connexion administrateur dédiée. Cette connexion DAC permet à un administrateur d'accéder à une instance active du moteur de base de données SQL Server afin de résoudre les problèmes sur le serveur, même si ce serveur ne répond pas aux autres connexions clientes. La connexion administrateur dédiée est disponible via l'utilitaire `sqlcmd` et [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Pour obtenir des conseils sur l’utilisation de DAC par le biais de SSMS ou de `sqlcmd`, consultez [Connexion de diagnostic pour les administrateurs de base de données](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md).  
  
###  <a name="bkmk_takeCorrectiveAction"></a> Prendre une mesure corrective  
 Pour résoudre une situation d'insuffisance de mémoire, vous devez libérer de la mémoire existante en réduisant son utilisation ou mettre plus de mémoire à la disposition de vos tables en mémoire.  
  
#### <a name="free-up-existing-memory"></a>Libérer de la mémoire existante  
  
##### <a name="delete-non-essential-memory-optimized-table-rows-and-wait-for-garbage-collection"></a>Supprimer les lignes non essentielles des tables mémoire optimisées et patienter jusqu'au prochain garbage collection  
 Vous pouvez supprimer les lignes non essentielles d'une table mémoire optimisée. Le garbage collector remet à disposition la mémoire utilisée par ces lignes. Le moteur de l'OLTP en mémoire collecte les lignes à nettoyer de façon agressive. Cependant, une transaction longue peut empêcher cette opération de garbage collection. Par exemple, si une transaction s'exécute pendant cinq minutes, les versions de ligne créées par des opérations de mise à jour ou de suppression alors que la transaction était active ne peuvent pas être récupérées par le garbage collector.  
  
##### <a name="move-one-or-more-rows-to-a-disk-based-table"></a>Déplacer une ou plusieurs lignes dans une table sur disque  
 Les articles TechNet suivants donnent des conseils pour déplacer des lignes d'une table mémoire optimisée vers une table sur disque.  
  
-   [Partitionnement au niveau de l'application](../../relational-databases/in-memory-oltp/application-level-partitioning.md)  
  
-   [Modèle d'application pour partitionner des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/application-pattern-for-partitioning-memory-optimized-tables.md)  
  
#### <a name="increase-available-memory"></a>Augmenter la mémoire disponible  
  
##### <a name="increase-value-of-maxmemorypercent-on-the-resource-pool"></a>Augmenter la valeur de MAX_MEMORY_PERCENT sur le pool de ressources  
 Si vous n'avez pas créé de pool de ressources nommé pour les tables en mémoire, vous devez le faire et lier les bases de données de l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)] à ce pool. Consultez la rubrique [Lier une base de données avec des tables mémoire optimisées à un pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md) pour obtenir des conseils sur la création et la liaison de vos bases de données [!INCLUDE[hek_2](../../includes/hek-2-md.md)] à un pool de ressources.  
  
 Si votre base de données de l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)] est liée à un pool de ressources, vous pouvez éventuellement augmenter le pourcentage de la mémoire à laquelle le pool peut accéder. Consultez la sous-rubrique [Modifier MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT sur un pool existant](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_ChangeAllocation) pour obtenir des conseils sur la modification des valeurs MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT pour un pool de ressources.  
  
 Augmentez la valeur de MAX_MEMORY_PERCENT.   
Cet extrait de code modifie la valeur de MAX_MEMORY_PERCENT pour le pool de ressources PoolHk à 70 % de la mémoire installée.  
  
> [!IMPORTANT]  
>  Si le serveur s'exécute sur une machine virtuelle sans être dédié, attribuez à MIN_MEMORY_PERCENT et à MAX_MEMORY_PERCENT la même valeur.   
> Pour plus d’informations, consultez la rubrique [Bonnes pratiques concernant l’utilisation de l’OLTP en mémoire dans un environnement de machine virtuelle](#bkmk_VMs).  
  
```sql  
-- disable resource governor  
ALTER RESOURCE GOVERNOR DISABLE  
  
-- change the value of MAX_MEMORY_PERCENT  
ALTER RESOURCE POOL PoolHk  
WITH  
     ( MAX_MEMORY_PERCENT = 70 )  
GO  
  
-- reconfigure the Resource Governor to enabled it
ALTER RESOURCE GOVERNOR RECONFIGURE  
GO  
```  
  
 Pour plus d’informations sur les valeurs maximales de MAX_MEMORY_PERCENT, consultez la section [Pourcentage de mémoire disponible pour les tables et index mémoire optimisés](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_PercentAvailable).  
  
##### <a name="install-additional-memory"></a>Installer de la mémoire supplémentaire  
 En fin de compte, la meilleure solution, le cas échéant, consiste à installer de la mémoire physique supplémentaire. Si vous choisissez cette solution, souvenez-vous que vous pourrez sans doute augmenter également la valeur de MAX_MEMORY_PERCENT (consultez la sous-rubrique [Modifier MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT sur un pool existant](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_ChangeAllocation)) dans la mesure où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’aura probablement pas besoin de davantage de mémoire. Cela vous permettra d’optimiser cette opération si la nouvelle mémoire installée n’est pas disponible dans son intégralité pour le pool de ressources.  
  
> [!IMPORTANT]  
>  Si le serveur s'exécute sur une machine virtuelle sans être dédié, attribuez à MIN_MEMORY_PERCENT et à MAX_MEMORY_PERCENT la même valeur.   
> Pour plus d’informations, consultez la rubrique [Bonnes pratiques concernant l’utilisation de l’OLTP en mémoire dans un environnement de machine virtuelle](#bkmk_VMs).  
  
##  <a name="bkmk_PageAllocFailure"></a> Résoudre les échecs d'allocation de pages dus à une mémoire insuffisante alors qu'il y a suffisamment de mémoire à disposition  
 Si vous obtenez le message d’erreur `Disallowing page allocations for database '*\<databaseName>*' due to insufficient memory in the resource pool '*\<resourcePoolName>*'. See 'http://go.microsoft.com/fwlink/?LinkId=330673' for more information.` dans le journal des erreurs quand la mémoire physique disponible est suffisante pour allouer la page, il peut être dû à la désactivation de Resource Governor. Lorsque Resource Governor est désactivé, MEMORYBROKER_FOR_RESERVE induit une sollicitation de la mémoire artificielle.  
  
 Pour corriger le problème, vous devez activer Resource Governor.  
  
 Consultez [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md) pour obtenir des informations sur les limites et les restrictions, ainsi que sur l’activation de Resource Governor à l’aide de l’Explorateur d’objets, sur les propriétés de Resource Governor et sur Transact-SQL.  
 
## <a name="bkmk_VMs"></a>Bonnes pratiques concernant l’utilisation de l’OLTP en mémoire dans un environnement de machine virtuelle
La virtualisation de serveur vous permet de réduire les coûts d’exploitation et de capital informatique et d’optimiser l’efficacité informatique, grâce à des processus de configuration, de maintenance, de disponibilité, de sauvegarde et de récupération d’applications optimisés. Avec les progrès technologiques récents, les charges de travail de base de données complexes peuvent être consolidées plus aisément à l'aide de la virtualisation. Cette rubrique porte sur les bonnes pratiques relatives à l’utilisation d’OLTP en mémoire SQL Server dans un environnement virtualisé.

### <a name="memory-pre-allocation"></a>Préallocation de mémoire
Pour la mémoire dans un environnement virtualisé, de meilleures performances et une prise en charge améliorée sont essentielles. Vous devez pouvoir allouer rapidement de la mémoire aux machines virtuelles en fonction des exigences (charges de pointe et creuses) et garantir que la mémoire n'est pas gaspillée. La fonctionnalité de mémoire dynamique d’Hyper-V augmente l’agilité de l’allocation et de la gestion de la mémoire entre les machines virtuelles exécutées sur un hôte.

Certaines bonnes pratiques pour virtualiser et gérer SQL Server ont besoin d’être modifiées lors de la virtualisation d’une base de données avec des tables mémoire optimisées. Sans les tables mémoire optimisées, les deux meilleures pratiques sont les suivantes :
-  Si vous utilisez le paramètre min server memory, il vaut mieux allouer uniquement la quantité de mémoire nécessaire, afin qu’une mémoire suffisante reste disponible pour d’autres processus (évitant ainsi la pagination).
-  Ne définissez pas la préallocation de mémoire sur une valeur trop élevée. Sinon, les autres processus ne disposeront pas de suffisamment de mémoire au moment où ils en ont besoin, ce qui peut entraîner une pagination de mémoire.

Si vous suivez les pratiques ci-dessus lorsque votre base de données possède présente des tables à mémoire optimisée, vous voyez que la tentative de récupération et restauration d’une base de données peut entraîner le blocage de la base de données à l’état « Récupération en attente », même si vous avez suffisamment de mémoire disponible pour la récupérer. En effet, au démarrage, l’OLTP en mémoire met les données en mémoire plus rapidement que l’allocation de mémoire dynamique n’alloue la mémoire nécessaire à la base de données.

### <a name="resolution"></a>Résolution
Pour atténuer ce problème, préallouez suffisamment de mémoire à la base de données pour la récupération ou le redémarrage ; ne vous contentez pas d'une valeur minimale en comptant sur la mémoire dynamique pour fournir la mémoire supplémentaire lorsque cela est nécessaire.
  
## <a name="see-also"></a> Voir aussi  
 [Gestion de la mémoire pour l'OLTP en mémoire](http://msdn.microsoft.com/library/d82f21fa-6be1-4723-a72e-f2526fafd1b6)   
 [Analyse et dépannage de l’utilisation de mémoire](../../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)   
 [Lier une base de données avec des tables mémoire optimisées à un pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)   
 [Guide d’architecture de gestion de la mémoire](../../relational-databases/memory-management-architecture-guide.md)  
 [server memory (options de configuration de serveur)](../../database-engine/configure-windows/server-memory-server-configuration-options.md) 
  
