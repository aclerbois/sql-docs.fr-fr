---
title: sp_add_alert (Transact-SQL) | Documents Microsoft
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
- sp_add_alert
- sp_add_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_alert
ms.assetid: d9b41853-e22d-4813-a79f-57efb4511f09
caps.latest.revision: 40
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: dd9e96ece5a5b8a3dc39c6246e0f2201bbffab1d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddalert-transact-sql"></a>sp_add_alert (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée une alerte.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_alert [ @name = ] 'name'   
     [ , [ @message_id = ] message_id ]   
     [ , [ @severity = ] severity ]   
     [ , [ @enabled = ] enabled ]  
     [ , [ @delay_between_responses = ] delay_between_responses ]   
     [ , [ @notification_message = ] 'notification_message' ]   
     [ , [ @include_event_description_in = ] include_event_description_in ]   
     [ , [ @database_name = ] 'database' ]   
     [ , [ @event_description_keyword = ] 'event_description_keyword_pattern' ]   
     [ , { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ]   
     [ , [ @raise_snmp_trap = ] raise_snmp_trap ]   
     [ , [ @performance_condition = ] 'performance_condition' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @wmi_namespace = ] 'wmi_namespace' ]  
     [ , [ @wmi_query = ] 'wmi_query' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@name =** ] **'***nom***'**  
 Le nom de l'alerte. Ce nom apparaît dans le message envoyé par courrier électronique ou par radiomessagerie en réponse à l'alerte. Il doit être unique et peut contenir le pourcentage (**%**) caractères. *nom* est **sysname**, sans valeur par défaut.  
  
 [  **@message_id =** ] *message_id*  
 Numéro du message d'erreur définissant l'alerte. (Il correspond généralement à un numéro d’erreur dans le **sysmessages** table.) *message_id* est **int**, avec une valeur par défaut **0**. Si *gravité* est utilisé pour définir l’alerte, *message_id* doit être **0** ou NULL.  
  
> [!NOTE]  
>  Uniquement **sysmessages** erreurs écrites dans le journal des applications Microsoft Windows peuvent provoquer une alerte à envoyer.  
  
 [  **@severity =** ] *gravité*  
 Le niveau de gravité (de **1** via **25**) qui définit l’alerte. N’importe quel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] message stocké dans le **sysmessages** table envoyé à la [!INCLUDE[msCoName](../../includes/msconame-md.md)] journal des applications Windows avec le niveau de gravité indiqué provoque l’envoi de l’alerte. *gravité* est **int**, avec 0 comme valeur par défaut. Si *message_id* est utilisé pour définir l’alerte, *gravité* doit être **0**.  
  
 [  **@enabled =** ] *activé*  
 Indique l'état actuel de l'alerte. *activé* est **tinyint**, par défaut est 1 (activé). Si **0**, l’alerte n’est pas activé et ne sont pas activés.  
  
 [  **@delay_between_responses =** ] *délai_entre_réponses*  
 Le délai d’attente, en secondes, entre les réponses à l’alerte. *délai_entre_réponses*est **int**, avec une valeur par défaut **0**, ce qui signifie il n’existe aucun délai d’attente entre les réponses (chaque occurrence de l’alerte génère une réponse). La réponse peut prendre l'une des formes suivantes, ou les deux :  
  
-   Une ou plusieurs notifications envoyées par courrier électronique ou par radiomessagerie  
  
-   Un travail à exécuter  
  
 En définissant cette valeur, il est possible d'éviter, par exemple, l'envoi d'un flot de messages par courrier électronique lorsqu'une alerte se produit à plusieurs reprises en peu de temps.  
  
 [  **@notification_message =** ] **'***message_notification***'**  
 Est un message supplémentaire facultatif envoyé à l’opérateur comme faisant partie du message électronique, **envoi réseau**, ou par radiomessagerie. *message_notification* est **nvarchar (512)**, avec NULL comme valeur par défaut. Spécification de *message_notification* est utile pour l’ajout de remarques particulières telles que des procédures correctives.  
  
 [  **@include_event_description_in =** ] *inclure_description_événement_dans*  
 Argument à utiliser si la description de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être incluse dans le message de notification. *inclure_description_événement_dans*est **tinyint**, avec une valeur par défaut **5** (courrier électronique et **envoi réseau**) et peut prendre l’une ou plusieurs de ces valeurs combinées avec un **Ou** opérateur logique.  
  
> [!IMPORTANT]  
>  Les options du récepteur de radiomessagerie et **net send** seront supprimées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans une version future de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Aucun|  
|**1**|Messagerie électronique|  
|**2**|Radiomessagerie|  
|**4**|**net send**|  
  
 [  **@database_name =** ] **'***base de données***'**  
 Base de données dans laquelle l'erreur doit survenir pour que l'alerte soit déclenchée. Si *base de données*n’est pas fourni, l’alerte se déclenche, quelle que soit l’endroit où l’erreur s’est produite. *base de données* est **sysname**. Les noms placés entre crochets ([ ]) ne sont pas autorisés. La valeur par défaut est NULL.  
  
 [ **@event_description_keyword =** ] **'***event_description_keyword_pattern***'**  
 Séquence de caractères à laquelle la description de l'erreur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être conforme. Les caractères correspondant au modèle d'expression [!INCLUDE[tsql](../../includes/tsql-md.md)] LIKE sont admis. *modèle_mots_clés_description_événement* est **nvarchar (100)**, avec NULL comme valeur par défaut. Ce paramètre est utile pour filtrer les noms d’objet (par exemple, **% customer_table %**).  
  
 [  **@job_id =** ] *job_id*  
 Numéro d'identification du travail à exécuter en réponse à l'alerte. *job_id* est **uniqueidentifier**, avec NULL comme valeur par défaut.  
  
 [  **@job_name =** ] **'***job_name***'**  
 Nom du travail à exécuter en réponse à cette alerte. *job_name*est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Soit *job_id* ou *job_name* doit être spécifié, mais ne peut pas être spécifiés.  
  
 [  **@raise_snmp_trap =** ] *raise_snmp_trap*  
 Non implémenté dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0. *raise_snmp_trap* est **tinyint**, avec 0 comme valeur par défaut.  
  
 [  **@performance_condition =** ] **'***l’argument condition_performances***'**  
 Est une valeur exprimée dans le format '*itemcomparatorvalue*'. *l’argument condition_performances* est **nvarchar (512)** avec NULL comme valeur par défaut et se compose de ces éléments.  
  
|Élément de format| Description|  
|--------------------|-----------------|  
|*Élément*|Objet de performances, compteur de performances ou instance nommée du compteur.|  
|*Comparateur*|Un des opérateurs suivants : >, < ou =|  
|*Value*|Valeur numérique du compteur.|  
  
 [  **@category_name =** ] **'***catégorie***'**  
 Nom de la catégorie d'alerte. *catégorie* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ **@wmi_namespace**=] **'***wmi_namespace***'**  
 Espace de noms WMI permettant de rechercher des événements via des requêtes. *wmi_namespace* est **sysname**, avec NULL comme valeur par défaut. Seuls les espaces de noms situés sur le serveur local sont pris en charge.  
  
 [ **@wmi_query**=] **'***wmi_query***'**  
 Requête spécifiant l'événement WMI pour l'alerte. *wmi_query* est **nvarchar (512)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_add_alert** doit être exécuté à partir de la **msdb** base de données.  
  
 La liste suivante indique les circonstances pour lesquelles les erreurs ou les messages générés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont transmis au journal des applications Windows et peuvent dès lors déclencher des alertes :  
  
-   Niveau de gravité supérieur ou égal à 19 **sys.messages** erreurs  
  
-   toute instruction RAISERROR appelée avec la syntaxe WITH LOG ;  
  
-   N’importe quel **sys.messages** erreur modifié ou créé à l’aide **sp_altermessage**  
  
-   Tout événement consigné à l’aide de **xp_logevent**  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil simple, fonctionnant en mode graphique, qui permet de gérer tout le système d'alerte. Son utilisation est recommandée pour configurer une infrastructure d'alertes.  
  
 Si une alerte ne fonctionne pas correctement, vérifiez que :  
  
-   Le service Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution.  
  
-   L'événement est consigné dans le journal des applications Windows.  
  
-   L'alerte est activée.  
  
-   Les événements créés à l’aide de **xp_logevent** surviennent dans la base de données master. Ainsi, la procédure **xp_logevent** ne déclenche pas d’alerte sauf si la valeur de **@database_name** pour l’alerte est **'master'** ou NULL.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter la procédure **sp_add_alert**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant ajoute une alerte (Test Alert) qui exécute le travail `Back up the AdventureWorks2012 Database` lorsqu'elle se déclenche.  
  
> [!NOTE]  
>  Dans cet exemple, on suppose que le message 55001 et le travail `Back up the AdventureWorks2012 Database` existent déjà. L’exemple est présenté à titre d’illustration uniquement.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_alert  
    @name = N'Test Alert',  
    @message_id = 55001,   
   @severity = 0,   
   @notification_message = N'Error 55001 has occurred. The database will be backed up...',   
   @job_name = N'Back up the AdventureWorks2012 Database' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_notification &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_altermessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_delete_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md)   
 [sp_help_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-alert-transact-sql.md)   
 [sp_update_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
