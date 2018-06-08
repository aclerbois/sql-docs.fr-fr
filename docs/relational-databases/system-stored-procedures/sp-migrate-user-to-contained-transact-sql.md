---
title: sp_migrate_user_to_contained (Transact-SQL) | Documents Microsoft
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
- sp_migrate_user_to_contained
- sp_migrate_user_to_contained_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_migrate_user_to_contained
ms.assetid: b3a49ff6-46ad-4ee7-b6fe-7e54213dc33e
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 606898bf92ff727cd3d48f49f7f352cf06e8f9fe
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spmigrateusertocontained-transact-sql"></a>sp_migrate_user_to_contained (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Convertit un utilisateur de la base de données mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en utilisateur de base de données à relation contenant-contenu avec mot de passe. Dans une base de données à relation contenant-contenu, utilisez cette procédure pour supprimer les dépendances sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où la base de données est installée. **sp_migrate_user_to_contained** sépare l’utilisateur d’origine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion, afin que les paramètres tels que la langue par défaut et le mot de passe puissent être administrés séparément pour cette base de données. **sp_migrate_user_to_contained** peut être utilisé avant de déplacer la base de données à une autre instance de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] d’éliminer les dépendances sur actuel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions d’instance.  
  
 **Remarque** cette procédure est utilisée uniquement dans une base de données. Pour plus d’informations, consultez [Contained Databases](../../relational-databases/databases/contained-databases.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_migrate_user_to_contained [ @username = ] N'user' ,   
    [ @rename = ] { N'copy_login_name' | N'keep_name' } ,   
    [ @disablelogin = ] { N'disable_login' | N'do_not_disable_login' }   
```  
  
## <a name="arguments"></a>Arguments  
 [ **@username =** ] **N'***utilisateur***'**  
 Nom d'un utilisateur dans la base de données à relation contenant-contenu actuelle mappée à un compte de connexion authentifié [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur est **sysname**, avec une valeur par défaut **NULL**.  
  
 [ **@rename =** ] **N'***copy_login_name***'** | **N'***keep_name***'**  
 Lorsqu’un utilisateur de base de données basé sur une connexion a un autre nom d’utilisateur que le nom de connexion, utilisez *keep_name* pour conserver le nom d’utilisateur de base de données pendant la migration. Utilisez *copy_login_name* pour créer le nouvel utilisateur de base de données avec le nom de la connexion, au lieu de l’utilisateur. Lorsqu'un utilisateur de la base de données basé sur un compte de connexion a le même nom d'utilisateur que le nom de connexion, les deux options créent l'utilisateur de base de données à relation contenant-contenu sans modifier le nom.  
  
 [ **@disablelogin =** ] **N'***disable_login***'** | **N'***do_not_disable_login***'**  
 *disable_login* désactive la connexion dans la base de données master. Pour vous connecter lors de la connexion est désactivée, la connexion doit fournir le nom de la base de données que le **catalogue initial** dans le cadre de la chaîne de connexion.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_migrate_user_to_contained** crée l’utilisateur de base de données de relation contenant-contenu avec mot de passe, quel que soit les propriétés ou les autorisations du compte de connexion. Par exemple, la procédure peut réussir si la connexion est désactivée ou si l’utilisateur se voit refuser le **CONNECT** autorisé à la base de données.  
  
 **sp_migrate_user_to_contained** présente les restrictions suivantes.  
  
-   Le nom d'utilisateur ne doit pas déjà exister dans la base de données.  
  
-   Les utilisateurs intégrés, par exemple dbo et guest, ne peuvent pas être convertis.  
  
-   L’utilisateur ne peut pas être spécifié dans le **EXECUTE AS** clause d’une procédure stockée signée.  
  
-   L’utilisateur ne peut pas posséder une procédure stockée qui inclut le **EXECUTE AS OWNER** clause.  
  
-   **sp_migrate_user_to_contained** ne peut pas être utilisé dans une base de données système.  
  
## <a name="security"></a>Sécurité  
 Lorsque vous migrez des utilisateurs, veillez à ne pas désactiver ou supprimer tous les comptes de connexion de l'administrateur dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si toutes les connexions sont supprimées, consultez [se connecter à SQL Server lorsque système les administrateurs ont plus accès](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md).  
  
 Si le **BUILTIN\Administrateurs** connexion n’est présente, les administrateurs peuvent se connecter en démarrant leur application à l’aide de la **exécuter en tant qu’administrateur** option.  
  
### <a name="permissions"></a>Autorisations  
 Requiert l’autorisation **CONTROL SERVER** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-migrating-a-single-user"></a>A. Migration d'un seul utilisateur  
 L'exemple suivant migre un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommé `Barry`, vers un utilisateur de base de données à relation contenant-contenu avec mot de passe. L'exemple ne modifie pas le nom d'utilisateur et conserve le compte de connexion actif.  
  
```sql  
sp_migrate_user_to_contained   
@username = N'Barry',  
@rename = N'keep_name',  
@disablelogin = N'do_not_disable_login' ;  
  
```  
  
### <a name="b-migrating-all-database-users-with-logins-to-contained-database-users-without-logins"></a>B. Migration de tous les utilisateurs de la base de données avec des comptes de connexion vers des utilisateurs de base de données à relation contenant-contenu sans comptes de connexion  
 L'exemple suivant migre tous les utilisateurs basés sur des comptes de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers des utilisateurs de base de données à relation contenant-contenu avec mots de passe. L'exemple exclut les comptes de connexion qui ne sont pas activés. L'exemple doit être exécuté dans la base de données à relation contenant-contenu.  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Migrate to a Partially Contained Database](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)  
  
  
