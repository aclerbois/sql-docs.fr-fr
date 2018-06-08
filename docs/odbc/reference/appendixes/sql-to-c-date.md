---
title: 'SQL à c : Date | Documents Microsoft'
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
- converting data from SQL to C types [ODBC], date
- date data type [ODBC]
- data conversions from SQL to C types [ODBC], date
ms.assetid: 703c7960-9cf4-4d7a-9920-53b29c184f97
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d99ebd45ffa463ccfd66bd751dd7415b35a8f5d1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-to-c-date"></a>SQL à c : Date
Identificateur de la date est de type de données SQL ODBC :  
  
 SQL_TYPE_DATE  
  
 Le tableau suivant présente les types de données à laquelle la date de données SQL peut-être être convertie à ODBC C. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > longueur en octets de caractères<br /><br /> 11 < = *BufferLength* < = longueur d’octet de caractère<br /><br /> *BufferLength* < 11|data<br /><br /> Données tronquées<br /><br /> Indéfini|10<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* > longueur des caractères<br /><br /> 11 < = *BufferLength* < = longueur en caractères<br /><br /> *BufferLength* < 11|data<br /><br /> Données tronquées<br /><br /> Indéfini|10<br /><br /> Longueur des données de caractères<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longueur en octets de données < = *BufferLength*<br /><br /> Longueur en octets de données > *BufferLength*|data<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_TYPE_DATE|Aucun [a]|data|6 (c)|n/a|  
|SQL_C_TYPE_TIMESTAMP|Aucun [a]|Données [b]|16 (c)|n/a|  
  
 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote part du principe que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] les champs d’heure de la structure d’horodatage sont définies à zéro.  
  
 [c] Il s’agit de la taille du type de données C correspondante.  
  
 Lors de la date de données SQL est convertie en données de type caractère C, la chaîne résultante est dans le «*aaaa*-*mm*-*jj*« format. Ce format n’est pas affecté par le paramètre de pays Windows®.
