---
title: sp_syspolicy_configure (Transact-SQL) | Documents Microsoft
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
- sp_syspolicy_configure
- sp_syspolicy_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_configure
ms.assetid: 70c10922-9345-4190-ba69-808a43f760da
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: c51d30c8453cd5a9c2a92a3eb2ad22016c461b8f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spsyspolicyconfigure-transact-sql"></a>sp_syspolicy_configure (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Configure des paramètres pour la Gestion basée sur des stratégies, tels que l'activation, ou non, de cette dernière.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syspolicy_configure [ @name = ] 'name'  
    , [ @value = ] value  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@name =** ] **'***nom***'**  
 Nom du paramètre à configurer. *nom* est **sysname**, est requis et ne peut pas être une chaîne NULL ou vide.  
  
 *nom* peut être une des valeurs suivantes :  
  
-   'Enabled' - Détermine si la Gestion basée sur des stratégies est activée.  
  
-   'HistoryRetentionInDays' - Spécifie le nombre de jours pendant lesquels l'historique des évaluations de stratégies doit être conservé. Si la valeur est 0, l'historique n'est pas supprimé automatiquement.  
  
-   'LogOnSuccess' - Spécifie si la Gestion basée sur des stratégies consigne les évaluations de stratégies réussies.  
  
 [  **@value =** ] *valeur*  
 Est la valeur associée à la valeur spécifiée pour *nom*. *valeur* est **sql_variant**et est requis.  
  
-   Si vous spécifiez 'Enabled' pour *nom*, vous pouvez utiliser une des valeurs suivantes :  
  
    -   0 = Désactive la Gestion basée sur des stratégies.  
  
    -   1 = Active la Gestion basée sur des stratégies.  
  
-   Si vous spécifiez 'HistoryRententionInDays' pour *nom*, spécifiez le nombre de jours comme une valeur entière.  
  
-   Si vous spécifiez 'LogOnSuccess' pour *nom*, vous pouvez utiliser une des valeurs suivantes :  
  
    -   0 = Consigne uniquement les évaluations de stratégies ayant échoué.  
  
    -   1 = Consigne aussi bien les évaluations de stratégies réussies que celles ayant échoué.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez exécuter sp_syspolicy_configure dans le contexte de la base de données système msdb.  
  
 Pour afficher les valeurs actuelles de ces paramètres, interrogez la vue système msdb.dbo.syspolicy_configuration.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Élévation possible des informations d’identification : les utilisateurs du rôle PolicyAdministratorRole peuvent créer des déclencheurs de serveur et planifier des exécutions de stratégie qui peuvent affecter le fonctionnement de l’instance de la [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Par exemple, les utilisateurs du rôle PolicyAdministratorRole peuvent créer une stratégie qui peut empêcher la plupart des objets soient créés dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Étant donné cette possible élévation des informations d’identification, le rôle PolicyAdministratorRole doit être accordé uniquement aux utilisateurs qui sont approuvés avec contrôle de la configuration de la [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant active la Gestion basée sur des stratégies.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'Enabled'  
, @value = 1;  
  
GO  
```  
  
 L'exemple suivant définit la rétention de l'historique de la stratégie à 14 jours.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'HistoryRetentionInDays'  
, @value = 14;  
  
GO  
```  
  
 L'exemple suivant configure la Gestion basée sur des stratégies pour qu'aussi bien les évaluations de stratégies réussies que celles ayant échoué soient consignées.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'LogOnSuccess'  
, @value = 1;  
  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de gestion basée sur la stratégie &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_set_config_enabled &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-enabled-transact-sql.md)   
 [sp_syspolicy_set_config_history_retention &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-history-retention-transact-sql.md)   
 [sp_syspolicy_set_log_on_success &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-log-on-success-transact-sql.md)  
  
  
