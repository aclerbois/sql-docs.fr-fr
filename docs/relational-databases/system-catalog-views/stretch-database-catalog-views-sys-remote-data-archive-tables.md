---
title: Sys.remote_data_archive_tables (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stretch
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.remote_data_archive_tables
- sys.remote_data_archive_tables_TSQL
- remote_data_archive_tables
- remote_data_archive_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_data_archive_tables catalog view
ms.assetid: 765069b7-60fd-414c-875f-3455460b75cd
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5d71c317ef36f254f83af70b3b4a2b2428fcbfa4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stretch-database-catalog-views---sysremotedataarchivetables"></a>Étendre des affichages catalogue de base de données - sys.remote_data_archive_tables
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque table distante qui stocke les données d’une table locale prenant en charge Stretch.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|L’ID d’objet de la table locale prenant en charge Stretch.|  
|**remote_database_id**|**int**|L’identificateur local généré automatiquement de la base de données distante.|  
|**remote_table_name**|**sysname**|Le nom de la table dans la base de données à distance qui correspond à la table locale prenant en charge Stretch.|  
|**filter_predicate**|**nvarchar(max)**|Le prédicat de filtre, éventuellement, qui identifie les lignes de la table à migrer. Si la valeur est null, la table entière peut être migrée.<br /><br /> Pour plus d’informations, consultez [Enable Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) et [sélectionner les lignes à migrer à l’aide d’un prédicat de filtre](~/sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).|  
|**migration_direction**|**tinyint**|La direction dans laquelle données sont en cours de migration. Les valeurs disponibles sont les suivantes :<br/>1 (sortant)<br/>2 (entrant)|  
|**migration_direction_desc**|**nvarchar(60)**|La description de la direction dans laquelle données sont en cours de migration. Les valeurs disponibles sont les suivantes :<br/>sortante (1)<br/>trafic entrant (2)|  
|**is_migration_paused**|**bit**|Indique si la migration est actuellement suspendue.|  
|**is_reconciled**|**bit**| Indique si la table distante et la table SQL Server sont synchronisées.<br/><br/>Lorsque la valeur de **is_reconciled** est 1 (true), la table distante et la table SQL Server sont synchronisées, et vous pouvez exécuter des requêtes qui incluent les données distantes.<br/><br/>Lorsque la valeur de **is_reconciled** est 0 (false), la table distante et la table SQL Server ne sont pas synchronisées. Récemment lignes migrées doivent être migrés à nouveau. Cela se produit lorsque vous restaurez la base de données Azure distante, ou lorsque vous supprimez des lignes manuellement à partir de la table distante. Jusqu'à ce que vous rapprochez les tables, vous ne peut pas exécuter des requêtes qui incluent les données distantes. Pour rapprocher les tables, exécutez [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md). |  
  
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  

