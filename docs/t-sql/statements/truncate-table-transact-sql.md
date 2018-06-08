---
title: TRUNCATE TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- TRUNCATE
- TRUNCATE TABLE
- TRUNCATE_TSQL
- TRUNCATE_TABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server], TRUNCATE TABLE statement
- table truncating [SQL Server]
- removing rows
- TRUNCATE TABLE statement
- deleting rows
- dropping rows
ms.assetid: 3d544eed-3993-4055-983d-ea334f8c5c58
caps.latest.revision: 41
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: c5550dbfb1c245470b6ea0e2ab6d25c7139e82b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="truncate-table-transact-sql"></a>TRUNCATE TABLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Supprime toutes les lignes d’une table ou des partitions spécifiées d’une table, sans journaliser les suppressions de ligne individuelles. L'instruction TRUNCATE TABLE est semblable à l'instruction DELETE sans la clause WHERE. Cependant, l'instruction TRUNCATE TABLE est plus rapide et utilise moins de ressources système et de ressources du journal des transactions.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
TRUNCATE TABLE   
    [ { database_name .[ schema_name ] . | schema_name . } ]  
    table_name  
    [ WITH ( PARTITIONS ( { <partition_number_expression> | <range> }   
    [ , ...n ] ) ) ]  
[ ; ]  
  
<range> ::=  
<partition_number_expression> TO <partition_number_expression>  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
TRUNCATE TABLE [ { database_name . [ schema_name ] . | schema_name . ] table_name  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel appartient la table.  
  
 *table_name*  
 Nom de la table à tronquer ou dont toutes les lignes sont supprimées. *table_name* doit être un littéral. *table_name* ne peut pas être la fonction **OBJECT_ID()** ou une variable.  
  
 WITH ( PARTITIONS ( { \<*partition_number_expression*> | \<*range*> } [ , ...n ] ) )  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658))
  
 Spécifie les partitions à tronquer ou à partir desquelles toutes les lignes sont supprimées. Si la table n’est pas partitionnée, l’argument **WITH PARTITIONS** génère une erreur. Si la clause **WITH PARTITIONS** n’est pas fournie, la totalité de la table est tronquée.  
  
 L’argument *\<partition_number_expression>* peut être spécifié des manières suivantes : 
  
-   Spécifiez le numéro d’une partition, par exemple : `WITH (PARTITIONS (2))`  
  
-   Spécifiez les numéros de plusieurs partitions individuelles séparés par des virgules, par exemple : `WITH (PARTITIONS (1, 5))`  
  
-   Spécifiez à la fois des plages et des partitions individuelles, par exemple : `WITH (PARTITIONS (2, 4, 6 TO 8))`  
  
-   *\<range>* peut être spécifié sous la forme de numéros de partitions séparés par le mot **TO**, par exemple : `WITH (PARTITIONS (6 TO 8))`  
  
 Pour tronquer une table partitionnée, la table et les index doivent être alignés (partitionnés sur la même fonction de partition).  
  
## <a name="remarks"></a>Notes   
 Comparée à l'instruction DELETE, l'instruction TRUNCATE TABLE offre les avantages suivants :  
  
-   Moindre espace du journal des transactions utilisé.  
  
     L'instruction DELETE supprime une ligne à la fois et enregistre une entrée dans le journal des transactions pour chaque ligne supprimée. L'instruction TRUNCATE TABLE supprime les données en désallouant les pages de données utilisées pour stocker les données de la table. Elle enregistre uniquement les désallocations de page dans le journal des transactions.  
  
-   Moins de verrous utilisés.  
  
     Lors de l'exécution de l'instruction DELETE à l'aide d'un verrou de ligne, chaque ligne dans la table est verrouillée pour être supprimée. L'instruction TRUNCATE TABLE verrouille toujours la table (y compris un verrou de schéma SCH-M) et la page, mais pas les lignes.  
  
-   Sans exception, les pages zéros demeurent dans la table.  
  
     Après l'exécution d'une instruction DELETE, la table peut encore contenir des pages vides. Par exemple, des pages vides dans un segment ne peuvent pas être désallouées sans au moins un verrou de table exclusif (LCK_M_X). Si la suppression n'utilise pas de verrou de table, la table (le segment) contiendra beaucoup de pages vides. La suppression peut laisser des pages vides pour les index, bien que ces pages soient désallouées rapidement par un processus de nettoyage en arrière-plan.  
  
 L'instruction TRUNCATE TABLE supprime toutes les lignes d'une table, mais la structure de la table, ses colonnes, contraintes, index et autres éléments sont conservés. Pour supprimer la définition de la table en plus de ses données, utilisez l'instruction DROP TABLE.  
  
 Si la table contient une colonne d'identité, le compteur pour celle-ci est redéfini sur sa valeur de départ. Si aucune valeur de départ n'est définie, la valeur par défaut 1 est utilisée. Utilisez plutôt l'instruction DELETE pour conserver le compteur d'identité.  
  
## <a name="restrictions"></a>Restrictions  
 Vous ne pouvez pas utiliser l'instruction TRUNCATE TABLE sur les tables qui :  
  
-   sont référencées par une contrainte FOREIGN KEY ; (Vous pouvez tronquer une table qui possède une clé étrangère qui fait référence à elle-même.)  
  
-   participent à une vue indexée ;  
  
-   sont publiées à l'aide d'une réplication transactionnelle ou de fusion.  
  
 Utilisez plutôt l'instruction DELETE pour les tables ayant au moins une de ces caractéristiques.  
  
 L'instruction TRUNCATE TABLE ne peut pas activer un déclencheur car l'opération ne consigne pas les suppressions de lignes dans le journal. Pour plus d’informations, consultez [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md). 
 
 Dans [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[sspdw](../../includes/sspdw-md.md)] :

- L’instruction TRUNCATE TABLE n’est pas autorisée dans l’instruction EXPLAIN.

- L’instruction TRUNCATE TABLE ne peut pas être exécutée à l’intérieur d’une transaction.
  
## <a name="truncating-large-tables"></a>Troncation de grandes tables  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet de supprimer ou de tronquer des tables dotées de plus de 128 extensions, sans maintenir de verrous simultanés sur toutes les extensions exigées pour la suppression.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation minimale exigée est ALTER sur *table_name*. Les autorisations TRUNCATE TABLE sont octroyées par défaut au propriétaire de la table, aux membres du rôle serveur fixe sysadmin et des rôles de base de données fixes db_owner et db_ddladmin. Elles ne sont pas transférables. Toutefois, vous pouvez incorporer l’instruction TRUNCATE TABLE dans un module, par exemple une procédure stockée, et accorder les autorisations appropriées au module à l’aide de la clause EXECUTE AS.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-truncate-a-table"></a>A. Tronquer une table  
 L'exemple suivant supprime toutes les données de la table `JobCandidate`. Des instructions `SELECT` sont incluses avant et après l'instruction `TRUNCATE TABLE` pour comparer les résultats.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT COUNT(*) AS BeforeTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
TRUNCATE TABLE HumanResources.JobCandidate;  
GO  
SELECT COUNT(*) AS AfterTruncateCount   
FROM HumanResources.JobCandidate;  
GO  
```  
  
### <a name="b-truncate-table-partitions"></a>B. Tronquer les partitions de table  
  
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658))
  
 L'exemple suivant tronque les partitions spécifiées d'une table partitionnée. La syntaxe `WITH (PARTITIONS (2, 4, 6 TO 8))` provoque la troncation des numéros de partitions 2, 4, 6, 7 et 8.  
  
```sql  
TRUNCATE TABLE PartitionTable1   
WITH (PARTITIONS (2, 4, 6 TO 8));  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [IDENTITY &#40;propriété&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)  
  
  

