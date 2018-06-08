---
title: DBCC CLEANTABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CLEANTABLE_TSQL
- DBCC_CLEANTABLE_TSQL
- DBCC CLEANTABLE
- CLEANTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- disk space [SQL Server], reclaiming
- reclaiming space
- reallocating space
- removing columns
- DBCC CLEANTABLE statement
- space [SQL Server], reclaiming
- deleting columns
- dropping columns
ms.assetid: 0dbbc956-15b1-427b-812c-618a044d07fa
caps.latest.revision: 53
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 69a27059322729c71bd4791cded935d983ad205d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-cleantable-transact-sql"></a>DBCC CLEANTABLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]
Récupère l'espace des colonnes de longueur variable supprimées dans les tables ou les vues indexées.
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
  
DBCC CLEANTABLE  
(  
    { database_name | database_id | 0 }  
    , { table_name | table_id | view_name | view_id }  
    [ , batch_size ]  
)  
[ WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name* | *database_id* | 0  
 Base de données à laquelle la table à nettoyer appartient. Si 0 est spécifié, la base de données active est utilisée. Les noms de base de données doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 *table_name* | *table_id* | *view_name*| *view_id*  
 Table ou vue indexée à nettoyer.  
  
 *batch_size*  
 Nombre de lignes traitées par transaction. Si cette valeur n'est pas spécifiée, ou si 0 est spécifié, l'instruction traite la totalité de la table dans une seule transaction.  
  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
## <a name="remarks"></a>Notes   
DBCC CLEANTABLE récupère l'espace consécutif à la suppression d'une colonne de longueur variable. Une colonne de longueur variable peut être l’un des types de données suivants : **varchar**, **nvarchar**, **varchar(max)**, **nvarchar(max)**, **varbinary**, **varbinary(max)**, **text**, **ntext**, **image**, **sql_variant** et **xml**. La commande ne récupère pas l'espace résultant de la suppression d'une colonne de longueur fixe.
Si les colonnes supprimées étaient stockées en ligne, DBCC CLEANTABLE récupère l'espace à partir de l'unité d'allocation IN_ROW_DATA de la table. Si les colonnes étaient stockées hors ligne, l'espace est récupéré à partir de l'unité d'allocation ROW_OVERFLOW_DATA ou LOB_DATA, suivant le type de données de la colonne supprimée. Si la récupération de l'espace à partir d'une page ROW_OVERFLOW_DATA ou LOB_DATA aboutit à une page vide, DBCC CLEANTABLE supprime la page.
DBCC CLEANTABLE s'exécute en une ou plusieurs transactions. Si aucune taille de traitement n'est spécifiée, la commande traite la totalité de la table dans une seule transaction et la table est verrouillée exclusivement au cours de l'opération. Pour certaines tables volumineuses, la longueur de la transaction unique et l'espace journal requis peuvent être trop importants. Si une taille de traitement est spécifiée, la commande s'exécute dans une série de transactions, dont chacune inclut le nombre de lignes spécifié. DBCC CLEANTABLE ne peut pas être exécuté en tant que transaction à l'intérieur d'une autre transaction.
Cette opération est entièrement journalisée.
DBCC CLEANTABLE n'est pas pris en charge pour une utilisation sur les tables système, les tables temporaires ou la partie d'index columnstore optimisé en mémoire d'une table.
  
## <a name="best-practices"></a>Bonnes pratiques  
DBCC CLEANTABLE ne doit pas être exécuté en tant que tâche de maintenance courante. Par contre, utilisez DBCC CLEANTABLE si vous avez apporté des modifications significatives aux colonnes de longueur variable d'une table ou d'une vue indexée et que vous devez récupérer immédiatement l'espace inutilisé. Une autre solution consiste à reconstruire les index sur la table ou vue ; toutefois, cette opération consomme davantage de ressources.
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC CLEANTABLE retourne :
  
```sql
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  
 L’appelant doit être propriétaire de la table ou de la vue indexée, ou être membre du rôle serveur fixe **sysadmin**, du rôle de base de données fixe **db_owner** ou du rôle de base de données fixe **db_ddladmin**.  
  
## <a name="examples"></a>Exemples  
### <a name="a-using-dbcc-cleantable-to-reclaim-space"></a>A. Utilisation de DBCC CLEANTABLE pour récupérer de l'espace  
L'exemple suivant exécute DBCC CLEANTABLE pour la table `Production.Document` de l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql  
DBCC CLEANTABLE (AdventureWorks2012,'Production.Document', 0)  
WITH NO_INFOMSGS;  
GO  
```  
  
### <a name="b-using-dbcc-cleantable-and-verifying-results"></a>B. Utilisation de DBCC CLEANTABLE et vérification des résultats  
L'exemple suivant crée une table puis la remplit avec plusieurs colonnes de longueur variable. Deux des colonnes sont ensuite supprimées et DBCC CLEANTABLE est exécuté pour récupérer l'espace inutilisé. Une requête est exécutée pour vérifier les valeurs du nombre de pages et d'espace utilisé avant et après l'exécution de la commande DBCC CLEANTABLE.
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.CleanTableTest', 'U') IS NOT NULL  
    DROP TABLE dbo.CleanTableTest;  
GO  
CREATE TABLE dbo.CleanTableTest  
    (FileName nvarchar(4000),   
    DocumentSummary nvarchar(max),  
    Document varbinary(max)  
    );  
GO  
-- Populate the table with data from the Production.Document table.  
INSERT INTO dbo.CleanTableTest  
    SELECT REPLICATE(FileName, 1000),   
           DocumentSummary,   
           Document  
    FROM Production.Document;  
GO  
-- Verify the current page counts and average space used in the dbo.CleanTableTest table.  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.dbo.CleanTableTest');  
SELECT alloc_unit_type_desc,   
       page_count,   
       avg_page_space_used_in_percent,   
       record_count  
FROM sys.dm_db_index_physical_stats(@db_id, @object_id, NULL, NULL , 'Detailed');  
GO  
-- Drop two variable-length columns from the table.  
ALTER TABLE dbo.CleanTableTest  
DROP COLUMN FileName, Document;  
GO  
-- Verify the page counts and average space used in the dbo.CleanTableTest table  
-- Notice that the values have not changed.  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.dbo.CleanTableTest');  
SELECT alloc_unit_type_desc,   
       page_count,   
       avg_page_space_used_in_percent,   
       record_count  
FROM sys.dm_db_index_physical_stats(@db_id, @object_id, NULL, NULL , 'Detailed');  
GO  
-- Run DBCC CLEANTABLE.  
DBCC CLEANTABLE (AdventureWorks2012,'dbo.CleanTableTest');  
GO  
-- Verify the values in the dbo.CleanTableTest table after the DBCC CLEANTABLE command.  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.dbo.CleanTableTest');  
SELECT alloc_unit_type_desc,   
       page_count,   
       avg_page_space_used_in_percent,   
       record_count  
FROM sys.dm_db_index_physical_stats(@db_id, @object_id, NULL, NULL , 'Detailed');  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)  
  
  
