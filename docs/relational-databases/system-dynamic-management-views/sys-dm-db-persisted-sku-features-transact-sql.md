---
title: Sys.dm_db_persisted_sku_features (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_db_persisted_sku_features_TSQL
- sys.dm_db_persisted_sku_features
- dm_db_persisted_sku_features_TSQL
- dm_db_persisted_sku_features
dev_langs:
- TSQL
helpviewer_keywords:
- editions [SQL Server], feature restrictions
- sys.dm_db_persisted_sku_features dynamic management view
ms.assetid: b4b29e97-b523-41b9-9528-6d4e84b89e09
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 25333b8fc57764d2d99a5630d83907e00913d15f
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbpersistedskufeatures-transact-sql"></a>sys.dm_db_persisted_sku_features (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Certaines fonctionnalités de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] modifier la façon dont qui [!INCLUDE[ssDE](../../includes/ssde-md.md)] stocke les informations dans les fichiers de base de données. Ces fonctionnalités sont limitées à des éditions spécifiques de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une base de données qui contient ces fonctionnalités ne peut pas être déplacée vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne les prend pas en charge. Utilisez la vue de gestion dynamique sys.dm_db_persisted_sku_features pour répertorier les fonctionnalités spécifiques à l’édition activées dans la base de données actuelle.
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|feature_name|**sysname**|Nom externe de la fonctionnalité qui est activée dans la base de données, mais qui n'est pas prise en charge dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette fonctionnalité doit être supprimée avant que la base de données puisse être migrée vers toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles.|  
|feature_id|**int**|ID de fonctionnalité associée à la fonctionnalité. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW DATABASE STATE sur la base de données.  
  
## <a name="remarks"></a>Notes  
 Si aucune fonctionnalité qui peut être restreinte par une édition spécifique n’est utilisées par la base de données, la vue ne retourne aucune ligne.  
  
 Sys.dm_db_persisted_sku_features peut répertorier les fonctionnalités suivantes de la modification de base de données comme restreint spécifiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éditions :  
  
-   **ChangeCapture**: indique qu’une base de données a la capture de données modifiées activée. Pour supprimer la capture de données modifiées, utilisez le [sys.sp_cdc_disable_db](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md) procédure stockée. Pour plus d’informations, consultez [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md).  
  
-   **ColumnStoreIndex**: indique au moins une table possède un index columnstore. Pour activer une base de données à déplacer vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne prend pas en charge cette fonctionnalité, utilisez le [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md) ou [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) instruction pour supprimer l’index columnstore. Pour plus d’informations, consultez [Columnstore index](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
    **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).  
  
-   **La compression**: indique qu’au moins une table ou un index utilise la compression de données ou le format de stockage vardecimal. Pour activer une base de données à déplacer vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne prend pas en charge cette fonctionnalité, utilisez le [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) instruction pour supprimer la compression de données. Pour supprimer le format de stockage Vardecimal, exécutez l'instruction sp_tableoption. Pour plus d'informations, consultez [Data Compression](../../relational-databases/data-compression/data-compression.md).  
  
-   **MultipleFSContainers**: indique que la base de données utilise plusieurs conteneurs FILESTREAM. La base de données a un groupe de fichiers FILESTREAM avec plusieurs conteneurs (fichiers). Pour plus d’informations, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
-   **InMemoryOLTP**: indique que la base de données utilise l’OLTP en mémoire. La base de données a un groupe de fichiers MEMORY_OPTIMIZED_DATA. Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
  **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]). 
  
-   **Le partitionnement.** Indique que la base de données contient des tables partitionnées, des index partitionnés, des schémas de partition ou des fonctions de partition. Pour permettre le déplacement d'une base de données vers une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autre que les éditons Enterprise et Developer, il est insuffisant de modifier la table pour qu'elle se trouve sur une partition unique. Vous devez supprimer la table partitionnée. Si la table contient des données, utilisez SWITCH PARTITION pour convertir chaque partition en une table non partitionnée. Supprimez ensuite la table partitionnée, le schéma de partition et la fonction de partition.  
  
-   **TransparentDataEncryption.** Indique qu'une base de données est chiffrée à l'aide du chiffrement transparent des données. Pour supprimer le chiffrement transparent des données, exécutez l'instruction ALTER DATABASE. Pour plus d’informations, consultez [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  

> [!NOTE]
> En commençant par [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] Service Pack 1, ces fonctionnalités sont disponibles sur plusieurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éditions et pas uniquement limité à Enterprise ou Developer Edition.

 Pour déterminer si une base de données utilise des fonctionnalités limitées à des éditions spécifiques, exécutez l'instruction suivante dans la base de données :  
  
```sql  
SELECT feature_name FROM sys.dm_db_persisted_sku_features;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à la base de données &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
