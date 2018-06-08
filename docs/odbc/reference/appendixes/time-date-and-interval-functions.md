---
title: Fonctions d’heure, Date et intervalle | Documents Microsoft
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
- functions [ODBC], time functions
- functions [ODBC], date functions
- interval functions [ODBC]
- functions [ODBC], interval functions
- time functions [ODBC]
- date functions [ODBC]
ms.assetid: bdf054a0-7aba-4e99-a34a-799917376fd5
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d2939125046525ec73f25499f75a4b981f301615
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="time-date-and-interval-functions"></a>Fonctions d'heure, de date et d'intervalle
Le tableau suivant répertorie les fonctions de date et heure sont incluses dans l’ensemble de la fonction scalaire ODBC. Une application peut déterminer les fonctions de date et heure sont pris en charge par un pilote en appelant **SQLGetInfo** avec un *type d’information* de SQL_TIMEDATE_FUNCTIONS.  
  
 Arguments notée *exp_estampille* peut être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou un *échappement-heure ODBC*, *date ODBC-échappement*, ou *timestamp ODBC-échappement*, où le type de données sous-jacent peut être représenté en tant que SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME, SQL_TYPE_DATE ou SQL_TYPE_TIMESTAMP.  
  
 Arguments notée *date_exp* peut être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou un *date ODBC-échappement* ou *timestamp ODBC-échappement*, où le type de données sous-jacent peut être représenté en tant que SQL_CHAR, SQL_VARCHAR, SQL_TYPE_DATE ou SQL_TYPE_TIMESTAMP.  
  
 Arguments notée *time_exp* peut être le nom d’une colonne, le résultat d’une autre fonction scalaire, ou un *échappement-heure ODBC* ou *timestamp ODBC-échappement*, où le type de données sous-jacent peut être représenté en tant que SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP.  
  
 Les CURRENT_DATE, CURRENT_TIME et CURRENT_TIMESTAMP heureDATE fonctions scalaires ont été ajoutés dans ODBC 3.0 pour les aligner avec SQL-92.  
  
|Fonction| Description|  
|--------------|-----------------|  
|**(DE) CURRENT_DATE** (ODBC 3.0)|Retourne la date actuelle.|  
|**CURRENT_TIME [(** *précision de l’heure* **)]** (ODBC version 3.0)|Retourne l'heure locale actuelle. Le *précision de l’heure* argument détermine la précision en secondes de la valeur retournée.|  
|**CURRENT_TIMESTAMP**<br /> **[(** *timestamp précision* **)]** (ODBC version 3.0)|Retourne la date du jour et l’heure locale sous la forme d’une valeur d’horodatage. Le *timestamp précision* argument détermine la précision en secondes de l’horodatage retourné.|  
|**(DE) CURDATE** (ODBC VERSION 1.0)|Retourne la date actuelle.|  
|**(DE) CURTIME** (ODBC VERSION 1.0)|Retourne l'heure locale actuelle.|  
|**HEUREDAYNAME (** *date_exp* **)** (ODBC 2.0)|Retourne une chaîne de caractères contenant le nom spécifique à la source de données de la journée (par exemple, dimanche à samedi ou Sun. à sam pour une source de données qui utilise l’anglais, ou Sonntag à Samstag pour une source de données qui utilise l’allemand) pour la partie jour de *date_exp*.|  
|**DAYOFMONTH (** *date_exp* **)** (ODBC version 1.0)|Retourne le jour du mois basé sur le champ mois dans *date_exp* comme une valeur entière dans la plage de 1 à 31.|  
|**DAYOFWEEK (** *date_exp* **)** (ODBC version 1.0)|Retourne le jour de la semaine basé sur le champ semaine dans *date_exp* comme une valeur entière dans la plage de 1 à 7, où 1 représente le dimanche.|  
|**DAYOFYEAR (** *date_exp* **)** (ODBC version 1.0)|Retourne le jour de l’année en fonction du champ année de *date_exp* comme une valeur entière dans la plage de 1 à 366.|  
|**EXTRAIRE (** *champ extract FROM* *source d’extraction* **)** (ODBC version 3.0)|Retourne le *extract-champ* partie de la *source d’extraction*. Le *source d’extraction* argument est une expression datetime ou interval. Le *extract-champ* argument peut être un des mots clés suivants :<br /><br /> ANNÉE MOIS JOUR HEURE MINUTE SECONDE<br /><br /> La précision de la valeur retournée est défini par l’implémentation. L’échelle est 0 seconde, auquel cas l’échelle n’est pas inférieure à la précision en fractions de seconde de la *source d’extraction* champ.|  
|**HEURE (** *time_exp* **)** (ODBC version 1.0)|Retourne l’heure en fonction du champ d’heure dans *time_exp* comme une valeur entière comprise entre 0 et 23.|  
|**MINUTE (** *time_exp* **)** (ODBC version 1.0)|Retourne la minute basée sur le champ minute dans *time_exp* comme une valeur entière dans la plage de 0 à 59.|  
|**MOIS (** *date_exp* **)** (ODBC version 1.0)|Retourne le mois en fonction du champ mois dans *date_exp* comme une valeur entière comprise entre 1 et 12.|  
|**MONTHNAME (** *date_exp* **)** (ODBC 2.0)|Retourne une chaîne de caractères contenant le nom spécifique à la source de données du mois (par exemple, janvier à décembre ou Jan à déc pour une source de données qui utilise l’anglais, ou Januar à Dezember pour une source de données qui utilise l’allemand) pour la partie mois de *date_exp*.|  
|**MAINTENANT ()** (ODBC VERSION 1.0)|Retourne la date et l’heure sous la forme d’une valeur d’horodatage.|  
|**TRIMESTRE (** *date_exp* **)** (ODBC version 1.0)|Retourne le trimestre de *date_exp* comme une valeur entière comprise entre 1 et 4, où 1 représente le 1er janvier au 31 mars.|  
|**DEUXIÈME (** *time_exp* **)** (ODBC version 1.0)|Retourne la seconde basée sur le deuxième champ de *time_exp* comme une valeur entière dans la plage de 0 à 59.|
|**TIMESTAMPADD (** *intervalle*, *integer_exp*, *exp_estampille* **)** (ODBC 2.0)|Retourne l’horodateur calculée en additionnant *integer_exp* intervalles de type *intervalle* à *exp_estampille*. Les valeurs valides de *intervalle* sont les mots clés suivants :<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> où les fractions de seconde sont exprimées en milliardièmes de seconde. Par exemple, l’instruction SQL suivante retourne le nom de chaque employé et sa date d’anniversaire d’un an :<br /><br /> `SELECT NAME, {fn  TIMESTAMPADD(SQL_TSI_YEAR, 1, HIRE_DATE)} FROM  EMPLOYEES`<br /><br /> Si *exp_estampille* est une valeur d’heure et *intervalle* spécifie les jours, semaines, mois, trimestres ou années, la partie date de *exp_estampille* est définie sur la date actuelle avant de calculer l’horodatage qui en résulte.<br /><br /> Si *exp_estampille* est une valeur de date et *intervalle* spécifie les fractions de seconde secondes, en secondes, minutes ou heures, la partie heure de *exp_estampille* est définie sur 0 avant de calculer l’horodatage qui en résulte.<br /><br /> Une application détermine les intervalles d’une source de données prend en charge en appelant **SQLGetInfo** avec l’option SQL_TIMEDATE_ADD_INTERVALS.|  
|**TIMESTAMPDIFF (** *intervalle*, *timestamp_exp1*, *timestamp_exp2* **)** (ODBC 2.0)|Retourne le nombre d’intervalles de type entier *intervalle* par lequel *timestamp_exp2* est supérieur à *timestamp_exp1*. Les valeurs valides de *intervalle* sont les mots clés suivants :<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> où les fractions de seconde sont exprimées en milliardièmes de seconde. Par exemple, l’instruction SQL suivante retourne le nom de chaque employé et le nombre d’années qu'il ou elle a été employée :<br /><br /> `SELECT NAME, {fn  TIMESTAMPDIFF(SQL_TSI_YEAR, {fn CURDATE()}, HIRE_DATE)} FROM EMPLOYEES`<br /><br /> Si des expressions timestamp sont une valeur d’heure et *intervalle* spécifie les jours, semaines, mois, trimestres ou années, la partie date de cette valeur est définie sur la date actuelle avant de calculer la différence entre les horodateurs.<br /><br /> Si des expressions timestamp sont une valeur de date et *intervalle* spécifie les fractions de seconde secondes, en secondes, minutes ou heures, la partie heure de cette valeur est définie sur 0 avant de calculer la différence entre les horodateurs.<br /><br /> Une application détermine les intervalles d’une source de données prend en charge en appelant **SQLGetInfo** avec l’option SQL_TIMEDATE_DIFF_INTERVALS.|  
|**SEMAINE (** *date_exp* **)** (ODBC version 1.0)|Retourne la semaine de l’année selon le champ semaine dans *date_exp* comme une valeur entière dans la plage 1 à 53.|  
|**ANNÉE (** *date_exp* **)** (ODBC version 1.0)|Retourne l’année en fonction du champ année de *date_exp* comme une valeur entière. La plage est la source de données.|
