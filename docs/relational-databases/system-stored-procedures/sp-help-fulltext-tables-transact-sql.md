---
title: sp_help_fulltext_tables (Transact-SQL) | Documents Microsoft
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
- sp_help_fulltext_tables
- sp_help_fulltext_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_tables
ms.assetid: 86e24a5f-a869-43f6-b83e-c52b7b01b5ff
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 855a9a5bde98703909a01b44721c5972c1eefff2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpfulltexttables-transact-sql"></a>sp_help_fulltext_tables (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche la liste des tables qui sont inscrites pour l'indexation de texte intégral.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez **sys.fulltext_indexes** affichage du catalogue à la place. Pour plus d’informations, consultez [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_fulltext_tables [ [ @fulltext_catalog_name = ] 'fulltext_catalog_name' ]   
     [ , [ @table_name = ] 'table_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@fulltext_catalog_name=**] **'***fulltext_catalog_name***'**  
 Nom du catalogue de texte intégral. *fulltext_catalog_name* est **sysname**, avec NULL comme valeur par défaut. Si *fulltext_catalog_name* est omis ou a la valeur NULL, toutes les tables indexées en texte intégral associés à la base de données sont retournées. Si *fulltext_catalog_name* est spécifié, mais *table_name* est omis ou a la valeur NULL, les informations d’index de recherche en texte intégral sont récupérées pour chaque table indexée en texte intégral associé au catalogue. Si les deux *fulltext_catalog_name* et *table_name* sont spécifiés, une ligne est retournée si *table_name* associé *fulltext_catalog_name*; sinon, une erreur est générée.  
  
 [  **@table_name=**] **'***table_name***'**  
 Nom de la table en une ou deux parties pour laquelle les métadonnées d'index de texte intégral sont demandées. *nom_table* est **nvarchar (517)**, avec NULL comme valeur par défaut. Si une seule *table_name* est spécifié, la ligne concernant *table_name* est retourné.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Propriétaire de la table. Nom de l'utilisateur de la base de données qui a créé la table.|  
|**NOM_TABLE**|**sysname**|Nom de la table.|  
|**FULLTEXT_KEY_INDEX_NAME**|**sysname**|Index imposant la contrainte UNIQUE dans la colonne désignée comme colonne clé unique.|  
|**FULLTEXT_KEY_COLID**|**int**|ID de colonne de l'index unique identifié par FULLTEXT_KEY_NAME.|  
|**FULLTEXT_INDEX_ACTIVE**|**int**|Indique si les colonnes de cette table marquées pour l'indexation de texte intégral peuvent faire l'objet de requêtes :<br /><br /> 0 = Inactif<br /><br /> 1 = Actif|  
|**FULLTEXT_CATALOG_NAME**|**sysname**|Catalogue de texte intégral contenant les données d'index sur le texte intégral.|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution reviennent par défaut aux membres du rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne le nom des tables indexées sur le texte intégral associées au catalogue de texte intégral `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help_fulltext_tables 'Cat_Desc';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_fulltext_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
