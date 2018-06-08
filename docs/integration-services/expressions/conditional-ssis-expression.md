---
title: '? : (Conditionnel) (expression SSIS) | Microsoft Docs'
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
- conditional operator (?:)
- '?: (conditional operator)'
ms.assetid: d38e6890-7338-4ce0-a837-2dbb41823a37
caps.latest.revision: 49
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 411ac9180290955a22164ad5922b6d04e649f79c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="--conditional-ssis-expression"></a>? : (Conditionnel) (expression SSIS)
  Renvoie une des deux expressions selon l'évaluation d'une expression booléenne. Si l'expression booléenne donne la valeur TRUE, la première expression est évaluée et le résultat est le résultat de l'expression. Si l'expression booléenne donne la valeur FALSE, la deuxième expression est évaluée et son résultat est le résultat de l'expression.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
boolean_expression?expression1:expression2  
  
```  
  
## <a name="arguments"></a>Arguments  
 *boolean_expression*  
 Expression valide qui renvoie TRUE, FALSE ou NULL.  
  
 *expression1*  
 Toute expression valide.  
  
 *expression2*  
 Toute expression valide.  
  
## <a name="result-types"></a>Types des résultats  
 Type de données de *expression1* ou de *expression2*.  
  
## <a name="remarks"></a>Notes   
 Si l’argument *expression_booléenne* est évalué à NULL, le résultat de l’expression est NULL. Si une expression sélectionnée, *expression1* ou *expression2* est NULL, le résultat est NULL. Si une expression sélectionnée n'est pas NULL, mais que l'expression non sélectionnée est NULL, le résultat est la valeur de l'expression sélectionnée.  
  
 Si *expression1* et *expression2* ont le même type de données, le résultat est de ce type de données. Les règles supplémentaires suivantes s'appliquent aux types de résultats :  
  
-   Le type de données DT_TEXT nécessite que *expression1* et *expression2* aient la même page de codes.  
  
-   La longueur d'un résultat dont le type de données est DT_BYTES correspond à la longueur de l'argument le plus long.  
  
 L’ensemble d’expressions, *expression1* et *expression2*, doit s’évaluer à des types de données valides et être conforme à une des règles suivantes :  
  
-   **Numérique** : *expression1* et *expression2* doivent toutes deux être d’un type de données numériques. L'intersection des types de données doit être de type de données numérique, comme le spécifient les règles relatives aux conversions numériques implicites effectuées par l'évaluateur d'expression. L'intersection des deux types de données numériques ne peut pas être NULL. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md).  
  
-   **Chaîne** : *expression1* et *expression2* doivent toutes deux être d’un type de données chaîne : DT_STR ou DT_WSTR. Les deux expressions peuvent avoir une valeur de types de données chaîne différents. Le résultat a le type de données DT_WSTR et une longueur équivalente à celle de l'argument le plus long.  
  
-   **Date, Heure ou Date/Heure** : *expression1* et *expression2* doivent toutes deux correspondre à un des types de données suivants : DT_DBDATE, DT_DATE, DT_DBTIME, DT_DBTIME2, DT_DBTIMESTAMP, DT_DBTIMESTAMP2, DT_DBTIMESTAPMOFFSET ou DT_FILETIME.  
  
    > [!NOTE]  
    >  Le système ne prend pas en charge les comparaisons entre une expression qui correspond à un type de données heure et une expression qui correspond à un type de données date ou date/heure. Le système génère alors une erreur.  
  
     Lors de la comparaison des expressions, le système applique les règles de conversion suivantes dans l'ordre indiqué :  
  
    -   Lorsque les deux expressions correspondent au même type de données, une comparaison de ce type de données est effectuée.  
  
    -   Si une expression est d'un type de données DT_DBTIMESTAMPOFFSET, l'autre expression est implicitement convertie en DT_DBTIMESTAMPOFFSET et une comparaison DT_DBTIMESTAMPOFFSET est effectuée. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md).  
  
    -   Si une expression est d'un type de données DT_DBTIMESTAMP2, l'autre expression est implicitement convertie en DT_DBTIMESTAMP2 et une comparaison DT_DBTIMESTAMP2 est effectuée.  
  
    -   Si une expression est d'un type de données DT_DBTIME2, l'autre expression est implicitement convertie en DT_DBTIME2 et une comparaison DT_DBTIME2 est effectuée.  
  
    -   Si une expression est d'un type autre que DT_DBTIMESTAMPOFFSET, DT_DBTIMESTAMP2 ou DT_DBTIME2, les expressions sont converties en type de données DT_DBTIMESTAMP avant leur comparaison.  
  
     Lors de la comparaison des expressions, le système émet les hypothèses suivantes :  
  
    -   Si chaque expression est d'un type de données qui inclut des fractions de seconde, le système suppose que le type de données avec le moins grand nombre de chiffres pour les fractions de seconde inclut des zéros pour les chiffres restants.  
  
    -   Si chaque expression est d'un type de données date, mais qu'une seule a un décalage de fuseau horaire, le système suppose que le type de données date sans le décalage de fuseau horaire est exprimé en temps universel coordonné (UTC).  
  
 Pour plus d'informations sur les types de données, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant montre une expression qui selon une condition prend la valeur `savannah` ou `unknown`.  
  
```  
@AnimalName == "Elephant"? "savannah": "unknown"  
```  
  
 L’exemple suivant montre une expression qui fait référence une colonne **ListPrice** . La colonne**ListPrice** a le type de données DT_CY. L’expression multiplie selon une condition la valeur de la colonne **ListPrice** par 0,2 ou 0,1.  
  
```  
ListPrice < 350.00 ? ListPrice * .2 : ListPrice * .1  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)  
  
  
