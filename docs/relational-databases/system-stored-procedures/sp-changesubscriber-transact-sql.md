---
title: sp_changesubscriber (Transact-SQL) | Documents Microsoft
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
- sp_changesubscriber
- sp_changesubscriber_TSQL
helpviewer_keywords:
- sp_changesubscriber
ms.assetid: d453c451-e957-490f-b968-5e03aeddaf10
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c2cb0047dd66b0c3fd96d399e404b801401d202a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spchangesubscriber-transact-sql"></a>sp_changesubscriber (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie les options d'un Abonné. Toute tâche de distribution destinée aux Abonnés à ce serveur de publication est mise à jour. Cette procédure stockée écrit dans le **MSsubscriber_info** table dans la base de données de distribution. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changesubscriber [ @subscriber= ] 'subscriber'  
    [ , [ @type= ] type ]  
    [ , [ @login= ] 'login' ]  
    [ , [ @password= ] 'password' ]  
    [ , [ @commit_batch_size= ] commit_batch_size ]  
    [ , [ @status_batch_size= ] status_batch_size ]  
    [ , [ @flush_frequency= ] flush_frequency ]  
    [ , [ @frequency_type= ] frequency_type ]  
    [ , [ @frequency_interval= ] frequency_interval ]  
    [ , [ @frequency_relative_interval= ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor= ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday= ] frequency_subday ]  
    [ , [ @frequency_subday_interval= ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day= ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day= ] active_end_time_of_day ]  
    [ , [ @active_start_date= ] active_start_date ]  
    [ , [ @active_end_date= ] active_end_date ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @security_mode= ] security_mode ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@subscriber=**] **'***abonné***'**  
 Nom de l'Abonné sur lequel modifier les options. *abonné* est **sysname**, sans valeur par défaut.  
  
 [  **@type=**] *type*  
 Type de l'Abonné. *type* est **tinyint**, avec NULL comme valeur par défaut. **0** indique un [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonné. **1** spécifie non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un autre serveur de source de données ODBC abonné.  
  
 [  **@login=**] **'***connexion***'**  
 ID de connexion pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *login* est de type **sysname**, avec NULL comme valeur par défaut.  
  
 [  **@password=**] **'***mot de passe***'**  
 Est le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mot de passe d’authentification. *mot de passe* est **sysname**, avec une valeur par défaut **%**. **%** indique aucune modification n’est à la propriété de mot de passe.  
  
 [  **@commit_batch_size=**] *taille_lot_validé*  
 Pris en charge pour la compatibilité descendante uniquement.  
  
 [  **@status_batch_size=**] *taille_lot_état*  
 Pris en charge pour la compatibilité descendante uniquement.  
  
 [  **@flush_frequency=**] *fréquence_vidage*  
 Pris en charge pour la compatibilité descendante uniquement.  
  
 [  **@frequency_type=**] *frequency_type*  
 Fréquence de planification de la tâche de distribution. *frequency_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|À la demande|  
|**4**|Tous les jours|  
|**8**|Semaine|  
|**16**|Mois|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
|**128**|Périodique|  
  
 [  **@frequency_interval=**] *frequency_interval*  
 Intervalle de *frequency_type*. *frequency_interval* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@frequency_relative_interval=**] *frequency_relative_interval*  
 Date de la tâche de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuel relatif). *frequency_relative_interval* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Première|  
|**2**|Seconde|  
|**4**|Troisième|  
|**8**|Quatrième|  
|**16**|Dernière|  
  
 [  **@frequency_recurrence_factor=**] *frequency_recurrence_factor*  
 Indique la fréquence à laquelle la tâche de distribution doit se répéter pendant la période définie par *frequency_type*. *frequency_recurrence_factor* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@frequency_subday=**] *frequency_subday*  
 Fréquence de replanification nécessaire pendant la période définie. *frequency_subday* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
  
 [  **@frequency_subday_interval=**] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_start_time_of_day=**] *active_start_time_of_day*  
 Heure du jour de la première planification de la tâche de distribution, au format HHMMSS. *active_start_time_of_day* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_time_of_day=**] *active_end_time_of_day*  
 Heure à laquelle la tâche de distribution cesse d'être planifiée, exprimée au format HHMMSS. *active_end_time_of_day*est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_start_date=**] *active_start_date*  
 Date de première planification de la tâche de distribution, au format AAAAMMJJ. *active_start_date* est **int**, avec NULL comme valeur par défaut.  
  
 [  **@active_end_date=**] *active_end_date*  
 Date à laquelle la tâche de distribution cesse d'être planifiée, exprimée au format AAAAMMJJ. *active_end_date*est **int**, avec NULL comme valeur par défaut.  
  
 [  **@description=**] **'***description***'**  
 Description de texte facultative. *Description* est **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
 [  **@security_mode=**] *security_mode*  
 Représente le mode de sécurité implémenté. *security_mode* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentification|  
|**1**|Authentification Windows|  
  
 [ **@publisher**=] **'***publisher***'**  
 Spécifie un serveur de publication non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé lors de la modification des propriétés de l’article sur un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changesubscriber** est utilisée dans tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** du rôle serveur fixe peuvent exécuter **sp_changesubscriber**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [sp_helpserver & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
