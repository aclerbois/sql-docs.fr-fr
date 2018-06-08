---
title: sp_refreshsqlmodule (Transact-SQL) | Documents Microsoft
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
- sp_refreshsqlmodule_TSQL
- sp_refreshsqlmodule
dev_langs:
- TSQL
helpviewer_keywords:
- metadata [SQL Server], stored procedures
- metadata [SQL Server], triggers
- metadata [SQL Server], views
- triggers [SQL Server], refreshing metadata
- views [SQL Server], refreshing metadata
- sp_refreshsqlmodule
- metadata [SQL Server], functions
- stored procedures [SQL Server], refreshing metadata
- user-defined functions [SQL Server], refreshing metadata
ms.assetid: f0022a05-50dd-4620-961d-361b1681d375
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b54f1410be78cc1be6095a1870fc5b6b9e5b694f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sprefreshsqlmodule-transact-sql"></a>sp_refreshsqlmodule (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Met à jour les métadonnées pour les procédure stockée non liée au schéma, fonction définie par l'utilisateur, vue, déclencheur DML, déclencheur DDL au niveau de la base de données ou déclencheur DDL au niveau du serveur spécifiés dans la base de données actuelle. Les métadonnées persistantes pour ces objets, des types de données des paramètres par exemple, peuvent devenir obsolètes en raison des modifications apportées à leurs objets sous-jacents.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.sp_refreshsqlmodule [ @name = ] 'module_name'   
    [ , [ @namespace = ] ' <class> ' ]  
  
<class> ::=  
{  
  | DATABASE_DDL_TRIGGER  
  | SERVER_DDL_TRIGGER  
}  
  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@name=** ] **'***nom_module***'**  
 Nom de la procédure stockée, fonction définie par l'utilisateur, vue, déclencheur DML, déclencheur DDL au niveau de la base de données ou déclencheur DDL au niveau du serveur. *nom_module* ne peut pas être un common language runtime (CLR) procédure stockée ou une fonction CLR. *nom_module* ne peut pas être liée au schéma. *nom_module* est **nvarchar**, sans valeur par défaut. *nom_module* peut être un identificateur multipartie, mais ne peut faire référence à des objets dans la base de données actuelle.  
  
 [ **,** @**espace de noms** =] **'** \<classe > **'**  
 Classe du module spécifié. Lorsque *nom_module* est un déclencheur DDL, \<classe > est requise. *\<classe >* est **nvarchar**(20). Les entrées valides sont :  
  
|||  
|-|-|  
|DATABASE_DDL_TRIGGER||  
|SERVER_DDL_TRIGGER|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou un nombre différent de zéro (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_refreshsqlmodule** doit être exécuté lorsque des modifications sont apportées aux objets sous-jacents du module qui affectent sa définition. Sinon, le module risque de produire des résultats inattendus en cas d'interrogation ou d'appel. Pour actualiser une vue, vous pouvez utiliser **sp_refreshsqlmodule** ou **sp_refreshview** avec les mêmes résultats.  
  
 **sp_refreshsqlmodule** n’affecte pas les autorisations, les propriétés étendues ou définir les options qui sont associées à l’objet.  
  
 Pour actualiser un déclencheur DDL au niveau du serveur, exécutez cette procédure stockée à partir du contexte de toute base de données.  
  
> [!NOTE]  
>  Les signatures qui sont associés à l’objet sont supprimées lorsque vous exécutez **sp_refreshsqlmodule**.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER sur le module ainsi que l'autorisation REFERENCES sur les types CLR (Common Language Runtime) définis par l'utilisateur et sur les collections de schémas XML référencés par l'objet. Requiert l'autorisation ALTER ANY DATABASE DDL TRIGGER dans la base de données actuelle lorsque le module spécifié est un déclencheur DDL au niveau de la base de données. Requiert l'autorisation CONTROL SERVER lorsque le module spécifié est un déclencheur DDL au niveau du serveur.  
  
 De plus, pour les modules définis à l'aide de la clause EXECUTE AS, l'autorisation IMPERSONATE est nécessaire sur le principal spécifié. Généralement, l'actualisation d'un objet ne modifie pas son principal EXECUTE AS sauf si le module est défini avec EXECUTE AS USER et que le nom d'utilisateur du principal correspond désormais à un autre utilisateur que celui lors de la création du module.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-refreshing-a-user-defined-function"></a>A. Actualisation d'une fonction définie par l'utilisateur  
 L'exemple suivant actualise une fonction définie par l'utilisateur. L'exemple crée un type de données d'alias, `mytype`, et une fonction définie par l'utilisateur, `to_upper`, qui utilise `mytype`. Puis, le nom `mytype` est remplacé par `myoldtype`, et un nouveau `mytype` est créé avec une autre définition. La fonction `dbo.to_upper` est actualisée afin de référencer la nouvelle implémentation de `mytype` en remplacement de l'ancienne.  
  
```  
-- Create an alias type.  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT 'mytype' FROM sys.types WHERE name = 'mytype')  
DROP TYPE mytype;  
GO  
  
CREATE TYPE mytype FROM nvarchar(5);  
GO  
  
IF OBJECT_ID ('dbo.to_upper', 'FN') IS NOT NULL  
DROP FUNCTION dbo.to_upper;  
GO  
  
CREATE FUNCTION dbo.to_upper (@a mytype)  
RETURNS mytype  
WITH ENCRYPTION  
AS  
BEGIN  
RETURN upper(@a)  
END;  
GO  
  
SELECT dbo.to_upper('abcde');  
GO  
  
-- Increase the length of the alias type.  
sp_rename 'mytype', 'myoldtype', 'userdatatype';  
GO  
  
CREATE TYPE mytype FROM nvarchar(10);  
GO  
  
-- The function parameter still uses the old type.  
SELECT name, type_name(user_type_id)   
FROM sys.parameters   
WHERE object_id = OBJECT_ID('dbo.to_upper');  
GO  
  
SELECT dbo.to_upper('abcdefgh'); -- Fails because of truncation  
GO  
  
-- Refresh the function to bind to the renamed type.  
EXEC sys.sp_refreshsqlmodule 'dbo.to_upper';  
  
-- The function parameters are now bound to the correct type and the statement works correctly.  
SELECT name, type_name(user_type_id) FROM sys.parameters  
WHERE object_id = OBJECT_ID('dbo.to_upper');  
GO  
  
SELECT dbo.to_upper('abcdefgh');  
GO  
```  
  
### <a name="b-refreshing-a-database-level-ddl-trigger"></a>B. Actualisation d'un déclencheur DDL au niveau de la base de données  
 L'exemple suivant actualise un déclencheur DDL au niveau de la base de données.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_refreshsqlmodule @name = 'ddlDatabaseTriggerLog' , @namespace = 'DATABASE_DDL_TRIGGER';  
GO  
```  
  
### <a name="c-refreshing-a-server-level-ddl-trigger"></a>C. Actualisation d'un déclencheur DDL au niveau du serveur  
 L'exemple suivant actualise un déclencheur DDL au niveau du serveur.  
  
||  
|-|  
|**S'applique à**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].|  
  
```  
USE master;  
GO  
EXEC sys.sp_refreshsqlmodule @name = 'ddl_trig_database' , @namespace = 'SERVER_DDL_TRIGGER';  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_refreshview &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-refreshview-transact-sql.md)   
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
