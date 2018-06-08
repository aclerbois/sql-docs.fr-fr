---
title: sp_adduser (Transact-SQL) | Documents Microsoft
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
- sp_adduser
- sp_adduser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adduser
ms.assetid: 61a40eb4-573f-460c-9164-bd1bbfaf8b25
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d4f7afe6646fd22ff24aa6aee4e5dcde416420e9
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spadduser-transact-sql"></a>sp_adduser (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un nouvel utilisateur dans la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [CREATE USER](../../t-sql/statements/create-user-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adduser [ @loginame = ] 'login'   
    [ , [ @name_in_db = ] 'user' ]   
    [ , [ @grpname = ] 'role' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@loginame =** ] **'***connexion***'**  
 Nom de la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Windows. *connexion* est un **sysname**, sans valeur par défaut. *connexion* doit être un existant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion ou connexion Windows.  
  
 [  **@name_in_db =** ] **'***utilisateur***'**  
 Nom du nouvel utilisateur de base de données. *utilisateur* est un **sysname**, avec NULL comme valeur par défaut. Si *utilisateur* n’est pas spécifié, le nom du nouvel utilisateur de base de données par défaut est la *connexion* nom. Spécification de *utilisateur* donne au nouvel utilisateur un nom dans la base de données est différent du nom de connexion au niveau du serveur.  
  
 [  **@grpname =** ] **'***rôle***'**  
 Rôle de base de données dont le nouvel utilisateur devient membre. *rôle* est **sysname**, avec NULL comme valeur par défaut. *rôle* doit être un rôle de base de données valide dans la base de données actuelle.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adduser** créera également un schéma qui porte le nom de l’utilisateur.  
  
 Une fois qu'un utilisateur a été ajouté, utilisez les instructions GRANT, DENY et REVOKE afin de définir les autorisations contrôlant les activités effectuées par l'utilisateur.  
  
 Utilisez **sys.server_principals** pour afficher une liste de noms de connexion valide.  
  
 Utilisez **sp_helprole** pour afficher une liste de noms de rôle valides. Lorsque vous spécifiez un rôle, l'utilisateur obtient automatiquement les autorisations définies pour ce rôle. Si un rôle n’est pas spécifié, l’utilisateur bénéficie des autorisations accordées à la valeur par défaut **public** rôle. Pour ajouter un utilisateur à un rôle, une valeur pour le *nom d’utilisateur* doit être fourni. (*nom d’utilisateur* peut être le même que *login_id*.)  
  
 Utilisateur **invité** existe déjà dans chaque base de données. Ajout d’un utilisateur **invité** activera l’utilisateur, si elle a été précédemment désactivée. Par défaut, utilisateur **invité** est désactivée dans les nouvelles bases de données.  
  
 **sp_adduser** ne peut pas être exécutée à l’intérieur d’une transaction définie par l’utilisateur.  
  
 Vous ne pouvez pas ajouter un **invité** utilisateur car un **invité** utilisateur existe déjà à l’intérieur de chaque base de données. Pour activer la **invité** utilisateur, accordez **invité** autorisation CONNECT comme indiqué :  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
## <a name="permissions"></a>Autorisations  
 Il faut être propriétaire de la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-database-user"></a>A. Ajout d'un utilisateur de base de données  
 L'exemple suivant ajoute l'utilisateur de base de données `Vidur` au rôle `Recruiting` existant dans la base de données active, en utilisant la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `Vidur` existante.  
  
```  
EXEC sp_adduser 'Vidur', 'Vidur', 'Recruiting';  
```  
  
### <a name="b-adding-a-database-user-with-the-same-login-id"></a>B. Ajout d'un utilisateur de base de données avec le même ID de connexion  
 L'exemple suivant ajoute l'utilisateur `Arvind` à la base de données active pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `Arvind`. Cet utilisateur appartient à la valeur par défaut **public** rôle.  
  
```  
EXEC sp_adduser 'Arvind';  
```  
  
### <a name="c-adding-a-database-user-with-a-different-name-than-its-server-level-login"></a>C. Ajout d'un utilisateur de base de données avec un nom différent de sa connexion de niveau serveur  
 L'exemple suivant ajoute la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `BjornR` à la base de données active, qui a un nom d'utilisateur `Bjorn`, et ajoute l'utilisateur `Bjorn` dans le rôle de base de données `Production`.  
  
```  
EXEC sp_adduser 'BjornR', 'Bjorn', 'Production';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
