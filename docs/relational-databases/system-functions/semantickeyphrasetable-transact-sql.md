---
title: semantickeyphrasetable (Transact-SQL) | Documents Microsoft
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
- semantickeyphrasetable
- semantickeyphrasetable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- semantickeyphrasetable function
ms.assetid: d33b973a-2724-4d4b-aaf7-67675929c392
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 98d837abe05cf99051230e24fd0d418e1263aa4f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="semantickeyphrasetable-transact-sql"></a>semantickeyphrasetable (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne une table de zéro, une ou plusieurs lignes pour les expressions clés associées aux colonnes spécifiées de la table spécifiée.  
  
 Cette fonction d'ensemble de lignes peut uniquement être référencée dans la clause FROM d'une instruction SELECT comme tout nom de table standard.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
SEMANTICKEYPHRASETABLE  
    (  
    table,  
    { column | (column_list) | * }  
     [ , source_key ]  
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
  
 La clé est convertie implicitement en type de clé unique de recherche en texte intégral dans la table source, dans la mesure du possible. La clé peut être spécifiée en tant que constante ou variable, mais il ne peut pas s'agir d'une expression ni du résultat d'une sous-requête scalaire. Si source_key est omis, les résultats sont retournés pour toutes les lignes.  
  
## <a name="table-returned"></a>Table retournée  
 Le tableau suivant décrit les informations sur les expressions clés renvoyées par cette fonction d'ensemble de lignes.  
  
|Column_name|Type| Description|  
|------------------|----------|-----------------|  
|**column_id**|**int**|ID de la colonne à partir de laquelle l’expression clé actuelle a été extraite et indexée.<br /><br /> Consultez les fonctions COL_NAME et COLUMNPROPERTY pour plus d'informations sur la récupération d'un nom de colonne à partir de column_id et inversement.|  
|**document_key**|**\***<br /><br /> Cette clé correspond au type de la clé unique dans la table source.|Valeur de clé unique du document ou de la ligne à partir de laquelle l'expression clé actuelle a été indexée.|  
|**keyphrase**|**NVARCHAR**|Expression clé trouvée dans la colonne identifiée par column_id et associée au document spécifié par document_key.|  
|**Score**|**REAL**|Valeur relative de cette expression clé dans sa relation à toutes les autres expressions clés du même document dans la colonne indexée.<br /><br /> La valeur est une valeur décimale fractionnaire comprise dans la plage de [0.0, 1.0] dans laquelle un score élevé représente une pondération plus élevée, 1.0 étant le score parfait.|  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Pour plus d’informations, consultez [rechercher des expressions clés dans des Documents avec la recherche sémantique](../../relational-databases/search/find-key-phrases-in-documents-with-semantic-search.md).  
  
## <a name="metadata"></a>Métadonnées  
 Pour obtenir des informations et connaître l'état relatif à l'extraction d'expressions clés sémantiques et au remplissage, interrogez les vues de gestion dynamique suivantes :  
  
-   [sys.dm_db_fts_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-fts-index-physical-stats-transact-sql.md)  
  
-   [sys.dm_fts_index_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert des autorisations SELECT sur la table de base sur laquelle les index sémantiques et de recherche en texte intégral ont été créés.  
  
## <a name="examples"></a>Exemples  
  
###  <a name="HowToTopPhrases"></a>Exemple 1 : Rechercher les expressions de niveau supérieure dans un Document spécifique  
 L’exemple suivant extrait les 10 expressions clés de niveau supérieur du document spécifié par la variable @DocumentId dans la colonne Document de la table Production.Document de l’exemple de base de données AdventureWorks. La variable @DocumentId représente une valeur de la colonne clé de l’index de recherche en texte intégral. La fonction **SEMANTICKEYPHRASETABLE** récupère efficacement ces résultats en utilisant une recherche d'index au lieu d'une analyse de table. Cet exemple suppose que la colonne est configurée pour l'indexation de texte intégral et sémantique.  
  
```sql  
SELECT TOP(10) KEYP_TBL.keyphrase  
FROM SEMANTICKEYPHRASETABLE  
    (  
    Production.Document,  
    Document,  
    @DocumentId  
    ) AS KEYP_TBL  
ORDER BY KEYP_TBL.score DESC;  
  
```  
  
###  <a name="HowToTopDocuments"></a>Exemple 2 : Rechercher les Documents de niveau supérieur qui contiennent une expression clé spécifique  
 L'exemple suivant extrait les 25 documents de niveau supérieur qui contiennent l'expression clé « bracket » dans la colonne Document de la table Production.Document de l'exemple de base de données AdventureWorks. Cet exemple suppose que la colonne est configurée pour l'indexation de texte intégral et sémantique.  
  
```sql  
SELECT TOP (25) DOC_TBL.DocumentID, DOC_TBL.DocumentSummary  
FROM Production.Document AS DOC_TBL  
    INNER JOIN SEMANTICKEYPHRASETABLE  
    (  
    Production.Document,  
    Document  
    ) AS KEYP_TBL  
ON DOC_TBL.DocumentID = KEYP_TBL.document_key  
WHERE KEYP_TBL.keyphrase = 'Bracket'  
ORDER BY KEYP_TBL.Score DESC;  
  
```  
  
  
