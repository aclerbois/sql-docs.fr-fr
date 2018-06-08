---
title: Les attributs d’instruction | Documents Microsoft
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
- SQL statements [ODBC], statement attributes
- statement attributes [ODBC]
ms.assetid: 4c59cd8e-a713-4095-9065-20d5bdeafe43
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7fe7fd9cb7436470b8eba9984a4427a9e43e8490
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="statement-attributes"></a>Attributs d'instruction
Les attributs d’instruction sont caractéristiques de l’instruction. Par exemple, s’il faut pour utiliser des signets et quel type de curseur à utiliser avec le résultat de l’instruction sont des attributs d’instruction.  
  
 Attributs d’instruction sont définis avec **SQLSetStmtAttr** et leurs paramètres actuels sont récupérés avec **SQLGetStmtAttr**. N’est pas obligatoire qu’une application de définir des attributs d’instruction ; tous les attributs d’instruction ont des valeurs par défaut, dont certaines sont spécifiques au pilote.  
  
 Lorsqu’un attribut d’instruction peut être défini dépend de l’attribut lui-même. Les attributs d’instruction SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR et SQL_ATTR_USE_BOOKMARKS doivent être définis avant l’exécution de l’instruction. Les attributs d’instruction SQL_ATTR_ASYNC_ENABLE et SQL_ATTR_NOSCAN peuvent être définies à tout moment, mais ne sont pas appliquées tant que l’instruction est réutilisée. Les attributs d’instruction SQL_ATTR_MAX_LENGTH et SQL_ATTR_MAX_ROWS SQL_ATTR_QUERY_TIMEOUT peuvent être définies à tout moment, mais il est spécifique au pilote qu’ils sont appliqués avant l’instruction est réutilisée. Les autres attributs d’instruction peuvent être définies à tout moment.  
  
> [!NOTE]  
>  La possibilité de définir des attributs de l’instruction au niveau de la connexion en appelant **SQLSetConnectAttr** a été déconseillée dans ODBC 3. *x*. ODBC 3. *x* applications ne doivent jamais définir les attributs d’instruction au niveau de la connexion. ODBC 3. *x* pilotes doivent prennent uniquement en charge cette fonctionnalité s’ils doivent collaborer avec ODBC 2. *x* applications. Pour plus d’informations, consultez [SQLSetConnectOption mappage](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md) dans l’annexe g : pilote recommandations pour la compatibilité descendante.  
>   
>  Une exception concerne les attributs SQL_ATTR_METADATA_ID et SQL_ATTR_ASYNC_ENABLE, qui sont des attributs de connexion et les attributs d’instruction et peuvent être définies au niveau de la connexion ou le niveau de l’instruction.  
>   
>  Aucun des attributs d’instruction introduites dans ODBC 3. *x* (à l’exception de SQL_ATTR_METADATA_ID) peut être définie au niveau de la connexion.  
  
 Pour plus d’informations, consultez la [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md) description de fonction.
