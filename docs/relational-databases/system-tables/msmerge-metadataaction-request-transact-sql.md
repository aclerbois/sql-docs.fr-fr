---
title: MSmerge_metadataaction_request (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 03/03/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: 
ms.component: system-tables
ms.reviewer: 
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: 
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- MSmerge_metadataaction_request
- MSmerge_metadataaction_request_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_metadataaction_request system table
ms.assetid: cd31a114-900a-4218-ab58-d959e547c647
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 7cdb53489f18f2936a418e9a4a1d29145fcdb4ae
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="msmergemetadataactionrequest-transact-sql"></a>MSmerge_metadataaction_request (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_metadataaction_request** table stocke une ligne pour chaque action de compensation est requise. À l’aide de la synchronisation Web, si une erreur se produit et que la synchronisation doit être retentée, une entrée est inscrite dans **MSmerge_metadataaction_request**. Lors de la phase de téléchargement de la fusion suivante, les demandes de tous les articles appartenant à la publication à synchroniser sont extraites de cette table et téléchargées. Lorsque la synchronisation est effectuée, la ligne correspondante dans le **MSmerge_metadataaction_request** table est supprimée. Cette table est stockée dans la base de données de publication du serveur de publication et dans la base de données d'abonnement de l'Abonné.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Surnom de la table publiée.|  
|**ROWGUID**|**uniqueidentifier**|Identificateur de ligne pour la ligne concernée.|  
|**action**|**tinyint**|Définit l'action de compensation nécessaire.|  
|**génération**|**bigint**|Valeur de la génération pour laquelle l'action de compensation est nécessaire.|  
|**modifié**|**int**|Interne-usage.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40; Transact-SQL &#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40; Transact-SQL &#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [Synchronisation web pour la réplication de fusion](../../relational-databases/replication/web-synchronization-for-merge-replication.md)  
  
  
