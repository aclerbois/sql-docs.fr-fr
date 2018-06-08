---
title: Attribut de conformité | Documents Microsoft
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
- data sources [ODBC], conformance levels
- ODBC drivers [ODBC], conformance levels
- conformance levels [ODBC], attribute
- attribute conformance levels [ODBC]
ms.assetid: 34fea100-10f9-46d5-bc50-3aa867b70f24
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2c765982a35fd41fc36fdc82ddbd3434b2d90c07
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="attribute-conformance"></a>Mise en conformité de l’attribut
Le tableau suivant indique le niveau de conformité de chaque attribut d’environnement ODBC, où cela est bien défini.  
  
|Fonction|Niveau de conformité|  
|--------------|-----------------------|  
|SQL_ATTR_CONNECTION_POOLING|--[1]|  
|SQL_ATTR_CP_MATCH|--[1]|  
|SQL_ATTR_ODBC_VER|Noyau|  
|SQL_ATTR_OUTPUT_NTS|--[1]|  
  
 [1] Ceci est une fonctionnalité facultative et par conséquent ne fait pas partie des niveaux de conformité.  
  
 Le tableau suivant indique le niveau de conformité de chaque attribut de connexion ODBC, où cela est bien défini.  
  
|Fonction|Niveau de conformité|  
|--------------|-----------------------|  
|SQL_ATTR_ACCESS_MODE|Noyau|  
|SQL_ATTR_ASYNC_ENABLE|Niveau 2 1/niveau [1]|  
|SQL_ATTR_AUTO_IPD|Niveau 2|  
|SQL_ATTR_AUTOCOMMIT|Niveau 1|  
|SQL_ATTR_CONNECTION_DEAD|Niveau 1|  
|SQL_ATTR_CONNECTION_TIMEOUT|Niveau 2|  
|SQL_ATTR_CURRENT_CATALOG|Niveau 2|  
|SQL_ATTR_LOGIN_TIMEOUT|Niveau 2|  
|SQL_ATTR_ODBC_CURSORS|Noyau|  
|SQL_ATTR_PACKET_SIZE|Niveau 2|  
|SQL_ATTR_QUIET_MODE|Noyau|  
|SQL_ATTR_TRACE|Noyau|  
|SQL_ATTR_TRACEFILE|Noyau|  
|SQL_ATTR_TRANSLATE_LIB|Noyau|  
|SQL_ATTR_TRANSLATE_OPTION|Noyau|  
|SQL_ATTR_TXN_ISOLATION|Niveau 2 1/niveau [2]|  
  
 [1] les applications qui prennent en charge le comportement asynchrone au niveau de la connexion (requis pour le niveau 1) doivent prendre en charge la définition de cet attribut avec SQL_TRUE en appelant **SQLSetConnectAttr**; l’attribut ne sont pas nécessairement être définie à une valeur autre que sa valeur par défaut via **SQLSetStmtAttr**. Les applications qui prennent en charge le comportement asynchrone au niveau de l’instruction (requis pour le niveau 2) doivent prendre en charge la définition de cet attribut avec SQL_TRUE à l’aide d’une fonction.  
  
 [2] pour la conformité d’interface de niveau 1, le pilote doit prendre en charge une seule valeur, en plus de la valeur par défaut définie par le pilote (disponible en appelant **SQLGetInfo** avec l’option SQL_DEFAULT_TXN_ISOLATION). Conformité d’interface de niveau 2, le pilote doit également prendre en charge SQL_TXN_SERIALIZABLE.  
  
 Le tableau suivant indique le niveau de conformité de chaque attribut d’instruction ODBC, où cela est bien défini.  
  
|Fonction|Niveau de conformité|  
|--------------|-----------------------|  
|SQL_ATTR_APP_PARAM_DESC|Noyau|  
|SQL_ATTR_APP_ROW_DESC|Noyau|  
|SQL_ATTR_ASYNC_ENABLE|Niveau 2 1/niveau [1]|  
|SQL_ATTR_CONCURRENCY|Niveau 2 1/niveau [2]|  
|SQL_ATTR_CURSOR_SCROLLABLE|Niveau 1|  
|SQL_ATTR_CURSOR_SENSITIVITY|Niveau 2|  
|SQL_ATTR_CURSOR_TYPE|Niveau du noyau/2 [3]|  
|SQL_ATTR_ENABLE_AUTO_IPD|Niveau 2|  
|SQL_ATTR_FETCH_BOOKMARK_PTR|Niveau 2|  
|SQL_ATTR_IMP_PARAM_DESC|Noyau|  
|SQL_ATTR_IMP_ROW_DESC|Noyau|  
|SQL_ATTR_KEYSET_SIZE|Niveau 2|  
|SQL_ATTR_MAX_LENGTH|Niveau 1|  
|SQL_ATTR_MAX_ROWS|Niveau 1|  
|SQL_ATTR_METADATA_ID|Noyau|  
|SQL_ATTR_NOSCAN|Noyau|  
|SQL_ATTR_PARAM_BIND_OFFSET_PTR|Noyau|  
|SQL_ATTR_PARAM_BIND_TYPE|Noyau|  
|SQL_ATTR_PARAM_OPERATION_PTR|Noyau|  
|SQL_ATTR_PARAM_STATUS_PTR|Noyau|  
|SQL_ATTR_PARAMS_PROCESSED_PTR|Noyau|  
|SQL_ATTR_PARAMSET_SIZE|Noyau|  
|SQL_ATTR_QUERY_TIMEOUT|Niveau 2|  
|SQL_ATTR_RETRIEVE_DATA|Niveau 1|  
|SQL_ATTR_ROW_ARRAY_SIZE|Noyau|  
|SQL_ATTR_ROW_BIND_OFFSET_PTR|Noyau|  
|SQL_ATTR_ROW_BIND_TYPE|Noyau|  
|SQL_ATTR_ROW_NUMBER|Niveau 1|  
|SQL_ATTR_ROW_OPERATION_PTR|Niveau 1|  
|SQL_ATTR_ROW_STATUS_PTR|Noyau|  
|SQL_ATTR_ROWS_FETCHED_PTR|Noyau|  
|SQL_ATTR_SIMULATE_CURSOR|Niveau 2|  
|SQL_ATTR_USE_BOOKMARKS|Niveau 2|  
  
 [1] les applications qui prennent en charge le comportement asynchrone au niveau de la connexion (requis pour le niveau 1) doivent prendre en charge la définition de cet attribut avec SQL_TRUE en appelant **SQLSetConnectAttr**; l’attribut ne sont pas nécessairement être définie à une valeur autre que sa valeur par défaut via **SQLSetStmtAttr**. Les applications qui prennent en charge le comportement asynchrone au niveau de l’instruction (requis pour le niveau 2) doivent prendre en charge la définition de cet attribut avec SQL_TRUE à l’aide d’une fonction.  
  
 [2] pour la conformité d’interface de niveau 2, le pilote doit prendre en charge SQL_CONCUR_READ_ONLY et au moins une autre valeur.  
  
 [3] pour la conformité d’interface de niveau 1, le pilote doit prendre en charge SQL_CURSOR_FORWARD_ONLY et au moins une autre valeur. Pour la conformité d’interface de niveau 2, le pilote doit prendre en charge toutes les valeurs définies dans ce document.
