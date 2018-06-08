---
title: sp_addlogin (Transact-SQL) | Documents Microsoft
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
- sp_addlogin
- sp_addlogin_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlogin
ms.assetid: 030f19c3-a5e3-4b53-bfc4-de4bfca0fddc
caps.latest.revision: 25
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 5f9458c1459aef35bcf9d6e47963025f87000c32
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddlogin-transact-sql"></a>sp_addlogin (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée une nouvelle connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui permet à un utilisateur de se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) à la place.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addlogin [ @loginame = ] 'login'   
    [ , [ @passwd = ] 'password' ]   
    [ , [ @defdb = ] 'database' ]   
    [ , [ @deflanguage = ] 'language' ]   
    [ , [ @sid = ] sid ]   
    [ , [ @encryptopt = ] 'encryption_option' ]   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @loginame=] '*connexion*'  
 Est le nom du compte de connexion. *connexion* est **sysname**, sans valeur par défaut.  
  
 [ @passwd=] '*mot de passe*'  
 Est le mot de passe de connexion. *mot de passe* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
 [ @defdb=] '*base de données*'  
 Base de données par défaut de la connexion (la base de données avec laquelle la connexion est établie en premier). *base de données* est **sysname**, avec une valeur par défaut **master**.  
  
 [ @deflanguage=] '*langage*'  
 Est la langue par défaut du compte de connexion. *langage* est **sysname**, avec NULL comme valeur par défaut. Si *langage* n’est pas spécifié, la valeur par défaut *langage* du nouveau compte de connexion est définie sur la langue par défaut actuelle du serveur.  
  
 [ @sid=] '*sid*'  
 Correspond au numéro d'identification de sécurité. *SID* est **varbinary (16)**, avec NULL comme valeur par défaut. Si *sid* est NULL, le système génère un SID pour le compte de connexion. Malgré l’utilisation d’un **varbinary** type de données, valeurs autres que NULL doit être de 16 octets exactement et ne doit pas déjà exister. Spécification de *sid* est utile, par exemple, script ou de déplacement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions à partir d’un serveur à un autre et que vous souhaitez qu’ont le même SID sur des serveurs différents.  
  
 [ @encryptopt=] '*encryption_option*'  
 Spécifie si le mot de passe est transmis en texte clair ou sous forme de hachage du mot de passe en texte clair. Notez qu'il n'y a aucun chiffrement. Le mot « chiffrement » est utilisé dans cette description à des fins de compatibilité ascendante. Si un mot de passe en texte clair est transmis, il est haché. Le hachage est stocké. *encryption_option* est **varchar (20)**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|NULL|Le mot de passe est transmis en clair. Il s'agit du paramètre par défaut.|  
|**skip_encryption**|Le mot de passe est déjà haché. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit stocker la valeur sans hacher à nouveau.|  
|**procédure skip_encryption_old**|Le mot de passe fourni a été haché par une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit stocker la valeur sans hacher à nouveau. Option fournie pour des mises à niveau uniquement.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Les noms de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent compter de 1 à 128 caractères, comprenant des lettres, des symboles et des chiffres. Connexions d’accès ne peut pas contenir une barre oblique inverse (\\) ; être un nom de connexion réservé, par exemple sa ou public, ou exister déjà ; ou être une chaîne NULL ou vide (`''`).  
  
 Si le nom d'une base de données par défaut vous est fourni, vous pouvez vous connecter à la base de données spécifiée sans devoir exécuter l'instruction USE. Toutefois, vous ne pouvez pas utiliser la base de données par défaut jusqu'à ce que vous obtiennent un accès à cette base de données par le propriétaire de la base de données (à l’aide de [sp_adduser](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md) ou [sp_addrolemember](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)) ou [sp_addrole](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md).  
  
 Le numéro SID est un identificateur global unique (GUID) qui identifiera de manière unique la connexion dans le serveur.  
  
 La modification de la langue par défaut du serveur n'entraîne pas celle de la langue par défaut des noms de connexion existants. Pour modifier la langue par défaut du serveur, utilisez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 À l’aide de **skip_encryption** pour supprimer le mot de passe, le hachage est utile si le mot de passe est déjà haché lorsque la connexion est ajoutée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le mot de passe a été haché par une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez **procédure skip_encryption_old**.  
  
 La procédure sp_droplogin ne peut pas être exécutée dans une transaction définie par l'utilisateur.  
  
 Le tableau suivant présente plusieurs procédures stockées qui sont utilisées avec sp_addlogin.  
  
|Procédure stockée| Description|  
|----------------------|-----------------|  
|[sp_grantlogin](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)|Ajoute un utilisateur ou un groupe Windows.|  
|[sp_password](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)|Modifie le mot de passe d'un utilisateur.|  
|[sp_defaultdb](../../relational-databases/system-stored-procedures/sp-defaultdb-transact-sql.md)|Modifie la base de données par défaut d'un utilisateur.|  
|[sp_defaultlanguage](../../relational-databases/system-stored-procedures/sp-defaultlanguage-transact-sql.md)|Modifie la langue par défaut d'un utilisateur.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY LOGIN.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-sql-server-login"></a>A. Création d'un nom de connexion SQL Server  
 L'exemple suivant crée une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'utilisateur `Victoria`, avec le mot de passe `B1r12-36`, sans spécifier de base de données par défaut.  
  
```  
EXEC sp_addlogin 'Victoria', 'B1r12-36';  
GO  
```  
  
### <a name="b-creating-a-sql-server-login-that-has-a-default-database"></a>B. Création d'une connexion SQL Server qui a une base de données par défaut  
 L'exemple suivant crée une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'utilisateur `Albert`, avec le mot de passe `B5432-3M6`, et une base de données par défaut `corporate`.  
  
```  
EXEC sp_addlogin 'Albert', 'B5432-3M6', 'corporate';  
GO  
```  
  
### <a name="c-creating-a-sql-server-login-that-has-a-different-default-language"></a>C. Création d'une connexion SQL Server qui a une autre langue par défaut  
 L'exemple suivant crée une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'utilisateur `TzTodorov`, avec le mot de passe `709hLKH7chjfwv`, la base de données par défaut `AdventureWorks2012` et la langue par défaut `Bulgarian`.  
  
```  
EXEC sp_addlogin 'TzTodorov', '709hLKH7chjfwv', 'AdventureWorks2012', N'български'  
```  
  
### <a name="d-creating-a-sql-server-login-that-has-a-specific-sid"></a>D. Création d'une connexion SQL Server qui a un SID spécifique  
 L'exemple suivant crée une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'utilisateur `Michael`, avec le mot de passe `B548bmM%f6`, la base de données par défaut `AdventureWorks2012`, la langue par défaut `us_english` et le SID `0x0123456789ABCDEF0123456789ABCDEF`.  
  
```  
EXEC sp_addlogin 'Michael', 'B548bmM%f6', 'AdventureWorks2012', 'us_english', 0x0123456789ABCDEF0123456789ABCDEF  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_droplogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [xp_logininfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-logininfo-transact-sql.md)  
  
  
