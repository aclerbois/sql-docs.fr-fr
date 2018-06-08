---
title: REPLICATE (expression SSIS) | Microsoft Docs
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
- REPLICATE function
ms.assetid: e7a37b93-6d1d-42d5-9a65-de1790abf6a5
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 06026b418fac119f45868425e0a75d53fb514144
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="replicate-ssis-expression"></a>REPLICATE (expression SSIS)
  Renvoie une expression de caractères qui est répliquée plusieurs fois. L'argument *times* doit correspondre à un nombre entier.  
  
> [!NOTE]  
>  La fonction REPLICATE utilise habituellement des chaînes longues, elle est donc plus exposée à la limitation fixée à 4000 caractères sur la longueur d'expression. Si le résultat de l'évaluation d'une expression donne le type de données Integration Services DT_WSTR ou DT_STR, cette expression sera réduite à 4000 caractères. Si le type du résultat d'une sous-expression est DT_STR ou DT_WSTR, cette sous-expression sera également tronquée à 4000 caractères, peu importe le type de résultat obtenu dans l'expression générale. Les conséquences de la troncation peuvent être gérées naturellement ou être à l'origine d'un avertissement ou d'un message d'erreur. Pour plus d’informations, consultez [Syntaxe &#40;SSIS&#41;](../../integration-services/expressions/syntax-ssis.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
REPLICATE(character_expression,times)  
```  
  
## <a name="arguments"></a>Arguments  
 *expression_caractère*  
 Expression de caractères à répliquer.  
  
 *times*  
 Expression entière spécifiant le nombre de réplications de l’argument *character_expression* .  
  
## <a name="result-types"></a>Types des résultats  
 DT_WSTR  
  
## <a name="remarks"></a>Notes   
 Si l’argument *times* a pour valeur zéro, la fonction retourne une chaîne de longueur nulle.  
  
 Si l'argument *times* est un nombre négatif, la fonction renvoie une erreur.  
  
 L'argument *times* peut également utiliser des variables et des colonnes.  
  
 La fonction REPLICATE est opérationnelle seulement avec le type de données DT_WSTR. Un argument *character_expression* représentant un littéral de chaîne ou une colonne de données du type de données DT_STR est implicitement converti dans le type de données DT_WSTR avant que la fonction REPLICATE ne soit exécutée. Les autres types de données doivent être explicitement convertis vers le type de données DT_WSTR. Pour plus d’informations, consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md) et [Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
 La fonction REPLICATE renvoie un résultat NULL si l'un des arguments est NULL.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant réplique un littéral de chaîne trois fois. Le résultat obtenu est « Mountain BikeMountain BikeMountain Bike ».  
  
```  
REPLICATE("Mountain Bike", 3)  
```  
  
 L'exemple suivant réplique les valeurs de la colonne **Name** un nombre de fois égal à la valeur de la variable **Times** . Si la variable **Times** a pour valeur 3 et que la valeur de la colonne **Name** est « Tout-terrain », le résultat obtenu est « Tout-terrainTout-terrainTout-terrain ».  
  
```  
REPLICATE(Name, @Times)  
```  
  
 L'exemple suivant réplique la valeur de la variable **Name** un nombre de fois égal à la valeur de la colonne **Times** . La valeur**Times** a un type de données non-entier et l’expression comprend une conversion explicite vers un type de données entier. Si la variable **Name** contient « Helmet » et que la colonne **Times** a pour valeur 2, le résultat obtenu est « HelmetHelmet ».  
  
```  
REPLICATE(@Name, (DT_I4(Times))  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
