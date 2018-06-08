---
title: MSreplication_monitordata (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSreplication_monitordata_TSQL
- MSreplication_monitordata
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_monitordata system table
ms.assetid: 843d3ffd-a1ef-4fd5-a744-c2252199793e
caps.latest.revision: 16
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 44e7914937378e7a33c30ba4bc635c972c0c7848
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msreplicationmonitordata-transact-sql"></a>MSreplication_monitordata (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSreplication_monitordata** table contient des données mises en cache utilisées par le moniteur de réplication, avec une ligne pour chaque abonnement surveillé. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**lastrefresh**|**datetime**|Date et heure auxquelles les données du moniteur ont été actualisées.|  
|**computetime**|**int**|Durée, en secondes, du calcul des données du moniteur.|  
|**publication_id**|**int**|L’ID de publication.|  
|**publisher** (serveur de publication)|**sysname**|Le nom du serveur de publication.|  
|**publisher_srvid**|**int**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Le nom de la base de données de publication.|  
|**Publication**|**sysname**|Nom de la publication.|  
|**publication_type**|**int**|Type de publication, que peut prendre l'une des valeurs suivantes :<br /><br /> **0** = publication transactionnelle<br /><br /> **1** = publication d’instantané<br /><br /> **2** = publication de fusion|  
|**agent_type**|**int**|Type d'Agent de réplication, que peut prendre l'une des valeurs suivantes :<br /><br /> **1** = Agent de capture instantanée<br /><br /> **2** = Agent de lecture du journal<br /><br /> **3** = Agent de distribution<br /><br /> **4** = Agent de fusion<br /><br /> **9** = Agent de lecture de file d’attente|  
|**agent_id**|**int**|ID de l'Agent de réplication.|  
|**agent_name**|**sysname**|Nom du travail d'Agent de réplication.|  
|**job_id**|**uniqueidentifier**|GUID du travail d'Agent de réplication.|  
|**status**|**int**|État de l'Agent de réplication, qui peut prendre l'une des valeurs suivantes :<br /><br /> **1** = démarré<br /><br /> **2** = a réussi<br /><br /> **3** = en cours<br /><br /> **4** = inactif<br /><br /> **5** = reprise<br /><br /> **6** = Échec|  
|**isagentrunningnow**|**bit**|Un indicateur qui indique si le travail de l’agent est en cours d’exécution, où la valeur **1** signifie que la tâche est en cours d’exécution.|  
|**Avertissement**|**int**|Avertissement de seuil généré par un abonnement, qui peut être le résultat OR logique d'au moins l'une des valeurs suivantes.<br /><br /> **1** = expiration – un abonnement à une publication transactionnelle a dépassé la période de rétention au-delà du seuil autorisé, en pourcentage de la période de rétention.<br /><br /> **2** = latency - le temps nécessaire pour répliquer des données à partir d’un serveur de publication transactionnelle à l’abonné dépasse le seuil, en secondes.<br /><br /> **4** = mergeexpiration – un abonnement à une publication de fusion a dépassé la période de rétention au-delà du seuil autorisé, en pourcentage de la période de rétention. 8 = durée d'exécution rapide de la fusion ; la durée de la réalisation de la synchronisation d'un abonnement de fusion dépasse le seuil, en secondes, via une connexion réseau rapide.<br /><br /> **16** = mergeslowrunduration - le temps nécessaire pour effectuer la synchronisation d’un abonnement de fusion dépasse le seuil, en secondes, via une connexion réseau lente ou à distance.<br /><br /> **32** = mergefastrunspeed – la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas pu mettre à jour le taux du seuil, en lignes par seconde, via une connexion réseau rapide.<br /><br /> **64** = mergeslowrunspeed – la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas pu mettre à jour le taux du seuil, en lignes par seconde, via une connexion réseau lente ou à distance.|  
|**last_distsync**|**datetime**|Dernières date et heure d’exécution de l’Agent de Distribution.|  
|**agentstoptime**|**datetime**|Date et heure auxquelles l'Agent s'est arrêté.|  
|**distdb**|**sysname**|Nom de la base de données de distribution de l'abonnement.|  
|**retention**|**int**|La période de rétention pour la publication.|  
|**time_stamp**|**datetime**|Interne-usage.|  
|**worst_latency**|**int**|Latence maximale, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**best_latency**|**int**|Latence minimale, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**avg_latency**|**int**|Latence moyenne, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**cur_latency**|**int**|Latence, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pendant l'exécution en cours.|  
|**worst_runspeedPerf**|**int**|Durée la plus longue de la synchronisation de la publication de fusion|  
|**best_runspeedPerf**|**int**|Durée minimale de la synchronisation de la publication de fusion.|  
|**average_runspeedPerf**|**int**|La durée moyenne des synchronisations de fusion|  
|**mergePerformance**|**int**|Performances de la dernière synchronisation comparées à toutes les synchronisations de l'abonnement, calculées en divisant la vitesse de transmission de la dernière synchronisation par la moyenne de toutes les vitesses de transmission antérieures.|  
|**mergelatestsessionrunduration**|**int**|Durée de l'exécution la plus récente de l'Agent de fusion.|  
|**mergelatestsessionrunspeed**|**float(53)**|Vitesse de transmission de l'exécution la plus récente de l'Agent de fusion.|  
|**mergelatestsessionconnectiontype**|**int**|Connexion utilisée pour la session la plus récente de l'Agent de fusion, qui peut prendre l'une des valeurs suivantes :<br /><br /> **1** = réseau local (LAN)<br /><br /> **2** = connexion d’accès réseau à distance|  
|**retention_period_unit**|**tinyint**|Définit l'unité de rétention, qui peut prendre l'une des valeurs suivantes :<br /><br /> **1** = semaine<br /><br /> **2** = mois<br /><br /> **3** = année|  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programme](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_replmonitorhelpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpsubscription-transact-sql.md)   
 [sp_replmonitorhelppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublication-transact-sql.md)   
 [sp_replmonitorhelppublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublisher-transact-sql.md)   
 [sp_replmonitorhelpmergesession &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesession-transact-sql.md)   
 [sp_replmonitorhelppublicationthresholds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelppublicationthresholds-transact-sql.md)   
 [sp_replmonitorhelpmergesessiondetail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replmonitorhelpmergesessiondetail-transact-sql.md)  
  
  
