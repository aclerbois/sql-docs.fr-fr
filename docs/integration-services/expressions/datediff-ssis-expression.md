---
title: DATEDIFF (expression SSIS) | Microsoft Docs
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
- DATEDIFF statement
- dates [Integration Services], DATEDIFF
ms.assetid: 449b327f-47c7-4709-8bc6-4ee9a35cc330
caps.latest.revision: 40
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6f259723be0b8951770559a3e8212aedb062268b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datediff-ssis-expression"></a>DATEDIFF (expression SSIS)
  Renvoie le nombre de limites de date et d'heure traversées entre deux dates données. Le paramètre *datepart* identifie quelles limites de date et d’heure il faut comparer.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DATEDIFF(datepart, startdate, endate)  
```  
  
## <a name="arguments"></a>Arguments  
 *datepart*  
 Paramètre qui indique la partie de la date à comparer et pour laquelle une valeur doit être retournée.  
  
 *startdate*  
 Date de début de l'intervalle.  
  
 *endate*  
 Date de fin de l'intervalle.  
  
## <a name="result-types"></a>Types des résultats  
 DT_I4  
  
## <a name="remarks"></a>Notes   
 Le tableau suivant décrit les parties de date et les abréviations reconnues par l'évaluateur d'expression.  
  
|datepart|Abréviations|  
|--------------|-------------------|  
|Année|yy, yyyy|  
|Quarter|qq, q|  
|Month|mm, m|  
|Jour de l'année|dy, y|  
|Jour|dd, d|  
|Week|wk, ww|  
|JourSem|dw, w|  
|Heure|Hh|  
|Minute|mi, n|  
|Seconde|ss, s|  
|Milliseconde|Ms|  
  
 La fonction DATEDIFF renvoie un résultat NULL si un argument est NULL.  
  
 Un littéral de date doit être explicitement converti dans l'un des types de données date. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Une erreur se produit dans les cas suivants : une date n'est pas valide, l'unité de date ou d'heure n'est pas une chaîne, la date de début n'est pas une date ou la date de fin n'est pas une date.  
  
 Si la date de fin est antérieure à la date de début, la fonction renvoie un nombre négatif. Si les dates de début et de fin sont identiques ou qu'elles appartiennent au même intervalle, la fonction renvoie zéro.  
  
## <a name="ssis-expression-examples"></a>Exemples d'expressions SSIS  
 L'exemple suivant calcule le nombre de jours entre deux littéraux de date. Si le format de la date est « mm/jj/aaaa », la fonction renvoie 7.  
  
```  
DATEDIFF("dd", (DT_DBTIMESTAMP)"8/1/2003", (DT_DBTIMESTAMP)"8/8/2003")  
```  
  
 L'exemple suivant renvoie le nombre de mois entre un littéral de date et la date actuelle.  
  
```  
DATEDIFF("mm", (DT_DBTIMESTAMP)"8/1/2003",GETDATE())  
```  
  
 L’exemple suivant retourne le nombre de semaines entre la date de la colonne **ModifiedDate** et la variable **YearEndDate** . Si la variable **YearEndDate** est du type de données **date** , aucune conversion explicite n’est nécessaire.  
  
```  
DATEDIFF("Week", ModifiedDate,@YearEndDate)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DATEADD &#40;expression SSIS&#41;](../../integration-services/expressions/dateadd-ssis-expression.md)   
 [DATEPART &#40;expression SSIS&#41;](../../integration-services/expressions/datepart-ssis-expression.md)   
 [DAY &#40;expression SSIS&#41;](../../integration-services/expressions/day-ssis-expression.md)   
 [MONTH &#40;expression SSIS&#41;](../../integration-services/expressions/month-ssis-expression.md)   
 [YEAR &#40;expression SSIS&#41;](../../integration-services/expressions/year-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
