---
title: NULL (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: expressions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- NULL function
- null values [Integration Services]
ms.assetid: df144237-3fbb-41ac-8624-efd92b6522b9
caps.latest.revision: 30
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 616ca0fcfcde1764ed94c410c10834fd4eb1d2fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="null-ssis-expression"></a>NULL (expression SSIS)
  Renvoie une valeur NULL d'un type de données demandé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
NULL(typespec)  
```  
  
## <a name="arguments"></a>Arguments  
 *typespec*  
 Type de données valide. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="result-types"></a>Types des résultats  
 Tout type de données valide avec une valeur Null.  
  
## <a name="remarks"></a>Notes   
 La fonction NULL renvoie un résultat NULL si l'argument est NULL.  
  
 Des paramètres sont nécessaires pour demander une valeur NULL pour certains types de données. Le tableau suivant décrit ces types de données et leurs paramètres.  
  
|Type de données|Paramètre| Exemple|  
|---------------|---------------|-------------|  
|DT_STR|*charcount*<br /><br /> *codepage*|L'expression (DT_STR,30,1252) convertit 30 caractères vers le type de données DT_STR à l'aide de la page de codes 1252.|  
|DT_WSTR|*charcount*|L'expression (DT_WSTR,20) convertit 20 caractères vers le type de données DT_WSTR.|  
|DT_BYTES|*bytecount*|L'expression (DT_BYTES,50) convertit 50 octets vers le type de données DT_BYTES.|  
|DT_DECIMAL|*scale*|L'expression (DT_DECIMAL,2) convertit une valeur numérique dans le type de données DT_DECIMAL avec une échelle égale à 2.|  
|DT_NUMERIC|*precision*<br /><br /> *scale*|L'expression (DT_NUMERIC,10,3) convertit une valeur numérique dans le type de données DT_NUMERIC avec une précision de 10 et une échelle de 3.|  
|DT_TEXT|*codepage*|L'expression (DT_TEXT,1252) convertit une valeur vers le type de données DT_TEXT à l'aide de la page de codes 1252.|  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 Les exemples ci-après renvoient la valeur Null des types de données suivants : DT_STR, DT_DATE et DT_BOOL.  
  
```  
NULL(DT_STR,10,1252)  
NULL(DT_DATE)  
NULL(DT_BOOL)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ISNULL &#40;expression SSIS&#41;](../../integration-services/expressions/isnull-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
