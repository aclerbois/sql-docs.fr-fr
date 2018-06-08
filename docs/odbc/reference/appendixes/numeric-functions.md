---
title: Fonctions numériques | Documents Microsoft
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
- functions [ODBC], numeric functions
- numeric functions [ODBC]
ms.assetid: 4fa548dc-e8b0-4179-92ff-81d6a79d10c3
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 49aa14e9eafa96cdd8b563bac813338a245a85f4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="numeric-functions"></a>Fonctions numériques
Le tableau suivant décrit les fonctions numériques qui sont incluses dans l’ensemble de la fonction scalaire ODBC. En appelant **SQLGetInfo** avec un *type d’information* de SQL_NUMERIC_FUNCTIONS, une application peut déterminer les fonctions numériques sont pris en charge par un pilote.  
  
 Toutes les fonctions numériques retournent des valeurs de type de données SQL_FLOAT, à l’exception de ABS, ROUND, TRUNCATE, signe, FLOOR et CEILING, qui retournent des valeurs des mêmes données de type en tant que paramètres d’entrée.  
  
 Arguments notée *positions numeric_exp* peut être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou un *numérique-litera*l, où le type de données sous-jacent peut être représenté en tant que SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL ou SQL_DOUBLE.  
  
 Arguments notée *exp_float* peut être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou un *littéral numérique*, où le type de données sous-jacent peut être représenté comme SQL_FLOAT.  
  
 Arguments notée *integer_exp* peut être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou un *littéral numérique*, où le type de données sous-jacent peut être représenté en tant que SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER ou SQL_BIGINT.  
  
 Les fonctions scalaires CURRENT_TIMESTAMP, CURRENT_TIME et CURRENT_DATE ont été ajoutées dans ODBC 3.0 pour les aligner avec SQL-92.  
  
|Fonction| Description|  
|--------------|-----------------|  
|**ABS (** *positions numeric_exp* **)** (ODBC version 1.0)|Retourne la valeur absolue de *positions numeric_exp*.|  
|**ACOS (** *exp_float* **)** (ODBC version 1.0)|Retourne l’arc cosinus de *exp_float* à un angle, exprimé en radians.|  
|**ASIN (** *exp_float* **)** (ODBC version 1.0)|Retourne l’arc sinus de *exp_float* à un angle, exprimé en radians.|  
|**ATAN (** *exp_float* **)** (ODBC version 1.0)|Retourne l’arc tangente de *exp_float* à un angle, exprimé en radians.|  
|**ATAN2 (** *exp_float1*, *exp_float2 ***)** (ODBC version 2.0)|Retourne l’arc tangente de le *x* et *y* coordonnées par *exp_float1* et *exp_float2*, respectivement, sous la forme d’un angle, exprimé en radians.|  
|**CEILING (** *positions numeric_exp* **)** (ODBC version 1.0)|Retourne le plus petit entier supérieur ou égal à *positions numeric_exp*. La valeur de retour est du même type de données en tant que paramètre d’entrée.|  
|**COS (** *exp_float* **)** (ODBC version 1.0)|Retourne le cosinus de *exp_float*, où *exp_float* est un angle exprimé en radians.|  
|**COT (** *exp_float* **)** (ODBC version 1.0)|Renvoie la cotangente de *exp_float*, où *exp_float* est un angle exprimé en radians.|  
|**DEGRÉS (** *positions numeric_exp* **)** (ODBC 2.0)|Retourne le nombre de degrés convertis à partir de *positions numeric_exp* radians.|  
|**EXP (** *exp_float* **)** (ODBC version 1.0)|Retourne la valeur exponentielle de *exp_float*.|  
|**FLOOR (** *positions numeric_exp* **)** (ODBC version 1.0)|Retourne le plus grand entier inférieur ou égal à *positions numeric_exp*. La valeur de retour est du même type de données en tant que paramètre d’entrée.|  
|**JOURNAL (** *exp_float* **)** (ODBC version 1.0)|Retourne le logarithme naturel de *exp_float*.|  
|**LOG10 (** *exp_float* **)** (ODBC 2.0)|Logarithme retourne le logarithme de base 10 de *exp_float*.|  
|**MOD (** *exp_entier1*, *exp_entier2 ***)** (ODBC version 1.0)|Renvoie le reste (modulo) de *exp_entier1* divisé par *exp_entier2*.|  
|**PI ()** (ODBC VERSION 1.0)|Retourne la valeur constante de pi sous la forme d’une valeur à virgule flottante.|  
|**ALIMENTATION (** *positions numeric_exp*, *integer_exp ***)** (ODBC 2.0)|Retourne la valeur de *positions numeric_exp* à la puissance de *integer_exp*.|  
|**RADIANS (** *positions numeric_exp* **)** (ODBC 2.0)|Retourne le nombre de radians convertis à partir de *positions numeric_exp* degrés.|  
|**RAND (**[*integer_exp*]**)** (ODBC version 1.0)|Retourne une valeur à virgule flottante aléatoire à l’aide de *integer_exp* en tant que la valeur de départ facultative.|  
|**ROUND (** *positions numeric_exp*, *integer_exp ***)** (ODBC 2.0)|Retourne *positions numeric_exp* arrondi à *integer_exp* place à droite de la virgule décimale. Si *integer_exp* est négatif, *positions numeric_exp* est arrondi à &#124; *integer_exp* &#124; place à gauche de la virgule décimale.|  
|**SIGNE (** *positions numeric_exp* **)** (ODBC version 1.0)|Renvoie un indicateur du signe de *positions numeric_exp*. Si *positions numeric_exp* est inférieur à zéro, -1 est retournée. Si *positions numeric_exp* est égal à zéro, 0 est retourné. Si *positions numeric_exp* est supérieure à 0, 1 est retourné.|  
|**SIN (** *exp_float* **)** (ODBC version 1.0)|Retourne le sinus de *exp_float*, où *exp_float* est un angle exprimé en radians.|  
|**SQRT (** *exp_float* **)** (ODBC version 1.0)|Retourne la racine carrée de *exp_float*.|  
|**TAN (** *exp_float* **)** (ODBC version 1.0)|Retourne la tangente de *exp_float*, où *exp_float* est un angle exprimé en radians.|  
|**TRUNCATE (** *positions numeric_exp*, *integer_exp ***)** (ODBC 2.0)|Retourne *positions numeric_exp* tronqué à *integer_exp* place à droite de la virgule décimale. Si *integer_exp* est négatif, *positions numeric_exp* est tronqué à &#124; *integer_exp* &#124; place à gauche de la virgule décimale.|
