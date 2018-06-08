---
title: TRY_CAST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- TRY_CAST_TSQL
- TRY_CAST
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CAST function
ms.assetid: ea3a16de-995b-415c-b5f0-9355cf7bb401
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: f5e5ab081302ddb43f3e0bfd8084bd843dcb4965
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="trycast-transact-sql"></a>TRY_CAST (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Retourne une valeur convertie en type de données spécifié si la conversion aboutit ; sinon, retourne NULL.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
TRY_CAST ( expression AS data_type [ ( length ) ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Valeur à caster. Toute expression valide.  
  
 *data_type*  
 Type de données vers lequel effectuer le transtypage d’*expression*.  
  
 *length*  
 Entier facultatif qui spécifie la longueur du type de données cible.  
  
 La plage des valeurs acceptables est déterminée par la valeur de *data_type*.  
  
## <a name="return-types"></a>Types de retour  
 Retourne une valeur convertie en type de données spécifié si la conversion aboutit ; sinon, retourne NULL.  
  
## <a name="remarks"></a>Notes   
 **TRY_CAST** prend la valeur qui lui est transmise et tente de la convertir vers le type *data_type* spécifié. Si le transtypage réussit, **TRY_CAST** renvoie la valeur dans le type *data_type* spécifié ; si une erreur se produit, la valeur NULL est renvoyée. Toutefois, si vous demandez une conversion qui n’est pas autorisée explicitement, **TRY_CAST** échoue avec une erreur.  
  
 **TRY_CAST** n’est pas un nouveau mot clé réservé et est disponible à tous les niveaux de compatibilité. **TRY_CAST** a la même sémantique que **TRY_CONVERT** lors de la connexion à des serveurs distants.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-trycast-returns-null"></a>A. TRY_CAST retourne une valeur Null  
 L'exemple suivant montre que TRY_CAST retourne Null lorsque la conversion échoue.  
  
```sql  
SELECT   
    CASE WHEN TRY_CAST('test' AS float) IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------------  
Cast failed  
  
(1 row(s) affected)  
```  
  
 L'exemple suivant montre que l'expression doit être au format attendu.  
  
```sql  
SET DATEFORMAT dmy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-trycast-fails-with-an-error"></a>B. TRY_CAST échoue avec une erreur  
 L'exemple suivant montre que TRY_CAST retourne une erreur lorsque la conversion n'est pas autorisée explicitement.  
  
```sql  
SELECT TRY_CAST(4 AS xml) AS Result;  
GO  
```  
  
 Le résultat de cette instruction est une erreur, car un entier ne peut pas être converti en un type de données XML.  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-trycast-succeeds"></a>C. TRY-CAST réussit  
 Cet exemple suivant montre que l'expression doit être au format attendu.  
  
```  
SET DATEFORMAT mdy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------------------  
2010-12-31 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [TRY_CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/try-convert-transact-sql.md)   
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
