---
title: IHpublishercolumns (Transact-SQL) | Documents Microsoft
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
- IHpublishercolumns
- IHpublishercolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHpublishercolumns system table
ms.assetid: a5347750-224c-40d9-ae12-57e7213b7db9
caps.latest.revision: 24
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4bd15161e658348ea68c2f87c1468ede00bac5e5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ihpublishercolumns-transact-sql"></a>IHpublishercolumns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **IHpublishercolumns** (table système) représente les métadonnées stockées sur le serveur de publication. Cette table contient une ligne pour chaque colonne répliquée à partir de non - éditeurs SQL Server à l’aide du serveur de distribution en cours. Le type de donnes dans **IHpublishercolumns** est spécifiques au système de gestion de base de données non SQL Server (SGBD) à partir de laquelle les données sont publiées. Cette table est stockée dans la base de données de distribution.  
  
## <a name="definition"></a>Définition  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**publishercolumn_id**|**int**|Identifie une colonne publiée.|  
|**table_id**|**int**|Identifie la table source à partir de [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md) à laquelle appartient la colonne.|  
|**publisher_id**|**smallint**|Identifie le serveur de publication non-SQL à partir de laquelle la colonne est publiée.|  
|**nom**|**sysname**|Le nom de la colonne publiée.|  
|**column_ordinal**|**int**|Identifie la colonne par ordre.|  
|**type**|**varchar(255)**|Type de données de la colonne source sur le serveur de publication.|  
|**length**|**bigint**|Longueur de la colonne source sur le serveur de publication.|  
|**PREC**|**int**|Précision de la colonne source sur le serveur de publication.|  
|**scale**|**int**|Échelle de la colonne source sur le serveur de publication.|  
|**IsNullable**|**bit**|Indique si la colonne accepte les valeurs NULL, où **1** signifie que les valeurs NULL sont acceptées.|  
|**iscaptured**|**bit**|Indique si un déclencheur existe sur la colonne, ce qui peut être le cas même si celle-ci n'est pas publiée dans un article. La valeur **1** signifie que le déclencheur existe sur la colonne.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication de base de données hétérogène](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sysarticlecolumns &#40;vue système&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)   
 [sysarticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
