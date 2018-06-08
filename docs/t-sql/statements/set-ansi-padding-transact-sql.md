---
title: SET ANSI_PADDING (Transact-SQL) | Microsoft Docs
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
- ANSI_PADDING_TSQL
- ANSI_PADDING
- SET_ANSI_PADDING_TSQL
- SET ANSI_PADDING
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], short values
- ANSI_PADDING option
- short values [SQL Server]
- SET ANSI_PADDING statement
- trailing blanks
ms.assetid: 92bd29a3-9beb-410e-b7e0-7bc1dc1ae6d0
caps.latest.revision: 47
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: c2f958757d1f3afe0f4230c1873f1b1c8e3b22ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-ansipadding-transact-sql"></a>SET ANSI_PADDING (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Contrôle le mode de stockage dans la colonne des valeurs dont la longueur est inférieure à la taille définie pour la colonne et de celles contenant des espaces à droite pour les données de type **char**, **varchar**, **binary**et **varbinary** .  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe
  
```
-- Syntax for SQL Server

SET ANSI_PADDING { ON | OFF }
```

```
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse

SET ANSI_PADDING ON
```

## <a name="remarks"></a>Notes   
 Les colonnes définies avec les types de données **char**, **varchar**, **binaire** et **varbinary** ont une taille définie.  
  
 Cette valeur affecte uniquement la définition de nouvelles colonnes. Une fois la colonne créée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stocke les valeurs en fonction du paramètre en vigueur lors de la création de la colonne. Les colonnes existantes ne sont pas affectées par toute modification ultérieure du paramètre.  
  
> [!NOTE]  
>  Il est recommandé que l'option SET ANSI_PADDING soit toujours activée (ON).  
  
 Le tableau suivant montre les effets de l’option SET ANSI_PADDING quand des valeurs comportant des types de données **char**, **varchar**, **binary** et **varbinary** sont insérées dans des colonnes.  
  
|Paramètre|char(*n*) NOT NULL ou binary(*n*) NOT NULL|char(*n*) NULL ou binary(*n*) NULL|varchar(*n*) ou varbinary(*n*)|  
|-------------|----------------------------------------------------|--------------------------------------------|----------------------------------------|  
|ON|Complète la valeur d’origine (avec des espaces à droite pour les colonnes de type **char** et des zéros à droite pour les colonnes de type **binary**), à concurrence de la longueur de la colonne.|Suit les mêmes règles que pour **char(***n***)** ou **binary(***n***)** NOT NULL quand l’option SET ANSI_PADDING est ON.|Les espaces à droite figurant dans les valeurs de type character insérées dans des colonnes **varchar** ne sont pas tronqués. Les zéros à droite figurant dans les valeurs de type binary insérées dans des colonnes **varbinary** ne sont pas tronqués. Les valeurs ne sont pas complétées à concurrence de la longueur de la colonne.|  
|OFF|Complète la valeur d’origine (avec des espaces à droite pour les colonnes de type **char** et des zéros à droite pour les colonnes de type **binary**), à concurrence de la longueur de la colonne.|Suit les mêmes règles que pour **varchar** ou **varbinary** quand SET ANSI_PADDING est OFF.|Les espaces à droite dans les valeurs de type character insérées dans les colonnes **varchar** sont tronqués. Les zéros à droite dans les valeurs de type binary insérées dans les colonnes **varbinary** sont tronqués.|  
  
> [!NOTE]  
>  Quand les colonnes sont complétées, celles de type **char** le sont avec des espaces, et celles de type **binary** le sont avec des zéros. Quand les colonnes sont tronquées, celles de type **char** perdent les espaces à droite, tandis que celles de type **binary** perdent les zéros à droite.  
  
 SET ANSI_PADDING doit être activée (valeur ON) lors de la création ou de la modification d'index dans des colonnes calculées ou des vues indexées. Pour plus d’informations sur les paramètres de l’option SET obligatoire avec les vues indexées et les index sur des colonnes calculées, consultez « Remarques sur l’utilisation des instructions SET » dans [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).  
  
 La valeur par défaut de SET ANSI_PADDING est ON. Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affectent automatiquement la valeur ON à ANSI_PADDING lors de la connexion. Cette option peut être configurée dans les sources de données et les attributs de connexion ODBC, ou encore dans les propriétés de connexion OLE DB définies dans l'application avant la connexion. Dans le cas d'applications DB-Library, SET ANSI_PADDING prend par défaut la valeur OFF.  
  
 La valeur de SET ANSI_PADDING n’affecte pas les types de données **nchar**, **nvarchar**, **ntext**, **text**, **image**, **varbinary(max)**, **varchar(max)** et **nvarchar(max)**. Le comportement par défaut est toujours celui de l'option SET ANSI_PADDING ON. Les espaces et les zéros à droite ne sont donc pas tronqués.  
  
 Lorsque SET ANSI_DEFAULTS a la valeur ON, l'option SET ANSI_PADDING est activée.  
  
 La valeur de SET ANSI_PADDING est définie lors de l'exécution, et non durant l'analyse.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.  
  
```  
DECLARE @ANSI_PADDING VARCHAR(3) = 'OFF';  
IF ( (16 & @@OPTIONS) = 16 ) SET @ANSI_PADDING = 'ON';  
SELECT @ANSI_PADDING AS ANSI_PADDING;  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant présente comment l'option affecte les différents types de données.  
  
```  
PRINT 'Testing with ANSI_PADDING ON'  
SET ANSI_PADDING ON;  
GO  
  
CREATE TABLE t1 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t1 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t1 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '\<',  
   varbinarycol  
FROM t1;  
GO  
  
PRINT 'Testing with ANSI_PADDING OFF';  
SET ANSI_PADDING OFF;  
GO  
  
CREATE TABLE t2 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t2 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t2 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '<',  
   varbinarycol  
FROM t2;  
GO  
  
DROP TABLE t1;  
DROP TABLE t2;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)  
  
  
