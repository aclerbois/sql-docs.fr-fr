---
title: 'C en SQL : caractère | Documents Microsoft'
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
- character data type [ODBC]
- data conversions from C to SQL types [ODBC], character
- converting data from c to SQL types [ODBC], character
ms.assetid: be66188a-ebdb-4c9e-af72-c379886766fa
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3f6b47b9f7424736d066e1b805ba7aca7c3540e4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="c-to-sql-character"></a>C en SQL : caractère
Les identificateurs pour le caractère de type de données ODBC C sont :  
  
 SQL_C_CHAR  
  
 SQL_C_WCHAR  
  
 Le tableau suivant présente les types de données à laquelle les données de caractères C peuvent être converties à l’ODBC SQL. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de C en Types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
> [!NOTE]  
>  Lorsque les données de caractères C sont converties en données Unicode SQL, la longueur des données Unicode doit être un nombre pair.  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longueur en octets de données < = longueur de la colonne.<br /><br /> Longueur en octets de données > longueur de colonne.|n/a<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longueur des données de caractères < = longueur de la colonne.<br /><br /> Longueur des données de caractères > longueur de colonne.|n/a<br /><br /> 22001|  
|SQL_DECIMAL<br /><br /> SQL_NUMERIC<br /><br /> SQL_TINYINT<br /><br /> SQL_SMALLINT<br /><br /> SQL_INTEGER SQL_BIGINT|Converti sans troncation de données<br /><br /> Les données converties avec la troncation des fractions [e]<br /><br /> La conversion de données entraînerait une perte de chiffres de l’ensemble (par opposition aux fractions de seconde) [e]<br /><br /> Valeur de données n’est pas un *littéral numérique*|n/a<br /><br /> 22001<br /><br /> 22001<br /><br /> 22018|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|Les données sont dans la plage du type de données à laquelle le nombre est en cours de conversion<br /><br /> Données sont trouve en dehors de la plage du type de données à laquelle le nombre est en cours de conversion<br /><br /> Valeur de données n’est pas un *littéral numérique*|n/a<br /><br /> 22003<br /><br /> 22018|  
|SQL_BIT|Les données sont 0 ou 1<br /><br /> Données sont supérieures à 0, inférieure à 2 et non égal à 1<br /><br /> Données sont inférieur à 0 ou supérieur ou égal à 2<br /><br /> Données ne sont pas un *littéral numérique*|n/a<br /><br /> 22001<br /><br /> 22003<br /><br /> 22018|  
|SQL_BINARY<br /><br /> SQL_VARBINARY<br /><br /> SQL_LONGVARBINARY|(Longueur d’octet de données) / 2 < = longueur d’octet de colonne<br /><br /> (Longueur d’octet de données) / 2 > longueur d’octet de colonne<br /><br /> Valeur de données n’est pas une valeur hexadécimale|n/a<br /><br /> 22001<br /><br /> 22018|  
|SQL_TYPE_DATE|Valeur de données est valide *littéral de date ODBC*<br /><br /> Valeur de données est valide *ODBC-timestamp-literal*; partie heure est égale à zéro<br /><br /> Valeur de données est valide *ODBC-timestamp-literal*; partie heure est différent de zéro [a]<br /><br /> Valeur de données n’est pas valide *littéral de date ODBC* ou *littéral de l’horodatage ODBC*|n/a<br /><br /> n/a<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIME|Valeur de données est valide *littéral d’heure ODBC*<br /><br /> Valeur de données est valide *ODBC-timestamp-literal*; fractions de seconde partie des secondes est zéro [b]<br /><br /> Valeur de données est valide *ODBC-timestamp-literal*; fractions de seconde partie des secondes est différente de zéro [b]<br /><br /> Valeur de données n’est pas valide *littéral d’heure ODBC* ou *littéral de l’horodatage ODBC*|n/a<br /><br /> n/a<br /><br /> 22008<br /><br /> 22018|  
|SQL_TYPE_TIMESTAMP|Valeur de données est valide *ODBC-timestamp-literal*; fractions de seconde partie secondes ne pas tronqué<br /><br /> Valeur de données est valide *ODBC-timestamp-literal*; fractions de seconde partie secondes tronqué<br /><br /> Valeur de données est valide *littéral de date ODBC*(c)<br /><br /> Valeur de données est valide *littéral d’heure ODBC*[d]<br /><br /> Valeur de données n’est pas valide *littéral de date ODBC*, *littéral d’heure ODBC*, ou *littéral de l’horodatage ODBC*|n/a<br /><br /> 22008<br /><br /> n/a<br /><br /> n/a<br /><br /> 22018|  
|Tous les types d’intervalle SQL|Valeur de données est valide *la valeur d’intervalle*; n’est pas tronqué<br /><br /> Valeur de données est valide *la valeur d’intervalle*; la valeur dans un des champs est tronquée.<br /><br /> La valeur de données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22015<br /><br /> 22018|  
  
 [a] la partie heure de l’horodatage est tronquée.  
  
 [b] la partie de la date de l’horodatage est ignorée.  
  
 [c] la partie heure de l’horodatage est définie à zéro.  
  
 [d] la partie de la date de l’horodatage est définie à la date actuelle.  
  
 [e] la source de données/pilote efficacement attend la chaîne entière a été reçue (même si les données de caractères sont envoyées en fragments par les appels à **SQLPutData**) avant de tenter d’effectuer la conversion.  
  
 Lorsque les données de caractères C sont converti en numérique, date, heure ou timestamp de données SQL, de début et les espaces à droite sont ignorés.  
  
 Lorsque les données de caractères C sont converties en binaire de données SQL, chaque deux octets de données caractères sont converties en un seul octet (8 bits) de données binaires. Chaque deux octets de données caractère représente un nombre au format hexadécimal. Par exemple, « 01 » est convertie en un binaire 00000001 et « FF » est converti en un 11111111 binaire.  
  
 Le pilote convertit les paires de chiffres hexadécimaux en octets individuels toujours et ignore l’octet de valeur null. Pour cette raison, si la longueur de la chaîne de caractères est impaire, le dernier octet de la chaîne (à l’exclusion de l’octet de valeur null, le cas échéant) n’est pas converti.  
  
> [!NOTE]  
>  Les développeurs d’applications sont déconseillés à partir de la liaison de données de caractères C à un type de données SQL binaire. Cette conversion est généralement inefficace et lente.
