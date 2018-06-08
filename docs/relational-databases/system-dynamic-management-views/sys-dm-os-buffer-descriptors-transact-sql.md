---
title: Sys.dm_os_buffer_descriptors (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_os_buffer_descriptors_TSQL
- dm_os_buffer_descriptors_TSQL
- sys.dm_os_buffer_descriptors
- dm_os_buffer_descriptors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_descriptors dynamic management view
ms.assetid: 012aab95-8888-4f35-9ea3-b5dff6e3f60f
caps.latest.revision: 48
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3bae01f30cf7b6af860004f69effb4df44cf3c8b
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmosbufferdescriptors-transact-sql"></a>sys.dm_os_buffer_descriptors (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne des informations sur toutes les pages de données qui se trouvent actuellement dans le pool de mémoires tampons de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le résultat de cet affichage peut être utilisé pour déterminer la répartition des pages de base de données dans le pool de mémoires tampons par base de données, objet ou type. Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], cette vue de gestion dynamique retourne également des informations sur les pages de données dans le fichier d'extension du pool de mémoires tampons. Pour plus d’informations, consultez [Buffer Pool Extension](../../database-engine/configure-windows/buffer-pool-extension.md).  
  
 Lorsqu'une page de données est lue à partir du disque, cette page est copiée dans le pool de mémoires tampons de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et mise en cache en vue de sa réutilisation. Chaque page de données mise en cache est associée à un descripteur de mémoire tampon. Les descripteurs de mémoire tampon identifient de manière unique chaque page de données actuellement mise en cache dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_os_buffer_descriptors retourne les pages mises en cache pour toutes les bases de données utilisateur et système. Cela inclut les pages qui sont associées à la base de données Resource.  
  
> **Remarque :** à appeler à partir [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez le nom **sys.dm_pdw_nodes_os_buffer_descriptors**.  

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Identificateur de la base de données associée à la page dans le pool de mémoires tampons. Autorise la valeur NULL.|  
|file_id|**int**|Identificateur du fichier qui contient l'image persistante de la page. Autorise la valeur NULL.|  
|page_id|**int**|Identificateur de la page dans le fichier. Autorise la valeur NULL.|  
|page_level|**int**|Niveau d'index de la page. Autorise la valeur NULL.|  
|allocation_unit_id|**bigint**|Identificateur de l'unité d'allocation de la page. Cette valeur peut être utilisée pour la jointure de sys.allocation_units. Autorise la valeur NULL.|  
|page_type|**nvarchar(60)**|Type de la page, par exemple page de données ou page d'index. Autorise la valeur NULL.|  
|row_count|**int**|Nombre de lignes dans la page. Autorise la valeur NULL.|  
|free_space_in_bytes|**int**|Quantité d'espace disponible dans la page, en octets. Autorise la valeur NULL.|  
|is_modified|**bit**|1 = la page a été modifiée après avoir été lue sur le disque. Autorise la valeur NULL.|  
|numa_node|**int**|Nœud NUMA (Nonuniform Memory Access) pour la mémoire tampon. Autorise la valeur NULL.|  
|read_microsec|**bigint**|Temps réel (en microsecondes) requis pour lire la page dans la mémoire tampon. Ce nombre est réinitialisé lorsque la mémoire tampon est réutilisée. Autorise la valeur NULL.|  
|is_in_bpool_extension|**bit**|1 = Page est dans l’extension du pool de mémoires tampons. Autorise la valeur NULL.|  
|pdw_node_id|**int**|**S’applique aux**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> L’identificateur du nœud qui se trouve sur cette distribution.|  
  
## <a name="permissions"></a>Autorisations  

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
   
## <a name="remarks"></a>Notes  
 Sys.dm_os_buffer_descriptors retourne les pages qui sont utilisés par la base de données de la ressource. Sys.dm_os_buffer_descriptors ne retourne pas d’informations sur les pages libres ou occultées, ni sur les pages qui contenaient des erreurs lors de leur lecture.  
  
|From|Pour|Le|Relation|  
|----------|--------|--------|------------------|  
|sys.dm_os_buffer_descriptors|sys.databases|database_id|plusieurs-à-un|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.allocation_units|allocation_unit_id|plusieurs-à-un|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.database_files|file_id|plusieurs-à-un|  
|sys.dm_os_buffer_descriptors|sys.dm_os_buffer_pool_extension_configuration|file_id|plusieurs-à-un|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-cached-page-count-for-each-database"></a>A. Retour du nombre de pages mises en cache pour chaque base de données  
 L'exemple suivant retourne le nombre de pages chargées pour chaque base de données.  
  
```  
SELECT COUNT(*)AS cached_pages_count  
    ,CASE database_id   
        WHEN 32767 THEN 'ResourceDb'   
        ELSE db_name(database_id)   
        END AS database_name  
FROM sys.dm_os_buffer_descriptors  
GROUP BY DB_NAME(database_id) ,database_id  
ORDER BY cached_pages_count DESC;  
```  
  
### <a name="b-returning-cached-page-count-for-each-object-in-the-current-database"></a>B. Renvoi du nombre de pages mises en cache pour chaque objet de la base de données active  
 L'exemple suivant retourne le nombre de pages chargées pour chaque objet de la base de données active.  
  
```  
SELECT COUNT(*)AS cached_pages_count   
    ,name ,index_id   
FROM sys.dm_os_buffer_descriptors AS bd   
    INNER JOIN   
    (  
        SELECT object_name(object_id) AS name   
            ,index_id ,allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.hobt_id   
                    AND (au.type = 1 OR au.type = 3)  
        UNION ALL  
        SELECT object_name(object_id) AS name     
            ,index_id, allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.partition_id   
                    AND au.type = 2  
    ) AS obj   
        ON bd.allocation_unit_id = obj.allocation_unit_id  
WHERE database_id = DB_ID()  
GROUP BY name, index_id   
ORDER BY cached_pages_count DESC;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 
 [Vues de gestion dynamique liées à système d’exploitation SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Base de données Resource](../../relational-databases/databases/resource-database.md)   
 [Sys.dm_os_buffer_pool_extension_configuration & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)  
  
  


