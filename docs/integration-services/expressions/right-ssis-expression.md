---
title: RIGHT (expression SSIS) | Microsoft Docs
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
- RIGHT function
ms.assetid: 83e70e75-4be5-4783-a8cf-032f82afe16e
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 35bddfa34cd2d00da8f797634d465d15a62fe5be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="right-ssis-expression"></a>RIGHT (expression SSIS)
  Renvoie le nombre de caractères spécifié en commençant par la partie la plus à droite d'une expression de caractères donnée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RIGHT(character_expression,integer_expression)  
```  
  
## <a name="arguments"></a>Arguments  
 *expression_caractère*  
 Expression de caractères à partir de laquelle doivent être extraits les caractères.  
  
 *integer_expression*  
 Expression entière indiquant le nombre de caractères à renvoyer.  
  
## <a name="result-types"></a>Types des résultats  
 DT_WSTR  
  
## <a name="remarks"></a>Notes   
 Si *integer_expression* est supérieure à la longueur de *character_expression*, la fonction retourne *character_expression*.  
  
 Si l’argument *integer_expression* a pour valeur zéro, la fonction renvoie une chaîne de longueur nulle.  
  
 Si l’argument *integer_expression* est un nombre négatif, la fonction renvoie une erreur.  
  
 L’argument *integer_expression* peut accepter des variables et des colonnes.  
  
 La fonction RIGHT n'est opérationnelle qu'avec le type de données DT_WSTR. Un argument *character_expression* représentant un littéral de chaîne ou une colonne de données du type de données DT_STR est implicitement converti dans le type de données DT_WSTR avant que la fonction RIGHT ne soit exécutée. Les autres types de données doivent être explicitement convertis vers le type de données DT_WSTR. Pour plus d’informations, consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md) et [Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
 La fonction RIGHT renvoie un résultat NULL si l'un des arguments est NULL.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant utilise un littéral de chaîne. Le résultat obtenu est `"Bike"`.  
  
```  
RIGHT("Mountain Bike", 4)  
```  
  
 L'exemple suivant retourne le nombre de caractères situés le plus à droite dans la variable `Times` depuis la colonne `Name` . Si `Name` est `Touring Front Wheel` et `Times` est 5, le résultat retourné est `"Wheel"`.  
  
```  
RIGHT(Name, @Times)  
```  
  
 L'exemple suivant retourne également le nombre de caractères situés le plus à droite dans la variable `Times` de la colonne `Name` . `Times` La variable est du type de données noninteger et l’expression comprend une conversion explicite vers le type de données DT_I2. Si `Name` est `Touring Front Wheel` et `Times` est `4.32`, le résultat obtenu est `"heel"` parce que la fonction RIGHT convertit la valeur de 4.32 à 4, puis retourne les quatre caractères situés les plus à droite.  
  
```  
RIGHT(Name, (DT_I2)@Times))  
```  
  
## <a name="see-also"></a> Voir aussi  
 [LEFT &#40;expression SSIS&#41;](../../integration-services/expressions/left-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
