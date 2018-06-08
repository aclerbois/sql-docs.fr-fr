---
title: Tables système (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server]
- tables [SQL Server], system tables
- information retrieval [SQL Server]
- status information [SQL Server], system tables
- system information [SQL Server]
- system tables [SQL Server]
- system tables [SQL Server], about system tables
- system tables [SQL Server], retrieving information from
- retrieving system table information
ms.assetid: 56b8ad51-930c-4e5c-8d99-8c939d5b70ac
caps.latest.revision: 41
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2658b6ab0687a3eb3c63d6d658f93ff2c25b55c6
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="system-tables-transact-sql"></a>Tables système (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Les rubriques de cette section décrivent les tables système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les tables système ne doivent pas être modifiées directement par l'utilisateur. Par exemple, ne tentez pas de modifier les tables système à l'aide des instructions DELETE, UPDATE ou INSERT ou des déclencheurs définis par l'utilisateur.  
  
 La référence de colonnes documentées est autorisée dans les tables systèmes. Toutefois, la majeure partie des colonnes des tables système ne sont pas documentées. Les applications ne doivent pas prévoir l'interrogation directe de colonnes non documentées. Les applications doivent au contraire utiliser l'un des composants suivants pour extraire les informations stockées dans les tables système :  
  
-   Procédures stockées système  
  
-   Instructions et fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
-   Objets SMO ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects).  
  
-   Objets RMO (Replication Management Objects)  
  
-   Fonctions du catalogue API de la base de données  
  
 Ces composants constituent une API publiée permettant d'obtenir des informations sur le système à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[msCoName](../../includes/msconame-md.md)] maintient la compatibilité de ces composants d'une version à l'autre. Le format des tables système dépend de l'architecture interne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et peut changer d'une version à l'autre. Par conséquent, il se peut que les applications ayant directement accès aux colonnes non documentées des tables système doivent être préalablement modifiées avant de pouvoir accéder à une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>Dans cette section  
 Les rubriques relatives aux tables système sont organisées selon les fonctionnalités suivantes :  
  
|||  
|-|-|  
|[Sauvegarde et restauration des Tables &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)|[Tables de copie des journaux de transaction &#40;Transact-SQL&#41;](../../relational-databases/system-tables/log-shipping-tables-transact-sql.md)|  
|[Modifier des Tables de Capture de données &#40;Transact-SQL&#41;](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)|[Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)|  
|[Tables des plans de Maintenance de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-tables/database-maintenance-plan-tables-transact-sql.md)|[Tables de l’Agent SQL Server &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sql-server-agent-tables-transact-sql.md)|  
|[Étendue des Tables des événements SQL Server &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/6d52ff03-f5aa-4f0f-8c98-9b49dc76f94e)|[Sys.sysoledbusers &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysoledbusers-transact-sql.md)|  
|[Tables Integration Services &#40;Transact-SQL&#41;](../../relational-databases/system-tables/integration-services-tables-transact-sql.md)|[systranschemas &#40;Transact-SQL&#41;](../../relational-databases/system-views/systranschemas-transact-sql.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
