---
title: sp_addrolemember (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_addrolemember_TSQL
- sp_addrolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrolemember
ms.assetid: a583c087-bdb3-46d2-b9e5-3921b3e6d10b
caps.latest.revision: 59
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 147547c7392acaf528b7aef98c88affb8487fe99
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddrolemember-transact-sql"></a>sp_addrolemember (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Ajoute un utilisateur ou un rôle de base de données, un compte de connexion ou un groupe Windows à un rôle de base de données dans la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
sp_addrolemember [ @rolename = ] 'role',  
    [ @membername = ] 'security_account'  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
sp_addrolemember 'role', 'security_account'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @rolename=] '*rôle*'  
 Nom du rôle de base de données dans la base de données actuelle. *rôle* est un **sysname**, sans valeur par défaut.  
  
 [ @membername=] '*celui-ci*'  
 Compte de sécurité ajouté au rôle. *celui-ci* est un **sysname**, sans valeur par défaut. *celui-ci* peut être un utilisateur de base de données, un rôle de base de données, une connexion Windows ou un groupe Windows.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Un membre ajouté à un rôle à l'aide de sp_addrolemember hérite des autorisations de ce rôle. Si le nouveau membre est un principal au niveau Windows sans utilisateur de base de données correspondant, un utilisateur de base de données est créé, mais peut ne pas être entièrement mappé à la connexion. Vérifiez toujours que la connexion existe et a accès à la base de données.  
  
 Un rôle ne peut pas s'inclure lui-même en tant que membre. Des définitions « circulaires » de ce type ne sont pas valides, même si l'appartenance est seulement déduite indirectement par une ou plusieurs appartenances intermédiaires.  
  
 sp_addrolemember ne peut pas ajouter un rôle de base de données fixe, rôle serveur fixe ou dbo à un rôle. sp_addrolemember ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
 Utilisez uniquement sp_addrolemember pour ajouter un membre à un rôle de base de données. Pour ajouter un membre à un rôle de serveur, utilisez [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 L'ajout de membres à des rôles de base de données flexibles nécessite l'un des éléments suivants :  
  
-   Appartenance au rôle de base de données fixe db_securityadmin ou db_owner.  
  
-   Appartenance au rôle qui détient le rôle.  
  
-   **ALTER ANY ROLE** autorisation ou **ALTER** autorisation sur le rôle.  
  
 L'ajout de membres à des rôles de base de données fixe nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-windows-login"></a>A. Ajout d'une connexion Windows  
 L’exemple suivant ajoute la connexion Windows `Contoso\Mary5` à la `AdventureWorks2012` base de données en tant qu’utilisateur `Mary5`. L'utilisateur `Mary5` est ensuite ajouté au rôle `Production`.  
  
> [!NOTE]  
>  Comme `Contoso\Mary5` est connu comme l'utilisateur de base de données `Mary5` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], le nom d'utilisateur `Mary5` doit être spécifié. L'instruction échoue à moins qu'un nom de connexion `Contoso\Mary5` n'existe. Effectuez un test en utilisant une connexion à partir de votre domaine.  
  
```  
USE AdventureWorks2012;  
GO  
CREATE USER Mary5 FOR LOGIN [Contoso\Mary5] ;  
GO  
```  
  
### <a name="b-adding-a-database-user"></a>B. Ajout d'un utilisateur de base de données  
 L'exemple suivant ajoute l'utilisateur de base de données `Mary5` au rôle de base de données `Production` dans la base de données actuelle.  
  
```  
EXEC sp_addrolemember 'Production', 'Mary5';  
```  
  
## <a name="examples-includesssdwincludessssdw-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-adding-a-windows-login"></a>C. Ajout d'une connexion Windows  
 L’exemple suivant ajoute la connexion `LoginMary` à la `AdventureWorks2008R2` base de données en tant qu’utilisateur `UserMary`. L'utilisateur `UserMary` est ensuite ajouté au rôle `Production`.  
  
> [!NOTE]  
>  Étant donné que la connexion `LoginMary` est connu comme l’utilisateur de base de données `UserMary` dans les [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] de base de données, le nom d’utilisateur `UserMary` doit être spécifié. L'instruction échoue à moins qu'un nom de connexion `Mary5` n'existe. Connexions et les utilisateurs ont généralement le même nom. Cet exemple utilise des noms différents pour distinguer les actions qui affectent la connexion et l’utilisateur.  
  
```  
-- Uses AdventureWorks  
  
CREATE USER UserMary FOR LOGIN LoginMary ;  
GO  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
### <a name="d-adding-a-database-user"></a>D. Ajout d'un utilisateur de base de données  
 L'exemple suivant ajoute l'utilisateur de base de données `UserMary` au rôle de base de données `Production` dans la base de données actuelle.  
  
```  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_droprolemember & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md)  
  
  
