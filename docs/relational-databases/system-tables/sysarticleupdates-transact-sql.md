---
title: sysarticleupdates (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/04/2017
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
- sysarticleupdates_TSQL
- sysarticleupdates
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticleupdates system table
ms.assetid: 11a53bcd-a215-4d0b-9db8-233981d3ef5d
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 384e6abd93369bd1913bddf31b8dec6f8541e0d7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysarticleupdates-transact-sql"></a>sysarticleupdates (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque article prenant en charge les abonnements acceptant les mises à jour immédiates. Cette table est stockée dans la base de données répliquée.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Colonne d'identité fournissant un numéro d'identification unique à l'article|  
|**pubid**|**int**|Identificateur de la publication à laquelle appartient l'article|  
|**sync_ins_proc**|**int**|ID de la procédure stockée gérant les transactions d'insertion synchrone|  
|**sync_upd_proc**|**int**|ID de la procédure stockée gérant les transactions de mise à jour synchrone|  
|**sync_del_proc**|**int**|ID de la procédure stockée gérant les transactions de suppression synchrone|  
|**autogen**|**bit**|Signale que les procédures stockées sont générées automatiquement :<br /><br /> **0** = false, non automatique.<br /><br /> **1** = true, automatique.|  
|**sync_upd_trig**|**int**|ID du déclencheur d'application automatique de numéro de version pour la table de l'article|  
|**conflict_tableid**|**int**|ID de la table de conflits|  
|**ins_conflict_proc**|**int**|L’ID de la procédure utilisée pour écrire le conflit à le **conflict_table**.|  
|**identity_support**|**bit**|Indique si la gestion automatique des plages d'identités est activée lorsque la mise à jour en attente est utilisée. **0** signifie qu’il n’existe aucune identité de la plage prise en charge.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
