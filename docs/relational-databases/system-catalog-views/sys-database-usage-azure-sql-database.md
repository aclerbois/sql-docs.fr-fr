---
title: Sys.database_usage (base de données de SQL Azure) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: system-catalog-views
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- database_usage
- database_usage_TSQL
- sys.database_usage_TSQL
- sys.database_usage
dev_langs:
- TSQL
helpviewer_keywords:
- database_usage
- sys.database_usage
ms.assetid: be6820de-60bf-4ddd-ace7-4077893d630f
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: affdc08bb7ae507ca30edfa986cd68a81ba564f2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysdatabaseusage-azure-sql-database"></a>sys.database_usage (Azure SQL Database)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  **Remarque : Cela s’applique uniquement aux V11 de base de données SQL Azure.**  
  
 Indique le nombre, le type et la durée des bases de données de la [!INCLUDE[ssSDS](../../includes/sssds-md.md)] server.  
  
 Le **sys.database_usage** vue contient les colonnes suivantes.  
  
|Nom de la colonne| Description|  
|-----------------|-----------------|  
|time|Date à laquelle les événements d'utilisation se sont produits.|  
|sku|Le type de niveau de service pour la base de données : **Web**, **Business**, **base**, **Standard**, **Premium**|  
|quantity|Nombre maximal de bases de données d'un type de SKU ayant existé pendant cette journée.|  
  
## <a name="permissions"></a>Autorisations  
 Accès en lecture seule à cette vue est disponible pour tous les utilisateurs disposant d’autorisations pour se connecter à la **master** base de données.  
  
## <a name="remarks"></a>Notes  
 Le **sys.database_usage** vue retourne une ligne pour chaque jour de votre abonnement.  
  
## <a name="see-also"></a>Voir aussi  
 [Détails de tarification de base de données SQL](http://go.microsoft.com/fwlink/?LinkID=394978)   
 [Comptes et facturation dans Windows Azure SQL Database](http://msdn.microsoft.com/library/windowsazure/ee621788.aspx)  
  
  
