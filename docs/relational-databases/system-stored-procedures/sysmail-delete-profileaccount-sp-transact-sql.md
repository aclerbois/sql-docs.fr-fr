---
title: sysmail_delete_profileaccount_sp (Transact-SQL) | Documents Microsoft
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
- sysmail_delete_profileaccount_sp
- sysmail_delete_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_profileaccount_sp
ms.assetid: b58d06f2-d6c9-4c8e-95bd-027c50f4621a
caps.latest.revision: 45
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: f90e939bf47154850c2183261af4cb541b19538d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmaildeleteprofileaccountsp-transact-sql"></a>sysmail_delete_profileaccount_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime un compte d'un profil de messagerie de base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_delete_profileaccount_sp  {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    {   [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@profile_id** =] *profile_id*  
 L’ID de profil du profil à supprimer. *profile_id* est **int**, avec NULL comme valeur par défaut. Soit le *profile_id* ou *profile_name* peut être spécifié.  
  
 [ **@profile_name** =] **'***profile_name***'**  
 Le nom du profil du profil à supprimer. *profile_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *profile_id* ou *profile_name* peut être spécifié.  
  
 [ **@account_id** =] *account_id*  
 ID du compte à supprimer. *account_id* est **int**, avec NULL comme valeur par défaut. Soit le *account_id* ou *account_name* peut être spécifié.  
  
 [ **@account_name** =] **'***account_name***'**  
 Nom du compte à supprimer. *account_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *account_id* ou *account_name* peut être spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Renvoie une erreur si le compte spécifié n'est pas associé au profil spécifié.  
  
 Lorsqu'un compte est spécifié mais pas un profil, cette procédure stockée supprime le compte spécifié de tous les profils. Par exemple, pour fermer un serveur SMTP existant, supprimez de tous les profils les comptes qui utilisent le serveur SMTP en question au lieu de supprimer chaque compte de chaque profil.  
  
 Lorsqu'un profil est spécifié mais pas un compte, cette procédure stockée supprime tous les comptes du profil spécifié. Par exemple, pour modifier les serveurs SMTP utilisés par un profil, il peut être pratique de supprimer tous les comptes du profil et d'ajouter ensuite les nouveaux comptes.  
  
 La procédure stockée **sysmail_delete_profileaccount_sp** est dans le **msdb** de base de données et est détenue par le **dbo** schéma. La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre la suppression du compte `Audit Account` du profil `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte de messagerie de base de données](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Objets de Configuration de messagerie de base de données](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
