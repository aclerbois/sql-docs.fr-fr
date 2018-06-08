---
title: sp_add_agent_profile (Transact-SQL) | Documents Microsoft
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
- sp_add_agent_profile
- sp_add_agent_profile_TSQL
helpviewer_keywords:
- sp_add_agent_profile
ms.assetid: 5c246a33-2c21-4a77-9c2a-a2c9f0c5dda1
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 75184f18fafc70f4e88a4ee8b250c8d9eb80fbc9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddagentprofile-transact-sql"></a>sp_add_agent_profile (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un nouveau profil pour un Agent de réplication. Cette procédure stockée est exécutée sur une base de données du serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_agent_profile [ [ @profile_id= ] profile_id OUTPUT ]  
        , [ @profile_name= ] 'profile_name'   
        , [ @agent_type= ] 'agent_type' ]   
    [ , [ @profile_type= ] profile_type ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @default= ] default ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@profile_id=** ] *profile_id*  
 Numéro d'identification associé au profil nouvellement inséré. *profile_id* est **int** et est un paramètre OUTPUT facultatif. Si vous l'indiquez, la valeur définie est égale au numéro d'identification du nouveau profil.  
  
 [  **@profile_name=** ] **'***profile_name***'**  
 Nom du profil. *profile_name* est **sysname**, sans valeur par défaut.  
  
 [  **@agent_type=** ] **'***agent_type***'**  
 Type d'agent de réplication. *agent_type* est **int**, sans valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Agent d'instantané|  
|**2**|l'Agent de lecture du journal ;|  
|**3**|Agent de distribution|  
|**4**|Agent de fusion|  
|**9**|Agent de lecture de la file d'attente|  
  
 [  **@profile_type=** ] *type_profil*  
 Est le type de profil. *profile_type* est **int**, avec une valeur par défaut **1**.  
  
 **0** indique un profil de système. **1** indique un profil personnalisé. Seuls des profils personnalisés peuvent être créés à l’aide de cette procédure stockée ; Par conséquent, la seule valeur valide est **1**. Uniquement [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des profils de système.  
  
 [  **@description=** ] **'***description***'**  
 Est une description du profil. *Description* est **nvarchar (3000)**, sans valeur par défaut.  
  
 [  **@default=** ] *par défaut*  
 Indique si le profil est la valeur par défaut pour *agent_type **.* *par défaut* est **bits**, avec une valeur par défaut **0**. **1** indique que le profil ajouté deviendra le nouveau profil par défaut de l’agent spécifié par *agent_type*.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_add_agent_profile** est utilisé dans la réplication de capture instantanée, la réplication transactionnelle et la réplication de fusion.  
  
 Les profils d'agent personnalisés sont ajoutés avec les valeurs par défaut. Utilisez [sp_change_agent_parameter &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md) pour modifier ces valeurs par défaut ou [sp_add_agent_parameter &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md) pour ajouter des paramètres supplémentaires.  
  
 Lorsque **sp_add_agent_profile** est exécutée, une ligne est ajoutée pour le nouveau profil personnalisé dans le [MSagent_profiles &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/msagent-profiles-transact-sql.md) table et les paramètres par défaut associés pour ce profil sont ajoutés à la [MSagent_parameters &#40;Transact-SQL&#41; ](../../relational-databases/system-tables/msagent-parameters-transact-sql.md) table.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_add_agent_profile**.  
  
## <a name="see-also"></a>Voir aussi  
 [Utiliser des profils d’Agent de réplication](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)   
 [Profils de l’Agent de réplication](../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [sp_add_agent_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md)   
 [sp_change_agent_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md)   
 [sp_change_agent_profile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-agent-profile-transact-sql.md)   
 [sp_drop_agent_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md)   
 [sp_drop_agent_profile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md)   
 [sp_help_agent_parameter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md)   
 [sp_help_agent_profile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md)  
  
  
