---
title: Sys.Views (Transact-SQL) | Documents Microsoft
ms.custom: 
ms.date: 03/15/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: 
ms.component: system-catalog-views
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- views_TSQL
- views
- sys.views_TSQL
- sys.views
dev_langs:
- TSQL
helpviewer_keywords:
- sys.views catalog view
ms.assetid: f8a8ea39-5a09-4662-801e-b43519467def
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 99f0d003eb304a88f4a2fa656b5ed4e658b1b040
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="sysviews-transact-sql"></a>sys.views (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Contient une ligne pour chaque objet de vue, avec **sys.objects.type** = V.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**\<héritée de colonnes >**||Pour obtenir la liste des colonnes qui hérite de cette vue, consultez [sys.objects &#40; Transact-SQL &#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)|  
|**is_replicated**|**bit**|1 = La vue est répliquée.|  
|**has_replication_filter**|**bit**|1 = La vue comporte un filtre de réplication.|  
|**has_opaque_metadata**|**bit**|1 = L'option VIEW_METADATA est spécifiée pour la vue. Pour plus d’informations, consultez [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).|  
|**has_unchecked_assembly_data**|**bit**|1 = La vue contient des données persistantes qui dépendent d'un assembly dont la définition a changé au cours de la dernière instruction ALTER ASSEMBLY. Est réinitialisé à 0 après la dernière instruction DBCC CHECKDB ou DBCC CHECKTABLE réussie.|  
|**with_check_option**|**bit**|1 = WITH CHECK OPTION est spécifié dans la définition de la vue.|  
|**is_date_correlation_view**|**bit**|1 = Le système a créé automatiquement la vue pour stocker les informations de corrélation entre les colonnes de date/heure (datetime). L'attribution de la valeur ON à l'option DATE_CORRELATION_OPTIMIZATION a permis la création de cette vue.|  
  
## <a name="permissions"></a>Permissions  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue d’objets &#40; Transact-SQL &#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ALTER ASSEMBLY &#40; Transact-SQL &#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [Questions fréquentes (FAQ) sur l’interrogation des catalogues système SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
