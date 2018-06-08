---
title: SET QUOTED_IDENTIFIER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/03/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- QUOTED_IDENTIFIER_TSQL
- SET_QUOTED_IDENTIFIER_TSQL
- SET QUOTED_IDENTIFIER
- QUOTED_IDENTIFIER
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- identifiers [SQL Server], delimited
- QUOTED_IDENTIFIER option
- quoted identifiers
- ISO delimited identifiers rules
- SET QUOTED_IDENTIFIER statement
ms.assetid: 10f66b71-9241-4a3a-9292-455ae7252565
caps.latest.revision: 48
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b78a2ba8a3d9520376746da1810c4a2d64324781
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-quotedidentifier-transact-sql"></a>SET QUOTED_IDENTIFIER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Force [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à suivre les règles ISO se rapportant aux guillemets qui délimitent les identificateurs et les chaînes littérales. Les identificateurs entre guillemets doubles peuvent être des mots clés réservés [!INCLUDE[tsql](../../includes/tsql-md.md)] ou ils peuvent contenir des caractères généralement interdits dans les conventions de syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] concernant les identificateurs.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
SET QUOTED_IDENTIFIER { ON | OFF }  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
SET QUOTED_IDENTIFIER ON   
```  
  
## <a name="remarks"></a>Notes   
 Si SET QUOTED_IDENTIFIER a la valeur ON, les identificateurs peuvent être délimités par des guillemets doubles et les valeurs littérales doivent être délimitées par des guillemets simples. Lorsque SET QUOTED_IDENTIFIER a la valeur OFF, les identificateurs ne peuvent pas apparaître entre guillemets et doivent respecter les conventions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md). Les chaînes littérales peuvent être délimitées par des guillemets simples ou doubles.  
  
 Si SET QUOTED_IDENTIFIER a la valeur par défaut ON, toutes les chaînes délimitées par des guillemets doubles sont interprétées comme des identificateurs d'objets. Par conséquent, les identificateurs entre guillemets n'ont pas à respecter les règles [!INCLUDE[tsql](../../includes/tsql-md.md)] applicables aux identificateurs. Ils peuvent être des mots clés réservés et contenir des caractères généralement interdits dans les identificateurs [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les guillemets doubles ne peuvent pas servir à délimiter des expressions littérales ; seuls des guillemets simples peuvent encadrer des chaînes littérales. Si un guillemet simple (**'**) fait partie d’une chaîne littérale, il peut être représenté par deux guillemets simples accolés (**"**). SET QUOTED_IDENTIFIER doit avoir la valeur ON si des mots clés réservés sont utilisés pour des noms d'objets dans la base de données.  
  
 Si SET QUOTED_IDENTIFIER a la valeur OFF, les chaînes littérales figurant dans les expressions peuvent être délimitées par des guillemets simples ou doubles. Si une chaîne littérale est délimitée par des guillemets doubles, des apostrophes (guillemets simples) peuvent y être imbriquées.  
  
 SET QUOTED_IDENTIFIER doit avoir la valeur ON lors de la création ou de la modification d'index dans des colonnes calculées ou des vues indexées. Si SET QUOTED_IDENTIFIER a la valeur OFF, les instructions CREATE, UPDATE, INSERT et DELETE dans des tables comportant des index sur des colonnes calculées ou des vues indexées échouent. Pour plus d’informations sur les paramètres de l’option SET obligatoire avec les vues indexées et les index sur des colonnes calculées, consultez « Remarques sur l’utilisation des instructions SET » dans [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).  
  
 SET QUOTED_IDENTIFIER doit avoir la valeur ON lorsque vous créez un index filtré.  
  
 SET QUOTED_IDENTIFIER doit avoir la valeur ON lorsque vous appelez les méthodes de type de données XML.  
  
 Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribuent automatiquement la valeur ON à QUOTED_IDENTIFIER lors de la connexion. Cette option peut être configurée dans les sources de données et les attributs de connexion ODBC, ainsi que dans les propriétés de connexion OLE DB. Dans le cas d'une connexion depuis une application DB-Library, SET QUOTED_IDENTIFIER a la valeur OFF par défaut.  
  
 Lors de la création d'une table, l'option QUOTED IDENTIFIER est toujours stockée avec la valeur ON dans les métadonnées de la table, même si elle a été affectée de la valeur OFF au moment de sa création.  
  
 Lors de la création d'une procédure stockée, les options de SET QUOTED_IDENTIFIER et SET ANSI_NULLS sont capturées puis réutilisées lors d'appels ultérieurs de cette procédure.  
  
 La valeur de SET QUOTED_IDENTIFIER ne change pas lorsqu'elle est exécutée dans une procédure stockée.  
  
 Lorsque SET ANSI_DEFAULTS a la valeur ON, l'option SET QUOTED_IDENTIFIER est activée.  
  
 SET QUOTED_IDENTIFIER correspond aussi à l'option QUOTED_IDENTIFIER de ALTER DATABASE. Pour plus d’informations sur les paramètres de base de données, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 SET QUOTED_IDENTIFIER prend effet au moment de l’analyse. Il affecte uniquement l’analyse, et non l’exécution des requêtes.  
  
 Pour un lot ad hoc de niveau supérieur, l’analyse commence à utiliser le paramètre actuel de la session défini pour QUOTED_IDENTIFIER.  Lors de l’analyse du lot, toute occurrence de SET QUOTED_IDENTIFIER change le comportement de l’analyse à partir de ce point, et enregistre ce paramètre pour la session.  Par conséquent, une fois le lot analysé et exécuté, le paramètre QUOTED_IDENTIFER de la session est défini en fonction de la dernière occurrence de SET QUOTED_IDENTIFIER dans le lot.  
 Le code SQL statique d’une procédure stockée est analysée à l’aide de l’option QUOTED_IDENTIFIER en vigueur pour le lot qui a créé ou modifié la procédure stockée.  SET QUOTED_IDENTIFIER n’a aucun effet quand il apparaît dans le corps d’une procédure stockée sous forme de code SQL statique.  
  
 Pour un lot imbriqué à l’aide de sp_executesql ou exec(), l’analyse commence en utilisant l’option QUOTED_IDENTIFIER de la session.  Si le lot imbriqué se trouve à l’intérieur d’une procédure stockée, l’analyse commence en utilisant l’option QUOTED_IDENTIFIER de la procédure stockée.  Lors de l’analyse du lot imbriqué, toute occurrence de SET QUOTED_IDENTIFIER change le comportement de l’analyse à partir de ce point, mais le paramètre QUOTED_IDENTIFIER de la session n’est pas mis à jour.  
  
 L’utilisation de crochets, **[** et **]**, pour délimiter des identificateurs n’est pas affectée par le paramètre QUOTED_IDENTIFIER.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.  
  
```  
DECLARE @QUOTED_IDENTIFIER VARCHAR(3) = 'OFF';  
IF ( (256 & @@OPTIONS) = 256 ) SET @QUOTED_IDENTIFIER = 'ON';  
SELECT @QUOTED_IDENTIFIER AS QUOTED_IDENTIFIER;  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-quoted-identifier-setting-and-reserved-word-object-names"></a>A. Utilisation de l'option QUOTED IDENTIFIER et de noms d'objets avec des mots réservés  
 L'exemple suivant montre que l'option `SET QUOTED_IDENTIFIER` doit être `ON`, et les mots clés figurant dans les noms de tables doivent être placés entre guillemets doubles pour pouvoir créer et utiliser des objets dont les noms sont des mots clés réservés.  
  
```  
SET QUOTED_IDENTIFIER OFF  
GO  
-- An attempt to create a table with a reserved keyword as a name  
-- should fail.  
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);  
GO  
  
SET QUOTED_IDENTIFIER ON;  
GO  
  
-- Will succeed.  
CREATE TABLE "select" ("identity" INT IDENTITY NOT NULL, "order" INT NOT NULL);  
GO  
  
SELECT "identity","order"   
FROM "select"  
ORDER BY "order";  
GO  
  
DROP TABLE "SELECT";  
GO  
  
SET QUOTED_IDENTIFIER OFF;  
GO  
```  
  
### <a name="b-using-the-quoted-identifier-setting-with-single-and-double-quotation-marks"></a>B. Utilisation de QUOTED IDENTIFIER avec des guillemets simples et doubles  
 L'exemple suivant montre comment utiliser les guillemets simples et doubles dans des chaînes, lorsque `SET QUOTED_IDENTIFIER` a la valeur `ON` et `OFF`.  
  
```  
SET QUOTED_IDENTIFIER OFF;  
GO  
USE AdventureWorks2012;  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'Test')  
   DROP TABLE dbo.Test;  
GO  
USE AdventureWorks2012;  
CREATE TABLE dbo.Test (ID INT, String VARCHAR(30)) ;  
GO  
  
-- Literal strings can be in single or double quotation marks.  
INSERT INTO dbo.Test VALUES (1, "'Text in single quotes'");  
INSERT INTO dbo.Test VALUES (2, '''Text in single quotes''');  
INSERT INTO dbo.Test VALUES (3, 'Text with 2 '''' single quotes');  
INSERT INTO dbo.Test VALUES (4, '"Text in double quotes"');  
INSERT INTO dbo.Test VALUES (5, """Text in double quotes""");  
INSERT INTO dbo.Test VALUES (6, "Text with 2 """" double quotes");  
GO  
  
SET QUOTED_IDENTIFIER ON;  
GO  
  
-- Strings inside double quotation marks are now treated   
-- as object names, so they cannot be used for literals.  
INSERT INTO dbo."Test" VALUES (7, 'Text with a single '' quote');  
GO  
  
-- Object identifiers do not have to be in double quotation marks  
-- if they are not reserved keywords.  
SELECT ID, String   
FROM dbo.Test;  
GO  
  
DROP TABLE dbo.Test;  
GO  
  
SET QUOTED_IDENTIFIER OFF;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ID          String 
 ----------- ------------------------------ 
 1           'Text in single quotes' 
 2           'Text in single quotes' 
 3           Text with 2 '' single quotes 
 4           "Text in double quotes" 
 5           "Text in double quotes" 
 6           Text with 2 "" double quotes 
 7           Text with a single ' quote
 ```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)  
  
  
