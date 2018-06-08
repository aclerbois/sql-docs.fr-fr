---
title: Sys.database_scoped_configurations (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 05/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- database_scoped_configurations
- database_scoped_configurations_TSQL
- sys.database_scoped_configurations
- sys.database_scoped_configurations_TSQL
helpviewer_keywords:
- sys.database_scoped_configurations catalog view
ms.assetid: 8899310a-3464-4d38-9f2f-88396c4e7dc2
caps.latest.revision: 13
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 373d2933d362f565799518bfe1af516ad1943276
ms.sourcegitcommit: 0cc2cb281e467a13a76174e0d9afbdcf4ccddc29
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/15/2018
---
# <a name="sysdatabasescopedconfigurations-transact-sql"></a>Sys.database_scoped_configurations (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Contient une ligne par configuration. 
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|ID de l’option de configuration.|  
|**nom**|**nvarchar(60)**|Le nom de l’option de configuration. Pour plus d’informations sur les configurations possibles, consultez [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).|  
|**valeur**|**sqlvariant**|La valeur définie pour cette option de configuration pour le réplica principal.|  
|**value_for_secondary**|**sqlvariant**|La valeur définie pour cette option de configuration pour les réplicas secondaires.|  
|**elevate_online**|**nvarchar(60)** |La base de données d’une étendue par défaut défini pour l’option online pour les opérations d’index |
|**elevate_resumable**|nvarchar(60)|La base de données d’une étendue par défaut défini pour l’option de reprise des opérations d’index| 
  
##  <a name="Permissions"></a> Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="remarks"></a>Notes  
 Lorsque la valeur NULL est retournée en tant que la valeur de **value_for_secondary**, cela signifie que la base de données secondaire est définie au principal.  
 
 Les paramètres configurés au niveau de la base de données sont reportés avec la base de données. Cela signifie que lorsqu’une base de données est restaurée ou jointe, les paramètres de configuration existants sont conservés.
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)  
  
  
