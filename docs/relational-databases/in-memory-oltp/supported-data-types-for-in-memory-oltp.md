---
title: Types de données pris en charge pour l’OLTP en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a7380ef0-c9d7-49e4-b6de-fad34752b9f3
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: c4d69ffbb4c8efb71f31fc18009284ad0cfab9e6
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="supported-data-types-for-in-memory-oltp"></a>Types de données pris en charge pour l’OLTP en mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Cet article répertorie les types de données non pris en charge pour les fonctionnalités OLTP en mémoire des éléments suivants :  
  
-   Tables optimisées en mémoire  
  
-   Modules T-SQL compilés en mode natif  
  
## <a name="unsupported-data-types"></a>Types de données non pris en charge  
 Les types de données suivants ne sont pas pris en charge.  
  
||||  
|-|-|-|  
|[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)|[geography &#40;Transact-SQL&#41;](../../t-sql/spatial-geography/spatial-types-geography.md)|[geometry &#40;Transact-SQL&#41;](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md)|  
|[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)|[rowversion &#40;Transact-SQL&#41;](../../t-sql/data-types/rowversion-transact-sql.md)|[xml &#40;Transact-SQL&#41;](../../t-sql/xml/xml-transact-sql.md)|  
|[sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)|Types définis par l'utilisateur|.|  
  
## <a name="notable-supported-data-types"></a>Types de données remarquables pris en charge  
 La plupart des types de données sont pris en charge par les fonctionnalités de l’OLTP en mémoire. Les quelques éléments suivants sont particulièrement intéressants :  
  
|Types binaires et de chaîne|Informations supplémentaires|  
|-----------------------------|--------------------------|  
|binary et varbinary*|[binary et varbinary &#40;Transact-SQL&#41;](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|  
|char et varchar*|[char et varchar &#40;Transact-SQL&#41;](../../t-sql/data-types/char-and-varchar-transact-sql.md)|  
|nchar et nvarchar*|[nchar et nvarchar &#40;Transact-SQL&#41;](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)|  
  
Pour les types de données de chaîne et binaires précédents, à partir de SQL Server 2016 :  
  
- Une table optimisée en mémoire individuelle peut également présenter plusieurs colonnes longues, telles que `nvarchar(4000)`, dont la longueur peut même dépasser la taille physique des lignes de 8 060 octets.  
  
- Une table optimisée en mémoire peut afficher des colonnes de type chaîne et binaire de longueur maximale pour les types de données tels que `varchar(max)`.  


### <a name="identify-lobs-and-other-columns-that-are-off-row"></a>Identifier des objets LOB et d’autres colonnes qui sont hors ligne

À compter de SQL Server 2016, les tables optimisées en mémoire [prennent en charge les colonnes hors ligne](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md), ce qui permet à une ligne de table unique de dépasser 8 060 octets. L’instruction Transact-SQL SELECT suivante signale toutes les colonnes qui sont hors ligne pour des tables optimisées en mémoire. Sachez que :

- Toutes les colonnes de clés d’index sont stockées dans la ligne.
  - Les clés d’index non unique peuvent maintenant inclure des colonnes autorisant les valeurs Null sur des tables optimisées en mémoire.
  - Les index peuvent être déclarés comme uniques sur une table optimisée en mémoire.
- Toutes les colonnes LOB sont stockées hors ligne.
- Une valeur max_length égale à -1 indique une colonne LOB.


```sql
SELECT
        OBJECT_NAME(m.object_id) as [table],
        c.name                   as [column],
        c.max_length
    FROM
             sys.memory_optimized_tables_internal_attributes AS m
        JOIN sys.columns                                     AS c
                ON  m.object_id = c.object_id
                AND m.minor_id  = c.column_id
    WHERE
        m.type = 5;
```


### <a name="other-data-types"></a>Autres types de données


|Autres types|Informations supplémentaires|  
|-----------------|--------------------------|  
|types de tables|[Variables de table optimisée en mémoire](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en charge d'OLTP en mémoire par Transact-SQL](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)   
 [Implémentation de SQL_VARIANT dans un tableau mémoire optimisé](../../relational-databases/in-memory-oltp/implementing-sql-variant-in-a-memory-optimized-table.md)  
 [Taille de la table et des lignes dans une table à mémoire optimisée](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)  
  
  
