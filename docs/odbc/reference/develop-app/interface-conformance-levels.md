---
title: Niveaux de conformité de l’interface | Documents Microsoft
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
- interface conformance levels [ODBC]
- conformance levels [ODBC], interface
- data sources [ODBC], conformance levels
- ODBC drivers [ODBC], conformance levels
ms.assetid: 2c470e54-0600-4b2b-b1f3-9885cb28a01a
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 890684bf513b80cd484f7b15c75a04c38553513b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="interface-conformance-levels"></a>Niveaux de conformité de l’interface
L’objectif de l’audit est d’informer l’application quelles fonctionnalités sont disponibles à ce dernier à partir du pilote. Un schéma d’audit basé sur des fonctions ne pas suffisamment à atteindre cet objectif. Dans ODBC 3. *x*, pilotes sont classées selon les fonctionnalités qu’ils possèdent. La fonctionnalité de prise en charge peut inclure la prise en charge de la fonction ; Il peut également inclure un champ de descripteur, un attribut d’instruction, une valeur « Y » de prise en charge pour un type d’informations retourné par **SQLGetInfo**, et ainsi de suite.  
  
 Pour simplifier la spécification de mise en conformité de l’interface, ODBC définit trois niveaux de conformité. Pour répondre à un niveau de conformité spécifique, un pilote doit satisfaire toutes les conditions requises de ce niveau de conformité. Mise en conformité avec un niveau donné implique la mise en conformité complète avec tous les niveaux inférieurs.  
  
 Niveaux de conformité ne pas toujours diviser parfaitement la prise en charge pour une liste spécifique des fonctions ODBC, mais vous spécifient les fonctionnalités prises en charge comme indiqué dans les sections suivantes. Pour prendre en charge une fonctionnalité, un pilote doit prendre en charge certaines ou toutes les formes d’appels à certaines fonctions ODBC (pour plus d’informations, consultez [mise en conformité de la fonction](../../../odbc/reference/develop-app/function-conformance.md)), si certains attributs (consultez [mise en conformité de l’attribut](../../../odbc/reference/develop-app/attribute-conformance.md)) et certains champs de descripteur (consultez [mise en conformité du champ de descripteur](../../../odbc/reference/develop-app/descriptor-field-conformance.md)).  
  
 L’application découvre le niveau de conformité d’interface d’un pilote en vous connectant à une source de données et l’appel **SQLGetInfo** avec l’option SQL_ODBC_INTERFACE_CONFORMANCE.  
  
 Les pilotes sont libres implémenter des fonctionnalités au-delà du niveau auquel ils prétendent conformité complète. Applications découvrent des ces fonctions supplémentaires en appelant **SQLGetFunctions** (pour déterminer les fonctions ODBC sont présentes) et **SQLGetInfo** (pour interroger les différentes autres fonctionnalités ODBC).  
  
 Il existe trois niveaux de conformité ODBC interface : Core, niveau 1 et niveau 2.  
  
> [!NOTE]  
>  Ces niveaux de conformité des exigences différentes que les niveaux de conformité d’API ODBC portant le même nom dans ODBC 2 *.x*. En particulier, toutes les fonctionnalités implicitement par ODBC 2 *.x* conformité de l’API niveau 1 font partie du niveau de conformité de l’interface Core. Par conséquent, de nombreux pilotes ODBC peuvent signaler de conformité d’interface au niveau du noyau.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Conformité de l’interface principale](../../../odbc/reference/develop-app/core-interface-conformance.md)  
  
-   [Conformité de l’interface - niveau 1](../../../odbc/reference/develop-app/level-1-interface-conformance.md)  
  
-   [Conformité de l’interface - niveau 2](../../../odbc/reference/develop-app/level-2-interface-conformance.md)  
  
-   [Conformité des fonctions](../../../odbc/reference/develop-app/function-conformance.md)  
  
-   [Conformité des attributs](../../../odbc/reference/develop-app/attribute-conformance.md)  
  
-   [Conformité des champs de descripteur](../../../odbc/reference/develop-app/descriptor-field-conformance.md)
