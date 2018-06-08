---
title: Vues de gestion dynamique (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- database scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server], about dynamic management objects
- server state information [SQL Server]
- dynamic management functions [SQL Server]
- metadata [SQL Server], dynamic management objects
- dynamic management views [SQL Server]
- DMVs [SQL Server]
- functions [SQL Server], dynamic management
- server scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server]
ms.assetid: cf893ecb-0bf6-4cbf-ac00-8a1099e405b1
caps.latest.revision: 41
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 65d8a729c1bd96ccee48e687710e7dd06ed209e5
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34708787"
---
# <a name="system-dynamic-management-views"></a>Vues de gestion dynamique système
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Les fonctions et les vues de gestion dynamique renvoient des informations sur l'état du serveur qu'il est possible d'utiliser pour surveiller l'état d'une instance du serveur, diagnostiquer des problèmes et améliorer les performances.  
  
> [!IMPORTANT]  
>  Les fonctions et vues de gestion dynamique renvoient des données d'état internes propres à l'implémentation. Leurs schémas et les données qu'elles renvoient sont susceptibles de changer dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les fonctions et vues de gestion dynamique des futures versions ne seront donc peut-être pas compatibles avec celles de cette version. Par exemple, dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Microsoft peut augmenter la définition de la vue de gestion dynamique en ajoutant des colonnes à la fin de la liste des colonnes. Nous déconseillons l'utilisation de la syntaxe `SELECT * FROM dynamic_management_view_name` dans le code de production car le nombre de colonnes retourné peut changer et altérer votre application.  
  
 Il existe deux types de fonctions et vues de gestion dynamique :  
  
-   les fonctions et vues de gestion dynamique dont l'étendue est définie au niveau du serveur, qui nécessitent l'autorisation VIEW SERVER STATE sur le serveur ;  
  
-   Les fonctions et vues de gestion dynamique dont l'étendue est définie au niveau de la base de données, qui nécessitent l'autorisation VIEW DATABASE STATE sur la base de données.  
  
## <a name="querying-dynamic-management-views"></a>Interrogation des vues de gestion dynamique  
 Il est possible de référencer des vues de gestion dynamique dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en utilisant des noms en deux, trois ou quatre parties. En revanche, il est possible de référencer les fonctions de gestion dynamique dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en utilisant des noms en deux ou trois parties. Il n'est pas possible de référencer les fonctions et vues de gestion dynamique dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en utilisant des noms en une seule partie.  
  
 Toutes les fonctions et vues de gestion dynamique existent dans le schéma sys et respectent la convention de noms dm_*. Lorsque vous utilisez une fonction ou une vue de gestion dynamique, vous devez faire précéder le nom de la vue ou de la fonction du schéma sys. Par exemple, pour effectuer une requête dans la vue de gestion dynamique dm_os_wait_stats, exécutez :  
  
 ```sql
SELECT wait_type, wait_time_ms  
FROM sys.dm_os_wait_stats;  
```  
  
### <a name="required-permissions"></a>Autorisations requises  
 Une requête sur une fonction ou une vue de gestion dynamique nécessite l'autorisation SELECT sur un objet et l'autorisation VIEW SERVER STATE ou VIEW DATABASE STATE. Cela permet de limiter sélectivement l'accès d'un utilisateur ou d'une connexion aux fonctions et vues de gestion dynamique. Pour cela, créez d'abord l'utilisateur dans master, puis refusez à cet utilisateur l'autorisation SELECT sur les fonctions et vues de gestion dynamique auxquelles vous souhaitez interdire l'accès. Après cela, l'utilisateur ne peut pas sélectionner ces fonctions et vues de gestion dynamique, quel que soit le contexte de la base de données de l'utilisateur.  
  
> [!NOTE]  
>  Du fait que DENY est prioritaire, si un utilisateur a les autorisations VIEW SERVER STATE mais n'a pas l'autorisation VIEW DATABASE STATE, il peut visualiser des informations au niveau serveur, mais pas au niveau base de données.  
  
## <a name="in-this-section"></a>Dans cette section  
 Les fonctions et vues de gestion dynamique sont organisées selon les catégories suivantes.  
  
|||  
|-|-|  
|[Toujours sur les vues de gestion dynamique de groupes de disponibilité et de fonctions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)|[Vues de gestion dynamique des tables optimisées en mémoire &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)|  
|[Vues de gestion dynamique liées à la capture des données modifiées &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/2a771d7d-693a-4f56-9227-02cd00e0e200)|[Fonctions et vues de gestion dynamique liées à objet &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)|  
|[Le suivi des modifications liées à des vues de gestion dynamique](http://msdn.microsoft.com/library/dc8a0af9-fcd8-4c34-9453-5132717c9bdb)|[Vues de gestion dynamique liées aux Notifications de requête &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/92eb22d8-33f3-4c17-b32e-e23acdfbd8f4)|  
|[Vues de gestion dynamique liées à Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)|[Vues de gestion dynamique liées à la réplication &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)|  
|[Base de données mise en miroir des vues de gestion dynamique liées &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/04fb21de-1b5e-4a8e-9ca6-1b78ad278db1)|[Le gouverneur de ressources liées de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)|  
|[Vues de gestion dynamique liées à la base de données &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)|[Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)|  
|[Fonctions et vues de gestion dynamique liées à l’exécution &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)|[Fonctions et vues de gestion dynamique relatives au serveur &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)|  
|[Vues de gestion dynamique des événements étendus](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)|[Vues de gestion dynamique liées à Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)|  
|[FileStream et les vues de gestion dynamique FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)|[Fonctions et vues de gestion dynamique liées à des données spatiales &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/c542ac38-451f-43a5-bf8c-4edd38bb738e)|  
|[Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)|[Entrepôt de données SQL et les données en parallèle de l’entrepôt de vues de gestion dynamique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)|  
|[Fonctions et vues de gestion dynamique de géo-réplication &#40;base de données SQL Azure&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)|[Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)|  
|[Fonctions et vues de gestion dynamique liées aux index &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)|[Étirer les vues de gestion dynamique de base de données &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/1193efce-a105-49a9-a8b8-26b063485567)|  
|[I, O les fonctions et vues de gestion dynamique liées &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)|[Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)|  

  
## <a name="see-also"></a>Voir aussi  
 [ACCORDER des autorisations Server &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)   
 [GRANT - Octroyer des autorisations sur une base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md)   
 [Vues système &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)  
  
  
