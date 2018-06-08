---
title: Types de données C | Documents Microsoft
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data types [ODBC], C data types
- C data types [ODBC], about C data types
- C data types [ODBC]
- C buffers [ODBC]
ms.assetid: b681d260-3dbb-47df-a616-4910d727add7
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 288ca6cbd5553b963131d34b8e63640518f70ef4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="c-data-types"></a>Types de données C
Types de données ODBC C indiquent le type de données de mémoires tampons de C utilisé pour stocker les données dans l’application.  
  
 Tous les pilotes doivent prendre en charge tous les types de données C. Cela est nécessaire car tous les pilotes doivent prendre en charge tous les types C à laquelle les types SQL pris en charge peuvent être convertis et tous les pilotes prennent en charge au moins un caractère de type SQL. Étant donné que le type de caractère SQL peut être converti vers et à partir de tous les types C, tous les pilotes doivent prendre en charge tous les types C.  
  
 Le type de données C est spécifié dans le **SQLBindCol** et **SQLGetData** fonctionne avec le *TargetType* argument et, dans le **SQLBindParameter** fonctionne avec le *ValueType* argument. Il peut également être spécifié en appelant **SQLSetDescField** pour définir le champ SQL_DESC_CONCISE_TYPE un ARD ou l’APD, ou en appelant **SQLSetDescRec** avec la *Type* argument (et le *sous-type* argument si nécessaire) et le *DescriptorHandle* défini pour le handle d’un ARD ou d’APD.  
  
 Les tableaux suivants répertorient les identificateurs de type valide pour les types de données C. Le tableau indique également le type de données ODBC C qui correspond à chaque identificateur et la définition de ce type de données.  
  
|Identificateur de type C|Typedef d’ODBC C|Type C|  
|-----------------------|--------------------|------------|  
|SQL_C_CHAR|SQLCHAR *|unsigned char *|  
|SQL_C_WCHAR|SQLWCHAR *|wchar_t *|  
|SQL_C_SSHORT [j]|SQLSMALLINT|short int|  
|SQL_C_USHORT [j]|SQLUSMALLINT|entier court non signé|  
|SQL_C_SLONG [j]|SQLINTEGER|nombre entier long|  
|SQL_C_ULONG [j]|SQLUINTEGER|nombre entier long non signé|  
|SQL_C_FLOAT|SQLREAL|float|  
|SQL_C_DOUBLE|SQLDOUBLE, SQLFLOAT|double|  
|SQL_C_BIT|SQLCHAR|char non signé|  
|SQL_C_STINYINT [j]|SQLSCHAR|char signé|  
|SQL_C_UTINYINT [j]|SQLCHAR|char non signé|  
|SQL_C_SBIGINT|SQLBIGINT|_int64 [h]|  
|SQL_C_UBIGINT|SQLUBIGINT|non signé _int64 [h]|  
|SQL_C_BINARY|SQLCHAR *|unsigned char *|  
|SQL_C_BOOKMARK [i]|SIGNET|entier long non signé [d]|  
|SQL_C_VARBOOKMARK|SQLCHAR *|unsigned char *|  
|Tous les types de données d’intervalle C|SQL_INTERVAL_STRUCT|Consultez le [C intervalle Structure](../../../odbc/reference/appendixes/c-interval-structure.md) section, plus loin dans cette annexe.|  
  
 **Identificateur de type C** SQL_C_TYPE_DATE (c)  
  
 **ODBC C typedef** SQL_DATE_STRUCT  
  
 **Type C**  
  
```  
struct tagDATE_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;    
} DATE_STRUCT;[a]  
```  
  
 **Identificateur de type C** SQL_C_TYPE_TIME (c)  
  
 **ODBC C typedef** SQL_TIME_STRUCT  
  
 **Type C**  
  
```  
struct tagTIME_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
} TIME_STRUCT;[a]  
```  
  
 **Identificateur de type C** SQL_C_TYPE_TIMESTAMP (c)  
  
 **ODBC C typedef** SQL_TIMESTAMP_STRUCT  
  
 **Type C**  
  
```  
struct tagTIMESTAMP_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;[b]   
} TIMESTAMP_STRUCT;[a]  
```  
  
 **Identificateur de type C** SQL_C_NUMERIC  
  
 **ODBC C typedef** SQL_NUMERIC_STRUCT  
  
 **Type C**  
  
```  
struct tagSQL_NUMERIC_STRUCT {  
   SQLCHAR precision;  
   SQLSCHAR scale;  
   SQLCHAR sign[g];  
   SQLCHAR val[SQL_MAX_NUMERIC_LEN];[e], [f]   
} SQL_NUMERIC_STRUCT;  
```  
  
 **Identificateur de type C** SQL_C_GUID  
  
 **ODBC C typedef** SQLGUID  
  
 **Type C**  
  
```  
struct tagSQLGUID {  
   DWORD Data1;  
   WORD Data2;  
   WORD Data3;  
   BYTE Data4[8];  
} SQLGUID;[k]  
```  
  
 [a] les valeurs de l’année mois, jour, heure, minute et deuxième champs dans les types de données datetime C doivent être conforme aux contraintes du calendrier grégorien. (Consultez [contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md) plus loin dans cette annexe.)  
  
 [b] la valeur du champ de fraction est le nombre de milliardièmes de seconde et comprise entre 0 et 999 999 999 (1 inférieur à 1 milliard). Par exemple, la valeur du champ fraction d’une demi-seconde est 500,000,000, pour un millième de seconde (un millième de seconde) est 1 000 000, pour un million de seconde (une microseconde) est 1 000 et pour un milliardième de seconde (une nanoseconde) est 1.  
  
 [c] dans ODBC 2. *x*, les types de données date, time et timestamp C sont SQL_C_DATE, SQL_C_TIME et SQL_C_TIMESTAMP.  
  
 [d] ODBC 3 *.x* SQL_C_VARBOOKMARK, SQL_C_BOOKMARK pas les applications doivent utiliser. Lorsqu’un ODBC 3 *.x* application fonctionne avec une API ODBC 2. *x* pilote, la version 3 ODBC *.x* du Gestionnaire de pilotes mappera SQL_C_VARBOOKMARK à SQL_C_BOOKMARK.  
  
 [e] un numéro est stocké dans le *val* champ de la structure SQL_NUMERIC_STRUCT comme un entier à l’échelle, en mode de primauté peu (l’octet de plus à gauche en cours de l’octet le moins significatif). Par exemple, le nombre 10.001 base 10, avec une échelle de 4, est mis à l’échelle à un entier de 100010. Comme il s’agit 186AA au format hexadécimal, la valeur dans SQL_NUMERIC_STRUCT serait « AA 86 01 00 00... 00", avec le nombre d’octets défini par le SQL_MAX_NUMERIC_LEN **#define**.  
  
 Pour plus d’informations sur **SQL_NUMERIC_STRUCT**, consultez [comment faire : la récupération des données numériques avec SQL_NUMERIC_STRUCT](retrieve-numeric-data-sql-numeric-struct-kb222831.md).  
  
 [f] les champs de précision et l’échelle des données SQL_C_NUMERIC type areused pour l’entrée d’une application et pour la sortie à partir du pilote à l’application. Lorsque le pilote écrit une valeur numérique dans le SQL_NUMERIC_STRUCT, il utilise sa propre valeur par défaut spécifiques au pilote comme valeur pour le *précision* champ et il utilise la valeur dans le champ SQL_DESC_SCALE du descripteur d’application (qui est par défaut 0) pour le *échelle* champ. Une application peut fournir ses propres valeurs pour la précision et l’échelle en définissant les champs SQL_DESC_PRECISION et SQL_DESC_SCALE du descripteur de l’application.  
  
 [g], le champ de signe est 1 si elle est positif, 0 si elle est négative.  
  
 [h] _int64 ne peuvent pas être fournis par certains compilateurs.  
  
 [i] _SQL_C_BOOKMARK a été déconseillée dans ODBC 3 *.x*.  
  
 [j] _SQL_C_SHORT, SQL_C_LONG et SQL_C_TINYINT ont été remplacés dans ODBC en types signés et non signés : SQL_C_SSHORT et SQL_C_USHORT, SQL_C_SLONG et SQL_C_ULONG et SQL_C_STINYINT et SQL_C_UTINYINT. Un ODBC 3 *.x* pilote doit fonctionner avec ODBC 2. *x* applications doivent prendre en charge SQL_C_SHORT, SQL_C_LONG et SQL_C_TINYINT, étant donné que lorsqu’elles sont appelées, le Gestionnaire de pilotes transmet via le pilote.  
  
 [k] SQL_C_GUID peuvent être convertis uniquement aux SQL_CHAR ou SQL_WCHAR.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Structures d’entiers 64 bits](../../../odbc/reference/appendixes/64-bit-integer-structures.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)
