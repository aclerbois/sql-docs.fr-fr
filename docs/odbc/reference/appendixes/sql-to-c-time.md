---
title: 'SQL à c : heure | Documents Microsoft'
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
- converting data from SQL to C types [ODBC], time
- time data type [ODBC]
- data conversions from SQL to C types [ODBC], time
ms.assetid: 6dc59973-7bb5-40f1-87c8-5bf68b3bf2ee
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b66aeca41cd705a21e7d6aa6d306a9032e86bba9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-to-c-time"></a>SQL à l’heure de c :
L’identificateur de l’heure est de type de données SQL ODBC :  
  
 SQL_TYPE_TIME  
  
 Le tableau suivant présente les types de données à laquelle les données SQL de temps peuvent être converties à ODBC C. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > longueur en octets de caractères<br /><br /> *9* <= *BufferLength* < = longueur d’octet de caractère<br /><br /> *BufferLength* < 9|data<br /><br /> Données tronquées [a]<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* > longueur des caractères<br /><br /> *9* <= *BufferLength* < = longueur en caractères<br /><br /> *BufferLength* < 9|data<br /><br /> Données tronquées [a]<br /><br /> Indéfini|Longueur des données de caractères<br /><br /> Longueur des données de caractères<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Longueur en octets de données < = *BufferLength*<br /><br /> Longueur en octets de données > *BufferLength*|data<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_TYPE_TIME|Aucun [b]|data|6 [d]|n/a|  
|SQL_C_TYPE_TIMESTAMP|Aucun [b]|Données (c)|16 [d]|n/a|  
  
 [a] les fractions de secondes de l’heure sont tronqués.  
  
 [b] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote part du principe que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [c] les champs de date de la structure d’horodatage sont définies à la date actuelle, et le champ de fractions de secondes de la structure d’horodateur est défini à zéro.  
  
 [d] Il s’agit de la taille du type de données C correspondante.  
  
 Lorsque les données SQL de temps sont converties en données de type caractère C, la chaîne résultante est dans le «*hh*:*mm*:*ss*« format. Ce format n’est pas affecté par le paramètre de pays Windows®.
