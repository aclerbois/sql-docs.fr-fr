---
title: EXP (expression SSIS) | Microsoft Docs
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
- exponential functions
- EXP function
ms.assetid: 4cd96d3c-58c9-4a67-a6f6-b72758232912
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e570d60f4f6765b38d4c34d8fa92d2944b34d6da
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="exp-ssis-expression"></a>EXP (expression SSIS)
  Renvoie l'exposant en base e d'une expression numérique. La fonction EXP est complémentaire de l'action LN et est parfois appelée « antilogarithme ».  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
EXP(numeric_expression)  
```  
  
## <a name="arguments"></a>Arguments  
 *numeric_expression*  
 Expression numérique valide.  
  
## <a name="result-types"></a>Types des résultats  
 DT_R8  
  
## <a name="remarks"></a>Notes   
 L'expression numérique est convertie vers le type de données DT_R8 avant le calcul du l'exposant. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Le résultat obtenu est toujours un nombre positif.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 Les exemples suivants appliquent la fonction EXP à des valeurs positive, négative, ainsi qu'à la valeur zéro.  
  
```  
EXP(74)  
```  
  
 Renvoie 1,373382979540176E+32.  
  
```  
EXP(-27)  
```  
  
 Renvoie 1,879528816539083E-12.  
  
```  
EXP(0)  
```  
  
 Renvoie 1.  
  
## <a name="see-also"></a> Voir aussi  
 [LOG &#40;expression SSIS&#41;](../../integration-services/expressions/log-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
