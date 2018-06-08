---
title: sp_replmonitorhelppublicationthresholds (Transact-SQL) | Documents Microsoft
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
- sp_replmonitorhelppublicationthresholds
- sp_replmonitorhelppublicationthresholds_TSQL
helpviewer_keywords:
- sp_replmonitorhelppublicationthresholds
ms.assetid: d6b1aa4b-3369-4255-a892-c0e5cc9cb693
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c77f8e36cc33dfee704ef8f5b945a58c7011a23b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spreplmonitorhelppublicationthresholds-transact-sql"></a>sp_replmonitorhelppublicationthresholds (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne l'ensemble de mesures de seuil pour une publication analysée. Cette procédure stockée, utilisée pour surveiller la réplication, est exécutée sur la base de données du serveur de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorhelppublicationthresholds [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @thresholdmetricname = ] 'thresholdmetricname'  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publisher**=] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [ **@publisher_db**=] **'***publisher_db***'**  
 Nom de la base de données publiée. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [ **@publication**=] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [ **@publication_type**=] *publication_type*  
 Type de publication. *publication_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Publication transactionnelle.|  
|**1**|Publication d'instantané.|  
|**2**|Publication de fusion.|  
|NULL (par défaut)|La réplication tente de déterminer le type de publication.|  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**metric_id**|**int**|ID de la mesure de performance de réplication qui peut correspondre à l'une des valeurs suivantes.<br /><br /> **1expiration** -moniteurs d’expiration imminente des abonnements aux publications transactionnelles.<br /><br /> **2latency** -contrôle les performances des abonnements aux publications transactionnelles.<br /><br /> **4mergeexpiration** -contrôle l’expiration imminente des abonnements aux publications de fusion.<br /><br /> **5mergeslowrunduration** -contrôle la durée des synchronisations de fusion sur les connexions lentes (accès à distance).<br /><br /> **6mergefastrunduration** -contrôle la durée des synchronisations de fusion sur une connexion haut débit (LAN).<br /><br /> **7mergefastrunspeed** -contrôle la vitesse de synchronisation des synchronisations de fusion sur une connexion haut débit (LAN).<br /><br /> **8mergeslowrunspeed** -contrôle la vitesse de synchronisation des synchronisations de fusion sur les connexions lentes (accès à distance).|  
|**title**|**sysname**|Nom de la mesure de performance de réplication.|  
|**valeur**|**int**|Valeur seuil de la mesure de performance.|  
|**shouldalert**|**bit**|Indique si une alerte doit être créée lorsque la métrique dépasse le seuil défini pour cette publication. la valeur **1** indique qu’une alerte doit être déclenchée.|  
|**IsEnabled**|**bit**|Indique si l’analyse est activée pour cette métrique de performance de réplication pour cette publication. la valeur **1** indique que la surveillance est activée.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorhelppublicationthresholds** est utilisé avec tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** ou **replmonitor** du rôle de base de données fixe sur la base de données de distribution permettre exécuter **sp_replmonitorhelppublicationthresholds**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
