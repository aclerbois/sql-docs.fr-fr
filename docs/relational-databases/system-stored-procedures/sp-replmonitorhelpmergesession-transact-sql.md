---
title: sp_replmonitorhelpmergesession (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
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
- sp_replmonitorhelpmergesession_TSQL
- sp_replmonitorhelpmergesession
helpviewer_keywords:
- sp_replmonitorhelpmergesession
ms.assetid: a0400ba8-9609-4901-917e-925e119103a1
caps.latest.revision: 25
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: d1d59aba126e66d523e23c680c55f679cbbfa6ee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spreplmonitorhelpmergesession-transact-sql"></a>sp_replmonitorhelpmergesession (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne des informations sur les sessions passées d'un Agent de fusion de réplication, à raison d'une ligne par session correspondant au critère de filtrage. Cette procédure stockée, qui est utilisée pour surveiller la réplication de fusion, est exécutée sur la base de données de distribution du serveur de distribution ou sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorhelpmergesession [ [ @agent_name = ] 'agent_name' ]  
    [ , [ @hours = ] hours ]  
    [ , [ @session_type = ] session_type ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [ **@agent_name** =] **'***agent_name***'**  
 Nom de l'agent. *agent_name* est **nvarchar (100)** sans valeur par défaut.  
  
 [ **@hours** =] *heures*  
 Plage horaire pour laquelle sont retournées les informations d'historique des sessions de l'Agent. *heures* est **int**, qui peut être une des plages suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|< **0**|Retourne des informations sur les exécutions passées de l'Agent, dans la limite de 100 exécutions.|  
|**0** (valeur par défaut)|Retourne des informations sur toutes les exécutions passées de l'Agent.|  
|> **0**|Retourne des informations sur l’agent qui s’est produite au cours de la dernière *heures* nombre d’heures.|  
  
 [ **@session_type** =] *session_type*  
 Filtre l'ensemble de résultats en fonction du résultat final de la session. *SESSION_TYPE* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|**1** (par défaut)|Sessions de l'Agent se soldant par une nouvelle tentative ou par un succès.|  
|**0**|Sessions de l'Agent se soldant par un échec.|  
  
 [ **@publisher** =] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre est utilisé lors de l’exécution **sp_replmonitorhelpmergesession** sur l’abonné.  
  
 [ **@publisher_db** = ] **'***publisher_db***'**  
 Nom de la base de données de publication. *publisher_db* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre est utilisé lors de l’exécution **sp_replmonitorhelpmergesession** sur l’abonné.  
  
 [  **@publication=** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec NULL comme valeur par défaut. Ce paramètre est utilisé lors de l’exécution **sp_replmonitorhelpmergesession** sur l’abonné.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**ID de session**|**int**|ID de la session de travail d'Agent.|  
|**État**|**int**|État de l'exécution de l'Agent :<br /><br /> **1** = démarrage<br /><br /> **2** = réussite<br /><br /> **3** = en cours<br /><br /> **4** = inactif<br /><br /> **5** = nouvelle tentative<br /><br /> **6** = Échec|  
|**StartTime**|**datetime**|Début de la session de travail de l’agent de temps.|  
|**EndTime**|**datetime**|Session de travail de l’agent de temps a été effectuée.|  
|**Duration**|**int**|Durée cumulée de cette session de travail (en secondes)|  
|**UploadedCommands**|**int**|Nombre de commandes téléchargées (upload) pendant la session d'Agent.|  
|**DownloadedCommands**|**int**|Nombre de commandes téléchargées (download) pendant la session d'Agent.|  
|**Messages d’erreur**|**int**|Nombre de messages d'erreur générés pendant la session d'Agent.|  
|**Code d’erreur**|**int**|ID de l'erreur qui s'est produite|  
|**PercentageDone**|**decimal**|Pourcentage estimé des modifications déjà remises dans une session active.|  
|**TimeRemaining**|**int**|Nombre estimé de secondes restantes dans une session active.|  
|**CurrentPhase**|**int**|Phase actuelle d'une session active ; ce paramètre peut prendre l'une des valeurs suivantes.<br /><br /> **1** = téléchargement<br /><br /> **2** = téléchargement|  
|**LastMessage**|**nvarchar(500)**|Dernier message journalisé par l'Agent de fusion pendant la session.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorhelpmergesession** est utilisé pour surveiller la réplication de fusion.  
  
 Lors de l’exécution sur l’abonné, **sp_replmonitorhelpmergesession** retourne uniquement des informations sur les cinq dernières sessions de l’Agent de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** ou **replmonitor** du rôle de base de données fixe sur la base de données de distribution sur le serveur de distribution ou sur la base de données d’abonnement sur l’abonné permettre exécuter **sp_replmonitorhelpmergesession**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
