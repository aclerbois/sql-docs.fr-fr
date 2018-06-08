---
title: Les séquences d’échappement intervalle | Documents Microsoft
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
- interval literals [ODBC]
- escape sequences [ODBC], interval
- ODBC escape sequences [ODBC], interval
ms.assetid: 303e8dab-8f13-4fa5-857f-15cc1f75bdd6
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3973a5149aa5861b2d194cd4487a15b0f97e7f94
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="interval-escape-sequences"></a>Séquences d’échappement d’intervalle
ODBC utilise les séquences d’échappement pour les littéraux de l’intervalle. La syntaxe de cette séquence d’échappement est comme suit :  
  
 {*intervalle-literal*}  
  
 Pour la syntaxe BNF de *intervalle-literal*, consultez la [syntaxe de littéral d’intervalle](../../../odbc/reference/appendixes/interval-literal-syntax.md) section plus loin dans cette annexe.  
  
 La séquence d’échappement des littéraux d’intervalle est pris en charge si les types de données d’intervalle sont pris en charge par la source de données. Une application doit appeler **SQLGetTypeInfo** pour déterminer si ces types de données sont pris en charge.
