---
title: TRY_CONVERT (Transact-SQL) | Microsoft Docs
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
- TRY_CONVERT_TSQL
- TRY_CONVERT
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CONVERT function
ms.assetid: 3e6e7825-6482-4cb2-a8c2-9abc99e265a6
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6885d980636261843f99702c3e22f7584f9e71d6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="tryconvert-transact-sql"></a>TRY_CONVERT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  Retourne une valeur convertie en type de données spécifié si la conversion aboutit ; sinon, retourne NULL.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
TRY_CONVERT ( data_type [ ( length ) ], expression [, style ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *data_type [ ( length ) ]*  
 Type de données vers lequel effectuer le transtypage d’*expression*.  
  
 *expression*  
 Valeur à caster.  
  
 *style*  
 Expression entière facultative qui spécifie comment la fonction **TRY_CONVERT** doit traduire *expression*.  
  
 *style* accepte les mêmes valeurs que le paramètre *style* de la fonction **CONVERT**. Pour plus d’informations, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 La plage des valeurs acceptables est déterminée par la valeur de *data_type*. Si *style* est NULL, alors **TRY_CONVERT** retourne la valeur NULL.  
  
## <a name="return-types"></a>Types de retour  
 Retourne une valeur convertie en type de données spécifié si la conversion aboutit ; sinon, retourne NULL.  
  
## <a name="remarks"></a>Notes   
 **TRY_CONVERT** prend la valeur qui lui est transmise et tente de la convertir vers le type *data_type* spécifié. Si le transtypage réussit, **TRY_CONVERT** retourne la valeur dans le type *data_type* spécifié ; si une erreur se produit, la valeur NULL est retournée. Toutefois, si vous demandez une conversion explicitement non autorisée, **TRY_CONVERT** échoue avec une erreur.  
  
 **TRY_CONVERT** est un mot clé réservé avec le niveau de compatibilité 110 et supérieur.  
  
 Cette fonction peut être exécutée à distance sur des serveurs [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou version ultérieure. Elle ne peut pas être exécutée à distance sur des serveurs dont la version est antérieure à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-tryconvert-returns-null"></a>A. TRY_CONVERT retourne la valeur NULL.  
 L'exemple suivant montre que TRY_CONVERT retourne Null lorsque la conversion échoue.  
  
```sql  
SELECT   
    CASE WHEN TRY_CONVERT(float, 'test') IS NULL   
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
SELECT TRY_CONVERT(datetime2, '12/31/2010') AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-tryconvert-fails-with-an-error"></a>B. TRY_CONVERT échoue avec une erreur  
 L'exemple suivant montre que TRY_CONVERT retourne une erreur lorsque la conversion n'est pas autorisée explicitement.  
  
```sql  
SELECT TRY_CONVERT(xml, 4) AS Result;  
GO  
```  
  
 Le résultat de cette instruction est une erreur, car un entier ne peut pas être converti en un type de données XML.  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-tryconvert-succeeds"></a>C. TRY_CONVERT réussit  
 Cet exemple suivant montre que l'expression doit être au format attendu.  
  
```  
SET DATEFORMAT mdy;  
SELECT TRY_CONVERT(datetime2, '12/31/2010') AS Result;  
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
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
