---
title: sp_replmonitorhelppublication (Transact-SQL) | Documents Microsoft
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
- sp_replmonitorhelppublication_TSQL
- sp_replmonitorhelppublication
helpviewer_keywords:
- sp_replmonitorhelppublication
ms.assetid: 7928c50c-617f-41c5-9e0f-4e42e8be55dc
caps.latest.revision: 30
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1067d01bf7b4d510e81ac85f3e88b121b0f645e9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spreplmonitorhelppublication-transact-sql"></a>sp_replmonitorhelppublication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie des informations sur l'état actuel d'une ou plusieurs publications d'un serveur de publication. Cette procédure stockée, utilisée pour surveiller la réplication, est exécutée sur la base de données du serveur de distribution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorhelppublication [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db'   
    [ , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@publisher** =] **'***publisher***'**  
 Nom du serveur de publication dont l'état fait l'objet d'une surveillance. *serveur de publication* est **sysname**, avec NULL comme valeur par défaut. Si **null**, des informations sont retournées pour tous les éditeurs qui utilisent le serveur de distribution.  
  
 [ **@publisher_db** = ] **'***publisher_db***'**  
 Nom de la base de données publiée. *publisher_db* est **sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, les informations retournées concernent toutes les bases de données publiées situées sur le serveur de publication.  
  
 [ **@publication** =] **'***publication***'**  
 Nom de la publication en cours d'analyse. *publication* est **sysname**, avec NULL comme valeur par défaut.  
  
 [ **@publication_type** =] *publication_type*  
 Type de publication. *publication_type* est **int**, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Publication transactionnelle.|  
|**1**|Publication d'instantané.|  
|**2**|Publication de fusion.|  
|NULL (par défaut)|La réplication essaie de déterminer le type de publication.|  
  
 [  **@refreshpolicy=** ] *refreshpolicy*  
 À usage interne uniquement  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publisher_db**|**sysname**|Nom du serveur de publication.|  
|**Publication**|**sysname**|Nom d'une publication.|  
|**publication_type**|**int**|Type de publication, qui peut prendre l'une des valeurs suivantes.<br /><br /> **0** = publication transactionnelle<br /><br /> **1** = publication d’instantané<br /><br /> **2** = publication de fusion|  
|**status**|**int**|État maximal de tous les agents de réplication associés à la publication ; cet état peut prendre l'une des valeurs suivantes.<br /><br /> **1** = démarré<br /><br /> **2** = a réussi<br /><br /> **3** = en cours<br /><br /> **4** = inactif<br /><br /> **5** = reprise<br /><br /> **6** = Échec|  
|**Avertissement**|**int**|Avertissement de seuil maximal généré par un abonnement appartenant à la publication, qui peut être le résultat OR logique d'au moins l'une des valeurs suivantes.<br /><br /> **1** = expiration – un abonnement à une publication transactionnelle n’a pas été synchronisé dans le seuil de période de rétention.<br /><br /> **2** = latency - le temps nécessaire pour répliquer des données à partir d’un serveur de publication transactionnelle à l’abonné dépasse le seuil, en secondes.<br /><br /> **4** = mergeexpiration – un abonnement à une publication de fusion n’a pas été synchronisé dans le seuil de période de rétention.<br /><br /> **8** = mergefastrunduration - le temps nécessaire pour effectuer la synchronisation d’un abonnement de fusion dépasse le seuil, en secondes, via une connexion réseau rapide.<br /><br /> **16** = mergeslowrunduration - le temps nécessaire pour effectuer la synchronisation d’un abonnement de fusion dépasse le seuil, en secondes, via une connexion réseau lente ou à distance.<br /><br /> **32** = mergefastrunspeed – la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas pu mettre à jour le taux du seuil, en lignes par seconde, via une connexion réseau rapide.<br /><br /> **64** = mergeslowrunspeed – la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas pu mettre à jour le taux du seuil, en lignes par seconde, via une connexion réseau lente ou à distance.|  
|**worst_latency**|**int**|Latence maximale, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**best_latency**|**int**|Latence minimale, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**average_latency**|**int**|Latence moyenne, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**last_distsync**|**datetime**|Date et heure de la dernière exécution de l'Agent de distribution.|  
|**retention**|**int**|Période de rétention de la publication.|  
|**LatencyThreshold**|**int**|Seuil de latence défini pour la publication transactionnelle.|  
|**expirationthreshold**|**int**|Seuil d'expiration défini pour la publication s'il s'agit d'une publication de fusion.|  
|**agentnotrunningthreshold**|**int**|Seuil définissant la durée maximale d'inexécution d'un Agent.|  
|**subscriptioncount**|**int**|Nombre d'abonnements à une publication.|  
|**runningdistagentcount**|**int**|Nombre d'Agents de distribution en cours d'exécution pour la publication|  
|**snapshot_agentname**|**sysname**|Nom du travail d'Agent d'instantané pour la publication.|  
|**logreader_agentname**|**sysname**|Nom du travail d'Agent de lecture du journal pour la publication transactionnelle.|  
|**qreader_agentname**|**sysname**|Nom du travail d'Agent de lecture de la file d'attente pour une publication transactionnelle qui prend en charge la mise à jour en attente.|  
|**worst_runspeedPerf**|**int**|Durée maximale de la synchronisation de la publication de fusion.|  
|**best_runspeedPerf**|**int**|Est la durée la plus courte de la synchronisation de la publication de fusion.|  
|**average_runspeedPerf**|**int**|Durée moyenne de la synchronisation de la publication de fusion.|  
|**retention_period_unit**|**int**|Est l’unité utilisée pour exprimer *rétention*.|  
|**publisher** (serveur de publication)|**sysname**|Nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui publie la publication.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorhelppublication** est utilisé avec tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **db_owner** ou **replmonitor** du rôle de base de données fixe sur la base de données de distribution permettre exécuter **sp_replmonitorhelppublication**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
