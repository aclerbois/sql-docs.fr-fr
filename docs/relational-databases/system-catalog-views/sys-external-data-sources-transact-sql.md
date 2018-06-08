---
title: Sys.external_data_sources (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 1016db6e-9950-4ae2-a004-bd4171e27359
caps.latest.revision: 7
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 944a8096c20d6ab825503d4db9f824ba0bbe208e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysexternaldatasources-transact-sql"></a>Sys.external_data_sources (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  Contient une ligne pour chaque source de données externe dans la base de données actuelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)], et [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
 Contient une ligne pour chaque source de données externe sur le serveur pour [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
|Nom de la colonne|Type de données| Description|Plage|  
|-----------------|---------------|-----------------|-----------|  
|data_source_id|**int**|ID d’objet pour la source de données externe.||  
|name|**sysname**|Nom de la source de données externe.||  
|location|**nvarchar(4000)**|La chaîne de connexion, qui inclut le protocole, adresse IP et port pour la source de données externe.||  
|type_desc|**nvarchar(255)**|Type de source de données affiché sous forme de chaîne.|HADOOP, RDBMS, SHARD_MAP_MANAGER, RemoteDataArchiveTypeExtDataSource|  
|Type|**tinyint**|Type de source de données affiché sous la forme d’un nombre.|0 - HADOOP<br /><br /> 1 - SGBDR<br /><br /> 2 - SHARD_MAP_MANAGER<br /><br /> 3 - RemoteDataArchiveTypeExtDataSource|  
|resource_manager_location|**nvarchar(4000)**|Tapez HADOOP, l’adresse IP et le port de l’emplacement du Gestionnaire de ressources Hadoop. Cela est utilisé pour l’envoi d’un travail sur une source de données Hadoop.<br /><br /> NULL pour les autres types de sources de données externes.||  
|credential_id|**int**|ID d’objet de la base de données étendue des informations d’identification utilisées pour se connecter à la source de données externe.||  
|database_name|**sysname**|Pour type SGBDR, le nom de la base de données distante. Pour type, SHARD_MAP_MANAGER, le nom de la base de données du Gestionnaire de mappage partitions. NULL pour les autres types de sources de données externes.||  
|shard_map_name|**sysname**|Pour type SHARD_MAP_MANAGER, le nom de la carte de partitions. NULL pour les autres types de sources de données externes.||  
  
## <a name="permissions"></a>Autorisations  
 La visibilité des métadonnées dans les affichages catalogue est limitée aux éléments sécurisables qu'un utilisateur détient ou pour lesquels des autorisations lui ont été accordées. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)   
 [sys.external_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
