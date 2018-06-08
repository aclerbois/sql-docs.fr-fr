---
title: MSdbms_map (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSdbms_map
- MSdbms_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdbms_map system table
ms.assetid: df67e691-3a50-450a-99c5-8c4a041749ae
caps.latest.revision: 25
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b7a281666290cd18567691cb8b5e02abded8bd16
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msdbmsmap-transact-sql"></a>MSdbms_map (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSdbms_map** tableau contient les informations de type de données source, ainsi qu’un lien vers des informations de type de données de destination par défaut pour les paires DBMS source et de destination. Cette table est stockée dans le **msdb** de base de données et est utilisé pour la publication hétérogène.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**map_id**|**int**|Identifie de manière unique un mappage de type de données.|  
|**src_dbms_id**|**int**|Identifie la source DBMS en spécifiant son **ID DBMS** dans les [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md) table.|  
|**dest_dbms_id**|**int**|Identifie le SGBD de destination en spécifiant son **ID DBMS** dans les [MSdbms](../../relational-databases/system-tables/msdbms-transact-sql.md) table.|  
|**src_datatype_id**|**int**|Identifie la **datatype_id** à partir de la [MSdbms_datatype](../../relational-databases/system-tables/msdbms-datatype-transact-sql.md) table pour le type de source de données.|  
|**src_len_min**|**bigint**|Longueur minimale du type de données dans la source DBMS, où une valeur NULL indique que la longueur n'est pas utilisée.|  
|**src_len_max**|**bigint**|Longueur maximale du type de données dans la source DBMS, où une valeur NULL indique que la longueur n'est pas utilisée.|  
|**src_prec_min**|**bigint**|Précision minimale du type de données dans la source DBMS, où une valeur NULL indique que la précision n'est pas utilisée.|  
|**src_prec_max**|**bigint**|Précision maximale du type de données dans la source DBMS, où une valeur NULL indique que la précision n'est pas utilisée.|  
|**src_scale_min**|**int**|Échelle minimale du type de données dans la source DBMS, où une valeur NULL indique que l'échelle n'est pas utilisée.|  
|**src_scale_max**|**int**|Échelle maximale du type de données dans la source DBMS, où une valeur NULL indique que l'échelle n'est pas utilisée.|  
|**src_nullable**|**bit**|Indique si la colonne de destination dans le mappage autorise les valeurs NULL, où une valeur NULL signifie que cette définition n'est pas requise.|  
|**default_datatype_mapping_id**|**int**|Identifie le mappage de type de données par défaut en spécifiant son **map_id** dans la table [MSdbms_datatype_mapping](../../relational-databases/system-tables/msdbms-datatype-mapping-transact-sql.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Spécifier des mappages de Type de données pour un serveur de publication Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
