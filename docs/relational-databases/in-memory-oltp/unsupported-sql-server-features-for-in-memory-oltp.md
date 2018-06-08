---
title: Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c39f03a7-e223-4fd7-bd30-142e28f51654
caps.latest.revision: 55
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3a7393dfd780eb62aa6dff5ca70d89f297dc6952
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="unsupported-sql-server-features-for-in-memory-oltp"></a>Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Cette rubrique décrit les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne sont pas prises en charge pour une utilisation avec les objets à mémoire optimisée.  
  
## <a name="includessnoversionincludesssnoversion-mdmd-features-not-supported-for-in-memory-oltp"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Fonctionnalités non prises en charge pour l’OLTP en mémoire  

Les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivantes ne sont pas prises en charge sur une base de données qui contient des objets à mémoire optimisée (notamment des groupes de fichiers à mémoire optimisée).  

  
|Fonctionnalités non prises en charge|Description de la fonctionnalité|  
|-------------------------|-------------------------|  
|Compression de données pour les tables à mémoire optimisée.|Vous pouvez utiliser la fonctionnalité de compression de données pour compresser les données dans une base de données et réduire la taille de la base de données. Pour plus d’informations, consultez [Compression de données](../../relational-databases/data-compression/data-compression.md).|  
|Partitionnement des index HASH et des tables à mémoire optimisée, ainsi que des index non cluster.|Les données des tables et des index partitionnés sont divisées en unités qui peuvent être réparties sur plusieurs groupes de fichiers d'une base de données. Pour plus d’informations, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).|  
| REPLICATION | Les configurations de réplication autres que la réplication transactionnelle vers des tables à mémoire optimisée sur les abonnés sont incompatibles avec des tables ou des vues qui référencent des tables à mémoire optimisée.<br /><br />S’il existe un groupe de fichiers à mémoire optimisée, la réplication à l’aide de sync_mode=’database snapshot’ n’est pas prise en charge.<br /><br />Pour plus d’informations, consultez [Abonnés à la réplication de tables optimisées en mémoire](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).|
|Mise en miroir|La mise en miroir de bases de données n’est pas prise en charge pour les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA. Pour plus d’informations sur la mise en miroir, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).|  
|Reconstruire le journal|La reconstruction du journal, via un attachement ou ALTER DATABASE, n'est pas prise en charge pour les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.|  
|Serveur lié|Vous ne peut pas accéder à des serveurs liés dans la même requête ou transaction en tant que tables à mémoire optimisée. Pour plus d’informations, consultez [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md).|  
|Journalisation en bloc|Quel que soit le mode de récupération de la base de données, toutes les opérations sur les tables à mémoire optimisée durables sont toujours entièrement journalisées.|  
|Journalisation minimale|La journalisation minimale n'est pas prise en charge pour les tables à mémoire optimisée. Pour plus d’informations sur la journalisation minimale, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md) et [Prérequis pour une journalisation minimale dans l’importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).|  
|Suivi des modifications|Le suivi des modifications peut être activé sur une base de données avec des objets de l'OLTP en mémoire. Toutefois, les modifications apportées aux tables à mémoire optimisée ne sont pas suivies.|  
| déclencheurs DDL | Les déclencheurs DDL aux niveaux de la base de données et du serveur ne sont pas pris en charge avec les tables OLTP en mémoire ou les modules compilés en mode natif. |  
| Capture de données modifiées (CDC) | La capture de données modifiées ne peut pas être utilisée avec une base de données qui contient des tables à mémoire optimisée, car elle utilise en interne un déclencheur DDL pour exécuter une opération DROP TABLE. |  
| Mode fibre | Le mode fibre n’est pas pris en charge avec les tables à mémoire optimisée :<br /><br />Si le mode fibre est activé, vous ne pouvez pas créer de bases de données avec des groupes de fichiers à mémoire optimisée ni ajouter de tels groupes aux bases de données existantes.<br /><br />Vous pouvez activer le mode fibre s'il existe déjà des bases de données avec des groupes de fichiers à mémoire optimisée. Cependant, l'activation du mode fibre nécessite un redémarrage du serveur. Dans cette situation, les bases de données avec des groupes de fichiers à mémoire optimisée ne peuvent pas être récupérées. Vous voyez un message d’erreur vous suggérant de désactiver le mode fibre pour pouvoir utiliser les bases de données avec des groupes de fichiers à mémoire optimisée.<br /><br />Si le mode fibre est activé, l’attachement et la restauration de bases de données avec des groupes de fichiers à mémoire optimisée ne s’effectuent pas correctement. Les bases de données sont marquées comme suspectes.<br /><br />Pour plus d’informations, consultez [Regroupement léger (option de configuration de serveur)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md). |  
|Limitation de Service Broker|Impossible d'accéder à une file d'attente à partir d'une procédure stockée compilée en mode natif.<br /><br /> Impossible d'accéder à une file d'attente dans une base de données distante, dans une transaction qui accède à des tables à mémoire optimisée.|  
|Réplication sur les abonnés|La réplication transactionnelle vers des tables à mémoire optimisée sur les abonnés est prise en charge, mais avec certaines restrictions. Pour plus d’informations, consultez [Abonnés à la réplication de tables optimisées en mémoire](../../relational-databases/replication/replication-to-memory-optimized-table-subscribers.md).|  


#### <a name="cross-database-queries-and-transcations"></a>Requêtes et transactions de bases de données croisées

À quelques exceptions, les transactions de bases de données croisées ne sont pas prises en charge. Le tableau suivant décrit les cas sont pris en charge, et les restrictions correspondantes. (Voir aussi [Requêtes de bases de données croisées](../../relational-databases/in-memory-oltp/cross-database-queries.md).)  


|Bases de données|Autorisé|Description|  
|---------------|-------------|-----------------|  
| Bases de données utilisateur, **model** et **msdb**. | non | Dans la plupart des cas, les requêtes et transactions de bases de données croisées ne sont *pas* prises en charge.<br /><br />Une requête ne peut pas accéder à d’autres bases de données si la requête utilise une table à mémoire optimisée ou une procédure stockée compilée en mode natif. Cette restriction s’applique aux transactions aussi bien qu’aux requêtes.<br /><br />Les exceptions sont les bases de données système **tempdb** et **MASTER**. La base de données **MASTER** est ici accessible en lecture seule. |
| Base de données **Resource**, **tempdb** | Oui | Dans une transaction qui traite les objets OLTP en mémoire, les bases de données système **Resource** et **tempdb** peuvent être utilisées sans restriction.


## <a name="scenarios-not-supported"></a>Scénarios non pris en charge.  
  
- Accès aux tables à mémoire optimisée à l’aide de la connexion contextuelle depuis des procédures stockées CLR.  
  
- Curseurs de jeu de clés et dynamiques sur les requêtes qui accèdent aux tables à mémoire optimisée. Ces curseurs sont dégradés en statique et en lecture seule.  
  
- L’utilisation de **MERGE INTO***cible*, où *cible* est une table à mémoire optimisée, n’est pas prise en charge.
    - **MERGE USING** *source* est pris en charge pour les tables à mémoire optimisée.  
  
- Le type de données ROWVERSION (TIMESTAMP) n’est pas pris en charge. Pour plus d’informations, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).
  
- La fermeture automatique n’est pas prise en charge par les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.  
  
- Les instantanés de base de données ne sont pas pris en charge par les bases de données avec un groupe de fichiers MEMORY_OPTIMIZED_DATA.  
  
- Une opération DDL transactionnelle, comme CREATE/ALTER/DROP des objets OLTP en mémoire, n’est pas prise en charge dans les transactions utilisateur.  
  
- Notification d'événements  
  
- Gestion basée sur des stratégies (PBM).
    - Les modes Prévention et Journal uniquement de PBM ne sont pas pris en charge. L'existence de ces stratégies sur le serveur peut empêcher l'exécution des déclencheurs DDL de l'OLTP en mémoire. Les modes À la demande et Selon la planification sont pris en charge.  

- Le type de relation contenant-contenu de la base de donnée([bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)) n’est pas pris en charge avec l’OLTP en mémoire.
    - L'authentification de la base de données à relation contenant-contenu est prise en charge. Toutefois, tous les objets OLTP en mémoire sont marqués comme étant des « relations contenant-contenu essentielles » dans la vue de gestion dynamique **dm_db_uncontained_entities**.

  
## <a name="see-also"></a> Voir aussi  

- [Prise en charge d'OLTP en mémoire par SQL Server](../../relational-databases/in-memory-oltp/sql-server-support-for-in-memory-oltp.md)
