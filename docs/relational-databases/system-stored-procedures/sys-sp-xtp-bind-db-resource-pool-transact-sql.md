---
title: Sys.sp_xtp_bind_db_resource_pool (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_xtp_bind_db_resource_pool_TSQL
- sp_xtp_bind_db_resource_pool
- sys.sp_xtp_bind_db_resource_pool_TSQL
- sys.sp_xtp_bind_db_resource_pool
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xtp_bind_db_resource_pool
- sys.sp_xtp_bind_db_resource_pool
ms.assetid: c2a78073-626b-4159-996e-1808f6bfb6d2
caps.latest.revision: 9
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 4ce8af4df2491d6b80acf931ae769b1a0299504b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspxtpbinddbresourcepool-transact-sql"></a>sys.sp_xtp_bind_db_resource_pool (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Lie la base de données [!INCLUDE[hek_2](../../includes/hek-2-md.md)] spécifiée au pool de ressources spécifié. La base de données et le pool de ressources doivent exister avant l'exécution de `sys.sp_xtp_bind_db_resource_pool`.  
  
 Cette procédure système crée une liaison entre le pool du gouverneur de ressources identifié par resource_pool_name et la base de données désignée par database_name. Il n'est pas nécessaire que la base de données comporte des objets optimisés en mémoire au moment de la liaison. En l'absence d'objets optimisés en mémoire, aucune mémoire n'est extraite du pool de ressources. Cette liaison sera utilisée par le gouverneur de ressources pour gérer la mémoire allouée par les allocateurs [!INCLUDE[hek_2](../../includes/hek-2-md.md)], comme décrit ci-dessous.  
  
 Si une liaison est déjà en place pour une base de données particulière, la procédure retourne une erreur.  En aucun cas, une base de données ne peut avoir plusieurs liaisons actives.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
sys.sp_xtp_bind_db_resource_pool 'database_name', 'resource_pool_name'  
```  
  
## <a name="arguments"></a>Arguments  
 database_name  
 Nom d'une base de données compatible [!INCLUDE[hek_2](../../includes/hek-2-md.md)] existante.  
  
 resource_pool_name  
 Nom d'un pool de ressources existant.  
  
## <a name="messages"></a>Messages  
 Lorsqu'une erreur se produit, `sp_xtp_bind_db_resource_pool` retourne l'un de ces messages.  
  
 **Base de données n’existe pas**  
 Database_name doit faire référence à une base de données existante. En l'absence de base de données avec l'ID spécifié, le message suivant est retourné :   
*ID de base de données %d n’existe pas.  Utilisez un ID de base de données valide pour cette liaison.*  
  
```  
Msg 911, Level 16, State 18, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Database 'Hekaton_DB213' does not exist. Make sure that the name is entered correctly.  
```  
  
**Base de données est une base de données système**  
 Il n'est pas possible de créer des tables [!INCLUDE[hek_2](../../includes/hek-2-md.md)] dans des bases de données système.  Il n'est donc pas possible de créer une liaison de mémoire [!INCLUDE[hek_2](../../includes/hek-2-md.md)] pour une base de données de ce type.  L'erreur suivante est retournée :  
*Database_name %s fait référence à une base de données système.  Pools de ressources peuvent uniquement être liées à une base de données utilisateur.*  
  
```  
Msg 41371, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Binding to a resource pool is not supported for system database 'master'. This operation can only be performed on a user database.  
```  
  
**Pool de ressources n’existe pas**  
 Le pool de ressources identifié par resource_pool_name doit exister avant l'exécution de `sp_xtp_bind_db_resource_pool`.  S'il n'existe aucun pool avec l'ID spécifié, l'erreur suivante est retournée :  
*Pool de ressources %s n’existe pas.  Entrez un nom de pool de ressources valide.*  
  
```  
Msg 41370, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Resource pool 'Pool_Hekaton' does not exist or resource governor has not been reconfigured.  
```  
  
**Pool_name fait référence à un pool système réservé**  
 Les noms de pool « INTERNAL » et « DEFAULT » sont réservés aux pools système.  Il n'est pas possible de lier explicitement une base de données à l'un de ces pools.  Si un nom de pool système est fourni, l'erreur suivante est retournée :  
*Pool de ressources %s est un pool de ressources système.  Pools de ressources système ne peuvent pas être explicitement liés à une base de données à l’aide de cette procédure.*  
  
```  
Msg 41373, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Database 'Hekaton_DB' cannot be explicitly bound to the resource pool 'internal'. A database can only be bound only to a user resource pool.  
```  
  
**Base de données est déjà lié à un autre Pool de ressources**  
 Une base de données ne peut être liée qu'à un seul pool de ressources à un moment donné. Les liaisons de bases de données aux pools de ressources doivent être supprimées explicitement avant de pouvoir être associées à un autre pool. Consultez [sys.sp_xtp_unbind_db_resource_pool &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md).  
*Base de données %s est déjà lié au pool de ressources %s.  Vous devez annuler la liaison avant de pouvoir créer une nouvelle liaison.*  
  
```  
Msg 41372, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 54  
Database 'Hekaton_DB' is currently bound to a resource pool. A database must be unbound before creating a new binding.  
```  
  
 En cas de réussite de l'opération, `sp_xtp_bind_db_resource_pool` retourne le message suivant.  
  
**Liaison réussie**  
 En cas de réussite de l'opération, la fonction retourne le message suivant, qui est consigné dans le journal SQL ERRORLOG  
*Une liaison de ressource a été créée avec succès entre la base de données avec l’ID %d et le pool de ressources avec l’ID %d.*  
  
## <a name="examples"></a>Exemples  
A.  L'exemple de code suivant lie la base de données Hekaton_DB au pool de ressources Pool_Hekaton.  
  
```sql  
sys.sp_xtp_bind_db_resource_pool N'Hekaton_DB', N'Pool_Hekaton'  
```  
 
 La liaison prendra effet lors de la prochaine mise en ligne (ONLINE) de la base de données.  
 
 B. Exemple développé d’exemple qui inclut des vérifications de base ci-dessus.  Exécutez le code suivant [!INCLUDE[tsql](../../includes/tsql-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]\:
 
```sql
DECLARE @resourcePool sysname = N'Pool_Hekaton';
DECLARE @database sysname = N'Hekaton_DB';

-- Check whether resource pool exists
IF NOT EXISTS (
    SELECT * FROM sys.resource_governor_resource_pools WHERE name = @resourcePool
    )
BEGIN
    SELECT N'Resource pool "' + @resourcePool + N'" does not exist or resource governor has not been reconfigured.';
END
-- Check whether database is already bound to a resource pool
ELSE IF EXISTS (
    SELECT p.name
    FROM sys.databases d
    JOIN sys.resource_governor_resource_pools p
    ON d.resource_pool_id = p.pool_id
    WHERE d.name = @database
    )
BEGIN
    SELECT N'Database "' + @database + N'" is currently bound to resource pool "' + @resourcePool  + N'". A database must be unbound before creating a new binding.';
END
-- Bind resource pool to database.
ELSE BEGIN
    EXEC sp_xtp_bind_db_resource_pool @database, @resourcePool; 
END 
``` 
  
## <a name="requirements"></a>Spécifications  
  
-   La base de données spécifiée par `database_name`, ainsi que le pool de ressources spécifié par `resource_pool_name`, doivent exister avant de pouvoir procéder à leur liaison.  
  
-   Requiert l'autorisation CONTROL SERVER.  
  
## <a name="see-also"></a>Voir aussi  
 [Lier une base de données avec des tables optimisées en mémoire à un pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)   
 [Sys.sp_xtp_unbind_db_resource_pool & #40 ; Transact-SQL & #41 ;](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md)  
  
  
