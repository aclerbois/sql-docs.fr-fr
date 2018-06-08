---
title: ENTRE le prédicat | Documents Microsoft
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
- BETWEEN predicate [ODBC]
- SQL grammar [ODBC], between predicate
ms.assetid: 0cc7464b-d788-4720-98d8-411e1169185f
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9909930941c7dfd6a180ca3e33b7e89c7d9c4825
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="between-predicate"></a>ENTRE le prédicat
La syntaxe suivante :  
  
```  
expression1 BETWEEN expression2 AND expression3  
```  
  
 Retourne true uniquement si *expression1* est supérieur ou égal à *expression2* et *expression1* est inférieur ou égal à *expression3*.  
  
 La sémantique de cette syntaxe est différente pour les pilotes de base de données de bureau et le moteur Microsoft Jet. Dans Microsoft Jet SQL, *expression2* peut être supérieur à *expression3* afin que l’instruction renvoie la valeur TRUE uniquement si *expression1* est supérieur ou égal à *expression3*, et *expression1* est inférieur ou égal à *expression2*.
