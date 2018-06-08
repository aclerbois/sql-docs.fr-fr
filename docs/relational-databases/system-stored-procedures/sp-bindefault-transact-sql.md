---
title: sp_bindefault (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_bindefault
- sp_bindefault_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindefault
ms.assetid: 3da70c10-68d0-4c16-94a5-9e84c4a520f6
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a5bf898e8868006ab536bbd83bce3bd428aa5934
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spbindefault-transact-sql"></a>sp_bindefault (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Lie une valeur par défaut à une colonne ou à un type de données d'alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Nous vous recommandons de créer des définitions par défaut en utilisant le mot clé DEFAULT le [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) ou [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) instructions à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_bindefault [ @defname = ] 'default' ,   
    [ @objname = ] 'object_name'   
    [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@defname=** ] **'***par défaut***'**  
 Nom de la valeur par défaut créée par l'instruction CREATE DÉFAUT. *par défaut* est **nvarchar(776)**, sans valeur par défaut.  
  
 [  **@objname=** ] **'***nom_objet***'**  
 Nom de la table et de la colonne ou type de données d'alias, auquel est liée la valeur par défaut. *nom_objet* est **nvarchar(776)** sans valeur par défaut. *nom_objet* ne peut pas être défini avec la **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **xml**, ou CLR types définis par l’utilisateur.  
  
 Si *nom_objet* est un nom d’une seule partie, il est résolu en tant que type de données alias. S'il s'agit d'un nom à deux ou trois composantes, il est d'abord résolu en tant que table et colonne. Si la résolution échoue, il est résolu en tant que type de données d'alias. Par défaut, les colonnes existantes du type de données alias héritent *par défaut*, sauf si une valeur par défaut a été liée directement à la colonne. Une valeur par défaut ne peut pas être lié à un **texte**, **ntext**, **image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **xml**, **timestamp**, ou CLR colonne de type défini par l’utilisateur, une colonne avec la propriété IDENTITY, une colonne calculée ou une colonne qui a déjà une contrainte par défaut.  
  
> [!NOTE]  
>  *nom_objet* peut contenir des crochets **[]** en tant qu’identificateurs délimités. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
 [  **@futureonly=** ] **'***futureonly_flag***'**  
 S'utilise seulement pour lier une valeur par défaut à un type de données d'alias. *l’argument futureonly_flag* est **varchar(15)** avec NULL comme valeur par défaut. Lorsque ce paramètre a la valeur **futureonly**, les colonnes existantes de ce type de données ne peut pas hériter de la nouvelle valeur par défaut. Il ne s'emploie jamais lors de la liaison d'une valeur par défaut à une colonne. Si *futureonly_flag* est NULL, la nouvelle valeur par défaut est liée à toute colonne du type de données alias qui n’a aucune valeur par défaut ou qui sont à l’aide de la valeur par défaut existante du type de données alias.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser **sp_bindefault** pour lier une nouvelle valeur par défaut à une colonne, bien que l’utilisation de la contrainte par défaut est par défaut, ou à un type de données alias sans dissocier une valeur par défaut existante. L'ancienne valeur par défaut est remplacée par la nouvelle. Vous ne pouvez pas lier une valeur par défaut à une type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à un type CLR défini par l'utilisateur. En cas d'incompatibilité entre la valeur par défaut et la colonne à laquelle vous l'avez liée, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] retourne un message d'erreur quand il essaie d'insérer la valeur par défaut et non au moment de sa liaison.  
  
 Les colonnes existantes du type de données alias héritent la nouvelle valeur par défaut, sauf si une valeur par défaut est liée directement à ou *futureonly_flag* est spécifié en tant que **futureonly**. Les nouvelles colonnes qui utilisent le type de données d'alias héritent toujours la valeur par défaut.  
  
 Lorsque vous liez une valeur par défaut à une colonne, l’information correspondante est ajoutée à la **sys.columns** affichage catalogue. Lorsque vous liez une valeur par défaut à un type de données alias, l’information correspondante est ajoutée à la **sys.types** affichage catalogue.  
  
## <a name="permissions"></a>Autorisations  
 Utilisateur propriétaire de la table ou être membre du **sysadmin** rôle de serveur fixe ou **db_owner** et **db_ddladmin** base de données fixe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-binding-a-default-to-a-column"></a>A. Liaison d'une valeur par défaut à une colonne  
 Une valeur par défaut nommée `today` a été définie dans la base de données actuelle par une instruction CREATE DEFAULT, l'exemple suivant lie cette valeur par défaut à la colonne `HireDate` de la table `Employee`. Si aucune valeur n'est fournie pour la colonne `Employee` lors de l'ajout d'une ligne dans la table `HireDate`, la colonne prend la valeur de `today` par défaut.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-default-to-an-alias-data-type"></a>B. Liaison d'une valeur par défaut à un type de données d'alias  
 Une valeur par défaut nommée `def_ssn` et un type de données d'alias nommé `ssn` existent déjà. L'exemple suivant lie la valeur par défaut `def_ssn` à `ssn`. Lors de la création d'une table, toutes les colonnes affectées au type de données d'alias `ssn` héritent la valeur par défaut. Les colonnes existantes de type **ssn** héritent également la valeur par défaut **def_ssn**, sauf si **futureonly** est spécifiée pour *futureonly_flag* valeur, ou si la colonne est liée directement à la valeur par défaut. Les valeurs par défaut liées aux colonnes ont toujours priorité sur celles liées à des types de données.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonlyflag"></a>C. À l’aide de l’argument futureonly_flag  
 L'exemple suivant lie la valeur par défaut `def_ssn` à un type de données d'alias `ssn`. Étant donné que **futureonly** est spécifié, aucune colonne existante de type `ssn` sont affectées.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. À l’aide d’identificateurs délimités  
 L’exemple suivant illustre l’utilisation des identificateurs délimités, `[t.1]`, dans *nom_objet*.  
  
```  
USE master;  
GO  
CREATE TABLE [t.1] (c1 int);   
-- Notice the period as part of the table name.  
EXEC sp_bindefault 'default1', '[t.1].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name,   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées du moteur de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
