---
title: Remplacer non significatif et la précision en secondes pour les Types de données Interval | Documents Microsoft
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
- data types [ODBC], interval data types
- precision [ODBC], interval data types
- seconds precision [ODBC]
- interval data type [ODBC], precision
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: 3d65493f-dce7-4d29-9f59-c63a4e47918c
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ca22fe0d3153fc5ea4f0c5594ff537d65d60c066
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="overriding-default-leading-and-seconds-precision-for-interval-data-types"></a>Substitution de début de la valeur par défaut et la précision en secondes pour les Types d’intervalle
Lorsque le champ SQL_DESC_TYPE d’un ARD a la valeur datetime ou interval C type, en appelant **SQLBindCol** ou **SQLSetDescField**, le champ SQL_DESC_PRECISION (qui contient la précision de secondes d’intervalle) a la valeur par défaut suivants :  
  
-   6 pour timestamp et tous les types de données interval avec un second composant.  
  
-   0 pour tous les autres types de données.  
  
 Pour tous les types de données d’intervalle, le champ de descripteur SQL_DESC_DATETIME_INTERVAL_PRECISION, qui contient la précision de champ intervalle au début, a la valeur 2 comme valeur par défaut.  
  
 Quand le champ SQL_DESC_TYPE dans un APD a un type datetime ou interval de C, en appelant **SQLBindParameter** ou **SQLSetDescField**, les champs SQL_DESC_PRECISION et SQL_DESC_DATETIME_INTERVAL_PRECISION dans le descripteur APD sont définis avec la valeur par défaut fournie précédemment. Cela est vrai pour les paramètres d’entrée, mais pas pour les paramètres d’entrée/sortie ou de sortie.  
  
 Un appel à **SQLSetDescRec** définit l’intervalle de précision d’en-tête à la valeur par défaut, mais définit la précision de secondes d’intervalle (dans le champ SQL_DESC_PRECISION) à la valeur de sa *précision* argument.  
  
 Si une des valeurs par défaut étant donnés précédemment n’est pas acceptable pour une application, l’application doit définir le champ SQL_DESC_PRECISION ou SQL_DESC_DATETIME_INTERVAL_PRECISION en appelant **SQLSetDescField**.  
  
 Si l’application appelle **SQLGetData** pour renvoyer des données dans un datetime ou un intervalle de type C, la précision de début intervalle par défaut et la précision de secondes d’intervalle sont utilisés. Si une valeur par défaut n’est pas acceptable, l’application doit appeler **SQLSetDescField** pour définir un champ de descripteur, ou **SQLSetDescRec** définir SQL_DESC_PRECISION. L’appel à **SQLGetData** doit avoir un *TargetType* de SQL_ARD_TYPE pour utiliser les valeurs dans les champs de descripteur.  
  
 Lorsque **SQLPutData** est appelée, l’intervalle de début de précision et l’intervalle de secondes précision sont lues à partir des champs de l’enregistrement de descripteur qui correspondent au paramètre de data-at-execution ou colonne, qui sont des champs APD pour les appels à **SQLExecute** ou **SQLExecDirect**, ou les champs ARD pour les appels à **SQLBulkOperations** ou **SQLSetPos**.
