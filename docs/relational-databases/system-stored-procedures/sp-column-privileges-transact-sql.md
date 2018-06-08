---
title: sp_column_privileges (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_column_privileges_TSQL
- sp_column_privileges
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges
ms.assetid: a3784301-2517-4b1d-bbd9-47404483fad0
caps.latest.revision: 36
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5071d68cfe594b2b4266a5c83398ebdd5a9bfbea
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcolumnprivileges-transact-sql"></a>sp_column_privileges (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Fournit des informations sur les privilèges relatifs aux colonnes d'une table dans l'environnement actuel.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_column_privileges [ @table_name = ] 'table_name'   
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @table_qualifier = ] 'table_qualifier' ]   
     [ , [ @column_name = ] 'column' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @table_name=] '*table_name*'  
 Table utilisée pour retourner les informations de catalogue. *nom_table* est **sysname**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge.  
  
 [ @table_owner=] '*table_owner*'  
 Propriétaire de la table utilisée pour renvoyer des informations de catalogue. *TABLE_OWNER* est **sysname**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques n'est pas prise en charge. Si *table_owner* n’est pas spécifié, les règles de visibilité de table par défaut du système de gestion de base de données (SGBD) sous-jacent.  
  
 Si l'utilisateur actuel possède une table ayant le nom spécifié, ce sont les colonnes de cette table qui sont retournées. Si *table_owner* n’est pas spécifié et l’utilisateur actuel ne possède pas d’une table avec l’objet *table_name*, sp_column privileges recherche une table avec l’objet *table_name* appartenant au propriétaire de la base de données. S'il en existe une, les colonnes de cette table sont retournées.  
  
 [ @table_qualifier=] '*table_qualifier*'  
 Nom du qualificateur de la table. *TABLE_QUALIFIER* est *sysname*, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualificateur ***.*** propriétaire ***.*** nom de*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [ @column_name=] '*colonne*'  
 Colonne unique utilisée lorsqu'une seule colonne d'informations de catalogue est obtenue. *colonne* est **nvarchar (** 384 **)**, avec NULL comme valeur par défaut. Si *colonne* est ne pas spécifié, toutes les colonnes sont renvoyées. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], *colonne* représente le nom de colonne indiqué dans la table sys.columns. *colonne* peut inclure des caractères génériques basés sur les modèles de comparaison du SGBD sous-jacent. Pour assurer une interopérabilité maximale, le client de la passerelle ne doit utiliser que les modèles de comparaison standard ISO (caractères génériques % et _).  
  
## <a name="result-sets"></a>Jeux de résultats  
 sp_column_privileges est équivalent à SQLColumnPrivileges dans ODBC. Les résultats obtenus sont triés par TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME, COLUMN_NAME et PRIVILEGE.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Nom du qualificateur de table. Ce champ peut contenir la valeur NULL.|  
|TABLE_OWNER|**sysname**|Nom du propriétaire de la table. Ce champ retourne toujours une valeur.|  
|TABLE_NAME|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|COLUMN_NAME|**sysname**|Nom de colonne, pour chaque colonne de la TABLE_NAME retournée. Ce champ retourne toujours une valeur.|  
|GRANTOR|**sysname**|Nom de l'utilisateur de base de données qui a accordé les autorisations sur cette COLUMN_NAME au GRANTEE de la liste. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne doit toujours être identique à TABLE_OWNER. Ce champ retourne toujours une valeur.<br /><br /> La colonne GRANTOR peut être soit le propriétaire de la table (TABLE_OWNER), soit un utilisateur auquel le propriétaire de la base de données accorde des autorisations à l'aide de la clause WITH GRANT OPTION de l'instruction GRANT.|  
|GRANTEE|**sysname**|Nom de l'utilisateur de la base de données auquel des autorisations ont été accordées sur la colonne COLUMN_NAME par l'utilisateur GRANTOR mentionné. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne va toujours inclure un utilisateur de la base de données figurant dans la table sysusers. Ce champ retourne toujours une valeur.|  
|PRIVILEGE|**varchar(** 32 **)**|L'une des autorisations sur les colonnes disponibles. Les autorisations relatives aux colonnes peuvent prendre l'une des valeurs suivantes (ou d'autres valeurs prises en charge par la source des données si leur implémentation est définie) :<br /><br /> SELECT = GRANTEE permet de récupérer des données pour les colonnes.<br /><br /> INSERT = GRANTEE peut fournir des données pour cette colonne lorsque de nouvelles lignes sont ajoutées à la table par le GRANTEE.<br /><br /> UPDATE = GRANTEE peut modifier des données existantes dans la colonne.<br /><br /> REFERENCES = GRANTEE peut faire référence à une colonne d'une table étrangère dans une relation clé primaire/clé étrangère. Relations clé primaire/étrangère clées sont définies à l’aide des contraintes de table.|  
|IS_GRANTABLE|**varchar (** 3 **)**|Indique si le GRANTEE (bénéficiaire) est autorisé à accorder des autorisations à d'autres utilisateurs. Il y est souvent fait référence sous le terme de « droit d'accorder ». Les valeurs possibles sont YES, NO ou NULL. Une valeur inconnue, ou NULL, fait référence à une source de données où la « transmission des droits » ne s'applique pas.|  
  
## <a name="remarks"></a>Notes  
 Avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les autorisations sont accordées par l'instruction GRANT et révoquées par l'instruction REVOKE.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les informations sur les privilèges d'une colonne spécifique.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_column_privileges @table_name = 'Employee'   
    ,@table_owner = 'HumanResources'  
    ,@table_qualifier = 'AdventureWorks2012'  
    ,@column_name = 'SalariedFlag';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
