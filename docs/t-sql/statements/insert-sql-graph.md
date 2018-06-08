---
title: INSERT (SQL Graph) | Microsoft Docs
description: Syntaxe de l’instruction INSERT pour les tables de nœuds ou d’arêtes SQL Graph.
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- INSERT statement [SQL Server], SQL graph
- SQL graph, INSERT statement
ms.assetid: ''
caps.latest.revision: 1
author: shkale-msft
ms.author: shkale
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2017 || = sqlallproducts-allversions
ms.openlocfilehash: 0180eaa873484fa9061c01e0cc18e20ea3012da2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="insert-sql-graph"></a>INSERT (SQL Graph)
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

  Ajoute une ou plusieurs lignes à une table `node` ou `edge` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

> [!NOTE]   
>  Pour en savoir plus sur les instructions Transact-SQL standard, consultez [INSERT TABLE (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md).
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="insert-into-node-table-syntax"></a>Syntaxe de l’instruction INSERT en vue d’une insertion dans une table de nœuds 
La syntaxe permettant d’effectuer une insertion dans une table de nœuds est identique à celle prévue pour une table normale. 

```  
[ WITH <common_table_expression> [ ,...n ] ]  
INSERT   
{  
        [ TOP ( expression ) [ PERCENT ] ]   
        [ INTO ]   
        { <object> | rowset_function_limited   
          [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
        }  
    {  
        [ (column_list) ] | [(<edge_table_column_list>)]  
        [ <OUTPUT Clause> ]  
        { VALUES ( { DEFAULT | NULL | expression } [ ,...n ] ) [ ,...n     ]   
        | derived_table   
        | execute_statement  
        | <dml_table_source>  
        | DEFAULT VALUES   
        }  
    }  
}  
[;]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
      | database_name .[ schema_name ] .   
      | schema_name .   
    ]  
    node_table_name  | edge_table_name
}  
  
<dml_table_source> ::=  
    SELECT <select_list>  
    FROM ( <dml_statement_with_output_clause> )   
      [AS] table_alias [ ( column_alias [ ,...n ] ) ]  
    [ WHERE <on_or_where_search_condition> ]  
        [ OPTION ( <query_hint> [ ,...n ] ) ]  

<on_or_where_search_condition> ::=
    {  <search_condition_with_match> | <search_condition> }

<search_condition_with_match> ::=
    { <graph_predicate> | [ NOT ] <predicate> | ( <search_condition> ) }
    [ AND { <graph_predicate> | [ NOT ] <predicate> | ( <search_condition> ) } ]
    [ ,...n ]

<search_condition> ::=
    { [ NOT ] <predicate> | ( <search_condition> ) }
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition> ) } ]
    [ ,...n ]

<graph_predicate> ::=
    MATCH( <graph_search_pattern> [ AND <graph_search_pattern> ] [ , ...n] )

<graph_search_pattern>::=
    <node_alias> { { <-( <edge_alias> )- | -( <edge_alias> )-> } <node_alias> }

<edge_table_column_list> ::=
    ($from_id, $to_id, [column_list])

```  
  
 
## <a name="arguments"></a>Arguments  
 Ce document décrit les arguments en rapport avec un graphique SQL. Pour obtenir une liste complète et une description des arguments pris en charge dans l’instruction INSERT, consultez [INSERT TABLE (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)

 INTO  
 Mot clé facultatif qui peut être utilisé entre `INSERT` et la table cible.  
  
 *search_condition_with_match*   
 La clause `MATCH` peut être utilisée dans une sous-requête à l’occasion d’une opération d’insertion dans une table de nœuds ou d’arêtes. Pour en savoir plus sur la syntaxe de l’instruction `MATCH`, consultez [GRAPH MATCH (Transact-SQL)](../../t-sql/queries/match-sql-graph.md)

 *graph_search_pattern*   
 Modèle de recherche fourni à la clause `MATCH` comme partie intégrante du prédicat de graphique.

 *edge_table_column_list*   
 Les utilisateurs doivent fournir des valeurs pour `$from_id` et `$to_id` à l’occasion d’une opération d’insertion dans une arête. Une erreur est retournée si aucune valeur n’est fournie ou que des valeurs NULL sont insérées dans ces colonnes. 
  

## <a name="remarks"></a>Notes   
Une insertion dans un nœud équivaut à une insertion dans une table relationnelle. Les valeurs de la colonne $node_id sont générées automatiquement.

À l’occasion d’une insertion dans une table d’arêtes, les utilisateurs doivent fournir des valeurs pour les colonnes `$from_id` et `$to_id`.   

Une insertion en bloc (BULK) pour la table de nœuds reste identique à celle d’une table relationnelle.

Avant de procéder à une insertion en bloc dans une table d’arêtes, les tables de nœuds doivent être importées. Les valeurs pour `$from_id` et `$to_id` peuvent ensuite être extraites de la colonne `$node_id` de la table de nœuds, puis insérées comme arêtes. 

  
### <a name="permissions"></a>Autorisations  
 L'autorisation INSERT est obligatoire sur la table cible.  
  
 Les autorisations INSERT sont accordées par défaut aux membres du rôle serveur fixe **sysadmin**, aux rôles de base de données fixes **db_owner** et **db_datawriter**, ainsi qu’au propriétaire de la table. Les membres des rôles **sysadmin**, **db_owner** et **db_securityadmin** et le propriétaire de la table peuvent transférer des autorisations à d’autres utilisateurs.  
  
 Pour exécuter INSERT avec l’option BULK de la fonction OPENROWSET, vous devez être membre du rôle serveur fixe **sysadmin** ou **bulkadmin**.  
  

## <a name="examples"></a>Exemples  
  
#### <a name="a--insert-into-node-table"></a>A.  Insertion dans une table de nœuds  
 L’exemple suivant crée une table de nœuds Person et y insère 2 lignes.

 ```
 -- Create person node table
 CREATE TABLE dbo.Person (ID integer PRIMARY KEY, name varchar(50)) AS NODE;
 
 -- Insert records for Alice and John
 INSERT INTO dbo.Person VALUES (1, 'Alice');
 INSERT INTO dbo.Person VALUES (2,'John');
 ```
  
#### <a name="b--insert-into-edge-table"></a>B.  Insertion dans une table d’arêtes  
 L’exemple suivant crée une table d’arêtes nommée friend et y insère une arête.

 ```
 -- Create friend edge table
 CREATE TABLE dbo.friend (start_date DATE) AS EDGE;

 -- Create a friend edge, that connect Alice and John
 INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'John'), '9/15/2011');
 ```

  
## <a name="see-also"></a> Voir aussi  
 [INSERT TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [Traitement des graphiques avec SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md)  


