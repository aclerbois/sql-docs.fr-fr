---
title: MSpub_identity_range (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
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
- MSpub_identity_range_TSQL
- MSpub_identity_range
dev_langs:
- TSQL
helpviewer_keywords:
- MSpub_identity_range system table
ms.assetid: 68746eef-32e1-42bc-aff0-9798cd0e88b8
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1e74fd6c4f1352a98151beb525304cec2a518c51
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mspubidentityrange-transact-sql"></a>MSpub_identity_range (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSpub_identity_range** table fournit la prise en charge de gestion de plage identité. Cette table est stockée dans les bases de données de publication et d'abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**objid**|**int**|ID de la table dont la colonne d'identité est gérée par la réplication.|  
|**range**|**bigint**|Contrôle la taille de la plage des valeurs d'identité consécutives qui seraient affectées à l'abonnement dans le cadre d'un ajustement.|  
|**pub_range**|**bigint**|Contrôle la taille de la plage des valeurs d'identité consécutives qui seraient affectées à la publication dans le cadre d'un ajustement.|  
|**current_pub_range**|**bigint**|Plage actuellement utilisée par la publication. Il peut être différent de celui *pub_range* si affiché après avoir été modifiée par **sp_changearticle** et avant le réglage de la plage suivant.|  
|**Seuil**|**int**|Valeur de pourcentage qui contrôle le moment où l'Agent de distribution affecte une nouvelle plage d'identité. Lorsque le pourcentage de valeurs spécifié dans *seuil* est utilisé, l’Agent de Distribution crée une nouvelle plage d’identité.|  
|**last_seed**|**bigint**|Limite inférieure de la plage actuelle.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
