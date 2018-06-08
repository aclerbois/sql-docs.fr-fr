---
title: RENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/21/2018
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.component: t-sql|statements
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 0907cfd9-33a6-4fa6-91da-7d6679fee878
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 9ba9202ae949122d83c2690e62a645246b7796bb
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="rename-transact-sql"></a>RENAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Renomme une table créée par l’utilisateur dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Renomme une table ou une base de données créée par l’utilisateur dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
> [!NOTE]  
>  Pour renommer une base de données dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], utilisez [ALTER DATABASE (Azure SQL Data Warehouse](alter-database-azure-sql-data-warehouse.md).  Pour renommer une base de données dans Azure SQL Database, utilisez l’instruction [ALTER DATABASE (Azure SQL Database)](alter-database-azure-sql-database.md). Pour renommer une base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez la procédure stockée [sp_renamedb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-renamedb-transact-sql.md).
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for Azure SQL Data Warehouse  
  
-- Rename a table.  
RENAME OBJECT [::] [ [ database_name .  [schema_name ] ] . ] | [schema_name . ] ] table_name TO new_table_name  
[;]  
  
```  
  
```  
-- Syntax for Parallel Data Warehouse  
  
-- Rename a table  
RENAME OBJECT [::] [ [ database_name . [ schema_name ] . ] | [ schema_name . ] ] table_name TO new_table_name  
[;]  
  
-- Rename a database  
RENAME DATABASE [::] database_name TO new_database_name  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 RENAME OBJECT [::] [ [*database_name* . [ *schema_name* ] . ] | [ *schema_name* . ] ]*table_name* TO *new_table_name*  
 **S’APPLIQUE À :**  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
 Modifiez le nom d’une table définie par l’utilisateur. Spécifiez la table à renommer avec un nom en une, deux ou trois parties.    Spécifiez la nouvelle table *new_table_name* avec un nom en une partie.  
  
 RENAME DATABASE [::] [ *database_name* TO *new_database_name*  
 **S’APPLIQUE À :**  [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
 Modifiez le nom d’une base de données définie par l’utilisateur, de *database_name* à *new_database_name*.  Vous ne pouvez pas utiliser les noms de base de données réservés [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ci-dessous pour renommer une base de données :  
  
-   master  
  
-   model  
  
-   msdb  
  
-   tempdb  
  
-   pdwtempdb1  
  
-   pdwtempdb2  
  
-   DWConfiguration  
  
-   DWDiagnostics  
  
-   DWQueue  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette commande, vous avez besoin de cette autorisation :  
  
-   Autorisation **ALTER** au niveau de la table  
   
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
  
### <a name="cannot-rename-an-external-table-indexes-or-views"></a>Impossible de renommer une table externe, des index ou des vues
Vous ne pouvez pas renommer une table externe, des index ou des vues. Au lieu de renommer une table externe, un index ou une vue, vous pouvez supprimer l’élément en question et le recréer avec un nouveau nom.

### <a name="cannot-rename-a-table-in-use"></a>Impossible de renommer une table utilisée  
 Vous ne pouvez pas renommer une table ou une base de données en cours d’utilisation. Pour renommer une table, vous avez besoin d’un verrou exclusif au niveau de la table. Si la table est en cours d’utilisation, vous pouvez être amené à terminer les sessions qui utilisent la table. Pour terminer une session, vous pouvez utiliser la commande KILL. Utilisez KILL avec précaution, car lorsqu’une session se termine, le travail non validé est annulé. Les sessions dans SQL Data Warehouse ont le préfixe « SID ». Vous devez inclure le préfixe « SID » et le numéro de session lorsque vous appelez la commande KILL. Cet exemple présente une liste de sessions actives ou inactives, puis termine la session « SID1234 ».  
  
### <a name="views-are-not-updated"></a>Les vues ne sont pas mises à jour  
 Quand vous renommez une base de données, toutes les vues qui utilisent l’ancien nom de la base de données deviennent non valides. Ce comportement vaut pour les vues internes et externes à la base de données. Par exemple, si la base de données Sales est renommée, une vue qui contient `SELECT * FROM Sales.dbo.table1` devient non valide. Pour résoudre ce problème, évitez d’utiliser des noms en trois parties dans les vues ou mettez à jour les vues pour qu’elles référencent le nouveau nom de la base de données.  
  
 Quand une table est renommée, les vues ne sont pas mises à jour pour référencer le nouveau nom de la table. Chaque vue, interne ou externe à la base de données, qui référence l’ancien nom de la table devient non valide. Pour résoudre ce problème, mettez à jour chaque vue pour qu’elle référence le nouveau nom de la table.  
  
## <a name="locking"></a>Verrouillage  
 Le renommage d’une table nécessite un verrou partagé au niveau de l’objet DATABASE, un verrou partagé au niveau de l’objet SCHEMA et un verrou exclusif au niveau de la table.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-rename-a-database"></a>A. Renommer une base de données  
 **S’APPLIQUE À :**  [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] uniquement  
  
 Dans cet exemple, la base de données définie par l’utilisateur AdWorks est renommée AdWorks2.  
  
```  
-- Rename the user defined database AdWorks  
RENAME DATABASE AdWorks to AdWorks2;  
  
```  
  
 Quand vous renommez une table, tous les objets et les propriétés associés à la table sont mis à jour pour référencer le nouveau nom de la table. Par exemple, les définitions, les index, les contraintes et les autorisations de la table sont mis à jour. Les vues ne sont pas mises à jour.  
  
### <a name="b-rename-a-table"></a>B. Renommer une table  
 **S’APPLIQUE À :**  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
 Dans cet exemple, la table Customer est renommée Customer1.  
  
```  
-- Rename the customer table  
RENAME OBJECT Customer TO Customer1;  
  
RENAME OBJECT mydb.dbo.Customer TO Customer1;  
```  
  
 Quand vous renommez une table, tous les objets et les propriétés associés à la table sont mis à jour pour qu’ils référencent le nouveau nom de la table. Par exemple, les définitions, les index, les contraintes et les autorisations de la table sont mis à jour. Les vues ne sont pas mises à jour.  
   
  
### <a name="c-move-a-table-to-a-different-schema"></a>C. Déplacer une table vers un schéma différent  
 **S’APPLIQUE À :**  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
 Si votre intention est de déplacer l’objet vers un schéma différent, utilisez [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md). Par exemple, l’instruction suivante déplace l’élément table du schéma product vers le schéma dbo.  
  
```  
ALTER SCHEMA dbo TRANSFER OBJECT::product.item;  
```  
  
### <a name="d-terminate-sessions-before-renaming-a-table"></a>D. Terminer les sessions avant de renommer une table  
 **S’APPLIQUE À :**  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
 Il est important de se rappeler qu’une table en cours d’utilisation ne peut pas être renommée. Le renommage d’une table nécessite un verrou exclusif au niveau de la table. Si la table est en cours d’utilisation, vous serez peut-être amené à terminer la session qui utilise la table. Pour terminer une session, vous pouvez utiliser la commande KILL. Utilisez KILL avec précaution, car lorsqu’une session se termine, le travail non validé est annulé. Les sessions dans SQL Data Warehouse ont le préfixe « SID ». Vous devez inclure le préfixe « SID » et le numéro de session lorsque vous appelez la commande KILL. Cet exemple présente une liste de sessions actives ou inactives, puis termine la session « SID1234 ».  
  
```  
-- View a list of the current sessions  
SELECT session_id, login_name, status   
FROM sys.dm_pdw_exec_sessions   
WHERE status='Active' OR status='Idle';  
  
-- Terminate a session using the session_id.  
KILL 'SID1234';  
```  
  
  
