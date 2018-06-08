---
title: Types de données de paramètre | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data types [ODBC], parameters
- parameter data type [ODBC]
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: fd7e99d8-d26a-408c-9733-6ffccde99f75
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4517e5e228ed5bb89bf2d57f80be20078aa90f2b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameter-data-types"></a>Types de données de paramètre
Bien que chaque paramètre spécifié avec **SQLBindParameter** est définie à l’aide d’un type de données SQL, les paramètres dans une instruction SQL avoir aucun intrinsèques type de données. Par conséquent, des marqueurs de paramètre peuvent être inclus dans une instruction SQL uniquement si leurs types de données peuvent être déduits à partir d’un autre opérande dans l’instruction. Par exemple, dans une expression arithmétique, telle que ? + COLUMN1, le type de données du paramètre peut être déduit du type de données de la colonne nommée représenté par COLUMN1. Une application ne peut pas utiliser un marqueur de paramètre si le type de données ne peut pas être déterminé.  
  
 Le tableau suivant décrit la manière dont un type de données est déterminé pour plusieurs types de paramètres, conformément à SQL-92. Pour une spécification plus détaillée sur la déduction du type de paramètre lorsque d’autres clauses SQL sont utilisées, consultez la spécification de SQL-92.  
  
|Emplacement du paramètre|Supposé que le type de données|  
|---------------------------|-----------------------|  
|Un opérande d’un opérateur arithmétique ou de comparaison binaire|Identique à l’autre opérande|  
|Le premier opérande d’un **BETWEEN** clause|Identique à la deuxième opérande.|  
|Le deuxième ou troisième opérande dans une **BETWEEN** clause|Identique à celui du premier opérande.|  
|Une expression utilisée avec **IN**|Identique à la première valeur ou la colonne de résultats de la sous-requête|  
|Une valeur utilisée avec **IN**|Identique à l’expression ou la première valeur s’il existe un marqueur de paramètre dans l’expression|  
|Une valeur de modèle utilisée avec **comme**|VARCHAR|  
|Une valeur de mise à jour utilisée avec **mettre à jour**|La colonne de la mise à jour|
