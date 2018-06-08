---
title: CREATE ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE ROLE
- DATABASE ROLE
- ROLE_TSQL
- DATABASE_ROLE_TSQL
- CREATE_ROLE_TSQL
- CREATE DATABASE ROLE
- ROLE
- CREATE_DATABASE_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], creating
- CREATE DATABASE ROLE statement
- roles [SQL Server], creating
- CREATE ROLE statement
ms.assetid: b0cd54ad-e81d-4d71-acec-8a6d7261ca08
caps.latest.revision: 54
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 88babc3ef56449b2f3e7be43102a333635183bef
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-role-transact-sql"></a>CREATE ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Crée un rôle de base de données dans la base de données active.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE ROLE role_name [ AUTHORIZATION owner_name ]  
```  
  
## <a name="arguments"></a>Arguments  
 *role_name*  
 Nom du rôle à créer.  
  
 AUTHORIZATION *owner_name*  
 Utilisateur ou rôle de base de données qui doit posséder le nouveau rôle. Si aucun utilisateur n'est spécifié, le rôle appartient à l'utilisateur qui exécute CREATE ROLE.  
  
## <a name="remarks"></a>Notes   
 Les rôles sont des éléments sécurisables au niveau base de données. Lorsque vous avez créé un rôle, configurez les autorisations au niveau base de données à l'aide des instructions GRANT, DENY et REVOKE. Pour ajouter des membres à un rôle de base de données, utilisez [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md). Pour plus d’informations, consultez [Rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 Les rôles de base de données sont visibles dans les vues de catalogue sys.database_role_members et sys.database_principals.  
  
 Pour plus d’informations sur la conception d’un système d’autorisations, voir [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **CREATE ROLE** sur la base de données ou l’appartenance au rôle de base de données fixe **db_securityadmin**. Quand vous utilisez l’option **AUTHORIZATION**, les autorisations suivantes sont également exigées :  
  
-   Pour affecter la propriété d'un rôle à un autre utilisateur, l'autorisation IMPERSONATE est requise pour cet utilisateur.  
  
-   Pour affecter la propriété d'un rôle à un autre rôle, l'appartenance au rôle destinataire ou l'autorisation ALTER est requise pour ce rôle.  
  
-   Pour affecter la propriété d'un rôle à un rôle d'application, l'autorisation ALTER est requise pour ce rôle d'application.  
  
## <a name="examples"></a>Exemples  
Les exemples suivants utilisent tous la base de données AdventureWorks.   

### <a name="a-creating-a-database-role-that-is-owned-by-a-database-user"></a>A. Création d'un rôle de base de données possédé par un utilisateur de base de données  
 Le code exemple suivant crée le rôle de base de données `buyers` possédé par l'utilisateur `BenMiller`.  
  
```  
CREATE ROLE buyers AUTHORIZATION BenMiller;  
GO  
```  
  
### <a name="b-creating-a-database-role-that-is-owned-by-a-fixed-database-role"></a>B. Création d'un rôle de base de données possédé par un rôle de base de données fixe  
 Le code exemple suivant crée le rôle de base de données `auditors` possédé par le rôle de base de données fixe `db_securityadmin`.  
  
```  
CREATE ROLE auditors AUTHORIZATION db_securityadmin;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  


