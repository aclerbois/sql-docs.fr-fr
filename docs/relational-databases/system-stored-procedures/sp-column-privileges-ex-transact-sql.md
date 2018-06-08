---
title: sp_column_privileges_ex (Transact-SQL) | Documents Microsoft
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
- sp_column_privileges_ex
- sp_column_privileges_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges_ex
ms.assetid: 98cb6e58-4007-40fc-b048-449fb2e7e6be
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 36e3f3c95614fda36c12309c1252b76cdc2da208
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcolumnprivilegesex-transact-sql"></a>sp_column_privileges_ex (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne les privilèges de colonne de la table spécifiée sur le serveur lié spécifié.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_column_privileges_ex [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]   
     [ , [ @column_name = ] 'column_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table_server =** ] **'***serveur_de_la_table***'**  
 Nom du serveur lié pour lequel renvoyer les informations. *serveur_de_la_table* est **sysname**, sans valeur par défaut.  
  
 [  **@table_name =** ] **'***table_name***'**  
 Nom de la table qui contient la colonne spécifiée. *nom_table* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@table_schema =** ] **'***table_schema***'**  
 Schéma de la table. *table_schema* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@table_catalog =** ] **'***table_catalog***'**  
 Nom de la base de données dans laquelle le texte spécifié *table_name* réside. *table_catalog* est **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@column_name =** ] **'***column_name***'**  
 Nom de la colonne pour laquelle des informations de privilège doivent être fournies. *column_name* est **sysname**, avec une valeur par défaut NULL (tous les commun).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le tableau suivant présente les que colonnes du jeu de résultats. Les résultats obtenus sont triés par **TABLE_QUALIFIER**, **TABLE_OWNER**, **TABLE_NAME**, **COLUMN_NAME**, et **privilège**.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Nom du qualificateur de table. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualificateur ***.*** propriétaire ***.*** nom de*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table. Ce champ peut contenir la valeur NULL.|  
|**TABLE_SCHEM**|**sysname**|Nom du propriétaire de la table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de l'utilisateur de la base de données qui a créé la table. Ce champ retourne toujours une valeur.|  
|**NOM_TABLE**|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|**COLUMN_NAME**|**sysname**|Nom de colonne, pour chaque colonne de la **TABLE_NAME** retourné. Ce champ retourne toujours une valeur.|  
|**FOURNISSEUR D’AUTORISATIONS**|**sysname**|Nom d’utilisateur de base de données qui a accordé les autorisations sur ce **COLUMN_NAME** à la liste **bénéficiaire**. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne est toujours le même que le **TABLE_OWNER**. Ce champ retourne toujours une valeur.<br /><br /> Le **GRANTOR** colonne peut être soit le propriétaire de la base de données (**TABLE_OWNER**) ou un utilisateur auquel le propriétaire de la base de données a accordé des autorisations à l’aide de la clause WITH GRANT OPTION de l’instruction GRANT.|  
|**BÉNÉFICIAIRE**|**sysname**|Nom d’utilisateur de base de données qui a été accordé des autorisations sur **COLUMN_NAME** par le **GRANTOR**. Ce champ retourne toujours une valeur.|  
|**PRIVILÈGE**|**varchar(** 32 **)**|L'une des autorisations sur les colonnes disponibles. Les autorisations relatives aux colonnes peuvent prendre l'une des valeurs suivantes (ou d'autres valeurs prises en charge par la source des données si leur implémentation est définie) :<br /><br /> Sélectionnez = **bénéficiaire** peut récupérer des données pour les colonnes.<br /><br /> INSERT = **bénéficiaire** peut fournir des données pour cette colonne lorsque de nouvelles lignes sont insérées (par le **bénéficiaire**) dans la table.<br /><br /> Mise à jour = **bénéficiaire** peuvent modifier les données existantes dans la colonne.<br /><br /> RÉFÉRENCES = **bénéficiaire** peut faire référence à une colonne dans une table étrangère dans une relation de clé étrangère/clé primaire. Les relations clé primaire/clé étrangère sont définies grâce à des contraintes portant sur les tables.|  
|**IS_GRANTABLE**|**varchar (** 3 **)**|Indique si le **bénéficiaire** est autorisé à accorder des autorisations à d’autres utilisateurs (souvent appelés « droit d’accorder »). Les valeurs possibles sont YES, NO ou NULL. Une valeur inconnue ou NULL renvoie à une source de données où le « droit d'accorder » ne s'applique pas.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations de privilèges de colonne relatives à la table `HumanResources.Department` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] située sur le serveur lié `Seattle1`.  
  
```  
EXEC sp_column_privileges_ex @table_server = 'Seattle1',   
   @table_name = 'Department',   
   @table_schema = 'HumanResources',  
   @table_catalog ='AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_table_privileges_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-ex-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
