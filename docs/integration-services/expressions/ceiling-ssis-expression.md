---
title: CEILING (expression SSIS) | Microsoft Docs
ms.custom: 
ms.date: 03/01/2017
ms.prod: sql-non-specified
ms.prod_service: integration-services
ms.service: 
ms.component: expressions
ms.reviewer: 
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- smallest integer great than or equal to expression
- CEILING function [SSIS]
ms.assetid: c35bd4ee-1ab6-46ab-89a7-cf771527faa2
caps.latest.revision: 
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 0e3824c5cd07c2ba158f57056d1c746c0f04e0b0
ms.sourcegitcommit: 9e6a029456f4a8daddb396bc45d7874a43a47b45
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2018
---
# <a name="ceiling-ssis-expression"></a>CEILING (expression SSIS)
  Renvoie le plus petit entier qui est supérieur ou égal à une expression numérique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CEILING(numeric_expression)  
```  
  
## <a name="arguments"></a>Arguments  
 *numeric_expression*  
 Expression numérique valide.  
  
## <a name="result-types"></a>Types des résultats  
 Type de données de l'expression numérique envoyée à la fonction.  
  
## <a name="remarks"></a>Notes   
 La fonction CEILING renvoie un résultat NULL si l'argument est NULL.  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 Les exemples suivants appliquent la fonction CEILING à une valeur successivement positive, négative et nulle.  
  
```  
CEILING(123.74)  
```  
  
 Retourne 124.00  
  
```  
CEILING(-124.27)  
```  
  
 Retourne -124.00  
  
```  
CEILING(0.00)  
```  
  
 Renvoie 0,00  
  
## <a name="see-also"></a> Voir aussi  
 [FLOOR &#40;expression SSIS&#41;](../../integration-services/expressions/floor-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
