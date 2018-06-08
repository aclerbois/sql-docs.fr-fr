---
title: sp_table_privileges (Transact-SQL) | Documents Microsoft
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
- sp_table_privileges
- sp_table_privileges_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_table_privileges
ms.assetid: 0512e688-4fc0-4557-8dc8-016672c1e3fe
caps.latest.revision: 36
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 9d2863e17258fd0dea68548a15c059676c745424
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptableprivileges-transact-sql"></a>sp_table_privileges (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne la liste des autorisations de table (telles que INSERT, DELETE, UPDATE, SELECT, REFERENCES) pour la ou les tables spécifiées.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_table_privileges [ @table_name = ] 'table_name'     
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @table_qualifier = ] 'table_qualifier' ]   
     [ , [ @fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @table_name=] '*table_name*'  
 Table utilisée pour retourner les informations de catalogue. *nom_table* est **nvarchar (** 384 **)**, sans valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge.  
  
 [ @table_owner=] '*table_owner*'  
 Est le propriétaire de la table de la table utilisée pour retourner des informations de catalogue. *TABLE_OWNER*est **nvarchar (** 384 **)**, avec NULL comme valeur par défaut. La recherche de correspondance avec des caractères génériques est prise en charge. Si le propriétaire n'est pas précisé, les règles par défaut définissant la visibilité des tables du SGBD sous-jacent sont utilisées.  
  
 Si l'utilisateur actuel est propriétaire d'une table portant le nom spécifié, les colonnes de cette table sont renvoyées. Si *propriétaire* n’est pas spécifié et l’utilisateur actuel ne possède pas d’une table avec l’objet *nom*, cette procédure recherche une table avec l’objet *table_name* appartenant au propriétaire de la base de données. Si la recherche de la table aboutit, ce sont les colonnes de cette dernière qui sont retournées.  
  
 [ @table_qualifier=] '*table_qualifier*'  
 Nom du qualificateur de la table. *TABLE_QUALIFIER* est **sysname**, avec NULL comme valeur par défaut. Divers produits SGBD prennent en charge d’affectation de noms en trois parties pour les tables (*qualifier.owner.name*). Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Dans d'autres produits, elle représente le nom du serveur de l'environnement de base de données de la table.  
  
 [ @fUsePattern=] '*fUsePattern*'  
 Détermine si le trait de soulignement (_), pourcentage (%) et entre crochets ([ou]) caractères sont interprétés comme des caractères génériques. Les valeurs valides sont 0 (critères spéciaux désactivés) et 1 (critères spéciaux activés). *fUsePattern* est **bits**, avec 1 comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Nom du qualificateur de table. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne représente le nom de la base de données. Ce champ peut contenir la valeur NULL.|  
|TABLE_OWNER|**sysname**|Nom du propriétaire de la table. Ce champ retourne toujours une valeur.|  
|TABLE_NAME|**sysname**|Nom de la table. Ce champ retourne toujours une valeur.|  
|GRANTOR|**sysname**|Nom de l'utilisateur de la base de données qui a accordé des autorisations sur la table TABLE_NAME au GRANTEE répertorié. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne doit toujours être identique à TABLE_OWNER. Ce champ retourne toujours une valeur. D'autre part, la colonne GRANTOR représente soit le propriétaire de la table TABLE_OWNER, soit un utilisateur auquel le propriétaire de la base de données a accordé des autorisations à l'aide de la clause WITH GRANT OPTION de l'instruction GRANT.|  
|GRANTEE|**sysname**|Nom de l'utilisateur de la base de données qui a reçu des autorisations sur la table TABLE_NAME de l'utilisateur GRANTEE répertorié. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne va toujours inclure un utilisateur de base de données à partir de la vue sys.database_principalssystem. Ce champ retourne toujours une valeur.|  
|PRIVILEGE|**sysname**|L'une des autorisations disponibles sur la table. Les autorisations sur les tables peuvent prendre l'une des valeurs suivantes (ou d'autres valeurs prises en charge par la source de données si leur mise en œuvre est définie) :<br /><br /> SELECT = GRANTEE peut extraire des données pour une ou plusieurs colonnes.<br /><br /> INSERT = GRANTEE peut fournir des données à une ou plusieurs colonnes en rajoutant de nouvelles lignes.<br /><br /> UPDATE = GRANTEE peut modifier des données existantes dans une ou plusieurs colonnes.<br /><br /> DELETE = GRANTEE peut supprimer des lignes de la table.<br /><br /> REFERENCES = GRANTEE peut faire référence à une colonne d'une table étrangère dans une relation clé primaire/clé étrangère. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les relations clé primaire/clé étrangère sont définies grâce à des contraintes portant sur les tables.<br /><br /> Le rayon d'action qu'un privilège de table particulier accorde à l'utilisateur GRANTEE dépend de la source de données. Par exemple, le privilège UPDATE peut autoriser le GRANTEE à mettre à jour toutes les colonnes d'une table pour une source de données, et uniquement les colonnes pour lesquelles le GRANTOR possède le privilège UPDATE pour une autre source de données.|  
|IS_GRANTABLE|**sysname**|Indique si le GRANTEE est autorisé ou non à accorder des autorisations à d'autres utilisateurs. Il y est souvent fait référence sous le terme « transmission des droits ». Les valeurs possibles sont YES, NO ou NULL. Une valeur inconnue (ou NULL) fait référence à une source de données à laquelle la « transmission des droits » ne s'applique pas.|  
  
## <a name="remarks"></a>Notes  
 La procédure stockée sp_table_privileges est équivalente à SQLTablePrivileges dans ODBC. Les résultats retournés sont classés par TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME et PRIVILEGE.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation SELECT sur le schéma.  
  
## <a name="examples"></a>Exemples  
 Cet exemple retourne des informations de privilège sur toutes les tables dont le nom commence par le mot `Contact`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_table_privileges   
   @table_name = 'Contact%';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
