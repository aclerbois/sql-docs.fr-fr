---
title: Choix d’une grammaire SQL | Documents Microsoft
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
- interoperability of SQL statements [ODBC], SQL grammar
- SQL grammar [ODBC], selecting
ms.assetid: 4e0d189b-e407-47e0-92a9-f9982230dd0e
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f571ee10abc26d5de0fb0ff35fc0c931b759f2b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="choosing-an-sql-grammar"></a>Choix d’une grammaire SQL
La première décision à prendre lors de la construction d’instructions SQL est le grammaire à utiliser. Outre les grammaires disponibles dans les différentes instances de normes, telles que Open Group, ANSI et ISO, pratiquement tous les fournisseurs SGBD définit sa propre grammaire, chacune d’elles varie légèrement par rapport à la norme.  
  
 [Annexe c : SQL grammaire](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md), décrit la grammaire SQL minimale tous les pilotes ODBC doivent prendre en charge. Cette grammaire est un sous-ensemble du niveau d’entrée de SQL-92. Pilotes peuvent prendre en charge la grammaire supplémentaire pour être conforme à l’intermédiaire, complète ou FIPS 127-2 des niveaux transitoires définis par SQL-92. Pour plus d’informations, consultez [SQL Minimum grammaire](../../../odbc/reference/appendixes/sql-minimum-grammar.md) dans la grammaire de l’annexe c : SQL et SQL-92.  
  
 Annexe C définit également *séquences d’échappement* contenant une grammaire standard pour des fonctionnalités de langage courants, tels que les jointures externes, qui ne relèvent pas la grammaire SQL-92. Pour plus d’informations, consultez [les séquences d’échappement ODBC](../../../odbc/reference/appendixes/odbc-escape-sequences.md) dans l’annexe c : SQL, et [séquences d’échappement](../../../odbc/reference/develop-app/escape-sequences.md), plus loin dans cette section.  
  
 La grammaire est choisie affecte la façon dont le pilote traite l’instruction. Les pilotes doivent modifier SQL-92 SQL et les séquences d’échappement de définie par ODBC pour propres au SGBD SQL. Étant donné que la plupart des grammaires SQL sont basées sur un ou plusieurs des différentes normes, la plupart des pilotes travailler peu ou pas pour répondre à cette exigence. Il se compose souvent uniquement de rechercher les séquences d’échappement définies par ODBC et en les remplaçant par la grammaire des SGBD spécifiques. Lorsqu’un pilote rencontre la grammaire que ne reconnaît pas, il suppose la grammaire propres au SGBD et transmet l’instruction SQL sans modification à la source de données pour l’exécution.  
  
 Par conséquent, il existe réellement deux choix de grammaire à utiliser : la grammaire SQL-92 (et les séquences d’échappement ODBC) et une grammaire propres au SGBD. Des deux, uniquement la grammaire SQL-92 est interopérable, par conséquent, toutes les applications interopérables doivent l’utiliser. Les applications qui ne sont pas interopérables peuvent utiliser la grammaire SQL-92 ou une grammaire propres au SGBD. Propres au SGBD grammaires ont deux avantages : ils peuvent exploiter les fonctionnalités non traitées par SQL-92, et ils sont légèrement plus rapides, car le pilote n’a pas de les modifier. La fonctionnalité de ce dernier peut être partiellement appliquée en définissant l’attribut d’instruction SQL_ATTR_NOSCAN, qui arrête le pilote à partir de la recherche et remplacement de séquences d’échappement.  
  
 Si la grammaire SQL-92 est utilisée, l’application peut découvrir comment il est modifié par le pilote en appelant **SQLNativeSql**. Cela est souvent utile lors du débogage d’applications. **SQLNativeSql** accepte une instruction SQL et la retourne, une fois que le pilote a modifié. Cette fonction se trouve dans le niveau de conformité de l’interface de cœur, il est pris en charge par tous les pilotes.
