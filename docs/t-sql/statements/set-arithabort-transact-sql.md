---
title: SET ARITHABORT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ARITHABORT_TSQL
- ARITHABORT
- SET ARITHABORT
- SET_ARITHABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- terminating queries
- queries [SQL Server], terminating
- overflow errors [SQL Server]
- ARITHABORT option
- divide-by-zero errors
- SET ARITHABORT statement
- ending queries [SQL Server]
- stopping queries
ms.assetid: f938a666-fdd1-4233-b97f-719f27b1a0e6
caps.latest.revision: 46
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 407048b20d382eb1b12606ec998944dbd7f0e387
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-arithabort-transact-sql"></a>SET ARITHABORT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Arrête une requête lorsqu'un dépassement de capacité ou une division par zéro se produit durant son exécution.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```
-- Syntax for SQL Server and Azure SQL Database
  
SET ARITHABORT { ON | OFF }
```

```
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse

SET ARITHABORT ON
```
  
## <a name="remarks"></a>Notes   
 ARITHABORT doit toujours avoir la valeur ON dans vos sessions de connexion. L’affectation de la valeur OFF à ARITHABORT peut impacter l’optimisation des requêtes et entraîner des problèmes de performances.  
  
> [!WARNING]  
>  Le paramètre ARITHABORT par défaut pour [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est ON. Les applications clientes qui configurent ARITHABORT sur OFF peuvent recevoir des plans de requête différents, ce qui complique la résolution des celles dont les performances sont médiocres. Autrement dit, la même requête peut s'exécuter rapidement dans Management Studio, mais lentement dans l'application. Pour résoudre les requêtes avec [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] utilisez toujours le paramètre ARITHABORT du client.  
  
 Si les options SET ARITHABORT et SET ANSI WARNINGS sont activées (ON), ces conditions d'erreur provoquent l'arrêt de la requête.  
  
 Si l'option SET ARITHABORT est activée (ON) et l'option SET ANSI WARNINGS désactivée (OFF), ces conditions d'erreur provoquent l'arrêt du traitement. Si l'erreur se produit dans une transaction, cette dernière est annulée. Si SET ARITHABORT est défini à OFF et que l'une de ces erreurs se produit, un message d'avertissement s'affiche, et la valeur NULL est attribuée au résultat de l'opération arithmétique.  
  
 Si SET ARITHABORT et SET ANSI WARNINGS sont définis sur OFF et que l'une de ces erreurs se produit, un message d'avertissement s'affiche, et la valeur NULL est attribuée au résultat de l'opération arithmétique.  
  
> [!NOTE]  
>  Si ni SET ARITHABORT ni SET ARITHIGNORE ne sont définies, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie la valeur NULL avec un message d'avertissement après l'exécution de la requête.  
  
 L'affectation de la valeur ON à ANSI_WARNINGS affecte de manière implicite la valeur ON à ARITHABORT, lorsque le niveau de compatibilité de la base de données est d'au moins 90. Si le niveau de compatibilité de base de données est défini à 80 ou moins, la valeur ON doit être affectée de manière explicite à l'option ARITHABORT.  
  
 Si, au cours de l'évaluation de l'expression, une instruction INSERT, DELETE ou UPDATE rencontre une erreur arithmétique, un dépassement de capacité, une division par zéro ou une erreur de domaine alors que l'option SET ARITHABORT est définie à OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] insère ou met à jour une valeur NULL. Si la colonne cible ne peut pas prendre la valeur NULL, l'action d'insertion ou de mise à jour échoue et l'utilisateur reçoit une erreur.  
  
 Si la valeur de SET ARITHABORT ou de SET ARITHIGNORE est définie à OFF et que SET ANSI_WARNINGS a la valeur ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie malgré tout un message d'erreur quand il rencontre une erreur de division par zéro ou de dépassement de capacité.  
  
 Si SET ARITHABORT est défini à OFF et qu’une erreur d’abandon se produit pendant l’évaluation de la condition booléenne d’une instruction IF, la branche FALSE est exécutée.
  
 L'option SET ARITHABORT doit être activée (valeur ON) lorsque vous créez ou que vous modifiez des index dans des colonnes calculées ou des vues indexées. Si SET ARITHABORT est désactivée (OFF), les instructions CREATE, UPDATE, INSERT et DELETE appliquées à des tables comportant des index sur des colonnes calculées ou des vues indexées échouent.
  
 L'option SET ARITHABORT est appliquée lors de l'exécution, et non pas lors de l'analyse.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante :
  
```  
DECLARE @ARITHABORT VARCHAR(3) = 'OFF';  
IF ( (64 & @@OPTIONS) = 64 ) SET @ARITHABORT = 'ON';  
SELECT @ARITHABORT AS ARITHABORT;  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre des erreurs de dépassement et de division par zéro, avec l'option `SET ARITHABORT` tour à tour activée et désactivée.  
  
```  
-- SET ARITHABORT  
-------------------------------------------------------------------------------  
-- Create tables t1 and t2 and insert data values.  
CREATE TABLE t1 (  
   a TINYINT,   
   b TINYINT  
);  
CREATE TABLE t2 (  
   a TINYINT  
);  
GO  
INSERT INTO t1   
VALUES (1, 0);  
INSERT INTO t1   
VALUES (255, 1);  
GO  
  
PRINT '*** SET ARITHABORT ON';  
GO  
-- SET ARITHABORT ON and testing.  
SET ARITHABORT ON;  
GO  
  
PRINT '*** Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab   
FROM t1;  
GO  
  
PRINT '*** Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
  
PRINT '*** Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '*** Resulting data - should be no data';  
GO  
SELECT *   
FROM t2;  
GO  
  
-- Truncate table t2.  
TRUNCATE TABLE t2;  
GO  
  
-- SET ARITHABORT OFF and testing.  
PRINT '*** SET ARITHABORT OFF';  
GO  
SET ARITHABORT OFF;  
GO  
  
-- This works properly.  
PRINT '*** Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab    
FROM t1;  
GO  
  
-- This works as if SET ARITHABORT was ON.  
PRINT '*** Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
PRINT '*** Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '*** Resulting data - should be 0 rows';  
GO  
SELECT *   
FROM t2;  
GO  
  
-- Drop tables t1 and t2.  
DROP TABLE t1;  
DROP TABLE t2;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ARITHIGNORE &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithignore-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
  
