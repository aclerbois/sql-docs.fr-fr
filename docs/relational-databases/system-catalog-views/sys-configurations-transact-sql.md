---
title: Sys.configurations (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-catalog-views
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.configurations_TSQL
- configurations
- sys.configurations
- configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.configurations catalog view
ms.assetid: c4709ed1-bf88-4458-9e98-8e9b78150441
caps.latest.revision: 36
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 25de8a6eb70c79551caca8188c4f67a786e62172
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysconfigurations-transact-sql"></a>sys.configurations (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient une ligne pour chaque valeur d’option de configuration du serveur dans le système.  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|ID unique pour la valeur de configuration.|  
|**nom**|**nvarchar(35)**|Nom de l'option de configuration.|  
|**valeur**|**sql_variant**|Valeur configurée pour cette option.|  
|**minimum**|**sql_variant**|Valeur minimale pour l'option de configuration.|  
|**maximum**|**sql_variant**|Valeur maximale pour l'option de configuration.|  
|**value_in_use**|**sql_variant**|Valeur en cours d'exécution actuellement en effet pour cette option.|  
|**description**|**nvarchar(255)**|Description de l'option de configuration.|  
|**is_dynamic**|**bit**|1 = Variable qui prend effet lorsque l'instruction RECONFIGURE est exécutée.|  
|**is_advanced**|**bit**|1 = la variable est affichée uniquement lorsque le **afficher advancedoption** est défini.|  
  
 Pour obtenir la liste de toutes les options de configuration de serveur, consultez [les Options de Configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
> [!NOTE]  
>  Pour plus d’options de configuration au niveau de la base de données, consultez [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Pour configurer Soft-NUMA, consultez [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** . Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue de Configuration du serveur &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
