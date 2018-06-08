---
title: '!= (Différent de) (expression SSIS) | Microsoft Docs'
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
- unequal operator (!=)
- '!= (not equal to)'
ms.assetid: fad20e85-c0e6-42bf-af70-2bc80ee09be5
caps.latest.revision: 53
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 185ae93319c8c519753681b8a5be47b018be55ab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="-unequal-ssis-expression"></a>!= (Différent de) (expression SSIS)
  Effectue une comparaison pour déterminer si deux expressions de types de données compatibles sont différentes. L'évaluateur d'expression convertit automatiquement de nombreux types de données avant de réaliser la comparaison.  
  
 Toutefois, pour certains types de données, il est nécessaire que l'expression contienne une conversion explicite afin qu'elle puisse être évaluée correctement. Pour plus d’informations sur les conversions valides entre les types de données, consultez [Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
expression1 != expression2  
  
```  
  
## <a name="arguments"></a>Arguments  
 *expression1, expression2*  
 Toute expression valide.  
  
## <a name="result-types"></a>Types des résultats  
 DT_BOOL  
  
## <a name="remarks"></a>Notes   
 Si l'une des expressions de la comparaison est NULL, le résultat de la comparaison est NULL. Si les deux expressions sont NULL, le résultat est NULL.  
  
 Le jeu d’expressions, *expression1* et *expression2*, doit suivre une des règles suivantes :  
  
-   **Numérique** : *expression1* et *expression2* doivent toutes deux être d’un type de données numérique. L’intersection des types de données doit être un type de données numérique, comme le spécifient les règles relatives aux conversions numériques implicites effectuées par l’évaluateur d’expression. L'intersection des deux types de données numériques ne peut pas être NULL. Pour plus d’informations, consultez [Types de données Integration Services dans les expressions](../../integration-services/expressions/integration-services-data-types-in-expressions.md).  
  
-   **Caractère** : *expression1* et *expression2* doivent toutes deux s’évaluer à un type de données DT_STR ou DT_WSTR. Les deux expressions peuvent avoir une valeur de types de données chaîne différents.  
  
    > [!NOTE]  
    >  Les comparaisons de chaîne respectent la casse, les accents, le jeu de caractères Kana et la largeur.  
  
-   **Date, Heure ou Date/Heure** : *expression1* et *expression2* doivent toutes deux s’évaluer à un des types de données suivants : DT_DBDATE, DT_DATE, DT_DBTIME, DT_DBTIME2, DT_DBTIMESTAMP, DT_DBTIMESTAMP2, DT_DBTIMESTAPMOFFSET ou DT_FILETIME.  
  
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
  
-   **Logique** : *expression1* et *expression2* doivent toutes deux s’évaluer à une valeur booléenne.  
  
-   **GUID** : *expression1* et *expression2* doivent toutes deux s’évaluer au type de données DT_GUID.  
  
-   **Binaire** : *expression1* et *expression2* doivent toutes deux s’évaluer au type de données DT_BYTES.  
  
-   **BLOB** : *expression1* et *expression2* doivent toutes deux s’évaluer au même type de données BLOB (Binary Large Object Block) : DT_TEXT, DT_NTEXT ou DT_IMAGE.  
  
 Pour plus d'informations sur les types de données, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant renvoie la valeur TRUE uniquement si la date actuelle n'est pas le 4 juillet 2003. Pour plus d’informations, consultez [GETDATE &#40;expression SSIS&#41;](../../integration-services/expressions/getdate-ssis-expression.md).  
  
```  
"7/4/2003" != GETDATE()  
```  
  
 L’exemple suivant s’évalue à la valeur TRUE si la valeur de la colonne **ListPrice** n’est pas 500.  
  
```  
ListPrice != 500  
```  
  
 Cet exemple utilise la variable **LPrice**. Il s’évalue à TRUE si la valeur de la variable **LPrice** n’est pas 500. Le type de données de la variable doit être numérique pour permettre l’analyse de l’expression.  
  
```  
@LPrice != 500  
```  
  
## <a name="see-also"></a> Voir aussi  
 [== &#40;Égal&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/equal-ssis-expression.md)   
 [Priorités et associativité des opérateurs](../../integration-services/expressions/operator-precedence-and-associativity.md)   
 [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md)  
  
  
