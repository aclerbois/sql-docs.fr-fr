---
title: Sys.change_tracking_databases (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- change_tracking_databases
- sys.change_tracking_databases_TSQL
- sys.change_tracking_databases
- change_tracking_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.change_tracking_databases
- change tracking [SQL Server], sys.change_tracking_databases
ms.assetid: bb233baa-2991-4904-a0eb-3772b81121a4
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a3ec7102bbbdc01694bea11911d0de9a89dbacc8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="change-tracking-catalog-views---syschangetrackingdatabases"></a>Modifier les vues de catalogue de suivi - sys.change_tracking_databases
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Retourne une ligne pour chaque base de données pour laquelle le suivi des modifications est activé.  

|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID de la base de données. Cet ID est unique dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|is_auto_cleanup_on|**bit**|Indique si les données de suivi des modifications sont nettoyées automatiquement à l'issue de la période de rétention configurée :<br /><br /> 0 = désactivé<br /><br /> 1 = activé|  
|retention_period|**int**|Si le nettoyage automatique est utilisé, la période de rétention spécifie la durée pendant laquelle les données de suivi des modifications sont conservées dans la base de données.|  
|retention_period_units_desc|**nvarchar(60)**|Spécifie la description de la période de rétention :<br /><br /> Minutes<br /><br /> Heures<br /><br /> Jours|  
|retention_period_units|**tinyint**|Unité de temps de la période de rétention :<br /><br /> 1 = Minutes<br /><br /> 2 = Heures<br /><br /> 3 = Jours|  
  
## <a name="permissions"></a>Autorisations  
 Les vérifications d'autorisation effectuées pour sys.change_tracking_databases sont les mêmes que celles effectuées pour sys.databases. Si l'appelant de sys.change_tracking_databases n'est pas le propriétaire de la base de données, les autorisations minimales requises pour consulter la ligne correspondante sont des autorisations ALTER ANY DATABASE ou VIEW ANY DATABASE au niveau du serveur, ou encore l'autorisation CREATE DATABASE dans la base de données master ou la base de données active.  
  
## <a name="see-also"></a>Voir aussi  
 [Le suivi des modifications des affichages catalogue &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/6e8fd949-5560-4b34-879f-4e25aa24b183)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
