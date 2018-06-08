---
title: CDC.ddl_history (Transact-SQL) | Documents Microsoft
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
- cdc.ddl_history_TSQL
- cdc.ddl_history
dev_langs:
- TSQL
helpviewer_keywords:
- cdc.ddl_history
ms.assetid: cb97ea71-da2f-441a-bbd2-db1f5f48ab49
caps.latest.revision: 18
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c65fd834ac53f7d1835e9c7641dbe6d6fb78d082
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="cdcddlhistory-transact-sql"></a>cdc.ddl_history (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque modification du langage de définition de données (DDL) apportée aux tables qui sont activées pour la capture des données modifiées. Vous pouvez utiliser cette table pour déterminer le moment où une modification DDL a eu lieu sur une table source et identifier cette modification. Les tables sources qui n'ont pas subi de modifications DDL n'auront pas d'entrées dans cette table.  
  
 Nous vous recommandons de ne pas interroger les tables système directement. À la place, exécutez le [sys.sp_cdc_get_ddl_history](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md) procédure stockée.  
   
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**source_object_id**|**int**|Identificateur de la table source à laquelle la modification DDL a été appliquée.|  
|**object_id**|**int**|ID de la table de modifications associée à une instance de capture pour la table source.|  
|**required_column_update**|**bit**|Indique que le type de données d'une colonne capturée a été modifié dans la table source. Ce changement a modifié la colonne dans la table de modifications.|  
|**ddl_command**|**nvarchar(max)**|Instruction DDL appliquée à la table source.|  
|**ddl_lsn**|**binary(10)**|Numéro séquentiel dans le journal associé à la validation de la modification DDL.|  
|**ddl_time**|**datetime**|Date et heure auxquelles la modification DDL a été apportée à la table source.|  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)  
  
  
