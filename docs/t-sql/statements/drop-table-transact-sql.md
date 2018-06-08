---
title: DROP TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROP_TABLE_TSQL
- DROP TABLE
dev_langs:
- TSQL
helpviewer_keywords:
- removing indexes
- table removal [SQL Server]
- deleting indexes
- dropping data
- deleting tables
- dropping indexes
- removing constraints
- removing permissions
- DROP TABLE statement
- removing tables
- deleting triggers
- removing data
- dropping tables
- deleting permissions
- dropping triggers
- deleting constraints
- removing triggers
- deleting data
- dropping constraints
- dropping permissions
ms.assetid: 0b6f2b6f-3aa3-4767-943f-43df3c3c5cfd
caps.latest.revision: 61
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4709e7956101b030dd2f4c5f493dce0caf366ba4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="drop-table-transact-sql"></a>DROP TABLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Supprime une ou plusieurs définitions de table ainsi que toutes les données, index, déclencheurs, contraintes et spécifications d'autorisation se rapportant à celles-ci. Toute vue ou procédure stockée faisant référence à la table supprimée doit être supprimée explicitement au moyen de l’instruction [DROP VIEW](../../t-sql/statements/drop-view-transact-sql.md) ou [DROP PROCEDURE](../../t-sql/statements/drop-procedure-transact-sql.md). Pour signaler les dépendances sur une table, utilisez [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
DROP TABLE [ IF EXISTS ] [ database_name . [ schema_name ] . | schema_name . ]  
table_name [ ,...n ]  
[ ; ]  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
DROP TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données dans laquelle la table a été créée.  
  
 Microsoft Azure SQL Database prend en charge le format de nom en trois parties nom_bd.[nom_schéma].nom_objet lorsque nom_bd est la base de données active, ou lorsque nom_bd est la base de données tempdb et nom_objet commence par #. Microsoft Azure SQL Database ne prend pas en charge les noms en quatre parties.  
  
 *IF EXISTS*  
 **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Supprime, de manière conditionnelle, la table uniquement si elle existe déjà.  
  
 *schema_name*  
 Nom du schéma auquel appartient la table.  
  
 *table_name*  
 Nom de la table à supprimer.  
  
## <a name="remarks"></a>Notes   
 L'instruction DROP TABLE ne peut pas être utilisée pour supprimer une table référencée par une contrainte FOREIGN KEY. Vous devez au préalable supprimer la contrainte FOREIGN KEY ou la table qui la référence. Si la table de référence et la table qui contient la clé primaire sont supprimées dans la même instruction DROP TABLE, la table de référence doit figurer en premier dans la liste.  
  
 Il est possible de supprimer plusieurs tables de n'importe quelle base de données. Si une table qui est supprimée fait référence à la clé primaire d'une autre table qui est également en cours de suppression, la table de référence qui contient la clé étrangère doit être répertoriée avant la table contenant la clé primaire à laquelle il est fait référence.  
  
 Lorsqu'une table est supprimée, les règles et les valeurs par défaut liées à celle-ci sont dissociées et toutes les contraintes et les déclencheurs qui lui sont associés sont automatiquement supprimés. Si vous recréez la table, vous devez réassocier les règles et valeurs par défaut appropriées, recréer les déclencheurs et ajouter les toutes les contraintes nécessaires.  
  
 Si vous supprimez toutes les lignes d’une table à l’aide de DELETE *tablename* ou si vous utilisez l’instruction TRUNCATE TABLE, la table existe jusqu’à ce que vous la supprimiez.  
  
 Les tables et index volumineux qui utilisent plus de 128 étendues sont supprimés en deux phases distinctes : logique et physique. Au cours de la phase logique, les unités d'allocation existantes utilisées par la table sont marquées pour la désallocation et verrouillées jusqu'à la validation de la transaction. Au cours de la phase physique, les pages IAM marquées pour la désallocation sont supprimées physiquement dans des traitements.  
  
 Si vous supprimez une table qui contient une colonne VARBINARY(MAX) avec l'attribut FILESTREAM, toutes les données stockées dans le système de fichiers ne seront pas supprimées.  
  
> [!IMPORTANT]  
>  DROP TABLE et CREATE TABLE ne doivent pas être exécutés sur la même table dans le même lot. Sinon, une erreur inattendue risque de se produire.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation ALTER sur le schéma auquel appartient la table, l’autorisation CONTROL sur la table ou l’appartenance au rôle de base de données fixe **db_ddladmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dropping-a-table-in-the-current-database"></a>A. Suppression d'une table dans la base de données active  
 Cet exemple supprime la table `ProductVendor1` ainsi que ses données et ses index de la base de données active.  
  
```  
DROP TABLE ProductVendor1 ;  
```  
  
### <a name="b-dropping-a-table-in-another-database"></a>B. Suppression d'une table dans une autre base de données  
 L'exemple suivant supprime la table `SalesPerson2` de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Cet exemple peut être exécuté à partir de n'importe quelle base de données de l'instance de serveur.  
  
```  
DROP TABLE AdventureWorks2012.dbo.SalesPerson2 ;  
```  
  
### <a name="c-dropping-a-temporary-table"></a>C. Suppression d'une table temporaire  
 Cet exemple crée une table temporaire, teste son existence, la supprime et teste une nouvelle fois son existence. Cet exemple n’utilise pas la syntaxe **IF EXISTS** qui est disponible depuis [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)].  
  
```  
CREATE TABLE #temptable (col1 int);  
GO  
INSERT INTO #temptable  
VALUES (10);  
GO  
SELECT * FROM #temptable;  
GO  
IF OBJECT_ID(N'tempdb..#temptable', N'U') IS NOT NULL   
DROP TABLE #temptable;  
GO  
--Test the drop.  
SELECT * FROM #temptable;  
  
```  
  
### <a name="d-dropping-a-table-using-if-exists"></a>D. Suppression d’une table à l’aide de IF EXISTS  
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 L’exemple suivant crée une table nommée T1. Ensuite, la deuxième instruction supprime la table. La troisième instruction n’effectue aucune action, car la table est déjà supprimée, mais elle ne génère pas d’erreur.  
  
```  
CREATE TABLE T1 (Col1 int);  
GO  
DROP TABLE IF EXISTS T1;  
GO  
DROP TABLE IF EXISTS T1;  
```  
  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
