---
title: MSmerge_history (Transact-SQL) | Documents Microsoft
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
- MSmerge_history_TSQL
- MSmerge_history
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_history system table
ms.assetid: 936195ad-ca07-41a8-a1a0-6699b6e63403
caps.latest.revision: 29
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1e28c0aae7f29099ebef75d5c0aa7ef81eec0379
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmergehistory-transact-sql"></a>MSmerge_history (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_history** table contient des lignes d’historique avec une description détaillée des résultats des sessions précédentes du travail de l’Agent de fusion. Cette table contient une ligne par ligne de sortie de l'Agent. Cette table est utilisée dans la base de données de distribution et dans chaque base de données d'abonnement. Dans la base de données de distribution, elle contient l'historique de toutes les publications de fusion et de tous les abonnements de fusion qui utilisent le serveur de distribution. Dans chaque base de données d'abonnement, il contient l'historique des publications auxquelles l'Abonné a souscrit.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|ID du travail d'Agent de fusion|  
|**agent_id**|**int**|ID de l'Agent de fusion|  
|**Commentaires**|**nvarchar(255)**|Texte du message.|  
|**ID_erreur**|**int**|L’ID d’une erreur dans le [MSrepl_errors](../../relational-databases/system-tables/msrepl-errors-transact-sql.md) (table système).|  
|**timestamp**|**timestamp**|Colonne timestamp de cette table|  
|**updatable_row**|**bit**|La valeur **1** si la ligne d’historique peut être remplacée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
