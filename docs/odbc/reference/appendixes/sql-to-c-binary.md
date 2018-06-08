---
title: 'SQL C: binaire | Documents Microsoft'
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
- converting data from SQL to c types [ODBC], binary
- binary data type [ODBC]
- data conversions from SQL to C types [ODBC], binary
- binary data transfers [ODBC]
ms.assetid: 8c519072-ae4c-4d32-9d4e-775e3d3d6389
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b18b421173945869d5ff57fea8a7716aa0459fb5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-to-c-binary"></a>SQL à c : binaire
Les identificateurs pour les types de données ODBC SQL binaires sont :  
  
 SQL_BINARY  
  
 SQL_VARBINARY  
  
 SQL_LONGVARBINARY  
  
 Le tableau suivant présente les types de données à laquelle les données binaires de SQL peuvent être converties à ODBC C. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  
  
|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|(Longueur d’octet de données) \* 2 < *BufferLength*<br /><br /> (Longueur d’octet de données) \* 2 > = *BufferLength*|data<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données en octets|n/a<br /><br /> 01004|  
|SQL_C_WCHAR|(Caractère de longueur de données) \* 2 < *BufferLength*<br /><br /> (Caractère de longueur de données) \* 2 > = *BufferLength*|data<br /><br /> Données tronquées|Longueur des données de caractères<br /><br /> Longueur des données de caractères|n/a<br /><br /> 01004|  
|SQL_C_BINARY|Longueur en octets de données < = *BufferLength*<br /><br /> Longueur en octets de données > *BufferLength*|data<br /><br /> Données tronquées|Longueur des données en octets<br /><br /> Longueur des données en octets|n/a<br /><br /> 01004|  
  
 Lorsque les données binaires de SQL sont converties en données de type caractère C, chaque octet (8 bits) de la source de données est représentée par deux caractères ASCII. Ces caractères sont la représentation sous forme de caractères ASCII du nombre figurant dans sa forme hexadécimale. Par exemple, un 00000001 binaire est converti en « 01 » et un 11111111 binaire est converti en « FF ».  
  
 Le pilote convertit les octets individuels à des paires de chiffres hexadécimaux toujours et met fin à la chaîne de caractères avec un octet null. Pour cette raison, si *BufferLength* pair et est inférieure à la longueur des données converties, le dernier octet de la **TargetValuePtr* tampon n’est pas utilisé. (Les données converties nécessitent un nombre pair d’octets, l’octet suivant la dernière est un octet de valeur null, et le dernier octet ne peut pas être utilisé.)  
  
> [!NOTE]  
>  Les développeurs d’applications sont déconseillés à partir de la liaison de données SQL binaire à un type de données caractère C. Cette conversion est généralement inefficace et lente.
