---
title: CREATE TABLE (SQL Graph) | Microsoft Docs
ms.custom: ''
ms.date: 05/04/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SQL_GRAPH_TSQL
- TABLE
- CREATE_TABLE_TSQL
- NODE
- NODE_TSQL
- AS_NODE
- AS_NODE_TSQL
- EDGE
- EDGE_TSQL
- AS_EDGE
- AS_EDGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- graph
- SQL graph
- CREATE TABLE SQL graph
- NODE
- EDGE
- SQL graph, CREATE TABLE statement
ms.assetid: ''
caps.latest.revision: 1
author: shkale-msft
ms.author: shkale
manager: craigg
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 12582cd78a60d141008cdb05ff3e6d559f29944e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-table-sql-graph"></a>CREATE TABLE (SQL Graph)
[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

Crée une table graphique SQL en tant que table `NODE` ou `EDGE`. 
  
> [!NOTE]   
>  Pour plus d’informations sur les instructions Transact-SQL standard, consultez [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE TABLE   
    [ database_name . [ schema_name ] . | schema_name . ] table_name   
    ( { <column_definition> } [ ,...n ] )   
    AS [ NODE | EDGE ]
[ ; ]  
```  
  
  
## <a name="arguments"></a>Arguments  
Ce document répertorie uniquement les arguments appartenant à un graphe SQL. Pour obtenir la liste complète des arguments pris en charge et leur description, consultez [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).

 *database_name*    
 Nom de la base de données dans laquelle la table est créée. *database_name* doit spécifier le nom d’une base de données existante. Si aucun nom n’est spécifié, *database_name* correspond par défaut à la base de données actuelle. Le nom d’accès de la connexion actuelle doit être associé à un ID d’utilisateur existant dans la base de données spécifiée par *database_name*, et cet ID d’utilisateur doit disposer des autorisations CREATE TABLE.  
  
 *schema_name*    
 Nom du schéma auquel appartient la nouvelle table.  
  
 *table_name*    
 Est le nom de la table de nœuds ou d’arêtes. Les noms de tables doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). *table_name* peut comprendre un maximum de 128 caractères, à l’exception des noms de tables temporaires locales (noms précédés du signe #) qui ne peuvent pas dépasser 116 caractères.  
  
 NODE   
 Crée une table de nœuds.

 EDGE  
 Crée une table d’arêtes.  
  
## <a name="remarks"></a>Notes   
Le création d’une table temporaire en tant que nœud ou table d’arêtes n’est pas prise en charge.  

Le création d’une table de nœuds ou d’arêtes temporelle n’est pas prise en charge.

L’utilisation de Stretch Database n’est pas prise en charge pour les tables de nœuds et d’arêtes.

Les tables de nœuds et d’arêtes ne peuvent pas être des tables externes (aucune prise en charge PolyBase pour les tables graphiques). 
  
 
## <a name="examples"></a>Exemples  
  
### <a name="a-create-a-node-table"></a>A. Créer une table `NODE`
 L’exemple suivant montre comment créer une table `NODE`.

```
 CREATE TABLE Person (
        ID INTEGER PRIMARY KEY, 
        name VARCHAR(100), 
        email VARCHAR(100)
 ) AS NODE;
```

### <a name="b-create-an-edge-table"></a>B. Créer une table `EDGE`
Les exemples suivants montrent comment créer des tables `EDGE`.

```
 CREATE TABLE friends (
    id integer PRIMARY KEY,
    start_date date
 ) AS EDGE;

```

```
 -- Create a likes edge table, this table does not have any user defined attributes   
 CREATE TABLE likes AS EDGE;

```


## <a name="see-also"></a> Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Traitement des graphes avec SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md)

