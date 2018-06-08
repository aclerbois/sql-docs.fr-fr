---
title: Sys.dm_fts_index_keywords (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- dm_fts_index_keywords
- sys.dm_fts_index_keywords
- sys.dm_fts_index_keywords_TSQL
- dm_fts_index_keywords_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords dynamic management function
- full-text search [SQL Server], viewing keywords
- troubleshooting [SQL Server], full-text search
ms.assetid: fce7b2a1-7e74-4769-86a8-c77c7628decd
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fffcfdc4a7db8fafbe58b0abd914ce611edf3732
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmftsindexkeywords-transact-sql"></a>sys.dm_fts_index_keywords (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur le contenu d'un index de recherche en texte intégral pour la table spécifiée.  
  
 **Sys.dm_fts_index_keywords** est une fonction de gestion dynamique.  
  
> [!NOTE]  
>  Pour afficher les informations de l’index de recherche en texte intégral de niveau inférieur, utilisez la [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md) fonction de gestion dynamique au niveau du document.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_fts_index_keywords( DB_ID('database_name'), OBJECT_ID('table_name') )  
```  
  
## <a name="arguments"></a>Arguments  
 DB_ID ('*nom_base_de_données*')  
 Un appel à la [DB_ID()](../../t-sql/functions/db-id-transact-sql.md) (fonction). Cette fonction accepte un nom de base de données et retourne l’ID de la base de données, ce qui **sys.dm_fts_index_keywords** utilise pour rechercher la base de données spécifié. Si *database_name* est omis, la fonction retourne l’ID de la base de données active.  
  
 object_id ('*table_name*')  
 Un appel à la [OBJECT_ID()](../../t-sql/functions/object-id-transact-sql.md) (fonction). Cette fonction accepte un nom de table et retourne l'ID de la table contenant l'index de recherche en texte intégral à examiner.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**Mot clé**|**nvarchar(4000)**|La représentation hexadécimale du mot clé stocké à l’intérieur de l’index de recherche en texte intégral.<br /><br /> Remarque : OxFF représente le caractère spécial qui indique la fin d’un fichier ou un jeu de données.|  
|**display_term**|**nvarchar(4000)**|Format explicite du mot clé. Ce format est dérivé du format hexadécimal.<br /><br /> Remarque : Le **display_term** valeur de OxFF est « Fin de fichier ».|  
|**column_id**|**int**|ID de la colonne à partir de laquelle le mot clé actuel a été indexé en texte intégral.|  
|**document_count**|**int**|Nombre de documents ou de lignes contenant le terme actuel.|  
  
## <a name="remarks"></a>Notes  
 Les informations retournées par **sys.dm_fts_index_keywords** est utile pour rechercher les éléments suivants, entre autres choses :  
  
-   Si un mot clé fait partie de l'index de recherche en texte intégral.  
  
-   Le nombre de documents ou de lignes qui contiennent un mot clé donné.  
  
-   Le mot clé le plus courant dans l'index de recherche en texte intégral :  
  
    -   **document_count** de chaque *keyword_value* par rapport au total **document_count**, le nombre de documents de 0xFF.  
  
    -   En règle générale, les mots clés courants peuvent être déclarés en tant que mots vides.  
  
> [!NOTE]  
>  Le **document_count** retourné par **sys.dm_fts_index_keywords** peut être moins précis pour un document spécifique que le nombre retourné par **sys.dm_fts_index_keywords_by_document** ou un **CONTAINS** requête. Cette imprécision éventuelle est estimée inférieure à 1 %. Cette peut se produire un **document_id** risquent d’être comptées deux fois si elle se poursuit à travers plusieurs lignes dans le fragment d’index, ou lorsqu’il apparaît plusieurs fois dans la même ligne. Pour obtenir un nombre plus précis pour un document spécifique, utilisez **sys.dm_fts_index_keywords_by_document** ou un **CONTAINS** requête.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-high-level-full-text-index-content"></a>A. Affichage du contenu de l'index de recherche en texte intégral au niveau supérieur  
 L'exemple suivant affiche des informations sur le contenu du niveau supérieur de l'index de recherche en texte intégral de la table `HumanResources.JobCandidate`.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords(db_id('AdventureWorks2012'), object_id('HumanResources.JobCandidate'))  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral et les fonctions et vues de gestion dynamique de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
