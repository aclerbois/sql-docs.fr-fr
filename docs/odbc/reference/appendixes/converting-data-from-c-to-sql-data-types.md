---
title: Conversion de données à partir de C en Types de données SQL | Documents Microsoft
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
- converting data from c to SQL types [ODBC], about converting
- converting data from c to SQL types [ODBC]
- data conversions from C to SQL types [ODBC], about converting
- data types [ODBC], C data types
- data types [ODBC], converting data
- SQL data types [ODBC], converting from C types
- data types [ODBC], SQL data types
- data conversions from C to SQL types [ODBC]
- C data types [ODBC], converting to SQL types
ms.assetid: ee0afe78-b58f-4d34-ad9b-616bb23653bd
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f08f4294b5b6c44c54d9f577ec423f7d298181a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="converting-data-from-c-to-sql-data-types"></a>Conversion de données à partir de C en Types de données SQL
Lorsqu’une application appelle **SQLExecute** ou **SQLExecDirect**, le pilote récupère les données pour tous les paramètres liés avec **SQLBindParameter** à partir d’emplacements de stockage dans l’application. Lorsqu’une application appelle **SQLSetPos**, le pilote récupère les données pour une mise à jour ou l’opération d’ajout de colonnes liées avec **SQLBindCol**. Pour les paramètres de données d’exécution, l’application envoie les données de paramètre avec **SQLPutData**. Si nécessaire, le pilote convertit les données du type de données spécifié par le *ValueType* argument dans **SQLBindParameter** au type de données spécifié par le *ParameterType* argument dans **SQLBindParameter**, puis envoie les données à la source de données.  
  
 Le tableau suivant montre les conversions prises en charge d’ODBC C en types de données SQL ODBC, les types de données. Un cercle plein indique la conversion de la valeur par défaut pour un type de données SQL (le type de données C à partir de laquelle les données seront converties lorsque la valeur de *ValueType* ou le champ de descripteur SQL_DESC_CONCISE_TYPE est SQL_C_DEFAULT). Un cercle vide indique une conversion prises en charge.  
  
 Le format des données converties n’est pas affecté par le paramètre de pays Windows®.  
  
 ![Conversions prises en charge : ODBC C en types de données SQL](../../../odbc/reference/appendixes/media/apd1b.gif "apd1b")  
  
 Les tableaux dans les sections suivantes décrivent comment le pilote ou une source de données convertit les données envoyées à la source de données ; pilotes sont requis pour prendre en charge des conversions à partir de tous les types de données ODBC C pour les types de données SQL ODBC pris en charge. Pour un type de données ODBC C donné, la première colonne de la table répertorie les valeurs d’entrée juridiques de la *ParameterType* argument dans **SQLBindParameter**. La seconde colonne répertorie les résultats de test qui le pilote effectue pour déterminer si elle peut convertir les données. La troisième colonne contient la valeur SQLSTATE retournée pour chaque résultat par **SQLExecDirect**, **SQLExecute**, **SQLBulkOperations**, **SQLSetPos**, ou **SQLPutData**. Données sont envoyées à la source de données uniquement si la valeur SQL_SUCCESS est retournée.  
  
 Si le *ParameterType* argument dans **SQLBindParameter** contient l’identificateur d’un type de données SQL ODBC qui ne figure pas dans la table pour un type de données C donné, **SQLBindParameter** retourne SQLSTATE 07006 (violation de l’attribut de type de données restreint). Si le *ParameterType* argument contient un identificateur spécifique du pilote et le pilote ne prend pas en charge la conversion du type de données ODBC C spécifiques à ce type de données spécifiques au pilote SQL **SQLBindParameter** retourne SQLSTATE HYC00 (fonctionnalité facultative non implémentée).  
  
 Si le *ParameterValuePtr* et *StrLen_or_IndPtr* arguments spécifiés dans **SQLBindParameter** les deux pointeurs sont null, que cette fonction retourne SQLSTATE HY009 (utilisation non valide d’un pointeur null). Bien que cela n’est pas indiqué dans les tables, une application définit la valeur de la mémoire tampon de longueur / d’indicateur vers lequel pointée le *StrLen_or_IndPtr* argument de **SQLBindParameter** ou la valeur de la *StrLen_or_IndPtr* argument de **SQLPutData** à SQL_NULL_DATA pour spécifier une valeur de données NULL SQL. (Le *StrLen_or_IndPtr* argument correspond au champ SQL_DESC_OCTET_LENGTH_PTR de l’APD.) L’application définit ces valeurs pour SQL_NTS pour spécifier que la valeur de \* *ParameterValuePtr* dans **SQLBindParameter** ou \* *DataPtr* dans **SQLPutData** (vers lequel pointe le champ SQL_DESC_DATA_PTR de l’APD) est une chaîne se terminant par null.  
  
 Les termes suivants sont utilisés dans les tables :  
  
-   **Longueur en octets de données** : nombre d’octets de données SQL disponibles pour envoyer à la source de données, ou non les données sont tronquées avant d’être envoyée à la source de données. Pour les données de chaîne, cela n’inclut pas d’espace pour le caractère de fin de la valeur null.  
  
-   **Longueur d’octet colonne** : nombre d’octets requis pour stocker les données dans la source de données.  
  
-   **Longueur en octets de caractères** : nombre maximal d’octets nécessaires pour afficher des données sous forme de caractères. Il s’agit comme défini pour chaque type de données SQL dans [afficher la taille](../../../odbc/reference/appendixes/display-size.md), sauf la longueur d’octet de caractère est en octets, tandis que la taille d’affichage est en caractères.  
  
-   **Nombre de chiffres** : nombre de caractères utilisé pour représenter un nombre, y compris le signe moins, virgule décimale et un exposant (si nécessaire).  
  
-   **Mots dans**   
     ***italique*** : éléments de la grammaire SQL. Pour connaître la syntaxe des éléments de grammaire, consultez [annexe c : SQL grammaire](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [C en SQL : caractère](../../../odbc/reference/appendixes/c-to-sql-character.md)  
  
-   [C en SQL : numérique](../../../odbc/reference/appendixes/c-to-sql-numeric.md)  
  
-   [C en SQL : bit](../../../odbc/reference/appendixes/c-to-sql-bit.md)  
  
-   [C en SQL : binaire](../../../odbc/reference/appendixes/c-to-sql-binary.md)  
  
-   [C en SQL : date](../../../odbc/reference/appendixes/c-to-sql-date.md)  
  
-   [C en SQL : GUID](../../../odbc/reference/appendixes/c-to-sql-guid.md)  
  
-   [C en SQL : heure](../../../odbc/reference/appendixes/c-to-sql-time.md)  
  
-   [C en SQL : horodatage](../../../odbc/reference/appendixes/c-to-sql-timestamp.md)  
  
-   [C en SQL : intervalles d’années-mois](../../../odbc/reference/appendixes/c-to-sql-year-month-intervals.md)  
  
-   [C en SQL : intervalles de jours-heures](../../../odbc/reference/appendixes/c-to-sql-day-time-intervals.md)  
  
-   [Exemples de conversion de données C en SQL](../../../odbc/reference/appendixes/c-to-sql-data-conversion-examples.md)
