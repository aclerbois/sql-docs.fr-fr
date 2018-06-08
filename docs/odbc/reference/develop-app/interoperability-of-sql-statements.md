---
title: Interopérabilité des instructions SQL | Documents Microsoft
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
- interoperability of SQL statements [ODBC]
- interoperability of SQL statements [ODBC], about interoperability
ms.assetid: 3b24c499-829c-4e65-90cf-a3a0f6d0a186
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 205fd667e8891ba0bab0283c1d112af9d608423b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="interoperability-of-sql-statements"></a>Interopérabilité des instructions SQL
Comme le reste d’une application, les instructions SQL puissent être interopérable ou propres au SGBD. Et comme le reste de l’application, le choix de la façon dont l’interopérables instructions SQL doivent être dépend du type d’application. Applications personnalisées sont moins susceptibles d’utiliser des instructions SQL interopérables, car elles sont généralement conçues pour exploiter les fonctionnalités d’un ou deux éventuellement SGBD. Applications génériques utilisent des instructions SQL interopérables, car ils sont conçus pour fonctionner avec un large éventail de SGBD. Et les applications verticales généralement quelque part entre les deux, en exigeant un certain niveau de fonctionnalités, mais sinon à l’aide des instructions SQL interopérables.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Choix d’une grammaire SQL](../../../odbc/reference/develop-app/choosing-an-sql-grammar.md)  
  
-   [Construction d’instructions SQL interopérables](../../../odbc/reference/develop-app/constructing-interoperable-sql-statements.md)
