---
title: sp_addsubscriber_schedule (Transact-SQL) | Documents Microsoft
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
- sp_addsubscriber_schedule_TSQL
- sp_addsubscriber_schedule
helpviewer_keywords:
- sp_addsubscriber_schedule
ms.assetid: a6225033-5c3b-452f-ae52-79890a3590ed
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2c5d170d9060f232f2dbf6f5761a3c5ea51495fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spaddsubscriberschedule-transact-sql"></a>sp_addsubscriber_schedule (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute une planification de l'Agent de distribution et de l'Agent de fusion. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addsubscriber_schedule [ @subscriber = ] 'subscriber'  
    [ , [ @agent_type = ] agent_type ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@subscriber =** ] **'***abonné***'**  
 Nom de l'Abonné. *abonné* est **sysname**. Le nom de l'Abonné doit être unique dans la base de données, ne doit pas déjà exister et ne peut pas avoir la valeur NULL.  
  
 [  **@agent_type =** ] *agent_type*  
 Type de l'Agent. *agent_type* est **smallint**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|Agent de distribution|  
|**1**|Agent de fusion|  
  
 [  **@frequency_type =** ] *frequency_type*  
 Fréquence de planification de l'Agent de distribution. *frequency_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|À la demande|  
|**4**|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Mensuelle relative|  
|**64** (par défaut)|Démarrage automatique|  
|**128**|Périodique|  
  
 [  **@frequency_interval =** ] *frequency_interval*  
 Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est **int**, avec une valeur par défaut **1**.  
  
 [  **@frequency_relative_interval =** ] *frequency_relative_interval*  
 Date de l'Agent de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuel relatif). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**1** (par défaut)|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
  
 [  **@frequency_recurrence_factor =** ] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est **int**, avec une valeur par défaut **0**.  
  
 [  **@frequency_subday =** ] *frequency_subday*  
 Fréquence de replanification nécessaire pendant la période définie. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4** (par défaut)|Minute|  
|**8**|Heure|  
  
 [  **@frequency_subday_interval =** ] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est **int**, avec une valeur par défaut **5**.  
  
 [  **@active_start_time_of_day =** ] *active_start_time_of_day*  
 Heure à laquelle l’Agent de distribution est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est **int**, avec une valeur par défaut **0**.  
  
 [  **@active_end_time_of_day =** ] *active_end_time_of_day*  
 L’heure de la journée à laquelle l’Agent de Distribution cesse d’être planifié, représentée au format HHMMSS. *active_end_time_of_day*est **int**, avec 235959 par défaut, ce qui signifie 11:59:59 PM comme sur une horloge de 24 heures.  
  
 [ **@active_start_date =** ] *active_start_date*  
 Date à laquelle l’Agent de distribution est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est **int**, avec une valeur par défaut **0**.  
  
 [  **@active_end_date =** ] *active_end_date*  
 Date à laquelle l’Agent de distribution cesse d'être planifié, au format AAAAMMJJ. *active_end_date* est **int**, avec 99991231 par défaut, qui correspond au 31 décembre 9999.  
  
 [  **@publisher =** ] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être spécifié pour un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addsubscriber_schedule** est utilisé dans la réplication de capture instantanée, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_addsubscriber_schedule**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_changesubscriber_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscriber-schedule-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
