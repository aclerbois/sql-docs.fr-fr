---
title: ROUND (expression SSIS) | Microsoft Docs
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
- rounding expressions
- ROUND function [SSIS]
ms.assetid: 376f1947-4fc5-4611-ad86-823e4db1b468
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 2df98b2937d410d59fbc58cd9ee31374be02257e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="round-ssis-expression"></a>ROUND (expression SSIS)
  Renvoie une expression numérique, arrondie à la longueur ou à la précision indiquée. La valeur du paramètre de longueur doit correspondre à un entier.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ROUND(numeric_expression,length)  
```  
  
## <a name="arguments"></a>Arguments  
 *numeric_expression*  
 Expression d'un type numérique valide. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
 *length*  
 Expression entière. Il s’agit de la précision avec laquelle *numeric_expression* est arrondie.  
  
## <a name="result-types"></a>Types des résultats  
 Type similaire à celui de *numeric*_*expression*.  
  
## <a name="remarks"></a>Notes   
 L'argument *length* doit avoir une valeur positive entière ou égale à zéro.  
  
 La fonction ROUND renvoie un résultat NULL si l'argument est NULL.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 Les exemples suivants arrondissent des littéraux numériques à une longueur de trois. Le premier résultat obtenu est 137,1570, tandis que le second est 137,1580.  
  
```  
ROUND(137.1574,3)  
ROUND(137.1575,3)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
