---
title: sp_change_users_login (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 12/13/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_change_users_login
- sp_change_users_login_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_users_login
ms.assetid: 1554b39f-274b-4ef8-898e-9e246b474333
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f4a7b50619ecb4196b5c88ac0b237a5cff8ecb73
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spchangeuserslogin-transact-sql"></a>sp_change_users_login (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Mappe un utilisateur de base de données existant à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) à la place.  
  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_change_users_login [ @Action = ] 'action'   
    [ , [ @UserNamePattern = ] 'user' ]   
    [ , [ @LoginName = ] 'login' ]   
    [ , [ @Password = ] 'password' ]  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @Action=] '*action*'  
 Décrit l'action à effectuer par la procédure. *action* est **varchar (10)**. *action* peut avoir l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**Auto_Fix**|Lie une entrée d'utilisateur de l'affichage catalogue système sys.database_principals de la base de données active à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du même nom. Le compte de connexion de même nom est créé s'il n'existe pas déjà. Examinez les résultats de la **Auto_Fix** instruction pour confirmer que la liaison correcte est effectuée. Évitez d’utiliser **Auto_Fix** dans les situations sensibles à la sécurité.<br /><br /> Lorsque vous utilisez **Auto_Fix**, vous devez spécifier *utilisateur* et *mot de passe* si la connexion n’existe pas déjà, sinon vous devez spécifier *utilisateur* mais *mot de passe* sera ignoré. *connexion* doit être NULL. *utilisateur* doit être un utilisateur valide dans la base de données actuelle. Le compte de connexion ne doit être associé à aucun autre utilisateur.|  
|**Rapport**|Dresse la liste des utilisateurs qui ne sont pas liés à un compte de connexion dans la base de données active et indique les identificateurs de sécurité (SID) correspondants. *utilisateur*, *connexion*, et *mot de passe* doit être NULL ou non spécifié.<br /><br /> Pour remplacer l’option de rapport avec une requête utilisant les tables système, comparez les entrées de **sys.server_prinicpals** avec les entrées de **sys.database_principals**.|  
|**Update_One**|Liens spécifié *utilisateur* dans la base de données actuelle à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *connexion*. *utilisateur* et *connexion* doit être spécifié. *mot de passe* doit être NULL ou non spécifié.|  
  
 [ @UserNamePattern=] '*utilisateur*'  
 Nom d'un utilisateur dans la base de données active. *utilisateur* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ @LoginName=] '*connexion*'  
 Est le nom d'un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* est de type **sysname**, avec NULL comme valeur par défaut.  
  
 [ @Password=] '*mot de passe*'  
 Mot de passe affecté au nouveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion qui est créée en spécifiant **Auto_Fix**. Si une connexion correspondante existe déjà, l’utilisateur et le compte de connexion sont mappés et *mot de passe* est ignoré. Si une connexion correspondante n’existe pas, sp_change_users_login crée un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion et lui affecte *mot de passe* comme mot de passe pour le compte de connexion. *mot de passe* est **sysname**, et ne doit pas être NULL.  
  
> **IMPORTANT** Utilisez toujours un [mot de passe fort !](../../relational-databases/security/strong-passwords.md)
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|UserName|**sysname**|Nom de l'utilisateur de la base de données.|  
|UserSID|**varbinary(85)**|Identificateur de sécurité de l'utilisateur.|  
  
## <a name="remarks"></a>Notes  
 Utilisez sp_change_users_login pour lier un utilisateur de la base de données active à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le compte de connexion d'un utilisateur a changé, utilisez sp_change_users_login pour lier l'utilisateur au nouveau compte de connexion sans perdre les autorisations de l'utilisateur. La nouvelle *connexion* ne peut pas être sa et *utilisateur*ne peut pas être dbo, guest ou un utilisateur INFORMATION_SCHEMA.  
  
 La procédure sp_change_users_login ne peut pas être utilisée pour mapper des utilisateurs de base de données sur des principaux, des certificats ou des clés asymétriques de niveau Windows.  
  
 sp_change_users_login ne peut pas être utilisé avec une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] créée à partir d'un principal Windows, ni avec un utilisateur créé à l'aide de CREATE USER WITHOUT LOGIN.  
  
 La procédure sp_change_users_login ne peut pas être exécutée dans une transaction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner. Seuls les membres du rôle serveur fixe sysadmin peuvent spécifier le **Auto_Fix** option.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-showing-a-report-of-the-current-user-to-login-mappings"></a>A. Affichage d'un rapport des mappages utilisateur-connexion en cours  
 Cet exemple produit un rapport des utilisateurs définis dans la base de données active et de leurs identificateurs de sécurité (SID).  
  
```  
EXEC sp_change_users_login 'Report';  
```  
  
### <a name="b-mapping-a-database-user-to-a-new-sql-server-login"></a>B. Mappage d'un utilisateur de la base de données à une nouvelle connexion SQL Server  
 Dans l'exemple suivant, un utilisateur de la base de données est associé à un nouveau compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'utilisateur `MB-Sales`, associé au départ à un autre compte de connexion, est mappé sur le nouveau compte `MaryB`.  
  
```  
--Create the new login.  
CREATE LOGIN MaryB WITH PASSWORD = '982734snfdHHkjj3';  
GO  
--Map database user MB-Sales to login MaryB.  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Update_One', 'MB-Sales', 'MaryB';  
GO  
```  
  
### <a name="c-automatically-mapping-a-user-to-a-login-creating-a-new-login-if-it-is-required"></a>C. Mappage automatique d'un utilisateur à une connexion en créant une nouvelle connexion si nécessaire  
 L’exemple suivant montre comment utiliser `Auto_Fix` pour mapper un utilisateur existant à une connexion du même nom, ou pour créer le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion `Mary` dont le mot de passe `B3r12-3x$098f6` si la connexion `Mary` n’existe pas.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_change_users_login 'Auto_Fix', 'Mary', NULL, 'B3r12-3x$098f6';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_helplogins &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
