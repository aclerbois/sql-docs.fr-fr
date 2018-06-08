---
title: sp_syspolicy_delete_policy_execution_history (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_syspolicy_delete_policy_execution_history
- sp_syspolicy_delete_policy_execution_history_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_delete_policy_execution_history
ms.assetid: fe651af9-267e-45ec-b4e7-4b0698fb1be3
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: d4e7f496124727389993c1e249b80aeaa7414b5f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsyspolicydeletepolicyexecutionhistory-transact-sql"></a>sp_syspolicy_delete_policy_execution_history (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Supprime l'historique d'exécution pour les stratégies dans la Gestion basée sur des stratégies. Vous pouvez utiliser cette procédure stockée pour supprimer l'historique d'exécution pour une stratégie particulière ou pour toutes les stratégies, et pour supprimer l'historique d'exécution avant une date spécifique.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_delete_policy_execution_history [ @policy_id = ] policy_id ]  
    [ , [ @oldest_date = ] 'oldest_date' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@policy_id=** ] *policy_id*  
 Identificateur de la stratégie pour laquelle vous voulez supprimer l'historique d'exécution. *policy_id* est **int**et est requis. Sa valeur peut être NULL.  
  
 [  **@oldest_date=** ] **'***oldest_date***'**  
 Date la plus ancienne pour laquelle vous voulez conserver l'historique d'exécution de la stratégie. Tout historique d'exécution antérieur à cette date est supprimé. *l’argument oldest_date* est **datetime**et est requis. Sa valeur peut être NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_delete_policy_execution_history dans le contexte de la base de données système msdb.  
  
 Pour obtenir les valeurs de *policy_id*, et pour afficher les dates de l’historique d’exécution, vous pouvez utiliser la requête suivante :  
  
```  
SELECT a.name AS N'policy_name', b.policy_id, b.start_date, b.end_date  
FROM msdb.dbo.syspolicy_policies AS a   
INNER JOIN msdb.dbo.syspolicy_policy_execution_history AS b  
ON a.policy_id = b.policy_id  
```  
  
 Le comportement suivant s'applique si vous spécifiez Null pour l'une des deux valeurs suivantes, ou les deux :  
  
-   Pour supprimer tout l’historique d’exécution de stratégie, spécifiez la valeur NULL pour les deux *policy_id* et *oldest_date*.  
  
-   Pour supprimer tout l’historique d’exécution pour une stratégie spécifique, spécifiez un identificateur de stratégie pour *policy_id*, et spécifiez la valeur NULL en tant que *oldest_date*.  
  
-   Pour supprimer l’historique d’exécution pour toutes les stratégies avant une date spécifique, spécifiez NULL pour *policy_id*, spécifiez une date pour *oldest_date*.  
  
 Pour archiver l'historique d'exécution de la stratégie, vous pouvez ouvrir le journal Historique de la stratégie dans l'Explorateur d'objets et exporter l'historique d'exécution dans un fichier. Pour accéder à l’historique de stratégie, développez **gestion**, avec le bouton droit **gestion des stratégies de**, puis cliquez sur **afficher l’historique**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance de la [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la plupart des objets soient créés dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Étant donné cette possible élévation des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs qui sont approuvés avec contrôle de la configuration de la [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime l'historique d'exécution de la stratégie avant une date spécifique pour une stratégie ayant un ID égal à 7.  
  
```  
EXEC msdb.dbo.sp_syspolicy_delete_policy_execution_history @policy_id = 7  
, @oldest_date = '2009-02-16 16:00:00.000';  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur la stratégie &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_set_config_history_retention &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-history-retention-transact-sql.md)   
 [sp_syspolicy_purge_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-purge-history-transact-sql.md)  
  
  
