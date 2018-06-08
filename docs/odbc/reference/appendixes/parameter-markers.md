---
title: Marqueurs de paramètres | Documents Microsoft
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
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
- parameter markers [ODBC]
ms.assetid: 07213d04-cd31-45fd-a8c8-2e16e09eeaf4
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: df895c9e9956c1fde178824d1e14030246710946
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameter-markers"></a>Marqueurs de paramètres
Conformément à la spécification SQL-92, une application ne peut pas placer des marqueurs de paramètres dans les emplacements suivants. Pour une liste plus complète, consultez la spécification de SQL-92.  
  
-   Dans un **sélectionnez** liste  
  
-   À la fois comme *expressions* dans un *prédicat de comparaison*  
  
-   Comme les deux opérandes d’un opérateur binaire  
  
-   Comme les deux les premier et deuxième opérandes d’un **BETWEEN** opération  
  
-   Comme le premier et le troisième opérandes d’un **BETWEEN** opération  
  
-   En tant que l’expression et la première valeur d’une **IN** opération  
  
-   Comme opérande d’un opérateur unaire + ou – opération  
  
-   Comme l’argument d’un *référence de fonction de jeu*  
  
 Pour plus d’informations sur les marqueurs de paramètres, consultez la spécification de SQL-92. Pour plus d’informations, consultez [paramètres de l’instruction](../../../odbc/reference/develop-app/statement-parameters.md).
