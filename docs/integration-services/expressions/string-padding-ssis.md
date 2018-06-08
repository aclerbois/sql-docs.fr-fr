---
title: Remplissage des chaînes (SSIS) | Microsoft Docs
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
- padding strings [Integration Services]
- expressions [Integration Services], string padding
- string padding
ms.assetid: d3fed73d-e0d4-4c67-9355-fb7083a72dd6
caps.latest.revision: 33
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 927da3152459288a3f34a92c24898245df02df8e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="string-padding-ssis"></a>Remplissage des chaînes (SSIS)
  L'évaluateur d'expression ne vérifie pas si une chaîne contient des espaces de début et de fin et ne complète pas les chaînes pour qu'elles aient la même longueur avant de les comparer. Pour compléter une chaîne dans une expression, vous pouvez concaténer des valeurs de colonne et des chaînes vides à l'aide de l'opérateur « + ». Pour plus d’informations, consultez [+ &#40;Concaténer&#41; &#40;expression SSIS&#41;](../../integration-services/expressions/concatenate-ssis-expression.md).  
  
 Par contre, si vous souhaitez supprimer des espaces, l'évaluateur d'expression met à votre disposition les fonctions LTRIM, RTRIM et TRIM, qui permettent de supprimer les espaces de début et/ou de fin. Pour plus d’informations, consultez [LTRIM &#40;Expression SSIS&#41;](../../integration-services/expressions/ltrim-ssis-expression.md), [RTRIM &#40;Expression SSIS&#41;](../../integration-services/expressions/rtrim-ssis-expression.md) [TRIM &#40;Expression SSIS&#41;](../../integration-services/expressions/trim-ssis-expression.md).  
  
> [!NOTE]  
>  Le nombre de la fonction LEN inclut les espaces de début et de fin.  
  
## <a name="related-content"></a>Contenu associé  
 Article technique, [SSIS Expression Cheat Sheet](http://go.microsoft.com/fwlink/?LinkId=746575), sur pragmaticworks.com  
  
  
