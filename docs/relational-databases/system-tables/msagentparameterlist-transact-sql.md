---
title: MSagentparameterlist (Transact-SQL) | Documents Microsoft
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
- MSagentparameterlist_TSQL
- MSagentparameterlist
dev_langs:
- TSQL
helpviewer_keywords:
- Msagentparameterlist system table
ms.assetid: 4ea571a0-078d-4e13-95ee-f3d4bbd4dfb2
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7b74f4e4f5359f0fc0068b0847899585bc2b0ee9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msagentparameterlist-transact-sql"></a>MSagentparameterlist (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le **MSagentparameterlist** table contient des informations de paramètre de l’agent de réplication et qu’il est utilisé pour spécifier les paramètres qui peuvent être définies pour un type d’agent donné. Cette table est stockée dans le **msdb** base de données.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**agent_type**|**tinyint**|Type d'Agent:<br /><br /> **1** = Agent de capture instantanée.<br /><br /> **2** = Agent de lecture du journal.<br /><br /> **3** = Agent de distribution.<br /><br /> **4** = Agent de fusion.<br /><br /> **9** = Agent de lecture de file d’attente.|  
|**parameter_name**|**sysname**|Nom d'un paramètre d'agent valide.|  
|**default_value**|**nvarchar(4000)**|Valeur par défaut du paramètre d'agent, où NULL indique l'absence d'une telle valeur.|  
|**MIN_VALUE**|**int**|Définit une limite inférieure pour le paramètre d'agent, où NULL indique l'absence de limite inférieure.|  
|**MAX_VALUE**|**int**|Définit une limite supérieure pour le paramètre d'agent, où NULL indique l'absence de limite supérieure.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
