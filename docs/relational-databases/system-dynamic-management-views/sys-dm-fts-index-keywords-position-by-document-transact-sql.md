---
title: Sys.dm_fts_index_keywords_position_by_document (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document
- sys.dm_fts_index_keywords_position_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords_position_by_document dynamic management view
ms.assetid: 0d70184f-baa2-411b-a32d-a4c5af890edd
caps.latest.revision: 5
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b02615dbc260c951a08d3bfa5279b20464653203
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmftsindexkeywordspositionbydocument-transact-sql"></a>Sys.dm_fts_index_keywords_position_by_document (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne les informations de position de mot clé dans les documents indexés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Arguments  
 DB_ID ('*nom_base_de_données*')  
 Un appel à la [DB_ID()](../../t-sql/functions/db-id-transact-sql.md) (fonction). Cette fonction accepte un nom de base de données et retourne l’ID de base de données, les sys.dm_fts_index_keywords_position_by_document utilise pour rechercher la base de données spécifié.  
  
 object_id ('*table_name*')  
 Un appel à la [OBJECT_ID()](../../t-sql/functions/object-id-transact-sql.md) (fonction). Cette fonction accepte un nom de table et retourne l'ID de la table contenant l'index de recherche en texte intégral à examiner.  
  
## <a name="table-returned"></a>Table retournée  
  
|Colonne|Data type| Description|  
|------------|---------------|-----------------|  
|mot clé|**varbinary(128)**|Qui représente le mot clé de chaîne binaire.|  
|display_term|**nvarchar(4000)**|Format explicite du mot clé. Ce format est dérivé du format interne stocké dans l'index de recherche en texte intégral.|  
|column_id|**int**|ID de la colonne à partir de laquelle le mot clé actuel a été indexé en texte intégral.|  
|document_id|**bigint**|ID de la ligne ou du document à partir duquel le terme actuel a été indexé en texte intégral. Cet ID correspond à la valeur de clé de texte intégral de cette ligne ou de ce document.|  
|position|**int**|La position du mot clé dans le document.|  
  
## <a name="remarks"></a>Notes  
 Utilisez la vue de gestion dynamique pour identifier l’emplacement des mots indexés dans les documents indexés. Cette DMV peut être utilisée pour résoudre les problèmes lorsque **sys.dm_fts_index_keywords_by_document** indique les mots sont dans l’index de recherche en texte intégral, mais lorsque vous exécutez une requête à l’aide de ces mots, le document n’est pas retourné.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation SELECT sur les colonnes couvertes par l'index de recherche en texte intégral et les autorisations CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne des mots clés à partir de l’index de recherche en texte intégral de la `Production.Document` table de la `AdventureWorks` base de données exemple.  
  
```  
USE AdventureWorks2012;  
GO   
  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
);   
GO  
```  
  
 Vous pouvez ajouter un prédicat sur les autres columns_id comme dans l’exemple de requête suivant, afin de mieux identifier les emplacements.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
)  
WHERE document_id = 7 AND display_term = 'performance';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [Améliorer les performances des index de recherche en texte intégral](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [Recherche en texte intégral et les fonctions de recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-functions/full-text-search-and-semantic-search-functions-transact-sql.md)   
 [Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Procédures stockées de recherche en texte intégral et la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
