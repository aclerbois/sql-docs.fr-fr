---
title: CDC.index_columns (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- cdc.index_columns_TSQL
- cdc.index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.index_columns
ms.assetid: 256ec8a5-3031-40a8-9fdb-99db42ea453d
caps.latest.revision: 14
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 631566399cb1489286c36b7af9ed6bbb84e9bb7d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="cdcindexcolumns-transact-sql"></a>cdc.index_columns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque colonne d'index associée à une table de modifications. Les colonnes d'index sont utilisées par la capture des données modifiées pour identifier de façon unique les lignes dans la table source. Par défaut, les colonnes de la clé primaire de la table source sont incluses. Toutefois, si un index unique de la table source est spécifié quand la capture des données modifiées est activée sur la table source, les colonnes de cet index sont utilisées à la place. Une clé primaire ou un index unique est requis sur la table source si le suivi des modifications nettes est activé. Pour plus d’informations, consultez [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 Nous vous recommandons de ne pas interroger les tables système directement. À la place, exécutez le [sys.sp_cdc_help_change_data_capture](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md) procédure stockée.  

  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table de modifications.|  
|**column_name**|**sysname**|Nom de la colonne d'index.|  
|**index_ordinal**|**tinyint**|Ordinal (de base 1) de la colonne dans l'index.|  
|**column_id**|**int**|ID de la colonne dans la table source.|  
  
## <a name="see-also"></a>Voir aussi  
 [CDC.change_tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
  
  
