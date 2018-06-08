---
title: '- (Négatif) (expression SSIS) | Microsoft Docs'
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
- '- (negative)'
- negative operator (-)
ms.assetid: f0118dfc-aced-4de2-953e-5ebf9c962b8d
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 579b2c8471b02db4786c540b978f8a23c71527a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="--negate-ssis-expression"></a>- (Négatif) (expression SSIS)
  Inverse une expression numérique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
-numeric_expression  
  
```  
  
## <a name="arguments"></a>Arguments  
 *numeric_expression*  
 Toute expression valide d’un type de données numérique. Seuls les types de données numériques signés sont pris en charge. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="result-types"></a>Types des résultats  
 Retourne le type de données *numeric_expression*.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 Cet exemple inverse la valeur de la variable **Counter** et ajoute le littéral numérique 50.  
  
```  
-@Counter + 50  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)  
  
  
