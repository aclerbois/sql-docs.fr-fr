---
title: sysmail_add_profileaccount_sp (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_add_profileaccount_sp
- sysmail_add_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_profileaccount_sp
ms.assetid: 7cbf430f-1997-45ea-9707-0086184de744
caps.latest.revision: 42
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 776cabb53664a82bbd767301af0c62f4d86887dc
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailaddprofileaccountsp-transact-sql"></a>sysmail_add_profileaccount_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un compte de messagerie de base de données à un profil de messagerie de base de données. Exécutez **sysmail_add_profileaccount_sp** après la création d’un compte de base de données par [sysmail_add_account_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-account-sp-transact-sql.md), et un profil de base de données est créé avec [sysmail_add_profile_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_add_profileaccount_sp { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
    [ , [ @sequence_number = ] sequence_number ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@profile_id** =] *profile_id*  
 Identificateur du profil auquel le compte est ajouté. *profile_id* est **int**, avec NULL comme valeur par défaut. Soit le *profile_id* ou *profile_name* doit être spécifié.  
  
 [ **@profile_name** =] **'***profile_name***'**  
 Nom du profil auquel le compte est ajouté. *profile_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *profile_id* ou *profile_name* doit être spécifié.  
  
 [ **@account_id** =] *account_id*  
 Identificateur du compte à ajouter au profil. *account_id* est **int**, avec NULL comme valeur par défaut. Soit le *account_id* ou *account_name* doit être spécifié.  
  
 [ **@account_name** =] **'***account_name***'**  
 Nom du compte à ajouter au profil. *account_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *account_id* ou *account_name* doit être spécifié.  
  
 [ **@sequence_number** =] *numéros de séquence*  
 Numéro de séquence du compte dans le profil. *numéros de séquence* est **int**, sans valeur par défaut. Le numéro de séquence détermine l'ordre dans lequel les comptes sont utilisés dans le profil.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Le profil et le compte doivent déjà exister. Sinon, la procédure stockée retourne une erreur.  
  
 Notez que cette procédure stockée ne modifie pas le numéro de séquence d'un compte qui est déjà associé au profil spécifié. Pour plus d’informations sur la mise à jour le numéro de séquence d’un compte, consultez [sysmail_update_profileaccount_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-update-profileaccount-sp-transact-sql.md).  
  
 Le numéro de séquence détermine l'ordre d'utilisation des comptes de messagerie de base de données dans le profil. Pour un nouveau message électronique, la messagerie de base de données démarre avec le compte dont le numéro de séquence est le plus petit. Si ce compte échoue, la messagerie de base de données utilise le compte qui possède le numéro de séquence plus élevé suivant, et ainsi de suite jusqu'à ce qu'elle envoie le message correctement ou que le compte qui possède le numéro de séquence le plus élevé échoue. Si le compte qui a le numéro de séquence le plus élevé échoue, la messagerie de base de données interrompt les tentatives d’envoi du courrier électronique pour la durée configurée dans le paramètre *AccountRetryDelay* de **sysmail_configure_sp**, puis relance le processus de tentative d’envoi du courrier électronique en commençant à partir du numéro de séquence le moins élevé. Utilisez le paramètre *AccountRetryAttempts* de **sysmail_configure_sp**pour configurer le nombre de fois que le processus de messagerie externe tente d’envoyer le message électronique à l’aide de chaque compte du profil spécifié.  
  
 Si plusieurs comptes ont le même numéro de séquence, la messagerie de base de données utilisera uniquement l'un de ces comptes pour un message électronique donné. Dans ce cas, la messagerie de base de données exclut toute garantie en ce qui concerne le compte utilisé pour ce numéro de séquence ou l'utilisation du même compte d'un message à un autre.  
  
 La procédure stockée **sysmail_add_profileaccount_sp** est dans le **msdb** de base de données et est détenue par le **dbo** schéma. La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant associe le profil `AdventureWorks Administrator` au compte `Audit Account`. Le numéro de séquence de ce compte est 1.  
  
```  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account',  
    @sequence_number = 1 ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte de messagerie de base de données](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Objets de Configuration de messagerie de base de données](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
