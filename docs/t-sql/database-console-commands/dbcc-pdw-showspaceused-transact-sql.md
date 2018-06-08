---
title: DBCC PDW_SHOWSPACEUSED (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/17/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 73f598cf-b02a-4dba-8d89-9fc0b55a12b8
caps.latest.revision: 10
author: edmacauley
ms.author: umajay
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: b372a5cb516dbbeaa2732dfbfa676921d63708f3
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="dbcc-pdwshowspaceused-transact-sql"></a>DBCC PDW_SHOWSPACEUSED (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

Affiche le nombre de lignes, l’espace disque réservé et l’espace disque utilisé pour une table spécifique ou pour toutes les tables dans une base de données [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
-- Show the space used for all user tables and system tables in the current database  
DBCC PDW_SHOWSPACEUSED  
[;]  
  
-- Show the space used for a table  
DBCC PDW_SHOWSPACEUSED ( " [ database_name . [ schema_name ] . ] | [ schema_name .] table_name  " )  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ *database_name* . [ *schema_name* ] . | *schema_name* . ] *table_name*  
 Nom en une, deux ou trois parties de la table à afficher. Les noms de table en deux ou trois parties doivent être placés entre guillemets doubles (""). L’utilisation de guillemets autour d’un nom de table en une partie est facultative. Quand aucun nom de table n’est spécifié, les informations s’affichent pour la base de données actuelle.  
  
## <a name="permissions"></a>Autorisations  
Requiert l'autorisation VIEW SERVER STATE.
  
## <a name="result-sets"></a>Jeux de résultats  
Voici le jeu de résultats pour toutes les tables.
  
|colonne|Type de données|Description|  
|------------|---------------|-----------------|  
|reserved_space|BIGINT|Espace total utilisé pour la base de données, en Ko.|  
|data_space|BIGINT|Espace utilisé pour les données, en Ko.|  
|index_space|BIGINT|Espace utilisé pour les index, en Ko.|  
|unused_space|BIGINT|Espace qui fait partie de l’espace réservé et non utilisé, en Ko.|  
|pdw_node_id|INT|Nœud de calcul qui est utilisé pour les données.|  
  
Voici le jeu de résultats pour une table.
  
|colonne|Type de données|Description|Plage|  
|------------|---------------|-----------------|-----------|  
|lignes|BIGINT|Nombre de lignes.||  
|reserved_space|BIGINT|Espace total réservé pour l’objet, en Ko.||  
|data_space|BIGINT|Espace utilisé pour les données, en Ko.||  
|index_space|BIGINT|Espace utilisé pour les index, en Ko.||  
|unused_space|BIGINT|Espace qui fait partie de l’espace réservé et non utilisé, en Ko.||  
|pdw_node_id|INT|Nœud de calcul qui est utilisé pour signaler l’utilisation de l’espace.||  
|distribution_id|INT|Distribution qui est utilisée pour signaler l’utilisation de l’espace.|La valeur est -1 pour les tables répliquées.|  
  
## <a name="examples-includesssdwincludessssdw-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="a-dbcc-pdwshowspaceused-basic-syntax"></a>A. Syntaxe de base DBCC PDW_SHOWSPACEUSED  
Les exemples suivants indiquent plusieurs façons d’afficher le nombre de lignes, l’espace disque réservé et l’espace disque utilisé par la table FactInternetSales dans la base de données [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].
  
```sql
-- Uses AdventureWorks  
  
DBCC PDW_SHOWSPACEUSED ( "AdventureWorksPDW2012.dbo.FactInternetSales" );  
DBCC PDW_SHOWSPACEUSED ( "AdventureWorksPDW2012..FactInternetSales" );  
DBCC PDW_SHOWSPACEUSED ( "dbo.FactInternetSales" );  
DBCC PDW_SHOWSPACEUSED ( FactInternetSales );  
```  
  
### <a name="b-show-the-disk-space-used-by-all-tables-in-the-current-database"></a>B. Afficher l’espace disque utilisé par toutes les tables dans la base de données actuelle  
 L’exemple suivant montre l’espace disque réservé et utilisé par toutes les tables utilisateur et système dans la base de données [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].  
  
```sql
-- Uses AdventureWorks  
  
DBCC PDW_SHOWSPACEUSED;  
```  
 ## <a name="see-also"></a>Voir aussi
[DBCC PDW_SHOWEXECUTIONPLAN &#40;Transact-SQL&#41;](dbcc-pdw-showexecutionplan-transact-sql.md)  
[DBCC PDW_SHOWPARTITIONSTATS &#40;Transact-SQL&#41;](dbcc-pdw-showpartitionstats-transact-sql.md)

  
