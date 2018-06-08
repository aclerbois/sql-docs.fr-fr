---
title: sp_add_operator (Transact-SQL) | Documents Microsoft
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
- sp_add_operator
- sp_add_operator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_operator
ms.assetid: 817cd98a-4dff-4ed8-a546-f336c144d1e0
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3821ebe0886dd5a731e0459f3da686125c02a71c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddoperator-transact-sql"></a>sp_add_operator (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un opérateur (destinataire de la notification) à utiliser pour les alertes et les travaux.  
  
 
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_add_operator [ @name = ] 'name'   
     [ , [ @enabled = ] enabled ]   
     [ , [ @email_address = ] 'email_address' ]   
     [ , [ @pager_address = ] 'pager_address' ]   
     [ , [ @weekday_pager_start_time = ] weekday_pager_start_time ]   
     [ , [ @weekday_pager_end_time = ] weekday_pager_end_time ]   
     [ , [ @saturday_pager_start_time = ] saturday_pager_start_time ]   
     [ , [ @saturday_pager_end_time = ] saturday_pager_end_time ]   
     [ , [ @sunday_pager_start_time = ] sunday_pager_start_time ]   
     [ , [ @sunday_pager_end_time = ] sunday_pager_end_time ]   
     [ , [ @pager_days = ] pager_days ]   
     [ , [ @netsend_address = ] 'netsend_address' ]   
     [ , [ @category_name = ] 'category' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [  **@name=** ] **'***nom***'**  
 Nom de l'opérateur (destinataire de la notification). Ce nom doit être unique et ne peut pas contenir le pourcentage (**%**) caractères. *nom* est **sysname**, sans valeur par défaut.  
  
 [  **@enabled=** ] *activé*  
 Indique l'état actuel de l'opérateur. *activé* est **tinyint**, avec une valeur par défaut **1** (activé). Si **0**, l’opérateur n’est pas activé et ne reçoit pas de notifications.  
  
 [  **@email_address=** ] **'***email_address***'**  
 Adresse de courrier électronique de l'opérateur. Cette chaîne est transmise directement au système de messagerie électronique. *email_address* est **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
 Vous pouvez spécifier une adresse électronique physique ou un alias pour *email_address*. Par exemple :  
  
 '**jdoe**'ou'**jdoe@xyz.com**'  
  
> [!NOTE]  
>  Vous devez utiliser l'adresse de messagerie pour Messagerie de base de données.  
  
 [  **@pager_address=** ] **'***adresse_radiomessagerie***'**  
 L’adresse de radiomessagerie de l’opérateur. Cette chaîne est transmise directement au système de messagerie électronique. *adresse_radiomessagerie* est **narchar(100)**, avec NULL comme valeur par défaut.  
  
 [  **@weekday_pager_start_time=** ] *weekday_pager_start_time*  
 Heure après laquelle l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] envoie la notification de radiomessagerie à l'opérateur spécifié. Cette opération a lieu durant les jours de la semaine, du lundi au vendredi. *weekday_pager_start_time*est **int**, avec une valeur par défaut **090000**, ce qui indique 9 h 00. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [  **@weekday_pager_end_time=** ] *au vendredi*  
 Heure après laquelle **SQLServerAgent** service n’envoie plus de notification par radiomessagerie à l’opérateur spécifié de la semaine, du lundi au vendredi. *au vendredi*est **int**, avec une valeur par défaut 180000, ce qui indique 18 h 00. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [  **@saturday_pager_start_time =**] *heure_début_radiomessagerie_samedi*  
 Heure après laquelle **SQLServerAgent** service envoie la notification par radiomessagerie à l’opérateur spécifié sur son récepteur de radiomessagerie. *heure_début_radiomessagerie_samedi* est **int**, avec une valeur 090000 par défaut, ce qui indique 9 h 00. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [  **@saturday_pager_end_time=** ] *heure_fin_radiomessagerie_samedi*  
 Heure après laquelle **SQLServerAgent** service n’envoie plus de notification par radiomessagerie à l’opérateur spécifié sur son récepteur de radiomessagerie. *heure_fin_radiomessagerie_samedi*est **int**, avec une valeur par défaut **180000**, ce qui indique 18 h 00. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [  **@sunday_pager_start_time=** ] *heure_début_radiomessagerie_dimanche*  
 Heure après laquelle **SQLServerAgent** service envoie la notification par radiomessagerie à l’opérateur spécifié tous les dimanches. *heure_début_radiomessagerie_dimanche*est **int**, avec une valeur par défaut **090000**, ce qui indique 9 h 00. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [  **@sunday_pager_end_time =**] *sunday_pager_end_time*  
 Heure après laquelle **SQLServerAgent** service n’envoie plus de notification par radiomessagerie à l’opérateur spécifié tous les dimanches. *sunday_pager_end_time*est **int**, avec une valeur par défaut **180000**, ce qui indique 18 h 00. sur une horloge de 24 heures. Elle doit être au format HHMMSS.  
  
 [  **@pager_days=** ] *jours_radiomessagerie*  
 Nombre qui indique les jours pendant lesquels l'opérateur peut recevoir des notifications par radiomessagerie (argument utilisé avec un argument définissant les heures de début et de fin). *jours_radiomessagerie*est **tinyint**, avec une valeur par défaut **0** indique que l’opérateur n’est jamais disponible pour recevoir un message. Les valeurs valides sont comprises entre **0** via **127**. *jours_radiomessagerie*est calculée en ajoutant les valeurs représentant les jours voulus. Par exemple, du lundi au vendredi est **2**+**4**+**8**+**16**+**32** = **62**. Le tableau ci-après indique la valeur correspondant à chaque jour de la semaine.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Dimanche|  
|**2**|Lundi|  
|**4**|Mardi|  
|**8**|Mercredi|  
|**16**|Jeudi|  
|**32**|Vendredi|  
|**64**|Samedi|  
  
 [  **@netsend_address=** ] **'***adresse_envoiréseau***'**  
 Adresse réseau de l'opérateur à qui est envoyé le message réseau. *adresse_envoiréseau*est **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
 [  **@category_name=** ] **'***catégorie***'**  
 Nom de la catégorie pour cet opérateur. *catégorie* est **sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 **sp_add_operator** doit être exécuté à partir de la **msdb** base de données.  
  
 Les appels de radiomessagerie reposent sur le système de courrier électronique qui doit pouvoir passer du courrier électronique au récepteur de radiomessagerie si vous désirez utiliser ce dernier.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil dont l'interface graphique permet de gérer facilement les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_add_operator**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit les informations relatives à l'opérateur `danwi`. L'opérateur est activé. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent envoie des informations par radiomessagerie du lundi au vendredi de 8 heures à 17 heures.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_operator  
    @name = N'Dan Wilson',  
    @enabled = 1,  
    @email_address = N'danwi',  
    @pager_address = N'5551290AW@pager.Adventure-Works.com',  
    @weekday_pager_start_time = 080000,  
    @weekday_pager_end_time = 170000,  
    @pager_days = 62 ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_delete_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_help_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-operator-transact-sql.md)   
 [sp_update_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
