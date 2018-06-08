---
title: sp_replmonitorchangepublicationthreshold (Transact-SQL) | Documents Microsoft
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
- sp_replmonitorchangepublicationthreshold_TSQL
- sp_replmonitorchangepublicationthreshold
helpviewer_keywords:
- sp_replmonitorchangepublicationthreshold
ms.assetid: 2c3615d8-4a1a-4162-b096-97aefe6ddc16
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: bfc9b3ad0f67fa462db098a44603cc9cb1a4d586
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spreplmonitorchangepublicationthreshold-transact-sql"></a>sp_replmonitorchangepublicationthreshold (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie la mesure du seuil de supervision pour une publication. Cette procédure stockée, utilisée pour surveiller la réplication, est exécutée sur la base de données du serveur de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorchangepublicationthreshold [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @metric_id = ] metric_id ]   
    [ , [ @thresholdmetricname = ] 'thresholdmetricname'   
    [ , [ @value = ] value ]   
    [ , [ @shouldalert = ] shouldalert ]   
    [ , [ @mode = ] mode ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publisher** =] **'***publisher***'**  
 Nom du serveur de publication. *serveur de publication* est **sysname**, sans valeur par défaut.  
  
 [ **@publisher_db** = ] **'***publisher_db***'**  
 Nom de la base de données publiée. *publisher_db* est **sysname**, sans valeur par défaut.  
  
 [ **@publication** =] **'***publication***'**  
 Nom de la publication pour laquelle les attributs du seuil de supervision sont modifiés. *publication* est **sysname**, sans valeur par défaut.  
  
 [ **@publication_type** =] *publication_type*  
 Type de publication. *publication_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Publication transactionnelle.|  
|**1**|Publication d'instantané.|  
|**2**|Publication de fusion.|  
|NULL (par défaut)|La réplication essaie de déterminer le type de publication.|  
  
 [ **@metric_id** =] *metric_id*  
 ID de la mesure du seuil de publication en cours de modification. *metric_id* est **int**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Nom de métrique|  
|-----------|-----------------|  
|**1**|**expiration** : contrôle l'expiration imminente des abonnements aux publications transactionnelles.|  
|**2**|**latency** : contrôle les performances des abonnements aux publications transactionnelles.|  
|**4**|**mergeexpiration** : contrôle l'expiration imminente des abonnements aux publications de fusion.|  
|**5**|**mergeslowrunduration** -contrôle la durée des synchronisations de fusion sur les connexions lentes (accès à distance).|  
|**6**|**mergefastrunduration** -contrôle la durée des synchronisations de fusion sur les connexions à large bande passante réseau local (LAN).|  
|**7**|**mergefastrunspeed** - supervise le taux de synchronisation des synchronisations de fusion sur des connexions à bande passante élevée (LAN).|  
|**8**|**mergeslowrunspeed** -contrôle la vitesse de synchronisation des synchronisations de fusion sur les connexions lentes (accès à distance).|  
  
 Vous devez spécifier soit *metric_id* ou *thresholdmetricname*. Si *thresholdmetricname* est spécifié, puis *metric_id* doit être NULL.  
  
 [ **@thresholdmetricname** =] **'***thresholdmetricname***'**  
 Nom de la mesure du seuil de publication en cours de modification. *thresholdmetricname* est **sysname**, avec NULL comme valeur par défaut. Vous devez spécifier soit *thresholdmetricname* ou *metric_id*. Si *metric_id* est spécifié, puis *thresholdmetricname* doit être NULL.  
  
 [ **@value** =] *valeur*  
 Nouvelle valeur de la mesure du seuil de publication. *valeur* est **int**, avec NULL comme valeur par défaut. Si **null**, la valeur de mesure n’est pas mis à jour.  
  
 [ **@shouldalert** =] *shouldalert*  
 Indique si une alerte est créée lorsque vous atteignez une mesure du seuil de publication. *shouldalert* est **bits**, avec NULL comme valeur par défaut. La valeur **1** signifie qu’une alerte est générée et la valeur **0** signifie qu’une alerte n’est pas générée.  
  
 [ **@mode** =] *mode*  
 Indique si la mesure du seuil de publication est activée. *mode* est **tinyint**, avec une valeur par défaut **1**. La valeur **1** signifie que cette supervision est activée et la valeur **2** signifie que cette supervision est désactivée.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorchangepublicationthreshold** est utilisé avec tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** ou **replmonitor** du rôle de base de données fixe dans la base de données de distribution permettre exécuter **sp_replmonitorchangepublicationthreshold**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
