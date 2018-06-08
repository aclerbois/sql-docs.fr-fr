---
title: REPLACE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- REPLACE_TSQL
- REPLACE
dev_langs:
- TSQL
helpviewer_keywords:
- first string expression [SQL Server]
- replacing string expression
- third string expressions [SQL Server]
- second string expressions [SQL Server]
- REPLACE function
ms.assetid: 8a7aaaf2-62e3-46c0-8e44-fa22290dd86b
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b7d10c571ec359991ea53d46fae161abae641c52
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replace-transact-sql"></a>REPLACE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Remplace toutes les occurrences d'une valeur de type chaîne spécifiée par une autre valeur de type chaîne.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
REPLACE ( string_expression , string_pattern , string_replacement )  
```  
  
## <a name="arguments"></a>Arguments  
 *string_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de chaîne à rechercher. *string_expression* peut être de type de données binaire ou caractère.  
  
 *string_* pattern  
 Sous-chaîne à rechercher. *string_pattern* peut être de type de données binaire ou caractère. *string_pattern* ne peut pas être une chaîne vide ('') et ne doit pas dépasser le nombre maximal d’octets tenant sur une page.  
  
 *string_* replacement  
 Chaîne de remplacement. *string_replacement* peut être de type de données binaire ou caractère.  
  
## <a name="return-types"></a>Types de retour  
 Retourne **nvarchar** si l’un des arguments d’entrée est de type de données **nvarchar** ; dans le cas contraire, REPLACE retourne **varchar**.  
  
 Retourne NULL si n'importe lequel des arguments a pour valeur NULL.  
  
 Si *string_expression* n’est pas de type **varchar(max)** ou **nvarchar(max), REPLACE** tronque la valeur de retour à 8 000 octets. Pour retourner des valeurs supérieures à 8 000 octets, il est nécessaire d’effectuer explicitement le transtypage de *string_expression* vers un type de données de valeur de grande taille.  
  
## <a name="remarks"></a>Notes   
 REPLACE effectue des comparaisons basées sur le classement de l'entrée. Pour effectuer une comparaison dans un classement donné, vous pouvez utiliser [COLLATE](~/t-sql/statements/collations.md) pour appliquer à l’entrée un classement explicite.  
  
 0x0000 (**char(0)**) est un caractère non défini dans les classements Windows et ne peut pas être inclus dans REPLACE.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant remplace la chaîne `cde` dans `abcdefghi` par `xxx`.  
  
```sql  
SELECT REPLACE('abcdefghicde','cde','xxx');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
abxxxfghixxx  
(1 row(s) affected)  
```  
  
 L'exemple suivant utilise la fonction `COLLATE`.  
  
```sql  
SELECT REPLACE('This is a Test'  COLLATE Latin1_General_BIN,  
'Test', 'desk' );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
This is a desk  
(1 row(s) affected)  
```  

  
## <a name="see-also"></a> Voir aussi  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
