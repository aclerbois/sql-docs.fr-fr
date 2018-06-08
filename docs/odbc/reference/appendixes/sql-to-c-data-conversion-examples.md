---
title: SQL aux exemples de Conversion de données C | Documents Microsoft
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
- data conversions from SQL to C types [ODBC], examples
- converting data from SQL to C types [ODBC], examples
ms.assetid: 0190c76c-7f9b-42f4-be9d-cef7284840fd
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1c23e98067cafefbf44c39633aa8c11effa6594f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-to-c-data-conversion-examples"></a>SQL aux exemples de Conversion de données C
Les exemples présentés dans le tableau suivant illustrent la façon dont le pilote SQL convertit les données en données C :  
  
|Type SQL<br /><br /> identificateur|Données SQL<br /><br /> valeur|Type C<br /><br /> identificateur|Buffer<br /><br /> length|**TargetValuePtr*|SQLSTATE|  
|-----------------------------|------------------------|---------------------------|-----------------------|------------------------|--------------|  
|SQL_CHAR|abcdef|SQL_C_CHAR|7|abcdef\0 [a]|n/a|  
|SQL_CHAR|abcdef|SQL_C_CHAR|6|abcde\0 [a]|01004|  
|SQL_DECIMAL|1234.56|SQL_C_CHAR|8|1234.56\0 [a]|n/a|  
|SQL_DECIMAL|1234.56|SQL_C_CHAR|5|1234\0 [a]|01004|  
|SQL_DECIMAL|1234.56|SQL_C_CHAR|4|----|22003|  
|SQL_DECIMAL|1234.56|SQL_C_FLOAT|ignoré|1234.56|n/a|  
|SQL_DECIMAL|1234.56|SQL_C_SSHORT|ignoré|1234|01 S 07|  
|SQL_DECIMAL|1234.56|SQL_C_STINYINT|ignoré|----|22003|  
SQL_DOUBLE|1.2345678|SQL_C_DOUBLE|ignoré|1.2345678|n/a|  
|SQL_DOUBLE|1.2345678|SQL_C_FLOAT|ignoré|1.234567|n/a|  
|SQL_DOUBLE|1.2345678|SQL_C_STINYINT|ignoré|1|n/a|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|11|1992-12-31\0 [a]|n/a|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|10|-----|22003|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_TIMESTAMP|ignoré|1992,12,31, 0,0,0,0 [b]|n/a|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|23|1992-12-31 23:45:55.12\0 [a]|n/a|  
SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|22|1992-12-31 23:45:55.1\0 [a]|01004|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23:45:55.12|SQL_C_CHAR|18|----|22003|  
  
 [a] « \0 » représente un octet de valeur null. Le pilote toujours null-termine les données SQL_C_CHAR.  
  
 [b] les nombres dans cette liste sont les nombres stockés dans les champs de la structure TIMESTAMP_STRUCT.
