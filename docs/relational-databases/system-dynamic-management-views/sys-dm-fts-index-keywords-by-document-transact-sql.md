---
title: Sys.dm_fts_index_keywords_by_document (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_index_keywords_by_document_TSQL
- dm_fts_index_keywords_by_document_TSQL
- sys.dm_fts_index_keywords_by_document
- dm_fts_index_keywords_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], troubleshooting
- sys.dm_fts_index_keywords_by_document dynamic management function
- full-text search [SQL Server], viewing keywords
ms.assetid: 793b978b-c8a1-428c-90c2-a3e49d81b5c9
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 58377295a7bccadd1a1d273ba45469e817408949
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmftsindexkeywordsbydocument-transact-sql"></a>sys.dm_fts_index_keywords_by_document (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-pdw-md.md)]

  Retourne des informations sur le contenu de niveau document d'un index de recherche en texte intégral associé à la table spécifiée.  
  
 sys.dm_fts_index_keywords_by_document est une fonction de gestion dynamique.  
  
 **Pour afficher les informations d’index de recherche en texte intégral niveau supérieur**  
  
-   [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
 **Pour afficher les informations de contenu au niveau de la propriété liées à une propriété de document**  
  
-   [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_fts_index_keywords_by_document  
(   
    DB_ID('database_name'),     OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Arguments  
 DB_ID ('*nom_base_de_données*')  
 Un appel à la [DB_ID()](../../t-sql/functions/db-id-transact-sql.md) (fonction). Cette fonction accepte un nom de base de données et retourne l'ID de la base de données, que sys.dm_fts_index_keywords_by_document utilise pour rechercher la base de données spécifiée. Si *database_name* est omis, la fonction retourne l’ID de la base de données active.  
  
 object_id ('*table_name*')  
 Un appel à la [OBJECT_ID()](../../t-sql/functions/object-id-transact-sql.md) (fonction). Cette fonction accepte un nom de table et retourne l'ID de la table contenant l'index de recherche en texte intégral à examiner.  
  
## <a name="table-returned"></a>Table retournée  
  
|Colonne|Data type| Description|  
|------------|---------------|-----------------|  
|mot clé|**nvarchar(4000)**|Représentation hexadécimale du mot clé stocké dans l'index de recherche en texte intégral.<br /><br /> Remarque : OxFF représente le caractère spécial qui indique la fin d’un fichier ou un jeu de données.|  
|display_term|**nvarchar(4000)**|Format explicite du mot clé. Ce format est dérivé du format interne stocké dans l'index de recherche en texte intégral.<br /><br /> Remarque : OxFF représente le caractère spécial qui indique la fin d’un fichier ou un jeu de données.|  
|column_id|**int**|ID de la colonne à partir de laquelle le mot clé actuel a été indexé en texte intégral.|  
|document_id|**int**|ID de la ligne ou du document à partir duquel le terme actuel a été indexé en texte intégral. Cet ID correspond à la valeur de clé de texte intégral de cette ligne ou de ce document.|  
|occurrence_count|**int**|Nombre d’occurrences du mot clé actuel dans le document ou la ligne qui est indiqué par **document_id**. Lorsque '*search_property_name*' est spécifié, occurrence_count affiche uniquement le nombre d’occurrences du mot clé actuel dans la propriété de recherche spécifié dans le document ou la ligne.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par sys.dm_fts_index_keywords_by_document sont utiles pour déterminer, entre autres choses, les éléments ci-dessous :  
  
-   Nombre total de mots clés contenus dans un index de recherche en texte intégral.  
  
-   Si un mot clé fait partie d'une ligne ou d'un document donné.  
  
-   Nombre de fois qu'un mot clé apparaît dans l'index de recherche en texte intégral entier, à savoir :  
  
     ([Somme](../../t-sql/functions/sum-transact-sql.md)(**occurrence_count**) où **mot clé**=*keyword_value* )  
  
-   Nombre de fois qu'un mot clé apparaît dans une ligne ou un document donné.  
  
-   Nombre de mots clés contenus dans une ligne ou un document donné.  
  
 Vous pouvez également utiliser les informations fournies par sys.dm_fts_index_keywords_by_document pour récupérer tous les mots clés qui appartiennent à une ligne ou à un document donné.  
  
 Lorsque la colonne clé de texte intégral est un type de données Integer, comme cela est recommandé, document_id est directement mappé à la valeur de la clé de texte intégral dans la table de base.  
  
 En revanche, lorsque la colonne clé de texte intégral fait appel à un type de données non entier, document_id ne représente pas la clé de texte intégral dans la table de base. Dans ce cas, pour identifier la ligne dans la table de base qui est retournée par dm_fts_index_keywords_by_document, vous devez joindre cette vue avec les résultats retournés par [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md). Avant de pouvoir les joindre, vous devez stocker la sortie de la procédure stockée dans une table temp. Puis, vous pouvez joindre la colonne document_id de dm_fts_index_keywords_by_document avec la colonne DocId retournée par cette procédure stockée. Notez qu’un **timestamp** colonne ne peut pas recevoir des valeurs au moment de l’insertion, car elles sont générées automatiquement par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, le **timestamp** colonne doit être convertie en **varbinary (8)** colonnes. L'exemple suivant affiche ces étapes. Dans cet exemple, *table_id* est l’ID de votre table, *nom_base_de_données* est le nom de votre base de données, et *table_name* est le nom de votre table.  
  
```  
USE database_name;  
GO  
CREATE TABLE #MyTempTable   
   (  
      docid INT PRIMARY KEY ,  
      [key] INT NOT NULL  
   );  
DECLARE @db_id int = db_id(N'database_name');  
DECLARE @table_id int = OBJECT_ID(N'table_name');  
INSERT INTO #MyTempTable EXEC sp_fulltext_keymappings @table_id;  
SELECT * FROM sys.dm_fts_index_keywords_by_document   
   ( @db_id, @table_id ) kbd  
   INNER JOIN #MyTempTable tt ON tt.[docid]=kbd.document_id;  
GO  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation SELECT sur les colonnes couvertes par l'index de recherche en texte intégral et les autorisations CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-full-text-index-content-at-the-document-level"></a>A. Affichage du contenu de l'index de recherche en texte intégral au niveau du document  
 L'exemple suivant affiche le contenu de l'index de recherche en texte intégral au niveau du document dans la table `HumanResources.JobCandidate` de l'exemple de base de données `AdventureWorks2012`.  
  
> [!NOTE]  
>  Vous pouvez créer cet index en exécutant l’exemple fourni pour la `HumanResources.JobCandidate` table [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_by_document(db_id('AdventureWorks'),   
object_id('HumanResources.JobCandidate'));  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [sp_fulltext_keymappings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)   
 [Améliorer les performances des index de recherche en texte intégral](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)  
  
  
