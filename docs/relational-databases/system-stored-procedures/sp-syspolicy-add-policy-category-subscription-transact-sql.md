---
title: sp_syspolicy_add_policy_category_subscription (Transact-SQL) | Documents Microsoft
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
- sp_syspolicy_add_policy_category_subscription
- sp_syspolicy_add_policy_category_subscription_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_add_policy_category_subscription
ms.assetid: 4284f550-9a3f-4726-8181-15e407fbf08f
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5c3e5f4079a75fca4112da1185a941b3a77e6b85
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsyspolicyaddpolicycategorysubscription-transact-sql"></a>sp_syspolicy_add_policy_category_subscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un abonnement aux catégories de stratégies à la base de données spécifiée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_add_policy_category_subscription [ @target_type = ] 'target_type'  
    , [ @target_object = ] 'target_object'  
    , [ @policy_category = ] 'policy_category'  
    [ , [ @policy_category_subscription_id = ] policy_category_subscription_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@target_type=** ] **'***target_type***'**  
 Type de cible de l'abonnement aux catégories. *target_type* est **sysname**, est requis et doit avoir la valeur 'DATABASE'.  
  
 [ **@target_object=** ] **'***target_object***'**  
 Est le nom de la base de données qui peuvent s’abonner à la catégorie. *target_object* est **sysname**et est requis.  
  
 [  **@policy_category=** ] **'***policy_category***'**  
 Est le nom de la catégorie de stratégie pour vous abonner à. *policy_category* est **sysname**et est requis.  
  
 Pour obtenir les valeurs de *policy_category*, interrogez la vue système msdb.dbo.syspolicy_policy_categories.  
  
 [  **@policy_category_subscription_id=** ] *policy_category_subscription_id*  
 Identificateur de l'abonnement aux catégories. *policy_category_subscription_id* est **int**et est retourné en tant que sortie.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_add_policy_category_subscription dans le contexte de la base de données système msdb.  
  
 Si vous spécifiez une catégorie de stratégie qui n'existe pas, une nouvelle catégorie de stratégie est créée et l'abonnement est autorisé pour toutes les bases de données lorsque vous exécutez la procédure stockée. Si vous supprimez l’abonnement autorisé pour la nouvelle catégorie, il ne s’appliquera qu’à la base de données que vous avez spécifiée en tant que *target_object*. Pour plus d’informations sur la modification d’un paramètre d’abonnement autorisé, consultez [sp_syspolicy_update_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Cette procédure stockée est exécutée dans le contexte du propriétaire actuel de la procédure stockée.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant configure la base de données spécifiée pour qu'elle s'abonne à une catégorie de stratégie nommée « Table Naming Policies ».  
  
```  
EXEC msdb.dbo.sp_syspolicy_add_policy_category_subscription @target_type = N'DATABASE'  
, @target_object = N'AdventureWorks2012'  
, @policy_category = N'Table Naming Policies';  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur la stratégie &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_update_policy_category_subscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-update-policy-category-subscription-transact-sql.md)   
 [sp_syspolicy_unsubscribe_from_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md)  
  
  
