---
title: STUFF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- STUFF
- STUFF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting characters
- STUFF function
- length characters
- removing characters
- replacing characters
- characters [SQL Server], replacing
- inserting data
ms.assetid: abb0afa9-44f6-42a2-a871-5f471dfb222b
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4639fdd6044292bc2c3f34fac5d916f3183967b2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stuff-transact-sql"></a>STUFF (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  La fonction STUFF permet d'insérer une chaîne dans une autre chaîne. Elle efface d'abord le nombre de caractères spécifié dans la première chaîne à partir de la position de début. Ensuite, elle insère la seconde chaîne dans la première à partir de la position de début.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
STUFF ( character_expression , start , length , replaceWith_expression )  
```  
  
## <a name="arguments"></a>Arguments  
 *expression_caractère*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de données caractères. *character_expression* peut être une constante, une variable ou une colonne de données de type caractère ou binaire.  
  
 *start*  
 Entier précisant la position de départ de la suppression et de l'insertion. Si *start* est négatif ou nul, une chaîne NULL est renvoyée. Si *start* est plus long que le premier argument *character_expression*, une chaîne NULL est renvoyée. *start* peut être de type **bigint**.  
  
 *length*  
 Entier spécifiant le nombre de caractères à supprimer. Si *length* est négatif, une chaîne NULL est renvoyée. Si *length* est plus long que le premier argument *character_expression*, la suppression s’effectue jusqu’au dernier caractère du dernier argument *character_expression*.  Si *length* est égal à zéro, l’insertion s’effectue avant le premier caractère dans la chaîne. *length* peut être de type **bigint**.

 *replaceWith_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de données caractères. *character_expression* peut être une constante, une variable ou une colonne de données de type caractère ou binaire. Cette expression remplace *length* caractères de *character_expression* en commençant à *start*. Le fait de fournir `NULL` comme argument *replaceWith_expression* supprime les caractères sans rien insérer.   
  
## <a name="return-types"></a>Types de retour  
 Renvoie des données caractères si *character_expression* correspond à l’un des types de données caractères pris en charge. Renvoie des données binaires si *character_expression* correspond à l’un des types de données binaires pris en charge.  
  
## <a name="remarks"></a>Notes   
 Si la valeur de la position de début ou de la longueur est négative, ou bien si la valeur de la position de début est supérieure à la longueur de la première chaîne, une chaîne NULL est retournée. Si la position de départ est 0, une valeur NULL est retournée. Si la longueur à effacer est supérieure à celle de la première chaîne, tous les caractères de la première chaîne sont effacés à partir du caractère du début.  

Une erreur se produit si la valeur résultante est plus grande que le maximum pris en charge par le type retourné.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Lors de l’utilisation de classements SC, les deux arguments *character_expression* et *replaceWith_expression* peuvent inclure des paires de substitution. Le paramètre length compte chaque substitut dans *character_expression* comme un caractère unique.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, la procédure retourne une chaîne de caractères créée en supprimant trois caractères de la première chaîne (`abcdef`) à partir de la position `2` (c'est-à-dire au niveau du `b`) et en insérant la seconde chaîne au point de suppression.  
  
```  
SELECT STUFF('abcdef', 2, 3, 'ijklmn');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
aijklmnef   
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
