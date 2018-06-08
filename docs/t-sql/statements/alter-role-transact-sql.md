---
title: ALTER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER_ROLE_TSQL
- ALTER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database roles
- ALTER ROLE statement
- renaming database roles
- database roles [SQL Server], modifying
- names [SQL Server], database roles
ms.assetid: e1e83caa-17cc-4871-b2db-2711339fb64f
caps.latest.revision: 64
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4a3579566f88b2ba95286d5e153c169cd8527e05
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-role-transact-sql"></a>ALTER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

  Ajoute ou supprime des membres dans un rôle de base de données, ou change le nom d’un rôle de base de données défini par l’utilisateur.  
  
> [!NOTE]  
>  Pour modifier les rôles en ajoutant ou en supprimant des membres dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) et [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server (starting with 2012) and Azure SQL Database  
  
ALTER ROLE  role_name  
{  
       ADD MEMBER database_principal  
    |  DROP MEMBER database_principal  
    |  WITH NAME = new_name  
}  
[;]  
```  
  
 
```  
-- Syntax for SQL Server 2008, Azure SQL Data Warehouse and Parallel Data Warehouse
  
-- Change the name of a user-defined database role  
ALTER ROLE role_name   
    WITH NAME = new_name  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *role_name*  
 **S’APPLIQUE À :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis la version 2008), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Spécifie le rôle de base de données à changer.  
  
 ADD MEMBER *database_principal*l  
 **S’APPLIQUE À :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis la version 2012), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Indique que le principal de base de données doit être ajouté à l’appartenance à un rôle de base de données.  
  
-   *database_principal* est un utilisateur de base de données ou un rôle de base de données défini par l’utilisateur.  
  
-   *database_principal* ne peut être ni un rôle de base de données fixe, ni un principal de serveur.  
  
DROP MEMBER *database_principal*  
 **S’APPLIQUE À :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis la version 2012), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Indique qu’un principal de base de données doit être supprimé de l’appartenance à un rôle de base de données.  
  
-   *database_principal* est un utilisateur de base de données ou un rôle de base de données défini par l’utilisateur.  
  
-   *database_principal* ne peut être ni un rôle de base de données fixe, ni un principal de serveur.  
  
WITH NAME = *new_name*  
 **S’APPLIQUE À :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis la version 2008), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Indique que le nom d’un rôle serveur défini par l’utilisateur doit être changé. Le nouveau nom ne doit pas déjà exister dans la base de données.  
  
 La modification du nom d'un rôle de base de données ne modifie pas le numéro d'identification, le propriétaire ou les autorisations du rôle.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette commande, vous devez disposer d’une ou de plusieurs des autorisations ou appartenances suivantes :  
  
-   Autorisation **ALTER** sur le rôle  
-   Autorisation **ALTER ANY ROLE** sur la base de données  
-   Appartenance au rôle de base de données fixe **db_securityadmin**  
  
De plus, pour changer l’appartenance à un rôle de base de données fixe, vous devez disposer de ceci :  
  
-   Appartenance au rôle de base de données fixe **db_owner**  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Vous ne pouvez pas changer le nom d’un rôle de base de données fixe.  
  
## <a name="metadata"></a>Métadonnées  
 Les vues système suivantes contiennent des informations sur les rôles de base de données et les principaux de base de données.  
  
-   [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)  
  
-   [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-change-the-name-of-a-database-role"></a>A. Changer le nom d’un rôle de base de données  
 **S’APPLIQUE À :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis la version 2008), [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
 L'exemple suivant remplace le nom du rôle `buyers` par `purchasing`. [!INCLUDE[AdWorks-example](../../includes/adworks-example-md.md)]  
  
```sql  
ALTER ROLE buyers WITH NAME = purchasing;  
```  
  
### <a name="b-add-or-remove-role-members"></a>B. Ajouter ou supprimer des membres d’un rôle  
 **S’APPLIQUE À :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (depuis la version 2012), [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
 Cet exemple crée un rôle de base de données nommé `Sales`. Il ajoute un utilisateur de base de données nommé Barry à l’appartenance, puis montre comment supprimer le membre Barry. [!INCLUDE[AdWorks-example](../../includes/adworks-example-md.md)]  
  
```sql  
CREATE ROLE Sales;  
ALTER ROLE Sales ADD MEMBER Barry;  
ALTER ROLE Sales DROP MEMBER Barry;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
