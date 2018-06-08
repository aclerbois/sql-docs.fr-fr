---
title: sp_help_alert (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_help_alert
- sp_help_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_alert
ms.assetid: 850cef4e-6348-4439-8e79-fd1bca712091
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: afaa688907d80a37855890ff8fcf29acd80f9c27
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpalert-transact-sql"></a>sp_help_alert (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Fournit des informations sur les alertes définies pour le serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_alert [ [ @alert_name = ] 'alert_name' ]   
     [ , [ @order_by = ] 'order_by' ]   
     [ , [ @alert_id = ] alert_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @legacy_format = ] legacy_format ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@alert_name =**] **'***alert_name***'**  
 Le nom de l’alerte. *alert_name* est **nvarchar (128)**. Si *alert_name* est ne pas spécifié, les informations sur toutes les alertes sont retournées.  
  
 [  **@order_by =**] **'***order_by***'**  
 Ordre de tri à appliquer pour obtenir les résultats. *Trier par*est **sysname**, avec la valeur par défaut est N '*nom*'.  
  
 [  **@alert_id =**] *id_alerte*  
 Numéro d'identification de l'alerte sur laquelle on veut obtenir des informations. *id_alerte*est **int**, avec NULL comme valeur par défaut.  
  
 [  **@category_name =**] **'***catégorie***'**  
 Catégorie de l'alerte. *catégorie* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ **@legacy_format**=] *legacy_format*  
 Indique si un jeu de résultats hérité est créé. *legacy_format* est **bits**, avec une valeur par défaut **0**. Lorsque *legacy_format* est **1**, **sp_help_alert** retourne le jeu de résultats retourné par **sp_help_alert** dans Microsoft SQL Server 2000.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Lorsque **@legacy_format** est **0**, **sp_help_alert** génère le jeu de résultats suivant.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificateur entier unique attribué par le système.|  
|**nom**|**sysname**|Nom de l’alerte (par exemple Demo : Full **msdb** journal).|  
|**event_source**|**nvarchar(100)**|Source de l'événement. Il sera toujours **MSSQLServer** pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Numéro du message d'erreur définissant l'alerte (Correspond généralement à un numéro d’erreur dans le **sysmessages** table). Si le niveau de gravité est utilisé pour définir l’alerte, **message_id** est **0** ou NULL.|  
|**severity**|**int**|Niveau de gravité (de **9** via **25**, **110**, **120**, **130**, ou **140**) qui définit l’alerte.|  
|**enabled**|**tinyint**|État de si l’alerte est activée (**1**) ou non (**0**). Une alerte non activée ne peut pas être envoyée.|  
|**delay_between_responses**|**int**|Délai d'attente, en secondes, entre les réponses à l'alerte.|  
|**last_occurrence_date**|**int**|Date de la dernière apparition de l'alerte.|  
|**last_occurrence_time**|**int**|Heure de la dernière apparition de l'alerte.|  
|**last_response_date**|**int**|Date de l’alerte a été reçu la réponse de la **SQLServerAgent** service.|  
|**last_response_time**|**int**|Heure de l’alerte a été reçu la réponse de la **SQLServerAgent** service.|  
|**notification_message**|**nvarchar(512)**|Message supplémentaire facultatif qui sera envoyé à l'opérateur avec la notification par courrier électronique ou radiomessagerie.|  
|**include_event_description**|**tinyint**|Indique si la description de l'erreur de SQL Server contenue dans le journal des applications Windows doit apparaître dans le message de notification.|  
|**database_name**|**sysname**|Base de données dans laquelle l'erreur doit apparaître pour que l'alerte soit déclenchée. Si le nom de la base de données est NULL, l'alerte se déclenche où que se soit produite l'erreur.|  
|**event_description_keyword**|**nvarchar(100)**|Description de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le journal des applications Windows qui doit être identique à la séquence de caractères fournie.|  
|**occurrence_count**|**int**|Nombre de déclenchements de l'alerte.|  
|**count_reset_date**|**int**|Date de la **occurrence_count** dernière réinitialisation.|  
|**count_reset_time**|**int**|Heure du **occurrence_count** dernière réinitialisation.|  
|**job_id**|**uniqueidentifier**|Numéro d'identification du travail à exécuter en réponse à une alerte.|  
|**job_name**|**sysname**|Nom du travail à exécuter en réponse à une alerte.|  
|**has_notification**|**int**|Différent de zéro si un ou plusieurs opérateurs sont notifiés pour cette alerte. Le paramètre peut avoir les valeurs suivantes (liées par OR) :<br /><br /> **1**= notification par courrier électronique<br /><br /> **2**= notification par radiomessagerie<br /><br /> **4**= a **envoi réseau** notification.|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**argument condition_performances**|**nvarchar(512)**|Si **type** est **2**, cette colonne affiche la définition de la condition de performance ; sinon, la colonne est NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Sera toujours '[Uncategorized]' pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0.|  
|**wmi_namespace**|**sysname**|Si **type** est **3**, cette colonne indique l’espace de noms pour l’événement WMI.|  
|**wmi_query**|**nvarchar(512)**|Si **type** est **3**, cette colonne affiche la requête pour l’événement WMI.|  
|**type**|**int**|Type de l'événement :<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte d’événement<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte de performances<br /><br /> **3** = alerte d’événement WMI|  
  
 Lorsque **@legacy_format** est **1**, **sp_help_alert** génère le jeu de résultats suivant.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificateur entier unique attribué par le système.|  
|**nom**|**sysname**|Nom de l’alerte (par exemple Demo : Full **msdb** journal).|  
|**event_source**|**nvarchar(100)**|Source de l'événement. Il sera toujours **MSSQLServer** pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|Numéro du message d'erreur définissant l'alerte (Correspond généralement à un numéro d’erreur dans le **sysmessages** table). Si le niveau de gravité est utilisé pour définir l’alerte, **message_id** est **0** ou NULL.|  
|**severity**|**int**|Niveau de gravité (de **9** via **25**, **110**, **120**, **130**, ou 1**40**) qui définit l’alerte.|  
|**enabled**|**tinyint**|État de si l’alerte est activée (**1**) ou non (**0**). Une alerte non activée ne peut pas être envoyée.|  
|**delay_between_responses**|**int**|Délai d'attente, en secondes, entre les réponses à l'alerte.|  
|**last_occurrence_date**|**int**|Date de la dernière apparition de l'alerte.|  
|**last_occurrence_time**|**int**|Heure de la dernière apparition de l'alerte.|  
|**last_response_date**|**int**|Date de l’alerte a été reçu la réponse de la **SQLServerAgent** service.|  
|**last_response_time**|**int**|Heure de l’alerte a été reçu la réponse de la **SQLServerAgent** service.|  
|**notification_message**|**nvarchar(512)**|Message supplémentaire facultatif qui sera envoyé à l'opérateur avec la notification par courrier électronique ou radiomessagerie.|  
|**include_event_description**|**tinyint**|Indique si la description de l'erreur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenue dans le journal des applications Windows doit apparaître dans le message de notification.|  
|**database_name**|**sysname**|Base de données dans laquelle l'erreur doit apparaître pour que l'alerte soit déclenchée. Si le nom de la base de données est NULL, l'alerte se déclenche où que se soit produite l'erreur.|  
|**event_description_keyword**|**nvarchar(100)**|Description de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le journal des applications Windows qui doit être identique à la séquence de caractères fournie.|  
|**occurrence_count**|**int**|Nombre de déclenchements de l'alerte.|  
|**count_reset_date**|**int**|Date de la **occurrence_count** dernière réinitialisation.|  
|**count_reset_time**|**int**|Heure du **occurrence_count** dernière réinitialisation.|  
|**job_id**|**uniqueidentifier**|Numéro d’identification du travail.|  
|**job_name**|**sysname**|Nom d'un travail à la demande exécuté en réponse à une alerte.|  
|**has_notification**|**int**|Différent de zéro si un ou plusieurs opérateurs sont notifiés pour cette alerte. Le paramètre peut avoir une ou plusieurs des valeurs suivantes (combinées avec OR) :<br /><br /> **1**= notification par courrier électronique<br /><br /> **2**= notification par radiomessagerie<br /><br /> **4**= a **envoi réseau** notification.|  
|**flags**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)].|  
|**argument condition_performances**|**nvarchar(512)**|Si **type** est **2**, cette colonne affiche la définition de la condition de performances. Si **type** est **3**, cette colonne affiche la requête pour l’événement WMI. Dans les autres cas, cette colonne est NULL.|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] Sera toujours '**[Uncategorized]**' pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0.|  
|**type**|**int**|Type d'alerte :<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte d’événement<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alerte de performances<br /><br /> **3** = alerte d’événement WMI|  
  
## <a name="remarks"></a>Notes  
 **sp_help_alert** doit être exécuté à partir de la **msdb** base de données.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer du rôle de base de données fixe **SQLAgentOperatorRole** dans la base de données **msdb** .  
  
 Pour plus d’informations sur les **SQLAgentOperatorRole**, consultez [SQL Server Agent Fixed Database Roles](http://msdn.microsoft.com/library/719ce56b-d6b2-414a-88a8-f43b725ebc79).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur l'alerte `Demo: Sev. 25 Errors`.  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_alert @alert_name = 'Demo: Sev. 25 Errors';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_update_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
