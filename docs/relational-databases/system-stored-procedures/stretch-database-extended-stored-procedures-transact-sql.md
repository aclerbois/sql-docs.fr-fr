---
title: Stretch Database (Transact-SQL) de procédures stockées étendue | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stretch
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- Stretch Database, stored procedures
ms.assetid: bda29952-4b8b-4295-ab78-f24dcb0b03c6
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e302aba2baf35f7a01f7242b2fa0eaaad45ee18f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stretch-database-extended-stored-procedures-transact-sql"></a>Stretch Database (Transact-SQL) de procédures stockées étendue
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

 Cette section décrit les procédures stockées étendues qui sont liées à la base de données Stretch.  
  
## <a name="in-this-section"></a>Dans cette section  
[Sys.sp_rda_deauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md) supprime la connexion authentifiée entre une base de données prenant en charge Stretch locale et la base de données Azure distante.

[Sys.sp_rda_get_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md) Obtient le nombre d’heures de données migrées que SQL Server conserve dans une table intermédiaire afin de garantir une restauration complète de la base de données Azure à distance, si une restauration est nécessaire.
  
 [Sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) restaure la connexion authentifiée entre une base de données locale activée pour l’extension et la base de données distante.
  
 [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md)  
 Rapproche l’ID de lot stockée dans la table SQL Server compatible Stretch pour les données migrées le plus récemment avec l’ID de lot stocké dans la table Azure distante. 
 
[Sys.sp_rda_reconcile_columns](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-columns-transact-sql.md) rapproche les colonnes dans la table Azure distante les colonnes dans la la table SQL Server compatible Stretch.
 
 [Sys.sp_rda_reconcile_indexes](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-indexes-transact-sql.md) files d’attente d’une tâche de schéma pour réconcilier les index sur la table distante.
 
 [Sys.sp_rda_set_query_mode](../../relational-databases/system-stored-procedures/sys-sp-rda-set-query-mode-transact-sql.md) Spécifie si les requêtes sur la base de données prenant en charge Stretch en cours et ses tables de retournent les données locales et distantes (par défaut), ou uniquement les données locales.
 
 [Sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md) définit le nombre d’heures de données migrées que SQL Server conserve dans une table intermédiaire afin de garantir une restauration complète de la base de données Azure à distance, si une restauration est nécessaire.
 
 [Sys.sp_rda_test_connection](../../relational-databases/system-stored-procedures/sys-sp-rda-test-connection-transact-sql.md) teste la connexion à partir de SQL Server pour le serveur Azure distant et signale des problèmes qui peuvent empêcher la migration des données.
 
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
