---
title: Transfert de la longueur d’Octet | Documents Microsoft
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
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- data types [ODBC], transfer octet length
ms.assetid: 9fdc9762-e203-4cff-9212-54f450bf18d9
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 157dbea8954dd7823888360c7d9cf74d9c8db74d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transfer-octet-length"></a>Transfert de la longueur en octets
La longueur d’octet de transfert d’une colonne est le nombre maximal d’octets renvoyés à l’application lorsque les données sont transférées vers son type de données C par défaut. Pour les données de caractères, la longueur d’octet de transfert n’inclut pas d’espace pour le caractère de fin de la valeur null. La longueur d’octet de transfert d’une colonne peut être différente du nombre d’octets requis pour stocker les données sur la source de données.  
  
 La longueur d’octet transfert définie pour chaque type de données SQL ODBC est indiquée dans le tableau suivant.  
  
|Identificateur de type SQL|Longueur|  
|-------------------------|------------|  
|Tous les types de caractères [a]|La période définie par ou la longueur maximale (pour le type de variable) de la colonne en octets. Il s’agit de la même valeur que le champ de descripteur SQL_DESC_OCTET_LENGTH.|  
|SQL_DECIMAL<br />SQL_NUMERIC|Le nombre d’octets requis pour contenir la représentation sous forme de caractères de ces données si le jeu de caractères est ANSI et deux fois ce nombre si le jeu de caractères UNICODE. Il s’agit du nombre maximal de chiffres plus de deux, car les données sont retournées comme une chaîne de caractères et caractères sont nécessaires pour les chiffres, un signe et une virgule décimale. Par exemple, la longueur de transfert d’une colonne définie en tant que NUMERIC(10,3) est 12.|  
|SQL_TINYINT|1|  
|SQL_SMALLINT|2|  
|SQL_INTEGER|4|  
|SQL_BIGINT|Le nombre d’octets requis pour contenir la représentation sous forme de caractères de ces données si le jeu de caractères est ANSI et deux fois ce nombre si le jeu de caractères UNICODE, car ce type de données est retourné comme une chaîne de caractères par défaut. La représentation sous forme de caractère est composé de 20 caractères : 19 chiffres et un signe, si signé, ou 20 chiffres, si non signé. Par conséquent, la longueur est de 20.|  
|SQL_REAL|4|  
|SQL_FLOAT|8|  
|SQL_DOUBLE|8|  
|SQL_BIT|1|  
|Tous les types binaires [a]|Nombre d’octets requis pour contenir la période définie par (pour les types fixes) ou le nombre maximal (pour les types de variables) de caractères.|  
|SQL_TYPE_DATE<br />SQL_TYPE_TIME|6 (la taille de la structure SQL_DATE_STRUCT ou SQL_TIME_STRUCT).|  
|SQL_TYPE_TIMESTAMP|16 (la taille de la structure SQL_TIMESTAMP_STRUCT).|  
|Tous les types d’intervalle|34 (la taille de la structure de l’intervalle).|  
|SQL_GUID|16 (la taille de la structure GUID).|  
  
 [a] si le pilote ne peut pas déterminer la longueur de colonne ou de paramètre pour les types de variable, elle retourne SQL_NO_TOTAL.
