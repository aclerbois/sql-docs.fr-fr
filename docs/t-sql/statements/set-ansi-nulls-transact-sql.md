---
title: SET ANSI_NULLS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SET_ANSI_NULLS_TSQL
- ANSI_NULLS
- SET ANSI_NULLS
- ANSI_NULLS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SET ANSI_NULLS statement
- not equal to operator (<>)
- ANSI_NULLS option
- equals operator (=)
- null values [SQL Server], comparison operators
- comparison operators [SQL Server], null values
ms.assetid: aae263ef-a3c7-4dae-80c2-cc901e48c755
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 1e1a05133d905e6211cded5afc46dba8db75757f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "33073086"
---
# <a name="set-ansinulls-transact-sql"></a>SET ANSI_NULLS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Spécifie le comportement, compatible avec ISO, des opérateurs de comparaison Égal à (=) et Différent de (<>), lorsqu'ils sont utilisés avec des valeurs Null dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
> [!IMPORTANT]  
>  Dans une future ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS sera ON et toute application qui définira explicitement l’option à OFF générera une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntaxe

```
-- Syntax for SQL Server

SET ANSI_NULLS { ON | OFF }
```

```
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse

SET ANSI_NULLS ON
```

## <a name="remarks"></a>Notes   
 Quand SET ANSI_NULLS a la valeur ON, une instruction SELECT qui utilise la clause WHERE *nom_colonne* = **NULL** ne retourne aucune ligne, même si *nom_colonne* comporte des valeurs Null.  Une instruction SELECT qui utilise la clause WHERE *nom_colonne* <> **NULL** ne retourne aucune ligne, même si *nom_colonne* comporte des valeurs non Null.   
  
 Lorsque la valeur de SET ANSI_NULLS a la valeur OFF, les opérateurs de comparaison Égal à (=) et Différent de (<>) ne sont pas conformes à la norme ISO. Une instruction SELECT qui utilise la clause WHERE *nom_colonne* = **NULL** retourne les lignes qui ont des valeurs Null dans *nom_colonne*.  Une instruction SELECT qui utilise la clause WHERE *nom_colonne* <> **NULL** retourne les lignes qui ont des valeurs non Null dans la colonne. De plus, une instruction SELECT utilisant WHERE *nom_colonne* <> *valeur_XYZ* retourne toutes les lignes qui ne contiennent ni *valeur_XYZ*, ni la valeur Null.   
  
 Lorsque la valeur de SET ANSI_NULLS a la valeur ON, toutes les comparaisons effectuées sur une valeur Null retournent la valeur UNKNOWN (inconnu). Lorsque la valeur de SET ANSI_NULLS a la valeur OFF, les comparaisons de toutes les données par rapport à une valeur Null sont vraies (TRUE) si la valeur des données est Null. Si SET ANSI_NULLS n'est pas spécifié, le paramètre de l'option ANSI_NULLS de la base de données actuelle s'applique. Pour plus d’informations sur l’option de base de données ANSI_NULLS, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 SET ANSI_NULLS ON n'a d'influence sur une comparaison que si l'un des opérandes de la comparaison est une variable Null ou une valeur Null littérale. Si les deux termes de la comparaison sont des colonnes ou des expressions composées, le paramètre n'a pas d'incidence sur la comparaison.  
  
 Pour qu'un script s'exécute correctement, quelle que soit la valeur de l'option de base de données ANSI_NULLS ou du paramètre de SET ANSI_NULLS, utilisez IS NULL et IS NOT NULL dans des comparaisons susceptibles de contenir des valeurs Null.  
  
 Lors de l'exécution de requêtes distribuées, l'option SET ANSI_NULLS doit être activée (ON).  
  
 SET ANSI_NULLS doit également avoir la valeur ON lors de la création ou de la modification d'index dans des colonnes calculées ou des vues indexées. Si SET ANSI_NULLS est désactivée (OFF), les instructions CREATE, UPDATE, INSERT et DELETE appliquées à des tables comportant des index sur des colonnes calculées ou des vues indexées échouent. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur qui répertorie toutes les options SET non conformes aux valeurs requises. En outre, lors de l’exécution d’une instruction SELECT, si SET ANSI_NULLS a la valeur OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignore les valeurs d’index dans les vues ou les colonnes calculées et résout l’opération select comme si ces index n’existaient pas.  
  
> [!NOTE]  
>  ANSI_NULLS est l'une des sept options SET qui doivent avoir les valeurs requises lors du traitement des index dans des colonnes calculées et des vues indexées. Les options ANSI_PADDING, ANSI_WARNINGS, ARITHABORT, QUOTED_IDENTIFIER et CONCAT_NULL_YIELDS_NULL doivent également avoir la valeur ON et NUMERIC_ROUNDABORT la valeur OFF.  
  
 Le pilote ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et le fournisseur OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attribuent automatiquement la valeur ON à ANSI_NULLS lors de la connexion. Cette valeur peut être configurée dans les sources de données ou les attributs de connexion ODBC, ainsi que dans les propriétés de connexion OLE DB définies dans l'application avant la connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur par défaut de SET ANSI_NULLS est OFF.  
  
 Lorsque SET ANSI_DEFAULTS a la valeur ON, l'option SET ANSI_NULLS est activée.  
  
 La valeur de SET ANSI_NULLS est définie lors de l'exécution, et non durant l'analyse.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante :
  
```  
DECLARE @ANSI_NULLS VARCHAR(3) = 'OFF';  
IF ( (32 & @@OPTIONS) = 32 ) SET @ANSI_NULLS = 'ON';  
SELECT @ANSI_NULLS AS ANSI_NULLS;  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous utilise les opérateurs de comparaison Égal à (`=`) et Différent de (`<>`) pour effectuer une comparaison avec des valeurs `NULL` et non NULL dans une table. Il montre également que l’option `IS NULL` n’est pas influencée par le paramètre `SET ANSI_NULLS`.  
  
```  
-- Create table t1 and insert values.  
CREATE TABLE dbo.t1 (a INT NULL);  
INSERT INTO dbo.t1 values (NULL),(0),(1);  
GO  
  
-- Print message and perform SELECT statements.  
PRINT 'Testing default setting';  
DECLARE @varname int;   
SET @varname = NULL;  
  
SELECT a  
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- SET ANSI_NULLS to ON and test.  
PRINT 'Testing ANSI_NULLS ON';  
SET ANSI_NULLS ON;  
GO  
DECLARE @varname int;  
SET @varname = NULL  
  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- SET ANSI_NULLS to OFF and test.  
PRINT 'Testing SET ANSI_NULLS OFF';  
SET ANSI_NULLS OFF;  
GO  
DECLARE @varname int;  
SET @varname = NULL;  
SELECT a   
FROM t1   
WHERE a = @varname;  
  
SELECT a   
FROM t1   
WHERE a <> @varname;  
  
SELECT a   
FROM t1   
WHERE a IS NULL;  
GO  
  
-- Drop table t1.  
DROP TABLE dbo.t1;  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [= &#40;Equals&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/equals-transact-sql.md)   
 [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)   
 [&#60;&#62; &#40;Not Equal To&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  
  
  
