---
title: sp_fulltext_keymappings (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_fulltext_keymappings_TSQL
- sp_fulltext_keymappings
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], key column
- sp_fulltext_keymappings
- full-text indexes [SQL Server], troubleshooting
ms.assetid: 2818fa42-072d-4664-a2f7-7ec363b51d81
caps.latest.revision: 31
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e55e2b1494a8b575e240340969ce127a63b5f2e9
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spfulltextkeymappings-transact-sql"></a>sp_fulltext_keymappings (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-pdw-md.md)]

  Retourne des mappages entre identificateurs de document (DocIds) et valeurs de clé de texte intégral. La colonne DocId contient des valeurs pour un **bigint** entier qui correspond à une valeur de clé de recherche en texte intégral particulière dans une table indexée en texte intégral. Les valeurs de DocId qui répondent à une condition de recherche sont transmises du moteur de texte intégral au moteur de base de données, où elles sont mappées aux valeurs de clé de texte intégral de la table de base interrogée. La colonne clé de texte intégral est un index unique qui est requis sur une seule colonne de la table.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_fulltext_keymappings { table_id | table_id, docid | table_id, NULL, key }  
```  
  
#### <a name="parameters"></a>Paramètres  
 *table_id*  
 ID de l'objet de la table indexée en texte intégral. Si vous spécifiez un non valide *table_id*, une erreur est retournée. Pour plus d’informations sur l’obtention de l’ID d’objet d’une table, consultez [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md).  
  
 *ID de document*  
 Identificateur de document (DocId) interne qui correspond à la valeur de la clé. Une valeur *docid* non valide ne retourne pas de résultat.  
  
 *clé*  
 Valeur de la clé de texte intégral pour une table spécifiée. Une valeur *key* non valide ne retourne pas de résultat. Pour plus d’informations sur les valeurs de clé de recherche en texte intégral, consultez [gérer les index de recherche en texte intégral](http://msdn.microsoft.com/library/28ff17dc-172b-4ac4-853f-990b5dc02fd1).  
  
> [!IMPORTANT]  
>  Pour plus d'informations sur l'utilisation d'un, de deux ou de trois paramètres, consultez « Remarques » plus loin dans cette rubrique.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|DocId|**bigint**|Colonne de l'ID interne de document (DocId) qui correspond à la valeur de la clé.|  
|Key|*|Valeur de la clé de texte intégral pour une table spécifiée.<br /><br /> Si aucune clé de texte intégral n'existe dans la table de mappage, un ensemble de lignes vide est retourné.|  
  
 <sup>*</sup> Le type de données pour la clé est identique au type de données de la colonne de clé de recherche en texte intégral dans la table de base.  
  
## <a name="permissions"></a>Autorisations  
 Cette fonction est publique et ne requiert pas d'autorisation spéciale.  
  
## <a name="remarks"></a>Notes  
 Le tableau ci-dessous décrit l'impact de l'utilisation d'un, de deux ou de trois paramètres.  
  
|Cette liste de paramètres…|Donne le résultat suivant...|  
|--------------------------|----------------------|  
|*table_id*|Lorsqu’elle est appelée avec uniquement le *table_id* , sp_fulltext_keymappings retourne toutes les valeurs de clé de recherche en texte intégral (Key) à partir de la table de base spécifiée, avec le DocId qui correspond à chaque clé. Cela inclut des clés qui sont en attente de suppression.<br /><br /> Cette fonction est utile pour la résolution de divers problèmes. Elle est particulièrement utile pour afficher le contenu d'index de recherche en texte intégral lorsque la clé de texte intégral sélectionnée n'est pas un type de données integer. Cela implique de joindre les résultats de sp_fulltext_keymappings avec les résultats de **sys.dm_fts_index_keywords_by_document**. Pour plus d’informations, consultez [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md).<br /><br /> En général, toutefois, nous vous recommandons, si possible, d'exécuter sp_fulltext_keymappings avec les paramètres qui spécifient une clé de de texte intégral spécifique ou DocId. C'est beaucoup plus efficace que retourner un mappage de clés entier, surtout pour une table très volumineuse pour laquelle le coût de performance lié au retour d'un mappage de clés entier peut être conséquent.|  
|*table_id*, *docid*|Si seul le *table_id* et *docid* sont spécifiés, *docid* doit être non null et spécifier un DocId valide dans la table spécifiée. Cette fonction est utile pour isoler la clé de texte intégral personnalisée de la table de base qui correspond au DocId d'un index de recherche en texte intégral particulier.|  
|*table_id*, NULL, *clé*|Si les trois paramètres sont présents, le deuxième paramètre doit être NULL, et *clé* doit être non null et spécifier une valeur de clé de recherche en texte intégral valide à partir de la table spécifiée. Cette fonction est utile pour isoler le DocId qui correspond à une clé de texte intégral particulière de la table de base.|  
  
 Une erreur est retournée dans l'une des conditions suivantes :  
  
-   Vous spécifiez un non valide *table_id*.  
  
-   La table n'est pas indexée en texte intégral.  
  
-   Une valeur NULL est rencontrée pour un paramètre qui peut être non NULL  
  
## <a name="examples"></a>Exemples  
  
> [!NOTE]  
>  Les exemples de cette section utilisent la `Production.ProductReview` table de la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de données exemple. Vous pouvez créer cet index en exécutant l’exemple fourni pour la `ProductReview` table [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
### <a name="a-obtaining-all-the-key-and-docid-values"></a>A. Obtention de toutes les valeurs de clés et valeurs DocId  
 L’exemple suivant utilise un [DECLARE](../../t-sql/language-elements/declare-local-variable-transact-sql.md) instruction pour créer une variable locale, `@table_id` et affecter l’ID de la `ProductReview` table en tant que sa valeur. L’exemple exécute **sp_fulltext_keymappings** spécifiant `@table_id` pour le *table_id* paramètre.  
  
> [!NOTE]  
>  À l’aide de **sp_fulltext_keymappings** avec uniquement le *table_id* paramètre convient aux petites tables.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @table_id int = OBJECT_ID(N'Production.ProductReview');  
EXEC sp_fulltext_keymappings @table_id;  
GO  
  
```  
  
 Cet exemple retourne toutes les clés DocIds et toutes les clés de texte intégral de la table, comme suit :  
  
||||  
|-|-|-|  
||`docid`|`key`|  
|`1`|`1`|`1`|  
|`2`|`2`|`2`|  
|`3`|`3`|`3`|  
|`4`|`4`|`4`|  
  
### <a name="b-obtaining-the-docid-value-for-a-specific-key-value"></a>B. Obtention de la valeur DocId pour une valeur de clé spécifique  
 L'exemple suivant utilise une instruction DECLARE pour créer une variable locale, `@table_id`, et lui assigner comme valeur l'ID de la table `ProductReview` . L’exemple exécute **sp_fulltext_keymappings** spécifiant `@table_id` pour le *table_id* paramètre, NULL pour le *docid* paramètre et 4 pour le *clé* paramètre.  
  
> [!NOTE]  
>  À l’aide de **sp_fulltext_keymappings** avec uniquement le *table_id* paramètre est adapté aux petites tables.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @table_id int = OBJECT_ID(N'Production.ProductReview');  
EXEC sp_fulltext_keymappings @table_id, NULL, 4;  
GO  
  
```  
  
 Cet exemple retourne les résultats suivants.  
  
||||  
|-|-|-|  
||`docid`|`key`|  
|`4`|`4`|`4`|  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de recherche en texte intégral et la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
