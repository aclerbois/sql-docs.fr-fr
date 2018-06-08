---
title: Fonctions système | Documents Microsoft
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
- system functions [ODBC]
- functions [ODBC], system functions
ms.assetid: 36614b4c-e037-43ef-8692-67f4861b144d
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d5bfd40b587956595bfc8c35b4bb030543253cd9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="system-functions"></a>Fonctions système
Le tableau suivant répertorie les fonctions système qui sont incluses dans l’ensemble de la fonction scalaire ODBC. En appelant **SQLGetInfo** avec un *type d’information* de SQL_SYSTEM_FUNCTIONS, une application peut déterminer les fonctions système sont prises en charge par un pilote.  
  
 Arguments notée *exp* peut être pas le nom d’une colonne, le résultat d’une autre fonction scalaire ou un littéral, où le type de données sous-jacent peut être représenté en tant que SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL, SQL_DOUBLE, SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP.  
  
 Arguments notée *valeur* peut être pas une constante littérale, où le type de données sous-jacent peut être représenté comme les SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL, SQL_DOUBLE, SQL_TYPE_DATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP.  
  
 Les valeurs retournées sont représentées en tant que types de données ODBC.  
  
|Fonction| Description|  
|--------------|-----------------|  
|**(DE BASE DE DONNÉES)** (ODBC VERSION 1.0)|Retourne le nom de la base de données correspondant au descripteur de connexion. (Le nom de la base de données est également disponible en appelant **SQLGetConnectOption** avec l’option de connexion SQL_CURRENT_QUALIFIER.)|  
|**IFNULL (** *exp*,*valeur ***)** (ODBC version 1.0)|Si *exp* a la valeur null, *valeur* est retourné. Si *exp* n’est pas null, *exp* est retourné. L’ou les types de données possibles *valeur* doit être compatible avec le type de données *exp*.|  
|**UTILISATEUR ()** (ODBC VERSION 1.0)|Retourne le nom d’utilisateur dans le SGBD. (Le nom d’utilisateur est également disponible par le biais de **SQLGetInfo** en spécifiant le type d’informations : SQL_USER_NAME.) Cela peut être différent du nom de connexion.|
