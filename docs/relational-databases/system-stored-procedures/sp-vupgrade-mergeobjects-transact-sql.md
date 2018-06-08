---
title: sp_vupgrade_mergeobjects (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_vupgrade_mergeobjects
- sp_vupgrade_mergeobjects_TSQL
helpviewer_keywords:
- sp_vupgrade_mergeobjects
ms.assetid: 73257c2e-cc4c-48e7-9d66-7ef045bdd4f5
caps.latest.revision: 11
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 076eebfd551d24577c94f3a8092299cec8c9efd6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spvupgrademergeobjects-transact-sql"></a>sp_vupgrade_mergeobjects (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Régénère les affichages, les procédures stockées et les déclencheurs spécifiques aux articles qui sont utilisés pour suivre et appliquer les modifications de données pour la réplication de fusion. Exécutez cette procédure dans les cas de figure suivants :  
  
-   Si un objet requis par la réplication est accidentellement supprimé.  
  
-   Si vous appliquez une mise à jour, telle qu'un correctif, qui requiert une modification d'un ou de plusieurs des objets de réplication. Exécutez la procédure sur chaque nœud avant d'appliquer la mise à jour.  
  
 L'exécution de cette procédure stockée ne nécessite pas la réinitialisation des abonnements. Cette procédure n'est pas indispensable si vous installez un Service Pack ou une mise à niveau vers une nouvelle version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_vupgrade_mergeobjects [ [@login = ] 'login' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @security_mode = ] security_mode ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@login=**] **'***connexion***'**  
 Est la connexion d’administrateur système à utiliser lors de la création de nouveaux objets système dans la base de données de distribution. *login* est de type **sysname**, avec NULL comme valeur par défaut. Ce paramètre n’est pas nécessaire si *security_mode* a la valeur **1**, qui est l’authentification Windows.  
  
 [  **@password=**] **'***mot de passe***'**  
 Mot de passe de l'administrateur système à utiliser lors de la création d'objets système dans la base de données de distribution. *mot de passe* est **sysname**, avec une valeur par défaut **''** (chaîne vide). Ce paramètre n’est pas nécessaire si *security_mode* a la valeur **1**, qui est l’authentification Windows.  
  
 [  **@security_mode=**] **'***security_mode***'**  
 Est le mode de sécurité de connexion à utiliser lors de la création de nouveaux objets système dans la base de données de distribution. *security_mode* est **bits** avec la valeur par défaut **1**. Si **0**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification est utilisée. Si **1**, l’authentification Windows est utilisée. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_vupgrade_mergeobjects** est utilisé uniquement pour la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Mettre à niveau des bases de données répliquées](../../database-engine/install-windows/upgrade-replicated-databases.md)  
  
  
