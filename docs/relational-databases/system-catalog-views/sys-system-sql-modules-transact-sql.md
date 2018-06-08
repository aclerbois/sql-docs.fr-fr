---
title: Sys.system_sql_modules (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- system_sql_modules_TSQL
- sys.system_sql_modules
- sys.system_sql_modules_TSQL
- system_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_sql_modules catalog view
ms.assetid: ad3548bc-4780-4821-b962-b421d52daed9
caps.latest.revision: 19
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8cb9a47bc2801d5d2d519333700f582a726af804
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="syssystemsqlmodules-transact-sql"></a>sys.system_sql_modules (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie une ligne par objet système qui contient un module défini par le langage SQL. Les objets système de type FN, IF, P, PC, TF, V sont associés à un module SQL. Pour identifier l’objet conteneur, vous pouvez joindre cette vue à [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md).  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Numéro d'identification de l'objet conteneur, unique dans une base de données.|  
|**Définition**|**nvarchar(max)**|Texte SQL qui définit ce module.|  
|**uses_ansi_nulls**|**bit**|1 = Le module a été créé lorsque l'option de base de données SET ANSI_NULLS était activée (ON).<br /><br /> Retourne toujours 1.|  
|**uses_quoted_identifier**|**bit**|1 = Le module a été créé avec l'instruction SET QUOTED_IDENTIFIER ON.<br /><br /> Retourne toujours 1.|  
|**is_schema_bound**|**bit**|0 = Le module n'a pas été créé avec l'option SCHEMABINDING.<br /><br /> Retourne toujours 0.|  
|**uses_database_collation**|**bit**|0 = Le module ne dépend pas du classement par défaut de la base de données.<br /><br /> Retourne toujours 0.|  
|**is_recompiled**|**bit**|0 = La procédure n'a pas été créée en utilisant l'option WITH RECOMPILE.<br /><br /> Retourne toujours 0.|  
|**null_on_null_input**|**bit**|0 = Le module n'a pas été créé pour produire un résultat NULL pour toute entrée NULL.<br /><br /> Retourne toujours 0.|  
|**execute_as_principal_id**|**int**|Retourne toujours la valeur NULL|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Sys.all_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-sql-modules-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue de l’objet &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  
  
  
