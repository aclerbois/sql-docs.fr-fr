---
title: Types de données DateTime | Documents Microsoft
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
- time data type [ODBC]
- datetime data types [ODBC]
- date data type [ODBC]
- data types [ODBC], date
- backward compatibility [ODBC], datetime data types
- timestamp data type [ODBC]
- data types [ODBC], timestamp
- data types [ODBC], backward compatibility
- compatibility [ODBC], datetime data types
- data types [ODBC], time
ms.assetid: 6b9363c9-04bf-4492-a210-7aa15dea4af8
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 20d53f9d19845cb55af304ce08a321bfc07d44b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datetime-data-types"></a>Types de données DateTime
Dans ODBC 3 *.x*, les identificateurs de date, heure et types de données timestamp SQL ont changé depuis SQL_DATE, SQL_TIME et SQL_TIMESTAMP (avec des instances de **#define** dans le fichier d’en-tête de 9, 10 et 11) SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP (avec des instances de **#define** dans le fichier d’en-tête de 91, 92 et 93) , respectivement. Le type C correspondant identificateurs ont changé depuis SQL_C_DATE, SQL_C_TIME et SQL_C_TIMESTAMP SQL_C_TYPE_DATE, SQL_C_TYPE_TIME et SQL_C_TYPE_TIMESTAMP, respectivement et les instances de **#define** ont été modifiés en conséquence.  
  
 La taille de la colonne et de chiffres décimaux retournaient pour les types de données datetime SQL dans ODBC 3 *.x* sont le même que la précision et l’échelle retourné pour eux dans ODBC 2. *x*. Ces valeurs sont différentes des valeurs dans les champs de descripteur SQL_DESC_PRECISION et SQL_DESC_SCALE. (Pour plus d’informations, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.)  
  
 Ces modifications affectent les **SQLDescribeCol**, **SQLDescribeParam**, et **SQLColAttributes**; **SQLBindCol**, **SQLBindParameter**, et **SQLGetData**; et **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLStatistics**, et **SQLSpecialColumns**.  
  
 Un ODBC 3 *.x* pilote traite les appels de fonction répertoriés dans le paragraphe précédent en fonction du paramètre de l’attribut d’environnement SQL_ATTR_ODBC_VERSION. Pour **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLSpecialColumns**, et **SQLStatistics**si SQL_ATTR_ODBC_VERSION est défini à SQL_OV_ODBC3, les fonctions retournent SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP dans le champ DATA_TYPE. La colonne COLUMN_SIZE (dans le jeu de résultats retourné par **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, et **SQLSpecialColumns**) contient la précision binaire pour le type numérique approximatif. La colonne NUM_PREC_RADIX (dans le jeu de résultats retourné par **SQLColumns**, **SQLGetTypeInfo**, et **SQLProcedureColumns**) contient une valeur de 2. Si SQL_ATTR_ODBC_VERSION est définie sur SQL_OV_ODBC2, puis les fonctions retournent SQL_DATE, SQL_TIME et SQL_TIMESTAMP dans le champ DATA_TYPE, la colonne COLUMN_SIZE contient la précision décimale pour le type numérique approximatif, et la colonne NUM_PREC_RADIX contient une valeur de 10.  
  
 Lorsque tous les types de données sont demandés dans un appel à **SQLGetTypeInfo**, le jeu de résultats retourné par la fonction contient à la fois SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP tel que défini dans ODBC 3 *.x*et SQL_DATE, SQL_TIME et SQL_TIMESTAMP tel que défini dans ODBC 2. *x*.  
  
 En raison de la façon du ODBC 3 *.x* du Gestionnaire de pilotes effectue le mappage des types de données date, time et timestamp, ODBC 3 *.x* doivent reconnaissent uniquement les pilotes **#defines** 91, 92 et 93 pour la date, heure et types de données timestamp C est entré dans le *TargetType* arguments de **SQLBindCol** et **SQLGetData** ou le *ValueType* argument de **SQLBindParameter**et devez reconnaître uniquement **#defines** 91 , 92 et 93 pour la date, heure et types de données timestamp SQL entré dans le *ParameterType* argument de **SQLBindParameter** ou *DataType* argument de **SQLGetTypeInfo**. Pour plus d’informations, consultez [les modifications de types de données Datetime](../../../odbc/reference/develop-app/datetime-data-type-changes.md).
