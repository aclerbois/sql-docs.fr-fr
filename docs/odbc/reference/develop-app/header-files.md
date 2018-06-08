---
title: Fichiers d’en-tête | Documents Microsoft
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
- header files [ODBC]
ms.assetid: b4a03273-5e30-4d7b-826e-02f8f28ba078
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 62d31cdda9e97ba72374c60c551f37ecfeccd71c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="header-files"></a>Fichiers d’en-tête
Le fichier d’en-tête Sql.h contient des prototypes pour les fonctions et fonctionnalités dans le niveau de conformité de l’Interface ODBC Core. Le fichier d’en-tête Sqlext.h contient des prototypes pour les fonctions et fonctionnalités dans le niveau 1 et les niveaux de conformité au niveau 2 d’API. Le fichier d’en-tête Sqltypes.h contient les définitions de type et d’indicateurs pour les types de données SQL.  
  
 Les fichiers d’en-tête contiennent tous un **#define**, ODBCVER, une application ou un pilote pouvant définies pour être compilé pour différentes versions d’ODBC.  
  
 Pour s’aligner avec la CLI de ISO et groupe ouvert, les fichiers d’en-tête contiennent des alias pour les types d’informations utilisés dans les appels à **SQLGetInfo**. Dans le tableau suivant, la colonne « Nom ODBC » indique le nom ODBC pour le type d’informations dans [référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md). La colonne « Alias dans le fichier d’en-tête » indique le nom qui est utilisé dans le CLI ISO et le groupe ouvert. La valeur numérique réelle de ces noms de manifeste est identique dans ODBC et les interfaces CLI standard. Ces alias activer une application conforme aux normes ou le pilote à compiler avec ODBC 3 *.x* fichiers d’en-tête.  
  
 Ces alias incluent expansions des abréviations dans les noms ODBC afin que les noms sont plus faciles à comprendre. « MAX » est développé à « MAXIMUM », « Long » pour « Longueur », « MULT » à « Plusieurs », « JO » à « OUTER_JOIN » et « TXN » à « TRANSACTION ».  
  
|Nom de ODBC.|Alias dans le fichier d’en-tête|  
|---------------|--------------------------|  
|SQL_MAX_CATALOG_NAME_LEN|SQL_MAXIMUM_CATALOG_NAME_LENGTH|  
|SQL_MAX_COLUMN_NAME_LEN|SQL_MAXIMUM_COLUMN_NAME_LENGTH|  
|SQL_MAX_COLUMNS_IN_GROUP_BY|SQL_MAXIMUM_COLUMNS_IN_GROUP_BY|  
|SQL_MAX_COLUMNS_IN_ORDER_BY|SQL_MAXIMUM_COLUMNS_IN_ORDER_BY|  
|SQL_MAX_COLUMNS_IN_SELECT|SQL_MAXIMUM_COLUMNS_IN_SELECT|  
|SQL_MAX_COLUMNS_IN_TABLE|SQL_MAXIMUM_COLUMNS_IN_TABLE|  
|SQL_MAX_CONCURRENT_ACTIVITIES|SQL_MAXIMUM_CONCURRENT_ACTIVITIES|  
|SQL_MAX_CURSOR_NAME_LEN|SQL_MAXIMUM_CURSOR_NAME_LENGTH|  
|SQL_MAX_DRIVER_CONNECTIONS|SQL_MAXIMUM_DRIVER_CONNECTIONS|  
|SQL_MAX_IDENTIFIER_LEN|SQL_MAXIMUM_IDENTIFIER_LENGTH|  
|SQL_MAX_SCHEMA_NAME_LEN|SQL_MAXIMUM_SCHEMA_NAME_LENGTH|  
|SQL_MAX_STATEMENT_LEN|SQL_MAXIMUM_STATEMENT_LENGTH|  
|SQL_MAX_TABLE_NAME_LEN|SQL_MAXIMUM_TABLE_NAME_LENGTH|  
|SQL_MAX_TABLES_IN_SELECT|SQL_MAXIMUM_TABLES_IN_SELECT|  
|SQL_MAX_USER_NAME_LEN|SQL_MAXIMUM_USER_NAME_LENGTH|  
|SQL_MULT_RESULT_SETS|SQL_MULTIPLE_RESULT_SETS|  
|SQL_OJ_CAPABILITIES|SQL_OUTER_JOIN_CAPABILITIES|  
|SQL_TXN_CAPABLE|SQL_TRANSACTION_CAPABLE|  
|SQL_TXN_ISOLATION_OPTION|SQL_TRANSACTION_ISOLATION_OPTION|
