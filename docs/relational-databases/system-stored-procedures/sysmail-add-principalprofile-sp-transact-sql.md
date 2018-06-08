---
title: sysmail_add_principalprofile_sp (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_add_principalprofile_sp_TSQL
- sysmail_add_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_principalprofile_sp
ms.assetid: b2a0b313-abb9-4c23-8511-db77ca8172b3
caps.latest.revision: 36
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7d5eda99649aa5f27cc199a2a4676c0f79d36c80
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailaddprincipalprofilesp-transact-sql"></a>sysmail_add_principalprofile_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Accorde l'autorisation d'utilisation d'un profil de messagerie de base de données à un utilisateur de base de données ou à un rôle de base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_add_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
    [ , [ @is_default ] = 'is_default' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@principal_id** =] *principal_id*  
 L’ID de l’utilisateur de base de données ou d’un rôle dans le **msdb** base de données pour l’association. *principal_id* est **int**, avec NULL comme valeur par défaut. Soit *principal_id* ou *principal_name* doit être spécifié. A *principal_id* de **0** rend ce profil est public, accorder l’accès à tous les principaux dans la base de données.  
  
 [ **@principal_name** =] **'***principal_name***'**  
 Le nom de l’utilisateur de base de données ou d’un rôle dans le **msdb** base de données pour l’association. *principal_name* est **sysname**, avec NULL comme valeur par défaut. Soit *principal_id* ou *principal_name* doit être spécifié. A *principal_name* de **'public'** rend ce profil est public, accorder l’accès à tous les principaux dans la base de données.  
  
 [ **@profile_id** =] *profile_id*  
 Identificateur du profil pour l'association. *profile_id* est **int**, avec NULL comme valeur par défaut. Soit *profile_id* ou *profile_name* doit être spécifié.  
  
 [ **@profile_name** =] **'***profile_name***'**  
 Nom du profil pour l'association. *profile_name* est **sysname**, sans valeur par défaut. Soit *profile_id* ou *profile_name* doit être spécifié.  
  
 [ **@is_default** =] *is_default*  
 Indique si ce profil représente le profil par défaut pour le principal. Un principal ne peut avoir qu'un seul profil par défaut. *is_default* est **bits**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Pour rendre un profil public, vous devez spécifier un **@principal_id** de **0** ou un **@principal_name** de **public**. Un profil public est disponible pour tous les utilisateurs dans le **msdb** de base de données, bien que les utilisateurs doivent être également membre du **DatabaseMailUserRole** à exécuter **sp_send_dbmail**.  
  
 Un utilisateur de base de données ne peut avoir plus d'un seul profil par défaut. Lorsque **@is_default** est '**1**' et l’utilisateur est déjà associé à un ou plusieurs profils, le profil spécifié devient le profil par défaut pour l’utilisateur. L'ancien profil par défaut est toujours associé à l'utilisateur, mais n'est plus le profil par défaut.  
  
 Lorsque **@is_default** est '**0**' et aucune autre association n’existe, la procédure stockée renvoie une erreur.  
  
 La procédure stockée **sysmail_add_principalprofile_sp** est dans le **msdb** de base de données et est détenue par le **dbo** schéma. La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 **A. Création d’une association, la définition du profil par défaut**  
  
 L’exemple suivant crée une association entre le profil nommé `AdventureWorks Administrator Profile` et **msdb** utilisateur de base de données `ApplicationUser`. Le profil est le profil par défaut de l'utilisateur.  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @is_default = 1 ;  
```  
  
 **B. Modification d’un profil le profil public par défaut**  
  
 L’exemple suivant montre le profil `AdventureWorks Public Profile` le profil public par défaut pour les utilisateurs dans le **msdb** base de données.  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'AdventureWorks Public Profile',  
    @is_default = 1 ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Objets de Configuration de messagerie de base de données](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
