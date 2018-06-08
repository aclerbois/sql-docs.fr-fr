---
title: MSmerge_tombstone (Transact-SQL) | Documents Microsoft
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
- MSmerge_tombstone_TSQL
- MSmerge_tombstone
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_tombstone system table
ms.assetid: 8b3fc7bf-729b-40f2-8a26-e7dfbe8ddb38
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 99823a19970cf5732aa4e68b3588c6b5a6934a07
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmergetombstone-transact-sql"></a>MSmerge_tombstone (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSmerge_tombstone** table contient des informations sur les lignes supprimées et permet des suppressions soient propagées aux autres abonnés. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**ROWGUID**|**uniqueidentifier**|Identificateur de ligne.|  
|**tablenick**|**int**|Surnom de la table.|  
|**type**|**tinyint**|Type de suppression :<br /><br /> 1 = suppression de l'utilisateur<br /><br /> 5 = exclusion de la ligne de la partition filtrée<br /><br /> 6 = suppression par le système|  
|**lignage**|**varbinary(249)**|Indique la version de l'enregistrement objet de la suppression, ainsi que les mises à jour connues lors de la suppression. Permet d'obtenir la résolution cohérente d'un conflit lorsqu'un abonné modifie une ligne alors qu'un autre abonné est en train de la supprimer.|  
|**génération**|**int**|Est affecté lors de la suppression d'une ligne. Si un abonné demande la génération N, seules les informations résiduelles de génération >= N sont envoyées.|  
|**logical_record_parent_rowguid**|**uniqueidentifier**|Identifie l'enregistrement logique auquel une ligne supprimée appartient.|  
|**logical_record_lineage**|**Varbinary(501)**|Paires nom de l'Abonné/numéro de version permettant de gérer un historique des suppressions pour l'enregistrement logique auquel cette ligne appartient.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
