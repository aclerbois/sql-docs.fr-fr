---
title: Séquences d’échappement | Documents Microsoft
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
- SQL statements [ODBC], interoperability
- escape sequences [ODBC], determining if supported
- interoperability of SQL statements [ODBC], escape sequences
ms.assetid: 5913abfa-d280-43e4-a2f1-05a924388bf9
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a2e9dc60d4cb598c777527aa6825ef2ee3a35b2b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="escape-sequences"></a>Séquences d’échappement
ODBC définit des séquences d’échappement contenant une grammaire standard pour date, time, timestamp et littéraux de l’intervalle datetime, les appels de fonction scalaire, **comme** prédicat caractères d’échappement, des jointures externes et des appels de procédure. Applications interopérables doivent utiliser ces séquences de chaque fois que possible.  
  
 Pour déterminer si un pilote prend en charge les séquences d’échappement pour la date, heure, timestamp ou littéraux d’intervalle datetime, une application appelle **SQLGetTypeInfo**. Si la source de données prend en charge une date, heure, timestamp ou le type de données d’intervalle datetime, il doit également prendre en charge la séquence d’échappement correspondante. Pour déterminer si les autres séquences d’échappement sont pris en charge, une application appelle **SQLGetInfo**.  
  
 Pour plus d’informations, consultez [les séquences d’échappement dans ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md), plus loin dans cette section.
