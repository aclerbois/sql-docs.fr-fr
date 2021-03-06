---
title: ABS (Transact-SQL) | Microsoft Docs
ms.custom: 
ms.date: 07/24/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: 
ms.component: t-sql|functions
ms.reviewer: 
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords:
- ABS_TSQL
- ABS
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], positive
- values [SQL Server], absolute
- ABS function
- absolute positive value
ms.assetid: e2ea7a6d-3e2f-472c-afbc-437d3b835c03
caps.latest.revision: 
author: edmacauley
ms.author: edmaca
manager: craigg
ms.workload: Active
ms.openlocfilehash: 9f96d651f179120fab6fabfb78d08cca84404bd1
ms.sourcegitcommit: 45e4efb7aa828578fe9eb7743a1a3526da719555
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2017
---
# <a name="abs-transact-sql"></a>ABS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Fonction mathématique qui renvoie la valeur absolue (positive) de l'expression numérique spécifiée. (`ABS` change les valeurs négatives en valeurs positives. `ABS` n’a aucun effet sur les valeurs positives ou égales à zéro.)
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
ABS ( numeric_expression )  
```  
  
## <a name="arguments"></a>Arguments  
*numeric_expression*  
Expression de type numérique exact ou approché.
  
## <a name="return-types"></a>Types de retour  
Retourne le même type que *numeric_expression*.
  
## <a name="examples"></a>Exemples  
L'exemple suivant montre les résultats de la fonction `ABS` appliquée à trois nombres différents.
  
```sql
SELECT ABS(-1.0), ABS(0.0), ABS(1.0);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
---- ---- ----  
1.0  .0   1.0  
```  
  
La fonction `ABS` peut générer une erreur de dépassement lorsque la valeur absolue d'un nombre est supérieure au nombre maximal pouvant être représenté par le type de données spécifié. Par exemple, le type de données `int` peut contenir uniquement des valeurs comprises entre `-2,147,483,648` et `2,147,483,647`. Le calcul de la valeur absolue de l'entier signé `-2,147,483,648` provoque une erreur de dépassement car cette valeur absolue est supérieure à la plage positive du type de données `int`.
  
```sql
DECLARE @i int;  
SET @i = -2147483648;  
SELECT ABS(@i);  
GO  
```  
  
Texte du message d'erreur :
  
« Msg 8115, Niveau 16, État 2, Ligne 3 »
  
« Une erreur de dépassement arithmétique s'est produite lors de la conversion de l'expression en type de données int. »

  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[Fonctions mathématiques &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)  
[Fonctions intégrées &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)
  
  

