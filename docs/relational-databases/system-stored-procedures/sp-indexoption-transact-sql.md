---
title: sp_indexoption (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_indexoption
- sp_indexoption_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_indexoption
ms.assetid: 75f836be-d322-4a53-a45d-25bee6b42a52
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6bc44ee2cbce8c96b314172a2bb856a9c3346e2a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spindexoption-transact-sql"></a>sp_indexoption (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Définit les valeurs d'option de verrouillage des index cluster et non cluster ou des tables dépourvues d'index cluster définis par l'utilisateur.  
  
 Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] choisit automatiquement le niveau de verrouillage, à savoir table, ligne ou page. Vous n'avez pas besoin de définir ces options manuellement. **sp_indexoption** est destinée aux utilisateurs expérimentés qui savent avec certitude quel type de verrou est toujours.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)] Au lieu de cela, utilisez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_indexoption [ @IndexNamePattern = ] 'table_or_index_name'   
    , [ @OptionName = ] 'option_name'   
    , [ @OptionValue = ] 'value'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@IndexNamePattern=**] **'***table_or_index_name***'**  
 Spécifie le nom qualifié ou non qualifié d'une table ou d'un index défini par l'utilisateur. *table_or_index_name* est **nvarchar(1035)**, sans valeur par défaut. Les guillemets ne sont nécessaires que si l'on spécifie un nom qualifié de table ou d'index. Si un nom de table complet (incluant un nom de base de données) est fourni, le nom de base de données doit être celui de la base de données en cours. Si un nom de table est spécifié sans index, la valeur d'option spécifiée est définie pour tous les index de cette table et, si aucun index cluster n'existe, pour la table elle-même.  
  
 [  **@OptionName =**] **'***option_name***'**  
 Nom d'option d'index. *option_name* est **varchar (35)**, sans valeur par défaut. *option_name* peut avoir l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**AllowRowLocks**|Si la valeur est TRUE, les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés. Si la valeur est FALSE, les verrous de ligne ne sont pas utilisés. La valeur par défaut est TRUE.|  
|**AllowPageLocks**|Si la valeur est TRUE, les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés. Si la valeur est FALSE, les verrous de page ne sont pas utilisés. La valeur par défaut est TRUE.|  
|**DisAllowRowLocks**|Si la valeur est TRUE, les verrous de ligne ne sont pas utilisés. Si la valeur est FALSE, les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés.|  
|**DisAllowPageLocks**|Si la valeur est TRUE, les verrous de page ne sont pas utilisés. Si la valeur est FALSE, les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés.|  
  
 [  **@OptionValue =**] **'***valeur***'**  
 Spécifie si le *option_name* paramètre est activé (TRUE, ON, yes ou 1) ou désactivé (FALSE, OFF, no ou 0). *valeur* est **varchar(12)**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou supérieur à 0 (échec)  
  
## <a name="remarks"></a>Notes  
 Les index XML ne sont pas pris en charge. Si un index XML est spécifié ou qu'un nom de table est spécifié sans nom d'index et que la table contient un index XML, l'instruction échoue. Pour définir ces options, utilisez [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) à la place.  
  
 Pour afficher la page Propriétés de verrouillage et ligne actuelle, utilisez [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md) ou [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) vue de catalogue.  
  
-   Ligne, page et les verrous de niveau table ne sont autorisés lors de l’accès à l’index lors de la **AllowRowLocks** = TRUE ou **DisAllowRowLocks** = FALSE, et **AllowPageLocks** = TRUE ou **DisAllowPageLocks** = FALSE. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] choisit le verrou approprié et peut promouvoir un verrou de ligne ou de page en verrou de table.  
  
 Seul un verrou de niveau table est autorisé lorsque l’accès à l’index lors de la **AllowRowLocks** = FALSE ou **DisAllowRowLocks** = TRUE et **AllowPageLocks** = FALSE ou **DisAllowPageLocks** = TRUE.  
  
 Si un nom de table est spécifié sans index, les paramétrages sont appliqués à tous les index de cette table. Lorsque la table sous-jacente ne possède aucun index cluster (en d'autres termes, il s'agit d'un segment de mémoire), les paramétrages sont appliqués comme suit :  
  
-   Lorsque **AllowRowLocks** ou **DisAllowRowLocks** sont la valeur TRUE ou FALSE, le paramètre est appliqué au segment de mémoire et de tout index non-cluster associé.  
  
-   Lorsque **AllowPageLocks** option est définie sur TRUE ou **DisAllowPageLocks** est définie sur FALSE, le paramètre est appliqué au segment de mémoire et de tout index non-cluster associé.  
  
-   Lorsque **AllowPageLocks** option a la valeur FALSE ou **DisAllowPageLocks** est définie sur TRUE, le paramètre est appliqué entièrement aux index non cluster. En d'autres termes, tous les verrous de page sont interdits sur les index non-cluster. Sur le segment de mémoire, seuls les verrous partagés (S), de mise à jour (U) et exclusifs (X) de la page sont interdits. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut toujours acquérir un verrou de page intentionnel (IS, IU ou IX) à des fins internes.  
  
## <a name="permissions"></a>Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-an-option-on-a-specific-index"></a>A. Définition d'une option sur un index spécifique  
 L’exemple suivant interdit les verrous de page sur la `IX_Customer_TerritoryID` d’index sur la `Customer` table.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_indexoption N'Sales.Customer.IX_Customer_TerritoryID',  
    N'disallowpagelocks', TRUE;  
```  
  
### <a name="b-setting-an-option-on-all-indexes-on-a-table"></a>B. Définition d'une option sur tous les index d'une table  
 L'exemple suivant interdit les verrous de ligne sur tous les index associés à la table `Product`. L'interrogation de l'affichage catalogue `sys.indexes` avant et après l'exécution de la procédure `sp_indexoption` permet d'afficher les résultats de l'instruction.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
-- Set the disallowrowlocks option on the Product table.   
EXEC sp_indexoption N'Production.Product',  
    N'disallowrowlocks', TRUE;  
GO  
--Verify the row and page lock options for all indexes on the table.  
SELECT name, type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE object_id = OBJECT_ID(N'Production.Product');  
GO  
```  
  
### <a name="c-setting-an-option-on-a-table-with-no-clustered-index"></a>C. Définition d'une option sur une table dépourvue d'index cluster  
 L'exemple suivant interdit les verrous de page sur une table dépourvue d'index cluster (un segment de mémoire). Le `sys.indexes` affichage catalogue est interrogé avant et après le `sp_indexoption` procédure est exécutée pour afficher les résultats de l’instruction.  
  
```sql  
USE AdventureWorks2012;  
GO  
--Display the current row and page lock options of the table.   
SELECT OBJECT_NAME (object_id) AS [Table], type_desc, allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
-- Set the disallowpagelocks option on the table.   
EXEC sp_indexoption DatabaseLog,  
    N'disallowpagelocks', TRUE;  
GO  
--Verify the row and page lock settings of the table.  
SELECT OBJECT_NAME (object_id) AS [Table], allow_row_locks, allow_page_locks   
FROM sys.indexes  
WHERE OBJECT_NAME (object_id) = N'DatabaseLog';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
