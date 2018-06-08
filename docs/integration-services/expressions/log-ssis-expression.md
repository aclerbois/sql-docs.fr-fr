---
title: LOG (expression SSIS) | Microsoft Docs
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
- base-10 logarithms
- LOG function
ms.assetid: f7fccace-c178-4e13-bde9-7dc4ef1d98fa
caps.latest.revision: 31
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 87b582e91e84ffdd81a46eb94728d4b688957216
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="log-ssis-expression"></a>LOG (expression SSIS)
  Renvoie le logarithme de base 10 d'une expression numérique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
LOG(numeric_expression)  
```  
  
## <a name="arguments"></a>Arguments  
 *numeric_expression*  
 Expression numérique valide différente de zéro ou non négative.  
  
## <a name="result-types"></a>Types des résultats  
 DT_R8  
  
## <a name="remarks"></a>Notes   
 *L’expression numérique* est convertie vers le type de données DT_R8 avant le calcul du logarithme. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Si l’argument *numeric_expression* donne une valeur inférieure ou égale à zéro, le résultat obtenu est NULL.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant utilise un littéral numérique. La fonction renvoie la valeur 1,988291341907488.  
  
```  
LOG(97.34)  
```  
  
 L'exemple suivant utilise la colonne **Length**. Si la valeur de la colonne est 101,24, la fonction renvoie 2,005352136486217.  
  
```  
LOG(Length)   
```  
  
 L'exemple suivant utilise la variable **Length**. La variable doit être d'un type de données numérique ou l'expression doit comprendre une conversion explicite en un type de données [!INCLUDE[ssIS](../../includes/ssis-md.md)] numérique. Si la variable **Length** a pour valeur 234,567, la fonction renvoie 2,370266913465859.  
  
```  
LOG(@Length)   
```  
  
## <a name="see-also"></a> Voir aussi  
 [EXP &#40;expression SSIS&#41;](../../integration-services/expressions/exp-ssis-expression.md)   
 [LN &#40;expression SSIS&#41;](../../integration-services/expressions/ln-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
