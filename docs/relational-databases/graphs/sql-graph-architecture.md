---
title: Graphique de l’Architecture SQL | Documents Microsoft
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: graphs
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, architecture
ms.assetid: ''
caps.latest.revision: 1
author: shkale-msft
ms.author: shkale
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2017 || = sqlallproducts-allversions
ms.openlocfilehash: 7cfba1fc79e44bb28a433c3b31fe5f4236037d6e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-graph-architecture"></a>Graphique de l’Architecture SQL  
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

Découvrez l’architecture SQL graphique. Le fait de connaître les principes de base rend plus facile à comprendre les autres articles SQL graphique.
 
## <a name="sql-graph-database"></a>Base de données du graphique SQL
Les utilisateurs peuvent créer un graphique par base de données. Un graphique est une collection de tableaux de bord et de nœud. Les tableaux de bord ou le nœud peuvent être créés sous n’importe quel schéma dans la base de données, mais ils appartiennent à un graphique logique. Une table de nœud est la collection d’un type similaire de nœuds. Par exemple, une table de nœud Person conserve tous les nœuds de la personne qui appartiennent à un graphique. De même, un tableau de bord est une collection d’un type similaire de bords. Par exemple, un tableau de bord amis conserve toutes les arêtes qui se connectent une personne à une autre personne. Étant donné que les nœuds et les bords sont stockés dans les tables, la plupart des opérations prises en charge sur les tables normales est pris en charge sur les tableaux de bord ou le nœud. 
 
 
![architecture de graphique SQL](../../relational-databases/graphs/media/sql-graph-architecture.png "architecture de base de données de graphique de Sql")   

Figure 1 : Architecture de base de données SQL graphique
 
## <a name="node-table"></a>Table de nœud
Une table de nœud représente une entité dans un schéma de graphique. Chaque fois qu’une table de nœud est créée, ainsi que les colonnes définies par l’utilisateur, implicite `$node_id` colonne est créée, qui identifie de façon unique un nœud donné dans la base de données. Les valeurs de `$node_id` sont générées automatiquement et sont une combinaison de `object_id` de cette table de nœud et une valeur bigint généré en interne. Toutefois, lorsque la `$node_id` colonne est sélectionnée, une valeur calculée sous la forme d’une chaîne JSON s’affiche. En outre, `$node_id` est une pseudo-colonne, qui est mappé à un nom interne et qu’elle contient une chaîne hexadécimale. Lorsque vous sélectionnez `$node_id` à partir de la table, le nom de colonne s’affiche en tant que `$node_id_<hex_string>`. À l’aide des noms de colonnes pseudo-aléatoire dans les requêtes est la méthode recommandée de l’interrogation interne `$node_id` colonne et à l’aide du nom interne avec une chaîne hexadécimale doivent être évitées.

Il est recommandé que les utilisateurs créer une contrainte unique ou un index sur la `$node_id` colonne au moment de la création de la table des nœuds, mais si une n’est pas créée, une valeur par défaut des index non cluster unique est créé automatiquement. Toutefois, un index sur une colonne de pseudo graphique est créé sur les colonnes sous-jacentes d’internes. Autrement dit, un index créé sur le `$node_id` colonne, apparaît dans le texte interne `graph_id_<hex_string>` colonne.   


## <a name="edge-table"></a>Tableau de bord
Une table du bord représente une relation dans un graphique. Bords sont toujours dirigées et connectent les deux nœuds. Un tableau de bord permet aux utilisateurs de modéliser les relations plusieurs-à-plusieurs dans le graphique. Une table du bord peut ou peut-être pas tous les attributs définis par l’utilisateur qu’il contient. Chaque fois qu’un tableau de bord est créé, ainsi que les attributs définis par l’utilisateur, les trois colonnes implicites sont créés dans le tableau de bord :

|Nom de colonne    | Description  |
|---   |---  |
|`$edge_id`   |Identifie de façon unique un bord donné dans la base de données. Il s’agit d’une colonne générée et la valeur est une combinaison d’object_id de la table du bord et une valeur bigint généré en interne. Toutefois, lorsque la `$edge_id` colonne est sélectionnée, une valeur calculée sous la forme d’une chaîne JSON s’affiche. `$edge_id` est une pseudo-colonne, qui est mappé à un nom interne et qu’elle contient une chaîne hexadécimale. Lorsque vous sélectionnez `$edge_id` à partir de la table, le nom de colonne s’affiche en tant que `$edge_id_\<hex_string>`. À l’aide des noms de colonnes pseudo-aléatoire dans les requêtes est la méthode recommandée de l’interrogation interne `$edge_id` colonne et à l’aide du nom interne avec une chaîne hexadécimale doivent être évitées. |
|`$from_id`   |Stocke le `$node_id` du nœud, à partir de l’origine du bord.  |
|`$to_id`   |Stocke le `$node_id` du nœud, à laquelle la session se termine. |

Les nœuds d’un bord donné peut se connecter est régie par les données insérées dans la `$from_id` et `$to_id` colonnes. Dans la première version, il n’est pas possible de définir des contraintes sur la table du bord, pour empêcher la connexion des deux types de nœuds. Autrement dit, un bord peut se connecter les deux nœuds dans le graphique, quel que soit leur type.

Similaire à la `$node_id` colonne, il est recommandé que les utilisateurs créent un index unique ou une contrainte sur la `$edge_id` colonne au moment de la création de la table du bord, mais si une n’est pas créée, une valeur par défaut des index non cluster unique est automatiquement créé sur cette colonne. Toutefois, un index sur une colonne de pseudo graphique est créé sur les colonnes sous-jacentes d’internes. Autrement dit, un index créé sur le `$edge_id` colonne, apparaît dans le texte interne `graph_id_<hex_string>` colonne. Il est également recommandé pour des scénarios OLTP, que les utilisateurs créer un index sur (`$from_id`, `$to_id`) colonnes, pour les recherches plus rapides dans la direction du bord.  

Figure 2 montre comment les tables de nœud et de session sont stockées dans la base de données. 

![tables de vos amis personne](../../relational-databases/graphs/media/person-friends-tables.png "nœud Person et vos amis tableaux de bord")   

Figure 2 : Nœud et le bord de représentation sous forme de table



## <a name="metadata"></a>Métadonnées
Ces vues de métadonnées permet de voir les attributs d’un tableau de bord ou de nœud.
 
### <a name="systables"></a>sys.tables
Les nouveaux, suivants type bit, colonnes seront ajoutées à SYS. TABLES. Si `is_node` est définie sur 1, ce qui indique que la table est une table de nœud et, si `is_edge` est défini sur 1, ce qui indique que la table est une table du bord.
 
|Nom de la colonne |Type de données | Description |
|--- |---|--- |
|is_node |bit |1 = il s’agit d’une table de nœud |
|is_edge |bit |1 = il s’agit d’un tableau de bord |
 
### <a name="syscolumns"></a>sys.columns
Le `sys.columns` vue contient des colonnes supplémentaires `graph_type` et `graph_type_desc`, qui indiquent le type de la colonne dans les tables de nœud et le bord.
 
|Nom de la colonne |Type de données | Description |
|--- |---|--- |
|graph_type |int |Colonne interne avec un ensemble de valeurs. Les valeurs sont comprises entre 1-8 pour les colonnes de graphique et de valeur NULL pour les autres.  |
|graph_type_desc |nvarchar(60)  |colonne interne avec un ensemble de valeurs |
 
Le tableau suivant répertorie les valeurs valides pour `graph_type` colonne

|Valeur de colonne  | Description  |
|---   |---   |
|1  |GRAPH_ID  |
|2  |GRAPH_ID_COMPUTED  |
|3  |GRAPH_FROM_ID  |
|4  |GRAPH_FROM_OBJ_ID  |
|5  |GRAPH_FROM_ID_COMPUTED  |
|6  |GRAPH_TO_ID  |
|7  |GRAPH_TO_OBJ_ID  |
|8  |GRAPH_TO_ID_COMPUTED  |


`sys.columns` stocke également des informations sur les colonnes implicites créées dans les tableaux de bord ou le nœud. Informations suivantes peuvent être récupérées à partir de sys.columns, toutefois, les utilisateurs ne peuvent pas sélectionner ces colonnes à partir d’un tableau de bord ou de nœud. 

Colonnes implicites dans une table de nœud  
|Nom de la colonne    |Type de données  |is_hidden  |Commentaire  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |colonne de graph_id interne  |
|$node_id_\<hex_string > |NVARCHAR   |0  |Colonne d’id de nœud externe  |

Colonnes implicites dans un tableau de bord  
|Nom de la colonne    |Type de données  |is_hidden  |Commentaire  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |colonne de graph_id interne  |
|$edge_id_\<hex_string > |NVARCHAR   |0  |colonne d’id de session externe  |
|from_obj_id_\<hex_string>  |INT    |1  |interne à partir de l’id d’objet de nœud  |
|from_id_\<hex_string>  |bigint |1  |Interne à partir du nœud graph_id  |
|$from_id_\<hex_string > |NVARCHAR   |0  |externe à partir de l’id de nœud  |
|to_obj_id_\<hex_string>    |INT    |1  |interne à l’id d’objet de nœud  |
|to_id_\<hex_string>    |bigint |1  |Nœud graph_id internes  |
|$to_id_\<hex_string >   |NVARCHAR   |0  |id de nœud externe  |
 
### <a name="system-functions"></a>Fonctions système
Les fonctions intégrées suivantes sont ajoutées. Il aidera les utilisateurs à extraire des informations à partir des colonnes générées. Notez que, ces méthodes ne validera pas les entrées de l’utilisateur. Si l’utilisateur spécifie un non valide `sys.node_id` la méthode extrait la partie appropriée et renvoyez-le. Par exemple, OBJECT_ID_FROM_NODE_ID prendra un `$node_id` en entrée et retourne l’object_id de la table, ce nœud appartient. 
 
|Intégré   | Description  |
|---  |---  |
|OBJECT_ID_FROM_NODE_ID |Extraire l’object_id un node_id  |
|GRAPH_ID_FROM_NODE_ID  |Extraire le graph_id un node_id  |
|NODE_ID_FROM_PARTS |Construire un node_id à partir d’un object_id et un graph_id  |
|OBJECT_ID_FROM_EDGE_ID |Extraire object_id edge_id :  |
|GRAPH_ID_FROM_EDGE_ID  |Extraire l’identité à partir d’edge_id :  |
|EDGE_ID_FROM_PARTS |Edge_id : la construction d’object_id et identité  |



## <a name="transact-sql-reference"></a>Référence Transact-SQL 
En savoir plus les [!INCLUDE[tsql-md](../../includes/tsql-md.md)] extensions introduites dans SQL Server et la base de données SQL Azure, qui permettent aux création et l’interrogation des objets de graphique. Les extensions de langage de requête permettent de requête et parcourent le graphique à l’aide de la syntaxe d’art ASCII.
 
### <a name="data-definition-language-ddl-statements"></a>Instructions de langage de définition (DDL) de données
|Tâche   |Rubrique connexe  |Remarques
|---  |---  |---  |
|CREATE TABLE |[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-sql-graph.md)|`CREATE TABLE ` est maintenant étendue pour prendre en charge la création d’une table en tant que nœud ou en tant que session. Notez que le tableau de bord peut ou ne peut pas avoir de n’importe quel utilisateur les attributs définis.  |
|ALTER TABLE    |[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)|Tables de nœud et le bord peuvent être modifiées de la même façon qu’une table relationnelle, en utilisant le `ALTER TABLE`. Les utilisateurs peuvent ajouter ou modifier les colonnes définies par l’utilisateur, les index ou contraintes. Cependant, comme la modification des colonnes de graphique interne, `$node_id` ou `$edge_id`, génère une erreur.  |
|CREATE INDEX   |[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  |Les utilisateurs peuvent créer des index sur les pseudo colonnes et les colonnes définies par l’utilisateur dans les tables de nœud et le bord. Tous les types d’index sont pris en charge, y compris les index cluster et non cluster columnstore.  |
|DROP TABLE |[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)  |Nœud et le bord de tables peuvent être supprimés de la même façon qu’une table relationnelle, en utilisant le `DROP TABLE`. Toutefois, dans cette version, il y a aucune contrainte pour garantir qu’aucune bords ne pointent vers un nœud supprimé et suppression en cascade des bords, lors de la suppression d’un nœud ou d’une table de nœud n’est pas pris en charge. Il est recommandé que si une table de nœud est supprimée, les utilisateurs déposez les bords connectés aux nœuds de cette table de nœud manuellement pour maintenir l’intégrité du graphique.  |


### <a name="data-manipulation-language-dml-statements"></a>Instructions de données (DML, Data Manipulation Language)
|Tâche   |Rubrique connexe  |Remarques
|---  |---  |---  |
|INSERT |[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-sql-graph.md)|Insertion dans une table de nœud n’est pas différente de l’insertion dans une table relationnelle. Les valeurs de `$node_id` colonne est générée automatiquement. La tentative d’insertion d’une valeur dans `$node_id` ou `$edge_id` colonne entraîne une erreur. Les utilisateurs doivent fournir des valeurs pour `$from_id` et `$to_id` colonnes lors de l’insertion dans un tableau de bord. `$from_id` et `$to_id` sont la `$node_id` les valeurs des nœuds un bord donné se connecte.  |
|DELETE | [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Les données à partir des tableaux de bord ou le nœud peuvent être supprimées dans la même façon, car il est supprimé à partir des tables relationnelles. Toutefois, dans cette version, il y a aucune contrainte pour garantir qu’aucune bords ne pointent vers un nœud supprimé et suppression en cascade des bords, lors de la suppression d’un nœud n’est pas pris en charge. Il est recommandé que chaque fois qu’un nœud est supprimé, toutes les arêtes de connexion à ce nœud sont également supprimés, pour préserver l’intégrité du graphique.  |
|UPDATE |[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  |Les valeurs dans les colonnes définies par l’utilisateur peuvent être mis à jour à l’aide de l’instruction de mise à jour. Mise à jour les colonnes de graphique interne `$node_id`, `$edge_id`, `$from_id` et `$to_id` n’est pas autorisée.  |
|MERGE |[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  |`MERGE` instruction n’est pas prise en charge sur un tableau de bord ou de nœud.  |


### <a name="query-statements"></a>Instructions de requête
|Tâche   |Rubrique connexe  |Remarques
|---  |---  |---  |
|SELECT |[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)|Nœuds et bords sont stockées en interne en tant que tables, par conséquent, la plupart des opérations prises en charge sur une table dans SQL Server ou de la base de données SQL Azure est pris en charge sur les tables du nœud et de session  |
|MATCH  | [CORRESPONDANCE &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md)|CORRESPONDANCE intégré est introduit pour prendre en charge la recherche de correspondance et le parcours du graphique.  |



## <a name="limitations-and-known-issues"></a>Limitations et problèmes connus  
Il existe certaines restrictions sur les tables de nœud et le bord dans cette version :
* Les tables temporaires locales ou globales ne peut pas être des tableaux de bord ou de nœud.
* Types de tables et des variables de table ne peut pas être déclarés comme un tableau de bord ou le nœud. 
* Impossible de créer les tables de nœud et le bord en tant que tables temporelles avec version système.   
* Tables de nœud et le bord ne peut pas être des tables optimisées en mémoire.  
* Les utilisateurs ne peuvent pas mettre à jour le $from_id et des colonnes de to_id $ d’un bord à l’aide d’instruction de mise à jour. Pour mettre à jour les nœuds d’un bord connecte, les utilisateurs doivent insérer la nouvelle arête pointant vers les nouveaux nœuds et supprimer des précédent.
* Cross-de base de données des requêtes sur les objets de graphique ne sont pas pris en charge. 


## <a name="next-steps"></a>Étapes suivantes
Pour commencer avec la nouvelle syntaxe, consultez [SQL graphique de base de données - exemple](./sql-graph-sample.md)
 

