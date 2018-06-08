---
title: sysmail_update_profileaccount_sp (Transact-SQL) | Documents Microsoft
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
- sysmail_update_profileaccount_sp_TSQL
- sysmail_update_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_profileaccount_sp
ms.assetid: 92ca7488-29db-414e-8e36-08b0a8f542bb
caps.latest.revision: 41
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ee2f44070644e305163e6a7ae38eea9a81ba9fa1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailupdateprofileaccountsp-transact-sql"></a>sysmail_update_profileaccount_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Met à jour le numéro de séquence d'un compte dans un profil de messagerie de base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_update_profileaccount_sp  { [ @profile_id = ] profile_id   
| [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' } ,  
    [ @sequence_number = ] sequence_number  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@profile_id** =] *profile_id*  
 Identificateur du profil à mettre à jour. *profile_id* est **int**, avec NULL comme valeur par défaut. Soit le *profile_id* ou *profile_name* doit être spécifié.  
  
 [ **@profile_name** =] **'***profile_name***'**  
 Nom du profil à mettre à jour. *profile_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *profile_id* ou *profile_name* doit être spécifié.  
  
 [ **@account_id** =] *account_id*  
 ID du compte à mettre à jour. *account_id* est **int**, avec NULL comme valeur par défaut. Soit le *account_id* ou *account_name* doit être spécifié.  
  
 [ **@account_name** =] **'***account_name***'**  
 Nom du compte à mettre à jour. *account_name* est **sysname**, avec NULL comme valeur par défaut. Soit le *account_id* ou *account_name* doit être spécifié.  
  
 [ **@sequence_number** =] *numéros de séquence*  
 Nouveau numéro de séquence du compte. *numéros de séquence* est **int**, sans valeur par défaut. Le numéro de séquence détermine l'ordre dans lequel les comptes sont utilisés dans le profil.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Renvoie une erreur si le compte spécifié n'est pas associé au profil spécifié.  
  
 Le numéro de séquence détermine l'ordre d'utilisation des comptes de messagerie de base de données dans le profil. Pour un nouveau message électronique, la messagerie de base de données démarre avec le compte dont le numéro de séquence est le plus petit. Si ce compte échoue, la messagerie de base de données utilise le compte qui possède le numéro de séquence plus élevé suivant, et ainsi de suite jusqu'à ce qu'elle envoie le message correctement ou que le compte qui possède le numéro de séquence le plus élevé échoue. En cas d'échec du compte avec le numéro de séquence le plus élevé, le message échoue.  
  
 Si plusieurs comptes possèdent le même numéro de séquence, la messagerie de base de données utilise uniquement l'un d'eux pour un message électronique donné. Dans ce cas, la messagerie de base de données exclut toute garantie en ce qui concerne le compte utilisé pour ce numéro de séquence ou l'utilisation du même compte d'un message à un autre.  
  
 La procédure stockée **sysmail_update_profileaccount_sp** est dans le **msdb** de base de données et est détenue par le **dbo** schéma. La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Autorisations d’exécution de cette procédure reviennent par défaut aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie le numéro de séquence du compte `Admin-BackupServer` au sein du profil `AdventureWorks Administrator` dans les **msdb** base de données. Une fois ce code exécuté, le numéro de séquence du compte est `3`. Cela indique qu'il sera utilisé si les deux premiers comptes échouent.  
  
```  
EXECUTE msdb.dbo.sysmail_update_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator'  
    ,@account_name = 'Admin-BackupServer',  
    ,@sequence_number = 3;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte de messagerie de base de données](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Objets de Configuration de messagerie de base de données](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
