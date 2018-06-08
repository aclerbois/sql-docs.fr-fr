---
title: Taille de la colonne | Documents Microsoft
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
- data types [ODBC], column size
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
ms.assetid: 541b83ab-b16d-4714-bcb2-3c3daa9a963b
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1ce38cfd836aa9328838096763ba9191c4a9ec30
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-size"></a>Taille de la colonne
La taille de colonne (ou paramètre) des types de données numérique est définie en tant que le nombre maximal de chiffres utilisés par le type de données de la colonne ou du paramètre ou la précision des données. Pour les types de caractères, il s’agit la longueur en caractères des données ; pour les types de données binaires, taille de la colonne est définie comme la longueur en octets des données. Pour l’heure, timestamp et tous les types d’intervalle, il s’agit du nombre de caractères dans la représentation sous forme de caractères de ces données. La taille de colonne définie pour chaque type de données SQL concis est indiquée dans le tableau suivant.  
  
|Identificateur de type SQL|Taille de la colonne|  
|-------------------------|-----------------|  
|Tous les types de caractères [a] [b].|La taille de la colonne définie ou maximale, en caractères de la colonne ou du paramètre (tel qu’il est contenu dans le champ de descripteur SQL_DESC_LENGTH). Par exemple, la taille de la colonne d’une colonne de caractères codés sur définie en tant que char (10) est 10.|  
|SQL_DECIMAL SQL_NUMERIC|Le nombre de chiffres défini. Par exemple, la précision d’une colonne définie en tant que NUMERIC(10,3) est 10.|  
|SQL_BIT (c)|1|  
|SQL_TINYINT (c)|3|  
|SQL_SMALLINT (c)|5|  
|SQL_INTEGER (c)|10|  
|SQL_BIGINT (c)|19 (si signée) ou 20 (si non signé)|  
|SQL_REAL (c)|7|  
|SQL_FLOAT (c)|15|  
|SQL_DOUBLE (c)|15|  
|Tous les types binaires [a] [b].|Longueur maximale ou définie en octets de la colonne ou du paramètre. Par exemple, la longueur d’une colonne définie comme BINARY (10) est 10.|  
|SQL_TYPE_DATE (c)|10 (le nombre de caractères dans le *aaaa-mm-jj* format).|  
|SQL_TYPE_TIME (c)|8 (le nombre de caractères dans le *hh-mm-ss* format), ou 9 + *s* (le nombre de caractères dans le *hh : mm :*[.fff...] format, où *s* est la précision en secondes).|  
|SQL_TYPE_TIMESTAMP|16 (le nombre de caractères dans le *aaaa-mm-jj hh* format)<br /><br /> 19 (le nombre de caractères dans le *aaaa-mm-jj* *hh : mm :* format)<br /><br /> ou<br /><br /> 20 + *s* (le nombre de caractères dans le *aaaa-mm-jj hh : mm :*[.fff...] format, où *s* est la précision en secondes).|  
|SQL_INTERVAL_SECOND|Où *p* est l’intervalle de précision d’en-tête et *s* est la précision en secondes, *p* (si *s*= 0) ou *p*+*s*+ 1 (si *s*> 0). [ d]|  
|SQL_INTERVAL_DAY_TO_SECOND|Où *p* est l’intervalle de précision d’en-tête et *s* est la précision en secondes, 9 +*p* (si *s*= 0) ou 10 +*p*+*s* (si *s*> 0). [ d]|  
|SQL_INTERVAL_HOUR_TO_SECOND|Où *p* est l’intervalle de précision d’en-tête et *s* est la précision en secondes, 6 +*p* (si *s*= 0) ou 7 +*p*+*s* (si *s*> 0). [ d]|  
|SQL_INTERVAL_MINUTE_TO_SECOND|Où *p* est l’intervalle de précision d’en-tête et *s* est la précision en secondes, 3 +*p* (si *s*= 0) ou 4 +*p*+*s* (si *s*> 0). [ d]|  
|SQL_INTERVAL_YEAR SQL_INTERVAL_MONTH SQL_INTERVAL_DAY SQL_INTERVAL_HOUR SQL_INTERVAL_MINUTE|*p*, où *p* est la précision interval. [ d]|  
|SQL_INTERVAL_YEAR_TO_MONTH SQL_INTERVAL_DAY_TO_HOUR|3 +*p*, où *p* est la précision interval. [ d]|  
|SQL_INTERVAL_DAY_TO_MINUTE|6 +*p*, où *p* est la précision interval. [ d]|  
|SQL_INTERVAL_HOUR_TO_MINUTE|3 +*p*, où *p* est la précision interval. [ d]|  
|SQL_GUID|36 (le nombre de caractères dans le *aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee* format)|  
  
 [a] pour un ODBC 1.0 application appelant **SQLSetParam** dans un pilote ODBC 2.0 et pour une application ODBC 2.0 appelant **SQLBindParameter** dans un pilote ODBC version 1.0, lorsque \* *StrLen_or_IndPtr* est SQL_DATA_AT_EXEC pour un type SQL_LONGVARCHAR ou SQL_LONGVARBINARY, *ColumnSize* doit être définie à la longueur totale des données à envoyer, pas la précision tel que défini dans cette table.  
  
 [b] si le pilote ne peut pas déterminer la longueur de colonne ou un paramètre pour un type de variable, elle retourne SQL_NO_TOTAL.  
  
 [c] le *ColumnSize* argument de **SQLBindParameter** est ignorée pour ce type de données.  
  
 [d] pour les règles générales concernant la longueur de colonne dans les types d’intervalle, consultez [longueur de Type de données de l’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md), plus haut dans cette annexe.  
  
 Les valeurs retournées pour la taille de la colonne (ou paramètre) ne correspondent pas aux valeurs dans n’importe quel un champ de descripteur. Les valeurs peuvent provenir à partir de la SQL_DESC_PRECISION ou le champ SQL_DESC_LENGTH, selon le type de données, comme indiqué dans le tableau suivant.  
  
|Type SQL|Champ de descripteur correspondant à<br /><br /> taille de la colonne ou du paramètre|  
|--------------|--------------------------------------------------------------------|  
|Tous les types binaires et caractères|LENGTH|  
|Tous les types numériques|PRECISION|  
|Tous les types datetime et interval|LENGTH|  
|SQL_BIT|LENGTH|
