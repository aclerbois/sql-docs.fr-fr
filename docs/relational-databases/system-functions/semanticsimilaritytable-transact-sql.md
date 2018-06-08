---
title: semanticsimilaritytable (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- semanticsimilaritytable
- semanticsimilaritytable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- semanticsimilaritytable function
ms.assetid: b49d40ab-7552-438b-ad67-6237dcccb75b
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8b6b4b36580c35aead16780f4ada0f461dd58754
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="semanticsimilaritytable-transact-sql"></a>semanticsimilaritytable (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une table de zéro, une ou plusieurs lignes pour les documents dont le contenu dans la colonne spécifiée est sémantiquement similaire à un document spécifié.  
  
 Cette fonction d'ensemble de lignes peut être référencée dans la clause FROM d'une instruction SELECT comme un nom de table classique.  

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
SEMANTICSIMILARITYTABLE  
    (  
    table,  
    { column | (column_list) | * },  
    source_key  
    )  
```  
  
##  <a name="Arguments"></a> Arguments  
 **table**  
 Nom d'une table dont l'indexation sémantique et de texte intégral est activée.  
  
 Ce nom peut être en une à quatre parties, mais un nom de serveur distant n'est pas autorisé.  
  
 **column**  
 Nom de la colonne indexée pour laquelle les résultats doivent être retournés L'indexation sémantique doit être activée pour la colonne.  
  
 **column_list**  
 Indique plusieurs colonnes, séparées par une virgule et placées entre parenthèses. L'indexation sémantique doit être activée pour toutes les colonnes.  
  
 **\***  
 Indique que toutes les colonnes pour lesquelles l'indexation sémantique est activée sont incluses.  
  
 **source_key**  
 Clé unique de la ligne, pour demander des résultats d'une ligne spécifique.  
  
 La clé est convertie implicitement en type de clé unique de recherche en texte intégral dans la table source, dans la mesure du possible. La clé peut être spécifiée en tant que constante ou variable, mais il ne peut pas s'agir d'une expression ni du résultat d'une sous-requête scalaire.  
  
## <a name="table-returned"></a>Table retournée  
 Le tableau suivant décrit les informations sur les documents similaires ou associés renvoyés par cette fonction d'ensemble de lignes.  
  
 Les documents correspondants sont retournés colonne par colonne si les résultats sont demandés pour plusieurs colonnes.  
  
|Column_name|Type| Description|  
|------------------|----------|-----------------|  
|**source_column_id**|**int**|ID de la colonne depuis laquelle un document source a été utilisé pour rechercher des documents similaires.<br /><br /> Consultez les fonctions COL_NAME et COLUMNPROPERTY pour plus d'informations sur la récupération d'un nom de colonne à partir de column_id et inversement.|  
|**matched_column_id**|**int**|ID de la colonne depuis laquelle un document similaire a été trouvé.<br /><br /> Consultez les fonctions COL_NAME et COLUMNPROPERTY pour plus d'informations sur la récupération d'un nom de colonne à partir de column_id et inversement.|  
|**matched_document_key**|**\***<br /><br /> Cette clé correspond au type de la clé unique dans la table source.|Valeur de la clé unique de l'extraction sémantique et de recherche en texte intégral du document ou de la ligne considéré(e) comme similaire au document spécifié dans la requête.|  
|**Score**|**REAL**|Valeur relative de ressemblance pour ce document dans sa relation à tous les autres documents similaires.<br /><br /> La valeur est une valeur décimale fractionnaire dans la plage de [0.0, 1.0] dans laquelle un score élevé représente une correspondance plus proche, 1.0 étant un score parfait.|  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Pour plus d’informations, consultez [rechercher Documents similaires ou connexes avec la recherche sémantique](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md).  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Vous ne pouvez pas rechercher des documents similaires dans plusieurs colonnes. Le **SEMANTICSIMILARITYTABLE** fonction récupère uniquement les documents similaires provenant de la même colonne que la colonne source, qui est identifiée par le **source_key** argument.  
  
## <a name="metadata"></a>Métadonnées  
 Pour obtenir des informations et connaître l'état relatif à l'extraction de ressemblance sémantique et au remplissage, interrogez les vues de gestion dynamique suivantes :  
  
-   [sys.dm_db_fts_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-fts-index-physical-stats-transact-sql.md)  
  
-   [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations SELECT sur la table de base sur laquelle les index sémantiques et de recherche en texte intégral ont été créés.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant récupère les 10 premiers candidats similaires à un candidat spécifié dans la table HumanResources.JobCandidate de l'exemple de base de données AdventureWorks2012.  
  
```scr  
SELECT TOP(10) KEY_TBL.matched_document_key AS Candidate_ID  
FROMSEMANTICSIMILARITYTABLE  
    (  
    HumanResources.JobCandidate,  
    Resume,  
    @CandidateID  
    ) AS KEY_TBL  
ORDER BY KEY_TBL.score DESC;  
  
```  
  
  
