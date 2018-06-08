---
title: Conversion de données à partir de SQL pour Types de données C | Documents Microsoft
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
- data conversions from SQL to C types [ODBC]
- data conversions from SQL to C types [ODBC], about converting
- data types [ODBC], C data types
- data types [ODBC], converting data
- data types [ODBC], SQL data types
- SQL data types [ODBC], converting to C types
- converting data from SQL to c types [ODBC]
- converting data from SQL to c types [ODBC], about converting
- C data types [ODBC], converting from SQL types
ms.assetid: 029727f6-d3f0-499a-911c-bcaf9714e43b
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 370286bb4e955247e53d3c010b0bbb2088957bbe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="converting-data-from-sql-to-c-data-types"></a>Conversion de données à partir de SQL pour Types de données C
Lorsqu’une application appelle **SQLFetch**, **SQLFetchScroll**, ou **SQLGetData**, le pilote récupère les données à partir de la source de données. Si nécessaire, il convertit les données du type de données dans lequel, extraites par le pilote pour le type de données spécifié par le *TargetType* argument dans **SQLBindCol** ou **SQLGetData.** Enfin, il stocke les données dans l’emplacement vers lequel pointé le *TargetValuePtr* argument dans **SQLBindCol** ou **SQLGetData** (et le champ SQL_DESC_DATA_PTR de la ARD).  
  
 Le tableau suivant montre les conversions prises en charge d’ODBC SQL pour les types de données ODBC C, les types de données. Un cercle plein indique la conversion de la valeur par défaut pour un type de données SQL (le type de données C à laquelle les données seront converties lorsque la valeur de *TargetType* est SQL_C_DEFAULT). Un cercle vide indique une conversion prises en charge.  
  
 Pour un ODBC 3 *.x* application utilisant une API ODBC 2. *x* pilote, la conversion de données spécifiques au pilote types ne peuvent pas être pris en charge.  
  
 Le format des données converties n’est pas affecté par le paramètre de pays Windows®.  
  
 Les tableaux dans les sections suivantes décrivent comment le pilote ou une source de données convertit les données récupérées à partir de la source de données ; pilotes sont requis pour prendre en charge les conversions des types de données SQL ODBC pris en charge tous les types de données ODBC C. Pour un type de données SQL ODBC donné, la première colonne de la table répertorie les valeurs d’entrée juridiques de la *TargetType* argument dans **SQLBindCol** et **SQLGetData**. La seconde colonne répertorie les résultats d’un test, souvent à l’aide de la *BufferLength* argument spécifié dans **SQLBindCol** ou **SQLGetData**, dont le pilote effectue pour déterminer si elle peut convertir les données. Pour chaque résultat, les troisième et quatrième colonnes répertorient les valeurs placées dans les mémoires tampons spécifiés par le *TargetValuePtr* et *StrLen_or_IndPtr* arguments spécifiés dans **SQLBindCol** ou **SQLGetData** une fois que le pilote a tenté de convertir les données. (Le *StrLen_or_IndPtr* argument correspond au champ SQL_DESC_OCTET_LENGTH_PTR de la ARD.) La dernière colonne répertorie la valeur SQLSTATE retournée pour chaque résultat par **SQLFetch**, **SQLFetchScroll**, ou **SQLGetData**.  
  
 Si le *TargetType* argument dans **SQLBindCol** ou **SQLGetData** contient un identificateur pour un type de données ODBC C ne pas indiqué dans le tableau pour un type de données ODBC SQL donné, **SQLFetch**, **SQLFetchScroll**, ou **SQLGetData** retourne SQLSTATE 07006 (violation de l’attribut de type de données restreint). Si le *TargetType* argument contient un identificateur qui spécifie une conversion à partir d’un type de données spécifique au pilote SQL à un type de données ODBC C et cette conversion n’est pas pris en charge par le pilote, **SQLFetch**, **SQLFetchScroll**, ou **SQLGetData** retourne SQLSTATE HYC00 (fonctionnalité facultative non implémentée).  
  
 Bien que cela n’est pas indiqué dans les tables, le pilote retourne SQL_NULL_DATA dans la mémoire tampon spécifiée par le *StrLen_or_IndPtr* argument lorsque la valeur de données SQL est NULL. Pour obtenir une explication de l’utilisation de *StrLen_or_IndPtr* lorsque plusieurs appels sont effectués pour récupérer des données, consultez la [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)description de fonction. Lorsque les données SQL sont converties en données de type caractère C, le nombre de caractères retournés dans \* *StrLen_or_IndPtr* n’inclut pas l’octet de valeur null. Si *TargetValuePtr* est un pointeur null, **SQLGetData** retourne SQLSTATE HY009 (utilisation non valide d’un pointeur null) ; dans **SQLBindCol**, cela annule la liaison de la colonne.  
  
 Les conventions et les conditions suivantes sont utilisées dans les tables :  
  
-   **Longueur en octets de données** est le nombre d’octets de données C disponibles à renvoyer dans **TargetValuePtr*, les données sont tronquées avant d’être retournée à l’application ou non. Pour les données de chaîne, cela n’inclut pas l’espace pour le caractère de fin de la valeur null.  
  
-   **Longueur en octets de caractères** est le nombre total d’octets nécessaires pour afficher les données au format caractère. Il s’agit comme défini pour chaque type de données C dans la section [afficher la taille](../../../odbc/reference/appendixes/display-size.md), sauf que la longueur d’octet de caractère est en octets, tandis que la taille d’affichage est en caractères.  
  
-   Les mots dans *italique* représentent les arguments de fonction ou d’éléments de la grammaire SQL. Pour connaître la syntaxe des éléments de grammaire, consultez [annexe c : SQL grammaire](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [SQL en C : caractère](../../../odbc/reference/appendixes/sql-to-c-character.md)  
  
-   [SQL en C : numérique](../../../odbc/reference/appendixes/sql-to-c-numeric.md)  
  
-   [SQL en C : bit](../../../odbc/reference/appendixes/sql-to-c-bit.md)  
  
-   [SQL en C : binaire](../../../odbc/reference/appendixes/sql-to-c-binary.md)  
  
-   [SQL en C : date](../../../odbc/reference/appendixes/sql-to-c-date.md)  
  
-   [SQL en C : GUID](../../../odbc/reference/appendixes/sql-to-c-guid.md)  
  
-   [SQL en C : heure](../../../odbc/reference/appendixes/sql-to-c-time.md)  
  
-   [SQL en C : horodatage](../../../odbc/reference/appendixes/sql-to-c-timestamp.md)  
  
-   [SQL en C : intervalles d’années-mois](../../../odbc/reference/appendixes/sql-to-c-year-month-intervals.md)  
  
-   [SQL en C : intervalles de jours-heures](../../../odbc/reference/appendixes/sql-to-c-day-time-intervals.md)  
  
-   [Exemples de conversion de données SQL en C](../../../odbc/reference/appendixes/sql-to-c-data-conversion-examples.md)
