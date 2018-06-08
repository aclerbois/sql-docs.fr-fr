---
title: MSsubscriptions (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- MSsubscriptions_TSQL
- MSsubscriptions
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscriptions system table
ms.assetid: b7e8301d-d115-41f6-8d4f-e0d25f453b25
caps.latest.revision: 18
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 05f5100843227093cd11909adede12f449cf0051
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mssubscriptions-transact-sql"></a>MSsubscriptions (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSsubscriptions** table contient une ligne pour chaque article publié dans un abonnement pris en charge par le distributeur local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publisher_database_id**|**int**|Identificateur de la base de données du serveur de publication.|  
|**publisher_id**|**smallint**|L’ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**publication_id**|**int**|ID de la publication.|  
|**article_id**|**int**|L’ID de l’article.|  
|**subscriber_id**|**smallint**|L’ID de l’abonné.|  
|**bd_abonné**|**sysname**|Le nom de la base de données d’abonnement.|  
|**subscription_type**|**int**|Le type d’abonnement :<br /><br /> **0** = par envoi de données.<br /><br /> **1** = par extraction de données.<br /><br /> **2** = anonyme.|  
|**sync_type**|**tinyint**|Type de synchronisation :<br /><br /> **1** = automatique.<br /><br /> **2** = pas de synchronisation.|  
|**status**|**tinyint**|L’état de l’abonnement :<br /><br /> **0** = inactif.<br /><br /> **1** = abonné.<br /><br /> **2** = actif.|  
|**subscription_seqno**|**varbinary(16)**|Numéro de séquence de la transaction d'instantané.|  
|**snapshot_seqno_flag**|**bit**|Indique la source du numéro de séquence de transaction de capture instantanée, où la valeur **1** signifie que **subscription_seqno** est le numéro de séquence de capture instantanée.|  
|**independent_agent**|**bit**|Indique s'il existe un Agent de distribution autonome pour cette publication.|  
|**subscription_time**|**datetime**|À usage interne uniquement|  
|**loopback_detection**|**bit**|S'applique aux abonnements qui font partie d'une topologie de réplication transactionnelle bidirectionnelle. La détection de boucle détermine si l'Agent de distribution renvoie à l'Abonné les transactions émanant de ce dernier :<br /><br /> **1** = ne pas renvoyer.<br /><br /> **0** = renvoie les transactions.<br /><br /> Remarque : Cette colonne est pris en charge uniquement pour la compatibilité descendante avec les fonctionnalités de réplication bidirectionnelle dans [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Pour les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez plutôt la fonctionnalité de réplication d'égal à égal. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).|  
|**agent_id**|**int**|ID de l'Agent.|  
|**update_mode**|**tinyint**|Type de mise à jour.|  
|**publisher_seqno**|**varbinary(16)**|Numéro de séquence de la transaction au niveau du serveur de publication pour cet abonnement.|  
|**ss_cplt_seqno**|**varbinary(16)**|Numéro de séquence utilisé pour indiquer la fin du traitement de l'instantané concurrent.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
