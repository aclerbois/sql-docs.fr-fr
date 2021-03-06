---
title: "Fonctions numériques (le pilote ODBC Visual FoxPro) | Documents Microsoft"
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: odbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- ODBC numeric functions [ODBC]
- Visual FoxPro ODBC driver [ODBC], numeric functions
- numeric functions [ODBC]
- FoxPro ODBC driver [ODBC], numeric functions
ms.assetid: 7caab48e-cbb5-4bbc-a09b-5cf902e5bc45
caps.latest.revision: "8"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: b1801478a7586193dbb5861a4bc62472cde1ccbf
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="numeric-functions-visual-foxpro-odbc-driver"></a>Fonctions numériques (le pilote ODBC Visual FoxPro)
Le tableau suivant décrit les fonctions numériques ODBC pris en charge par le pilote ODBC Visual FoxPro ; lors de la grammaire Visual FoxPro pour la même fonction diffère de la syntaxe ODBC, le Visual FoxPro équivalent est répertorié.  
  
|Grammaire ODBC|Grammaire de Visual FoxPro|  
|------------------|---------------------------|  
|ABS *(positions numeric_exp)*||  
|ACOS *(exp_float)*||  
|ASIN *(exp_float)*||  
|ATAN *(exp_float)*||  
|ATAN2 *(exp_float1 exp_float2)*|ATN2 (*exp_float1, exp_float2*)|  
|PLAFOND *(positions numeric_exp)*||  
|COS *(exp_float)*||  
|COT *(exp_float)*||  
|DEGRÉS *(positions numeric_exp)*|RTOD *(positions numeric_exp)*|  
|EXP *(exp_float)*||  
|FLOOR *(positions numeric_exp)*||  
|JOURNAL *(exp_float)*||  
|LOG10 *(exp_float)*||  
|MOD *(exp_entier1 exp_entier2)*||  
|PI *)*||  
|RADIANS *(positions numeric_exp)*|DTOR *(positions numeric_exp)*|  
|RAND *([integer_exp])*||  
|ROUND *(positions numeric_exp integer_exp)*||  
|SIGNE *(positions numeric_exp)*||  
|SIN *(exp_float)*||  
|SQRT *(exp_float)*||  
|TAN *(exp_float)*||  
  
 Les fonctions numériques suivantes ne sont pas prises en charge :  
  
 ALIMENTATION *(positions numeric_exp integer_exp)*  
  
 TRONQUER *(positions numeric_exp integer_exp)*
