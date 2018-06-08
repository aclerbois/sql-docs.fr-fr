---
title: CDC.captured_columns (Transact-SQL) | Documents Microsoft
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
- cdc.captured_columns
- cdc.captured_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.captured_columns
ms.assetid: 7bb4d408-d764-4ef6-802c-f271c8d39c2a
caps.latest.revision: 14
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 51cd038691edf6a89382d4bb588646d70792678c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="cdccapturedcolumns-transact-sql"></a>cdc.captured_columns (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque colonne suivie dans une instance de capture. Par défaut, toutes les colonnes de la table source sont capturées. Toutefois, les colonnes peuvent être incluses ou exclues lorsque la table source est activée pour la capture des données modifiées en spécifiant une liste de colonnes. Pour plus d’informations, consultez [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 Il est recommandé que vous **ne pas interroger directement les tables système**. À la place, exécutez le [sys.sp_cdc_get_source_columns](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md) procédure stockée.  
   
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificateur de la table source à laquelle appartient la colonne capturée.|  
|**column_name**|**sysname**|Nom de la colonne capturée.|  
|**column_id**|**int**|ID de la colonne capturée dans la table source.|  
|**COLUMN_TYPE**|**sysname**|Type de la colonne capturée.|  
|**column_ordinal**|**int**|Ordinal de colonne (de base 1) dans la table de modifications. Les colonnes de métadonnées dans la table de modifications sont exclues. L'ordinal 1 est assigné à la première colonne capturée.|  
|**is_computed**|**bit**|Indique que la colonne capturée est une colonne calculée dans la table source.|  
  
## <a name="see-also"></a>Voir aussi  
 [CDC.change_tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
  
  
