---
title: Sys.internal_tables (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.internal_tables
- internal_tables
- sys.internal_tables_TSQL
- internal_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- internal tables
- sys.internal_tables catalog view
ms.assetid: a5821c70-f150-4676-8476-3a31f7403dca
caps.latest.revision: 52
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: fe0991279a517f10d3a00f56bc056aa6f0588ef7
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysinternaltables-transact-sql"></a>sys.internal_tables (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne par objet représentant une table interne. Les tables internes sont générées automatiquement par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la prise en charge de différentes fonctionnalités. Par exemple, lorsque vous créez un index XML primaire, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée automatiquement une table interne pour assurer la persistance des données du document XML fragmenté. Tables internes apparaissent dans le **sys** schéma de chaque base de données et avoir des noms uniques, généré par le système qui indique leur fonction, par exemple, **xml_index_nodes_2021582240_32001** ou  **queue_messages_1977058079**  
  
 Les tables internes ne contiennent pas de données accessibles aux utilisateurs et leur schéma est fixe et invariable. Vous ne pouvez pas faire référence à des noms de tables internes dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par exemple, vous ne pouvez pas exécuter une instruction telle que SELECT \* FROM  *\<sys.internal_table_name >*. Par contre, vous pouvez interroger les affichages catalogue pour voir les métadonnées des tables internes.  
  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**\<Colonnes héritent de sys.objects >**||Pour obtenir la liste des colonnes qui hérite de cette vue, consultez [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**internal_type**|**tinyint**|Type de la table interne :<br /><br /> 201 = **queue_messages**<br /><br /> 202 = **xml_index_nodes**<br /><br /> 203 = **fulltext_catalog_freelist**<br /><br /> 205 = **query_notification**<br /><br /> 206 = **service_broker_map**<br /><br /> 207 = **extended_indexes** (par exemple, un index spatial)<br /><br /> 208 = **filestream_tombstone**<br /><br /> 209 = **change_tracking**<br /><br /> 210 = **tracked_committed_transactions**|  
|**internal_type_desc**|**nvarchar(60)**|Description du type de table interne :<br /><br /> QUEUE_MESSAGES<br /><br /> XML_INDEX_NODES<br /><br /> FULLTEXT_CATALOG_FREELIST<br /><br /> FULLTEXT_CATALOG_MAP<br /><br /> QUERY_NOTIFICATION<br /><br /> SERVICE_BROKER_MAP<br /><br /> EXTENDED_INDEXES<br /><br /> FILESTREAM_TOMBSTONE<br /><br /> CHANGE_TRACKING<br /><br /> TRACKED_COMMITTED_TRANSACTIONS|  
|**parent_id**|**int**|ID du parent, qu'il soit ou non de portée de schéma. En l'absence de parent, a pour valeur 0.<br /><br /> **queue_messages** = **object_id** de file d’attente<br /><br /> **xml_index_nodes** = **object_id** de l’index xml<br /><br /> **fulltext_catalog_freelist** = **fulltext_catalog_id** du catalogue de texte intégral<br /><br /> **fulltext_index_map** = **object_id** de l’index de recherche en texte intégral<br /><br /> **query_notification**, ou **service_broker_map** = 0<br /><br /> **extended_indexes** = **object_id** d’un index étendu, tel qu’un index spatial<br /><br /> **object_id** de la table pour la table dans laquelle le suivi est activé = **change_tracking**|  
|**parent_minor_id**|**int**|ID mineur du parent.<br /><br /> **xml_index_nodes** = **index_id** de l’index XML<br /><br /> **extended_indexes** = **index_id** d’un index étendu, tel qu’un index spatial<br /><br /> 0 = **queue_messages**, **fulltext_catalog_freelist**, **fulltext_index_map**, **query_notification**,  **service_broker_map**, ou **change_tracking**|  
|**lob_data_space_id**|**int**|Une valeur différente de zéro représente l'ID d'espace de données (groupe de fichiers ou schéma de partition) qui contient les données LOB de cette table.|  
|**filestream_data_space_id**|**int**|Réservé pour un usage ultérieur.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Notes  
 Les tables internes sont placées dans le même groupe de fichiers que l'entité parente. Vous pouvez utiliser la requête de catalogue présentée dans l'exemple F ci-dessous pour retourner le nombre de pages que les tables internes utilisent pour les données sur ligne, hors ligne et LOB.  
  
 Vous pouvez utiliser la [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md) procédure système pour retourner les données d’utilisation de l’espace pour les tables internes. **sp_spaceused** signale l’espace de table interne des manières suivantes :  
  
-   Si un nom de file d'attente est spécifié, la table interne sous-jacente associée à la file d'attente est référencée et sa consommation de stockage est renseignée.  
  
-   Les pages qui sont utilisés par les tables internes des index XML, les index spatiaux et les index de texte intégral sont inclus dans le **index_size** colonne. Lorsqu’une table ou le nom de la vue indexée est spécifié, les pages de l’index XML, les index spatiaux et les index de recherche en texte intégral pour cet objet sont incluses dans les colonnes **réservé** et **index_size**.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent comment interroger les métadonnées des tables internes à l'aide des affichages catalogue.  
  
### <a name="a-show-internal-tables-that-inherit-columns-from-the-sysobjects-catalog-view"></a>A. Afficher les tables internes qui héritent des colonnes de l'affichage catalogue sys.objects  
  
```  
SELECT * FROM sys.objects WHERE type = 'IT';  
```  
  
### <a name="b-return-all-internal-table-metadata-including-that-which-is-inherited-from-sysobjects"></a>B. Retourner toutes les métadonnées des tables internes (y compris celles qui sont héritées de sys.objects)  
  
```  
SELECT * FROM sys.internal_tables;  
```  
  
### <a name="c-return-internal-table-columns-and-column-data-types"></a>C. Retourner les colonnes et les types de données des colonnes des tables internes  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    ,itab.name AS internal_table_name  
    ,typ.name AS column_data_type   
    ,col.*  
FROM sys.internal_tables AS itab  
JOIN sys.columns AS col ON itab.object_id = col.object_id  
JOIN sys.types AS typ ON typ.user_type_id = col.user_type_id  
ORDER BY itab.name, col.column_id;  
```  
  
### <a name="d-return-internal-table-indexes"></a>D. Retourner les index des tables internes  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    , itab.name AS internal_table_name  
    , idx.*  
FROM sys.internal_tables AS itab  
JOIN sys.indexes AS idx ON itab.object_id = idx.object_id  
ORDER BY itab.name, idx.index_id;  
```  
  
### <a name="e-return-internal-table-statistics"></a>E. Retourner les statistiques des tables internes  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    ,itab.name AS internal_table_name  
    , s.*  
FROM sys.internal_tables AS itab  
JOIN sys.stats AS s ON itab.object_id = s.object_id  
ORDER BY itab.name, s.stats_id;  
```  
  
### <a name="f-return-internal-table-partition-and-allocation-unit-information"></a>F. Retourner les informations relatives aux partitions et aux unités d'allocation des tables internes  
  
```  
SELECT SCHEMA_NAME(itab.schema_id) AS schema_name  
    ,itab.name AS internal_table_name  
    ,idx.name AS heap_or_index_name  
    ,p.*  
    ,au.*  
FROM sys.internal_tables AS itab  
JOIN sys.indexes AS idx  
--     JOIN to the heap or the clustered index  
    ON itab.object_id = idx.object_id AND idx.index_id IN (0,1)  
JOIN   sys.partitions AS p   
    ON p.object_id = idx.object_id AND p.index_id = idx.index_id  
JOIN   sys.allocation_units AS au  
--     IN_ROW_DATA (type 1) and ROW_OVERFLOW_DATA (type 3) => JOIN to partition's Hobt  
--     else LOB_DATA (type 2) => JOIN to the partition ID itself.  
ON au.container_id =    
    CASE au.type   
        WHEN 2 THEN p.partition_id   
        ELSE p.hobt_id   
    END  
ORDER BY itab.name, idx.index_id;  
```  
  
### <a name="g-return-internal-table-metadata-for-xml-indexes"></a>G. Retourner les métadonnées des tables internes pour les index XML  
  
```  
SELECT t.name AS parent_table  
    ,t.object_id AS parent_table_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
    ,xi.name AS primary_XML_index_name  
    ,xi.index_id as primary_XML_index_id  
FROM sys.internal_tables AS it  
JOIN sys.tables AS t   
    ON it.parent_id = t.object_id  
JOIN sys.xml_indexes AS xi   
    ON it.parent_id = xi.object_id  
    AND it.parent_minor_id  = xi.index_id  
WHERE it.internal_type_desc = 'XML_INDEX_NODES';  
GO  
```  
  
### <a name="h-return-internal-table-metadata-for-service-broker-queues"></a>H. Retourner les métadonnées des tables internes pour les files d'attente Service Broker  
  
```  
SELECT q.name AS queue_name  
    ,q.object_id AS queue_id  
    ,it.name AS internal_table_name  
    ,it.object_id AS internal_table_id  
FROM sys.internal_tables AS it  
JOIN sys.service_queues  AS  q ON it.parent_id = q.object_id  
WHERE it.internal_type_desc = 'QUEUE_MESSAGES';  
GO  
```  
  
## <a name="i-return-internal-table-metadata-for-all-service-broker-services"></a>I. Retourner les métadonnées des tables internes pour tous les services Service Broker  
  
```  
SELECT *   
FROM tempdb.sys.internal_tables   
WHERE internal_type_desc = 'SERVICE_BROKER_MAP';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de l’objet &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
