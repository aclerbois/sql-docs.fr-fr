---
title: Mise en conformité du champ de descripteur | Documents Microsoft
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
- descriptor field conformance levels [ODBC]
- conformance levels [ODBC], descriptor field
- data sources [ODBC], conformance levels
- ODBC drivers [ODBC], conformance levels
ms.assetid: 6c29d93b-696c-4960-bff3-4d6bc41bc513
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b01b9da55da1fd3decb46e69dc073781427ccfab
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="descriptor-field-conformance"></a>Mise en conformité du champ de descripteur
Le tableau suivant indique le niveau de conformité de chaque champ d’en-tête descripteur ODBC, où cela est bien défini.  
  
|Fonction|Niveau de conformité|  
|--------------|-----------------------|  
|SQL_DESC_ALLOC_TYPE|Noyau|  
|SQL_DESC_ARRAY_SIZE|Noyau|  
|SQL_DESC_ARRAY_STATUS_PTR|Core (pour APD dpi et IRD) ; Niveau 1 (ARD)|  
|SQL_DESC_BIND_OFFSET_PTR|Noyau|  
|SQL_DESC_BIND_TYPE|Noyau|  
|SQL_DESC_COUNT|Noyau|  
|SQL_DESC_ROWS_PROCESSED_PTR|Noyau|  
  
 Le tableau suivant indique le niveau de conformité de chaque champ d’enregistrement du descripteur ODBC où cela est bien défini.  
  
|Fonction|Niveau de conformité|  
|--------------|-----------------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|Niveau 2|  
|SQL_DESC_BASE_COLUMN_NAME|Noyau|  
|SQL_DESC_BASE_TABLE_NAME|Niveau 1|  
|SQL_DESC_CASE_SENSITIVE|Noyau|  
|SQL_DESC_CATALOG_NAME|Niveau 2|  
|SQL_DESC_CONCISE_TYPE|Noyau|  
|SQL_DESC_DATA_PTR|Noyau|  
|CODE DE SQL_DESC_DATETIME_INTERVAL_|Principaux [1]|  
|SQL_DESC_DATETIME_INTERVAL_ PRÉCISION|Principaux [1]|  
|SQL_DESC_DISPLAY_SIZE|Noyau|  
|SQL_DESC_FIXED_PREC_SCALE|Noyau|  
|SQL_DESC_INDICATOR_PTR|Noyau|  
|SQL_DESC_LABEL|Niveau 2|  
|SQL_DESC_LENGTH|Noyau|  
|SQL_DESC_LITERAL_PREFIX|Noyau|  
|SQL_DESC_LITERAL_SUFFIX|Noyau|  
|SQL_DESC_LOCAL_TYPE_NAME|Noyau|  
|SQL_DESC_NAME|Noyau|  
|SQL_DESC_NULLABLE|Noyau|  
|SQL_DESC_OCTET_LENGTH|Noyau|  
|SQL_DESC_OCTET_LENGTH_PTR|Noyau|  
|SQL_DESC_PARAMETER_TYPE|Niveau du noyau/2 [2]|  
|SQL_DESC_PRECISION|Noyau|  
|SQL_DESC_ROWVER|Niveau 1|  
|SQL_DESC_SCALE|Noyau|  
|SQL_DESC_SCHEMA_NAME|Niveau 1|  
|SQL_DESC_SEARCHABLE|Noyau|  
|SQL_DESC_TABLE_NAME|Niveau 1|  
|SQL_DESC_TYPE|Noyau|  
|SQL_DESC_TYPE_NAME|Noyau|  
|SQL_DESC_UNNAMED|Noyau|  
|SQL_DESC_UNSIGNED|Noyau|  
|SQL_DESC_UPDATABLE|Noyau|  
  
 [1] prise en charge pour ces champs d’enregistrement est requis uniquement si le pilote prend en charge les types de données applicable.  
  
 [2] pour la mise en conformité au niveau du noyau, le pilote doit prendre en charge SQL_PARAM_INPUT. Pour la conformité d’interface de niveau 2, le pilote doit également prendre en charge SQL_PARAM_INPUT_OUTPUT et SQL_PARAM_OUTPUT.
