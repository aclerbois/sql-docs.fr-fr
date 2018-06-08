---
title: 'C en SQL : numérique | Documents Microsoft'
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
- numeric data type [ODBC], converting
- data conversions from C to SQL types [ODBC], numeric
- converting data from c to SQL types [ODBC], numeric
ms.assetid: af4095ff-06c3-4b04-83bf-19f9ee098dc2
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0b47617c9905571095d09f06aab1ac5cceb9b9eb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="c-to-sql-numeric"></a>C en SQL : numérique
Les identificateurs pour les types de données ODBC C numériques sont :  
  
 SQL_C_STINYINT  
  
 SQL_C_SLONG  
  
 SQL_C_UTINYINT  
  
 SQL_C_ULONG  
  
 SQL_C_TINYINT  
  
 SQL_C_LONG  
  
 SQL_C_SSHORT  
  
 SQL_C_FLOAT  
  
 SQL_C_USHORT  
  
 SQL_C_DOUBLE  
  
 SQL_C_SHORT  
  
 SQL_C_NUMERIC  
  
 SQL_C_SBIGINT  
  
 SQL_C_UBIGINT  
  
 Le tableau suivant présente les types de données à laquelle les données C numériques peuvent être converties à ODBC SQL. Pour obtenir une explication des colonnes et des termes du contrat de la table, consultez [conversion des données à partir de C en Types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Nombre de chiffres < = longueur d’octet de colonne<br /><br /> Nombre de chiffres > longueur d’octet de colonne|n/a<br /><br /> 22001|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Nombre de caractères < = longueur de colonne<br /><br /> Nombre de caractères > longueur de colonne|n/a<br /><br /> 22001|  
|SQL_DECIMAL [b]<br /><br /> SQL_NUMERIC [b]<br /><br /> SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b]<br /><br /> SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b]|Données converti sans troncation ou avec tronquées de chiffres fractionnaires<br /><br /> Les données converties avec la troncation des chiffres entiers|n/a<br /><br /> 22003|  
|SQL_REAL<br /><br /> SQL_FLOAT<br /><br /> SQL_DOUBLE|Les données sont dans la plage du type de données à laquelle le nombre est en cours de conversion<br /><br /> Données sont trouve en dehors de la plage du type de données à laquelle le nombre est en cours de conversion|n/a<br /><br /> 22003|  
|SQL_BIT|Les données sont 0 ou 1<br /><br /> Données sont supérieures à 0, inférieure à 2 et non égal à 1<br /><br /> Données sont inférieur à 0 ou supérieur ou égal à 2|n/a<br /><br /> 22001<br /><br /> 22003|  
|SQL_INTERVAL_YEAR [a]<br /><br /> SQL_INTERVAL_MONTH [a]<br /><br /> SQL_INTERVAL_DAY [a]<br /><br /> SQL_INTERVAL_HOUR [a]<br /><br /> SQL_INTERVAL_MINUTE [a]<br /><br /> SQL_INTERVAL_SECOND [a]|Données ne pas tronquées.<br /><br /> Données ont été tronquées.|n/a<br /><br /> 22015|  
  
 [a] ces conversions sont prises en charge uniquement pour les types de données numériques exactes (SQL_C_STINYINT, SQL_C_UTINYINT, SQL_C_SSHORT, SQL_C_USHORT, SQL_C_SLONG, SQL_C_ULONG ou SQL_C_NUMERIC). Elles ne sont pas prises en charge pour les types de données numérique approximatif (SQL_C_FLOAT ou SQL_C_DOUBLE). Impossible de convertir les types de données C numériques exacts à un intervalle de type SQL dont la précision intervalle n’est pas un champ unique.  
  
 [b] dans le cas de « n/a », un pilote peut renvoyer éventuellement SQL_SUCCESS_WITH_INFO et 01 s 07 lorsqu’il existe une troncation de fractions de seconde.  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion des données à partir des types de données C numériques et suppose que la taille du tampon de données est la taille du type de données C numérique. La valeur de l’indicateur/longueur est passée dans le *StrLen_or_Ind* argument dans **SQLPutData** et dans la mémoire tampon spécifiée avec la *StrLen_or_IndPtr* argument dans **SQLBindParameter**. Le tampon de données est spécifié avec la *DataPtr* argument dans **SQLPutData** et *ParameterValuePtr* argument dans **SQLBindParameter**.
