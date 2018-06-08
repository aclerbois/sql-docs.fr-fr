---
title: TRIM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- TRIM
- TRIM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- TRIM function
ms.assetid: a00245aa-32c7-4ad4-a0d1-64f3d6841153
caps.latest.revision: 4
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2017 || = sqlallproducts-allversions
ms.openlocfilehash: 94808d98c9782d60247e867cc3b65b9956b9e104
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="trim-transact-sql"></a>TRIM (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

Supprime le caractère espace `char(32)` ou d’autres caractères spécifiés au début ou à la fin d’une chaîne.  
 
## <a name="syntax"></a>Syntaxe   
```
TRIM ( [ characters FROM ] string ) 
```
[//]: # "[ BOTH | LEADING | TRAILING ] pas encore disponible."

## <a name="arguments"></a>Arguments   

caractères   
Littéral, variable ou appel de fonction de n’importe quel type de caractère non LOB (`nvarchar`, `varchar`, `nchar` ou `char`) contenant des caractères à supprimer. Les types `nvarchar(max)` et `varchar(max)` ne sont pas autorisés.

chaîne   
Expression de n’importe quel type de caractère (`nvarchar`, `varchar`, `nchar` ou `char`) dans laquelle des caractères doivent être supprimés.

## <a name="return-types"></a>Types de retour   
Renvoie une expression de caractères avec un type d’argument de chaîne dans laquelle le caractère espace `char(32)` ou d’autres caractères spécifiés sont supprimés des deux côtés. Renvoie `NULL` si la chaîne d’entrée est `NULL`.

## <a name="remarks"></a>Notes    
Par défaut, la fonction `TRIM` supprime le caractère espace `char(32)` des deux côtés. Ceci équivaut à `LTRIM(RTRIM(@string))`. Le comportement de la fonction `TRIM ` avec des caractères spécifiés est identique au comportement de la fonction `REPLACE` quand les caractères de début ou de fin sont remplacés par des chaînes vides.


## <a name="examples"></a>Exemples
### <a name="a--removes-the-space-character-from-both-sides-of-string"></a>A.  Supprimer le caractère espace des deux côtés de la chaîne   
L’exemple suivant supprime les espaces avant et après le mot `test`.   
```sql
SELECT TRIM( '     test    ') AS Result;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   

`test`


### <a name="b--removes-specified-characters-from-both-sides-of-string"></a>B.  Supprimer les caractères spécifiés des deux côtés de la chaîne   
L’exemple suivant supprime un point final et les espaces à droite.
```sql
SELECT TRIM( '.,! ' FROM  '#     test    .') AS Result;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   
`#     test`


## <a name="see-also"></a> Voir aussi
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
