---
title: sp_helppublication_snapshot (Transact-SQL) | Documents Microsoft
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
- sp_helppublication_snapshot
- sp_helppublication_snapshot_TSQL
helpviewer_keywords:
- sp_helppublication_snapshot
ms.assetid: 97b4a7ae-40a5-4328-88f1-ff5d105bbb34
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 166ddc44f6543a5c95ee2650504dcd3f71d45236
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sphelppublicationsnapshot-transact-sql"></a>sp_helppublication_snapshot (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur l'Agent d'instantané d'une publication donnée. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helppublication_snapshot [ @publication = ] 'publication'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@publisher =** ] **'***publisher***'**  
 Spécifie un non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *serveur de publication* ne doit pas être utilisé lors de l’ajout d’un article à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de publication.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID de l'Agent d'instantané|  
|**nom**|**nvarchar(100)**|Nom de l'Agent d'instantané|  
|**publisher_security_mode**|**smallint**|Mode de sécurité utilisé par l'Agent lors de la connexion au serveur de publication. Il peut prendre l'une des valeurs suivantes :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification<br /><br /> **1** = authentification Windows.|  
|**publisher_login**|**sysname**|Nom de connexion utilisé lors de la connexion au serveur de publication.|  
|**publisher_password**|**nvarchar (524)**|Pour des raisons de sécurité, une valeur **\* \* \* \* \* \* \* \* \* \*** est toujours retournée.|  
|**job_id**|**uniqueidentifier**|ID unique du travail de l'Agent.|  
|**job_login**|**nvarchar(512)**|Est le compte Windows sous lequel l’agent d’instantané s’exécute, ce qui est retourné sous la forme *domaine*\\*nom d’utilisateur*.|  
|**job_password**|**sysname**|Pour des raisons de sécurité, une valeur **\* \* \* \* \* \* \* \* \* \*** est toujours retournée.|  
|**schedule_name**|**sysname**|Nom de la planification utilisée pour ce travail d'Agent.|  
|**frequency_type**|**int**|Fréquence à laquelle l'Agent s'exécute. La valeur peut être l'une des valeur suivantes.<br /><br /> **1** = une fois<br /><br /> **2** = à la demande<br /><br /> **4** = quotidienne<br /><br /> **8** = hebdomadaire<br /><br /> **16** = mensuelle<br /><br /> **32** = mensuelle relative<br /><br /> **64** = démarrage automatique<br /><br /> **128** = périodique|  
|**frequency_interval**|**int**|Jours d'exécution de l'Agent. La valeur peut être l'une des valeur suivantes.<br /><br /> **1** = dimanche<br /><br /> **2** = lundi<br /><br /> **3** = mardi<br /><br /> **4** = mercredi<br /><br /> **5** = jeudi<br /><br /> **6** = vendredi<br /><br /> **7** = samedi<br /><br /> **8** = jour<br /><br /> **9** = jours de la semaine<br /><br /> **10** = jours de week-end|  
|**frequency_subday_type**|**int**|Est le type qui définit la fréquence à laquelle l’agent s’exécute lorsque *frequency_type* est **4** (quotidienne), et peut prendre l’une des valeurs suivantes.<br /><br /> **1** = à l’heure spécifiée<br /><br /> **2** = secondes<br /><br /> **4** = minutes<br /><br /> **8** = heures|  
|**frequency_subday_interval**|**int**|Nombre d’intervalles de *frequency_subday_type* qui se produisent entre chaque exécution planifiée de l’agent.|  
|**frequency_relative_interval**|**int**|Semaine où l’agent s’exécute dans un mois donné lorsque *frequency_type* est **32** (mensuel relatif), et peut prendre l’une des valeurs suivantes.<br /><br /> **1** = premier<br /><br /> **2** = seconde<br /><br /> **4** = troisième<br /><br /> **8** = quatrième<br /><br /> **16** = dernier|  
|**frequency_recurrence_factor**|**int**|Nombre de semaines ou de mois entre les exécutions planifiées de l'Agent.|  
|**active_start_date**|**int**|Date de la première exécution planifiée de l'Agent, au format AAAAMMJJ.|  
|**active_end_date**|**int**|Date de la dernière exécution planifiée de l'Agent, au format AAAAMMJJ.|  
|**active_start_time**|**int**|Heure de la première exécution planifiée de l'Agent, au format HHMMSS.|  
|**active_end_time**|**int**|Heure de la dernière exécution planifiée de l'Agent, au format HHMMSS.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_help_publication_snapshot** est utilisée dans tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe sur le serveur de publication ou les membres de la **db_owner** du rôle de base de données fixe sur la base de données de publication permettre exécuter **sp_help_publication_snapshot**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)  
  
  
