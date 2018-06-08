---
title: Sys.dm_db_stats_histogram (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sys.dm_db_stats_histogram
- sys.dm_db_stats_histogram_TSQL
- dm_db_stats_histogram
- dm_db_stats_histogram_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_stats_histogram dynamic management function
ms.assetid: 1897fd4a-8d51-461e-8ef2-c60be9e563f2
caps.latest.revision: 11
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: af1930b1cd5f8536c9e9f196a8ea739538042ca0
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmdbstatshistogram-transact-sql"></a>Sys.dm_db_stats_histogram (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

Retourne l’histogramme des statistiques pour l’objet de base de données spécifiée (table ou vue indexée) dans le courant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données. Semblable à `DBCC SHOW_STATISTICS WITH HISTOGRAM`.

> [!NOTE] 
> Ce DMF est disponible à partir de [!INCLUDE[ssSQL15](../../includes/ssSQL15-md.md)] SP1 CU2

## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_db_stats_histogram (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Arguments  
 *object_id*  
 ID de l'objet dans la base de données active dont les propriétés d'une de ses statistiques sont demandées. *l’object_id* est **int**.  
  
 *stats_id*  
 ID des statistiques pour *l’object_id*. spécifié. L’ID des statistiques peut être obtenu à partir de la vue de gestion dynamique [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* correspond à **int**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|object_id |**int**|ID de l'objet (table ou vue indexée) pour lequel retourner les propriétés de l'objet de statistiques.|  
|stats_id |**int**|ID de l'objet de statistiques. Unique dans la table ou la vue indexée. Pour plus d’informations, consultez [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|  
|step_number |**int** |Le numéro de l’étape dans l’histogramme. |
|range_high_key |**sql_variant** |Valeur de colonne de limite supérieure pour une étape d'histogramme. La valeur de colonne est également appelée « valeur de clé ».|
|RANGE_ROWS |**real** |Nombre estimé de lignes dont la valeur de colonne est comprise dans une étape d'histogramme, à l'exception de la limite supérieure. |
|equal_rows |**real** |Nombre estimé de lignes dont la valeur de colonne est égale à la limite supérieure de l'étape d'histogramme. |
|DISTINCT_RANGE_ROWS |**bigint** |Nombre estimé de lignes ayant une valeur de colonne distincte dans une étape d'histogramme, à l'exception de la limite supérieure. |
|average_range_rows |**real** |Nombre moyen de lignes avec des valeurs de colonne en double dans une étape d’histogramme, à l’exclusion de la limite supérieure (`RANGE_ROWS / DISTINCT_RANGE_ROWS` pour `DISTINCT_RANGE_ROWS > 0`). |
  
 ## <a name="remarks"></a>Notes  
 
 Le jeu de résultats pour `sys.dm_db_stats_histogram` retourne des informations semblables à `DBCC SHOW_STATISTICS WITH HISTOGRAM` et inclut également `object_id`, `stats_id`, et `step_number`.

 Étant donné que la colonne `range_high_key` est de type données sql_variant type, vous devrez peut-être utiliser `CAST` ou `CONVERT` si un prédicat effectue la comparaison avec une constante de chaîne non.

### <a name="histogram"></a>Histogramme
  
 Un histogramme mesure la fréquence des occurrences de chaque valeur distincte dans un jeu de données. L'optimiseur de requête calcule un histogramme sur les valeurs de colonnes de la première colonne clé de l'objet de statistiques, en sélectionnant les valeurs de colonnes au moyen d'un échantillonnage statistique des lignes ou d'une analyse complète de toutes les lignes dans la table ou la vue. Si l'histogramme est créé à partir d'un jeu de lignes échantillonnées, les totaux stockés pour le nombre de lignes et le nombre de valeurs distinctes sont des estimations et ne doivent pas nécessairement être des nombres entiers.  
  
 Pour créer l'histogramme, l'optimiseur de requête trie les valeurs de colonnes, calcule le nombre de valeurs qui correspondent à chaque valeur de colonne distincte, puis regroupe les valeurs de colonnes dans 200 étapes d'histogramme contiguës au maximum. Chaque étape inclut une plage de valeurs de colonnes suivie d'une valeur de colonne de limite supérieure. La plage comprend toutes les valeurs de colonnes possibles entre des valeurs limites, à l'exception des valeurs limites elles-mêmes. La plus basse des valeurs de colonnes triées est la valeur de limite supérieure pour la première étape d'histogramme.  
  
 Le diagramme suivant illustre un histogramme avec six étapes : La zone située à gauche de la première valeur limite supérieure représente la première étape.  
  
 ![](../../relational-databases/system-dynamic-management-views/media/histogram_2.gif "Histogramme")  
  
 Pour chaque étape d'histogramme :  
  
-   La ligne en gras représente la valeur limite supérieure (*range_high_key*) et le nombre d’occurrences (*equal_rows*) correspondant.  
  
-   La zone pleine située à gauche de *range_high_key* représente la plage de valeurs de colonnes et le nombre moyen d’occurrences de chacune des valeurs de colonnes (*average_range_rows*). Pour la première étape de l’histogramme, la valeur de *average_range_rows* est toujours égale à 0.  
  
-   Les lignes pointillées représentent les valeurs échantillonnées utilisées pour estimer le nombre total de valeurs distinctes dans la plage (*distinct_range_rows*) et le nombre total de valeurs dans la plage (*range_rows*). L’optimiseur de requête utilise *range_rows* et *distinct_range_rows* pour calculer *average_range_rows*, et ne stocke pas les valeurs échantillonnées.  
  
 L'optimiseur de requête définit les étapes d'histogramme en fonction de leur importance statistique. Il utilise un algorithme de nombre maximal de différences pour réduire le nombre d'étapes dans l'histogramme tout en augmentant la différence entre les valeurs limites. Le nombre maximal d'étapes est 200. Le nombre d'étapes d'histogramme peut être inférieur au nombre de valeurs distinctes, même pour les colonnes comportant moins de 200 points de limite. Par exemple, une colonne avec 100 valeurs distinctes peut avoir un histogramme comportant moins de 100 points de limite.  
  
## <a name="permissions"></a>Autorisations  

L'utilisateur doit avoir sélectionné des autorisations sur les colonnes de statistiques, ou bien il doit être le propriétaire de la table, ou encore il doit être membre du rôle serveur fixe `sysadmin`, du rôle de base de données fixe `db_owner` ou du rôle de base de données fixe `db_ddladmin`.

## <a name="examples"></a>Exemples  

### <a name="a-simple-example"></a>A. Exemple simple    
L’exemple suivant crée et remplit une table simple. Puis crée des statistiques sur les `Country_Name` colonne.

```sql
CREATE TABLE Country
(Country_ID int IDENTITY PRIMARY KEY,
Country_Name varchar(120) NOT NULL);
INSERT Country (Country_Name) VALUES ('Canada'), ('Denmark'), ('Iceland'), ('Peru');

CREATE STATISTICS Country_Stats  
    ON Country (Country_Name) ;  
```   
La clé primaire occupe `stat_id` numéro 1, appelez `sys.dm_db_stats_histogram` pour `stat_id` numéro 2, pour retourner l’histogramme des statistiques pour les `Country` table.    
```sql     
SELECT * FROM sys.dm_db_stats_histogram(OBJECT_ID('Country'), 2);
```

### <a name="b-useful-query"></a>B. Requêtes utiles :   
```sql  
SELECT hist.step_number, hist.range_high_key, hist.range_rows, 
    hist.equal_rows, hist.distinct_range_rows, hist.average_range_rows
FROM sys.stats AS s
CROSS APPLY sys.dm_db_stats_histogram(s.[object_id], s.stats_id) AS hist
WHERE s.[name] = N'<statistic_name>';
```

### <a name="c-useful-query"></a>C. Requêtes utiles :
L’exemple suivant sélectionne à partir de la table `Country` avec un prédicat sur une colonne `Country_Name`.

```sql  
SELECT * FROM Country 
WHERE Country_Name = 'Canada';
```

L’exemple suivant présente la statistique créée précédemment sur la table `Country` et de la colonne `Country_Name` pour l’étape d’histogramme qui correspondent au prédicat dans la requête ci-dessus.

```sql  
SELECT ss.name, ss.stats_id, shr.steps, shr.rows, shr.rows_sampled, 
    shr.modification_counter, shr.last_updated, sh.range_rows, sh.equal_rows
FROM sys.stats ss
INNER JOIN sys.stats_columns sc 
    ON ss.stats_id = sc.stats_id AND ss.object_id = sc.object_id
INNER JOIN sys.all_columns ac 
    ON ac.column_id = sc.column_id AND ac.object_id = sc.object_id
CROSS APPLY sys.dm_db_stats_properties(ss.object_id, ss.stats_id) shr
CROSS APPLY sys.dm_db_stats_histogram(ss.object_id, ss.stats_id) sh
WHERE ss.[object_id] = OBJECT_ID('Country') 
    AND ac.name = 'Country_Name'
    AND sh.range_high_key = CAST('Canada' AS CHAR(8));
```
  
## <a name="see-also"></a>Voir aussi  
[DBCC SHOW_STATISTICS (Transact-SQL)](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
[Fonctions (Transact-SQL) et les vues de gestion dynamique relatives aux objets](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)  
[sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)  
