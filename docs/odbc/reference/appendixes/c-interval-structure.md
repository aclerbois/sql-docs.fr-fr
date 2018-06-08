---
title: Structure d’intervalle C | Documents Microsoft
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
- data types [ODBC], interval data types
- interval data type [ODBC], structure
- C data types [ODBC], interval
ms.assetid: 52b42b56-50aa-4ce6-8d79-0963c7a71437
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2f3a2c8f0e3ad967b3c0b7b02255774c2603a1b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="c-interval-structure"></a>Structure d’intervalle C
Chacun des types de données C intervalle répertoriées dans le [les Types de données C](../../../odbc/reference/appendixes/c-data-types.md) section utilise la même structure pour contenir les données de l’intervalle. Lorsque **SQLFetch**, **SQLFetchScroll**, ou **SQLGetData** est appelée, le pilote retourne les données dans la structure SQL_INTERVAL_STRUCT, utilise la valeur qui a été spécifiée par l’application pour les types de données C (dans l’appel à **SQLBindCol**, **SQLGetData**, ou **SQLBindParameter**) pour interpréter le contenu de SQL_INTERVAL_STRUCT et remplit la *interval_type* champ de la structure avec la *enum* valeur correspondant au type C. Notez que les pilotes ne lisent pas les *interval_type* champ pour déterminer le type de l’intervalle ; ils récupèrent la valeur du champ de descripteur SQL_DESC_CONCISE_TYPE. Lorsque la structure est utilisée pour les données de paramètre, le pilote utilise la valeur spécifiée par l’application dans le champ SQL_DESC_CONCISE_TYPE de l’APD interpréter le contenu de SQL_INTERVAL_STRUCT, même si l’application définit la valeur de la *interval_type* champ sur une autre valeur.  
  
 Cette structure est définie comme suit :  
  
```  
typedef struct tagSQL_INTERVAL_STRUCT  
{  
   SQLINTERVAL interval_type;   
   SQLSMALLINT interval_sign;  
   union {  
         SQL_YEAR_MONTH_STRUCT   year_month;  
         SQL_DAY_SECOND_STRUCT   day_second;  
         } intval;  
} SQL_INTERVAL_STRUCT;  
typedef enum   
{  
   SQL_IS_YEAR = 1,  
   SQL_IS_MONTH = 2,  
   SQL_IS_DAY = 3,  
   SQL_IS_HOUR = 4,  
   SQL_IS_MINUTE = 5,  
   SQL_IS_SECOND = 6,  
   SQL_IS_YEAR_TO_MONTH = 7,  
   SQL_IS_DAY_TO_HOUR = 8,  
   SQL_IS_DAY_TO_MINUTE = 9,  
   SQL_IS_DAY_TO_SECOND = 10,  
   SQL_IS_HOUR_TO_MINUTE = 11,  
   SQL_IS_HOUR_TO_SECOND = 12,  
   SQL_IS_MINUTE_TO_SECOND = 13  
} SQLINTERVAL;  
  
typedef struct tagSQL_YEAR_MONTH  
{  
   SQLUINTEGER year;  
   SQLUINTEGER month;   
} SQL_YEAR_MONTH_STRUCT;  
  
typedef struct tagSQL_DAY_SECOND  
{  
   SQLUINTEGER day;  
   SQLUINTEGER hour;  
   SQLUINTEGER minute;  
   SQLUINTEGER second;  
   SQLUINTEGER fraction;  
} SQL_DAY_SECOND_STRUCT;  
```  
  
 Le *interval_type* champ de la SQL_INTERVAL_STRUCT indique à l’application quelle structure est maintenu dans l’union et les membres de la structure sont également pertinents. Le *interval_sign* champ a la valeur SQL_FALSE si l’en-tête de champ de l’intervalle n’est pas signé ; si elle est SQL_TRUE, le champ de début est négatif. La valeur dans le champ Début lui-même est toujours non signée, quelle que soit la valeur de *interval_sign*. Le *interval_sign* champ fonctionne comme un bit de signe.
