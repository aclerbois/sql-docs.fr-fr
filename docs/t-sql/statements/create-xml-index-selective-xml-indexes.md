---
title: CREATE XML INDEX (index XML sélectifs) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 1f510151-41d5-45c2-9cd0-b1ca0246fffe
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 4d7af44ccf8808b9be6ad15f5b937fe94f5fc107
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-xml-index-selective-xml-indexes"></a>CREATE XML INDEX (index XML sélectifs)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Crée un index XML secondaire sélectif sur un chemin d'accès unique déjà indexé par un index XML sélectif existant. Vous pouvez également créer des index XML primaires sélectifs. Pour plus d’informations, consultez [Créer, modifier ou supprimer des index XML sélectifs](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE XML INDEX index_name  
    ON <table_object> ( xml_column_name )  
    USING XML INDEX sxi_index_name  
    FOR ( <xquery_or_sql_values_path> )  
    [WITH ( <index_options> )]  
  
<table_object> ::=   
{ [database_name. [schema_name ] . | schema_name. ] table_name }  
  
<xquery_or_sql_values_path>::=   
<path_name>   
  
<path_name> ::=   
character string literal  
  
<xmlnamespace_list> ::=   
<xmlnamespace_item> [, <xmlnamespace_list>]  
  
<xmlnamespace_item> ::=   
xmlnamespace_uri AS xmlnamespace_prefix  
  
<index_options> ::=   
(    
  | PAD_INDEX  = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
)  
```  
  
##  <a name="Arguments"></a> Arguments  
 *index_name*  
 Nom de l'index à créer. Les noms d’index doivent être uniques dans une table, mais pas dans une base de données. Les noms d’index doivent se conformer aux règles régissant les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 ON *\<table_object>* Table contenant la colonne XML à indexer. Vous pouvez utiliser les formats suivants :  
  
-   `database_name.schema_name.table_name`  
  
-   `database_name..table_name`  
  
-   `schema_name.table_name`  
  
 *xml_column_name*  
 Nom de la colonne XML qui contient le chemin d'accès à indexer.  
  
 USING XML INDEX *sxi_index_name*  
 Nom de l'index XML sélectif existant.  
  
 FOR **(** \<xquery_or_sql_values_path> **)** Nom du chemin indexé sur lequel créer l’index XML secondaire sélectif. Le chemin d'accès à indexer est le nom affecté dans l'instruction CREATE SELECTIVE XML INDEX. Pour plus d’informations, consultez [CREATE SELECTIVE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
 WITH \<index_options> Pour plus d’informations sur les options d’index, consultez [CREATE XML INDEX](../../t-sql/statements/create-xml-index-selective-xml-indexes.md).  
  
## <a name="remarks"></a>Notes   
 Il peut y avoir plusieurs index XML secondaires sélectifs sur chaque colonne XML dans la table de base.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Un index XML sélectif sur une colonne XML doit exister pour pouvoir créer des index XML secondaires sélectifs sur la colonne.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite une autorisation ALTER sur la table ou la vue. L’utilisateur doit être membre du rôle serveur fixe **sysadmin** ou des rôles de base de données fixes **db_ddladmin** et **db_owner** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée un index XML secondaire sélectif sur le chemin d'accès `pathabc`. Le chemin à indexer est le nom affecté dans l’instruction [CREATE SELECTIVE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-selective-xml-index-transact-sql.md).  
  
```  
CREATE XML INDEX filt_sxi_index_c  
ON Tbl(xmlcol)  
USING XML INDEX sxi_index  
FOR ( pathabc );  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Index XML sélectifs &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [Créer, modifier ou supprimer des index XML secondaires sélectifs](../../relational-databases/xml/create-alter-and-drop-secondary-selective-xml-indexes.md)  
  
  

