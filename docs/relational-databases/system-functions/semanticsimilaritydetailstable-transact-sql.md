---
title: semanticsimilaritydetailstable (Transact-SQL) | Documents Microsoft
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
- semanticsimilaritydetailstable
- semanticsimilaritydetailstable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- semanticsimilaritydetailstable function
ms.assetid: 038d751a-fca5-4b4c-9129-cba741a4e173
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 31e23931b0b4b22df06cde0981209c28aecef373
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="semanticsimilaritydetailstable-transact-sql"></a>semanticsimilaritydetailstable (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une table de zéro, une ou plusieurs lignes d'expressions clés communes à travers deux documents (document source et un document mis en correspondance) dont le contenu est similaire sémantiquement.  
  
 Cette fonction d’ensemble de lignes peut être référencée dans la clause FROM d’une instruction SELECT 
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
SEMANTICSIMILARITYDETAILSTABLE  
    (  
    table,  
    source_column,  
    source_key,  
    matched_column,  
    matched_key  
    )  
```  
  
##  <a name="Arguments"></a> Arguments  
 **table**  
 Nom d'une table dont l'indexation sémantique et de texte intégral est activée.  
  
 Ce nom peut être en une à quatre parties, mais un nom de serveur distant n'est pas autorisé.  
  
 **source_column**  
 Nom de la colonne sur la ligne source qui contient le contenu dont la similarité doit être comparée.  
  
 **source_key**  
 Clé unique qui représente la ligne du document source.  
  
 Cette clé est convertie implicitement en type de clé unique de recherche en texte intégral dans la table source, dans la mesure du possible. La clé peut être spécifiée en tant que constante ou variable, mais il ne peut pas s'agir d'une expression ni du résultat d'une sous-requête scalaire. Si une clé non valide est spécifiée, aucune ligne n'est retournée.  
  
 **matched_column**  
 Nom de la colonne sur la ligne correspondante qui contient le contenu dont la similarité doit être comparée.  
  
 **matched_key**  
 Clé unique qui représente la ligne du document correspondant.  
  
 Cette clé est convertie implicitement en type de clé unique de recherche en texte intégral dans la table source, dans la mesure du possible. La clé peut être spécifiée en tant que constante ou variable, mais il ne peut pas s'agir d'une expression ni du résultat d'une sous-requête scalaire.  
  
## <a name="table-returned"></a>Table retournée  
 Le tableau suivant décrit les informations sur les expressions clés renvoyées par cette fonction d'ensemble de lignes.  
  
|Column_name|Type| Description|  
|------------------|----------|-----------------|  
|**keyphrase**|**NVARCHAR**|Expression clé qui contribue à la similarité entre le document source et le document correspondant.|  
|**Score**|**REAL**|Valeur relative de cette expression clé dans sa relation à toutes les autres expressions clés qui sont semblables dans les deux documents.<br /><br /> La valeur est une valeur décimale fractionnaire comprise dans la plage de [0.0, 1.0] dans laquelle un score élevé représente une pondération plus élevée, 1.0 étant le score parfait.|  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Pour plus d’informations, consultez [rechercher Documents similaires ou connexes avec la recherche sémantique](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md).  
  
## <a name="metadata"></a>Métadonnées  
 Pour obtenir des informations et connaître l'état relatif à l'extraction de ressemblance sémantique et au remplissage, interrogez les vues de gestion dynamique suivantes :  
  
-   [sys.dm_db_fts_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-fts-index-physical-stats-transact-sql.md)  
  
-   [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations SELECT sur la table de base sur laquelle les index sémantiques et de recherche en texte intégral ont été créés.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant récupère les 5 expressions clés qui avaient le score de similarité le plus élevé parmi les candidats spécifiés dans **HumanResources.JobCandidate** table de la base de données AdventureWorks2012. Le @CandidateId et @MatchedID les variables représentent des valeurs de la colonne clé de l’index de recherche en texte intégral.  
  
```sql  
SELECT TOP(5) KEY_TBL.keyphrase, KEY_TBL.score  
FROMSEMANTICSIMILARITYDETAILSTABLE  
    (  
    HumanResources.JobCandidate,  
    Resume, @CandidateID,  
    Resume, @MatchedID  
    ) AS KEY_TBL  
ORDER BY KEY_TBL.score DESC;  
  
```  
  
  
