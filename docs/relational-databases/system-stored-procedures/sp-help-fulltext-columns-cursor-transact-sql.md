---
title: sp_help_fulltext_columns_cursor (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-stored-procedures
ms.reviewer: 
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- sp_help_fulltext_columns_cursor
- sp_help_fulltext_columns_cursor_TSQL
dev_langs: TSQL
helpviewer_keywords: sp_help_fulltext_columns_cursor
ms.assetid: 26054e76-53b7-4004-8d48-92ba3435e9d7
caps.latest.revision: "28"
author: douglaslMS
ms.author: douglasl
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: cd12180fc4c5c4a923c8a89b5a204468072d94bd
ms.sourcegitcommit: 66bef6981f613b454db465e190b489031c4fb8d3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2017
---
# <a name="sphelpfulltextcolumnscursor-transact-sql"></a>sp_help_fulltext_columns_cursor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Utilise un curseur pour retourner les colonnes marquées pour l'indexation de texte intégral.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Utilisez le [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) affichage du catalogue à la place.  
  
||  
|-|  
|**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].|  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_fulltext_columns_cursor [ @cursor_return = ] @cursor_variable OUTPUT   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@cursor_return =**]  *@cursor_variable*  SORTIE  
 Variable de sortie de type **curseur**. Le curseur résultant est en lecture seule, dynamique et autorise les défilements.  
  
 [  **@table_name =**] **'***table_name***'**  
 Nom de la table en une ou deux parties pour laquelle les informations sur l'indexation de texte intégral sont nécessaires. *nom_table* est **nvarchar (517)**, avec NULL comme valeur par défaut. Si *table_name* est omis, les informations de colonne de l’index de texte intégral sont récupérées pour chaque table indexée en texte intégral.  
  
 [  **@column_name =**] **'***column_name***'**  
 Nom de la colonne dont vous souhaitez obtenir les métadonnées d'index de texte intégral. *column_name* est **sysname** avec une valeur par défaut NULL. Si *column_name* est omis ou a la valeur NULL, les informations de colonne de recherche en texte intégral sont retournées pour chaque colonne indexée en texte intégral de *table_name*. Si *table_name* est également omis ou a la valeur NULL, informations de colonne d’index de texte intégral sont retournées pour chaque colonne indexée en texte intégral pour toutes les tables dans la base de données.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Propriétaire de la table. Nom de l'utilisateur de la base de données qui a créé la table.|  
|**TABLE_ID**|**int**|ID de la table.|  
|**NOM_TABLE**|**sysname**|Nom de la table.|  
|**FULLTEXT_COLUMN_NAME**|**sysname**|Colonne d'une table indexée de texte intégral désignée pour l'indexation.|  
|**FULLTEXT_COLID**|**int**|ID de la colonne indexées sur le texte intégral.|  
|**FULLTEXT_BLOBTP_COLNAME**|**sysname**|Colonne d'une table indexée sur le texte intégral qui désigne le type de document de la colonne indexée sur le texte intégral. Cette valeur s’applique uniquement lorsque la colonne indexée en texte intégral est un **varbinary (max)** ou **image** colonne.|  
|**FULLTEXT_BLOBTP_COLID**|**int**|Identification de la colonne du type de document. Cette valeur s’applique uniquement lorsque la colonne indexée en texte intégral est un **varbinary (max)** ou **image** colonne.|  
|**FULLTEXT_LANGUAGE**|**sysname**|Langue utilisée pour la recherche en texte intégral sur la colonne.|  
  
## <a name="permissions"></a>Permissions  
 Les autorisations d'exécution reviennent par défaut aux membres du rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur les colonnes désignées pour l'indexation de texte intégral dans toutes les tables de la base de données.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @mycursor CURSOR;  
EXEC sp_help_fulltext_columns_cursor @mycursor OUTPUT  
FETCH NEXT FROM @mycursor;  
WHILE (@@FETCH_STATUS <> -1)  
   BEGIN  
      FETCH NEXT FROM @mycursor;  
   END;  
CLOSE @mycursor;  
DEALLOCATE @mycursor;  
GO   
```  
  
## <a name="see-also"></a>Voir aussi  
 [COLUMNPROPERTY &#40; Transact-SQL &#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [sp_fulltext_column &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/sp-fulltext-column-transact-sql.md)   
 [sp_help_fulltext_columns &#40; Transact-SQL &#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  