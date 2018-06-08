---
title: Sys.sp_xtp_unbind_db_resource_pool (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_xtp_unbind_db_resource_pool_TSQL
- sp_xtp_unbind_db_resource_pool
- sys.sp_xtp_unbind_db_resource_pool_TSQL
- sys.sp_xtp_unbind_db_resource_pool
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xtp_unbind_db_resource_pool
- sys.sp_xtp_unbind_db_resource_pool
ms.assetid: 695a796d-087e-4bc8-99d0-ddc342604c75
caps.latest.revision: 7
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 852d8db43c417d828678929f46ee28f12623a4af
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspxtpunbinddbresourcepool-transact-sql"></a>sys.sp_xtp_unbind_db_resource_pool (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Cette procédure système supprime une liaison existante entre une base de données et un pool de ressources aux fins de suivi de l'utilisation de la mémoire [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  Si aucun pool n'est actuellement lié à la base de données spécifiée, un message de réussite est retourné. Lorsque la liaison de la base de données est annulée, la mémoire allouée précédemment aux objets mémoire optimisés reste allouée au pool de ressources précédent. Vous devez redémarrer la base de données pour libérer la mémoire allouée. Une fois que la base de données n'est plus liée au pool de ressources, la liaison recourt au pool de ressources DEFAULT.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
sys.sp_xtp_unbind_db_resource_pool 'database_name'  
```  
  
## <a name="arguments"></a>Arguments  
 database_name  
 Nom d'une base de données compatible [!INCLUDE[hek_2](../../includes/hek-2-md.md)] existante.  
  
#### <a name="parameters"></a>Paramètres  
  
## <a name="messages"></a>Messages  
 Si la base de données était liée à un pool de ressources nommé, la procédure aboutit. Toutefois, vous devez redémarrer la base de données pour que l'annulation de la liaison prenne effet.  
 En l'absence de liaison existante pour la base de données spécifiée, `sp_xtp_unbind_db_resource_pool` retourne un message de réussite, mais affiche le message d'information suivant :  
  
```  
Msg 41374, Level 16, State 1, Procedure sp_xtp_unbind_db_resource_pool_internal, Line 140.  
Database 'Hekaton_DB' does not have a binding to a resource pool.  
  
```  
  
## <a name="example"></a>Exemple  
 Le code suivant annule la liaison de la base de données Hekaton_DB du pool de ressources [!INCLUDE[hek_2](../../includes/hek-2-md.md)] auquel elle est liée.  Si Hekaton_DB n'est pas actuellement lié à un pool de ressources [!INCLUDE[hek_2](../../includes/hek-2-md.md)], un message est fourni. La base de données doit être redémarrée pour que l'annulation de la liaison prenne effet.  
  
```sql  
sys.sp_xtp_unbind_db_resource_pool 'Hekaton_DB'  
```  
  
## <a name="requirements"></a>Spécifications  
  
-   La base de données spécifiée par `database_name` doit avoir une liaison à un pool de ressources [!INCLUDE[hek_2](../../includes/hek-2-md.md)].  
  
-   Requiert l'autorisation CONTROL SERVER.  
  
## <a name="see-also"></a>Voir aussi  
 [Lier une base de données avec des tables optimisées en mémoire à un pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)   
 [Sys.sp_xtp_bind_db_resource_pool & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sys-sp-xtp-bind-db-resource-pool-transact-sql.md)  
  
  
