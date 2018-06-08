---
title: Écrire des Applications ODBC 3.x | Documents Microsoft
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
- application upgrades [ODBC], about upgrading
- ODBC drivers [ODBC], backward compatibility
- upgrading applications [ODBC]
- compatibility [ODBC], upgrading applications
- application upgrades [ODBC]
- upgrading applications [ODBC], about upgrading
- backward compatibility [ODBC], upgrading applications
ms.assetid: 19c54fc5-9dd6-49b6-8c9f-a38961b40a65
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ee0aa19520ec0d6e3f2f0f6852cb40578f81247c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="writing-odbc-3x-applications"></a>Écriture d’Applications ODBC 3.x
Lorsqu’une application ODBC 2. *x* application est mise à niveau vers ODBC 3. *x*, il doit être écrit tel qu’il fonctionne avec ODBC 2. *x* et 3. *x* pilotes. L’application doit incorporer le code conditionnel pour tirer pleinement parti de ODBC 3. *x* fonctionnalités.  
  
 L’attribut d’environnement SQL_ATTR_ODBC_VERSION doit être définie à SQL_OV_ODBC2. Cela permet de garantir que le pilote se comporte comme un ODBC 2 *.x* pilote en ce qui concerne les modifications décrites dans la section [modifications comportementales](../../../odbc/reference/develop-app/behavioral-changes.md).  
  
 Si l’application utilise les fonctionnalités décrites dans la section [nouvelles fonctionnalités](../../../odbc/reference/develop-app/new-features.md), code conditionnel doit être utilisé pour déterminer si le pilote est un ODBC 3. *x* ou ODBC 2 *.x* pilote. L’application utilise **SQLGetDiagField** et **SQLGetDiagRec** obtenir ODBC 3. *x* SQLSTATE pendant cette opération d’erreur lors du traitement de ces fragments de code conditionnelle. Prenez en compte les points suivants concernant les nouvelles fonctionnalités :  
  
-   Une application affectée par le changement de comportement de taille d’ensemble de lignes doit être faites attention de ne pas appeler **SQLFetch** lorsque la taille du tableau est supérieure à 1. Ces applications doivent remplacer les appels aux **SQLExtendedFetch** avec les appels de **SQLSetStmtAttr** pour définir l’attribut d’instruction SQL_ATTR_ARRAY_STATUS_PTR et **SQLFetchScroll**, afin qu’ils aient un code commun qui fonctionne avec les deux ODBC 3. *x* et ODBC 2. *x* pilotes. Étant donné que **SQLSetStmtAttr** avec SQL_ATTR_ROW_ARRAY_SIZE sera mappé à **SQLSetStmtAttr** avec SQL_ROWSET_SIZE pour ODBC 2. *x* pilotes, les applications peuvent définir simplement SQL_ATTR_ROW_ARRAY_SIZE pour leurs opérations d’extraction de lignes multiples.  
  
-   La plupart des applications qui sont la mise à niveau ne sont pas affectées par les modifications des codes SQLSTATE. Pour les applications qui sont affectées, ils peuvent effectuer une recherche mécanique et remplacer dans la plupart des cas, à l’aide de la table de conversion d’erreur dans la section « Mappage SQLSTATE » pour convertir ODBC 3. *x* codes d’erreur à ODBC 2 *.x* codes. Depuis la version 3 ODBC *.x* du Gestionnaire de pilotes effectuera le mappage à partir d’ODBC 2. *x* SQLSTATE pour ODBC 3. *x* SQLSTATE, les auteurs d’applications doivent uniquement à cocher pour ODBC 3. *x* SQLSTATE et sans vous soucier y compris ODBC 2. *x* SQLSTATE dans le code conditionnel.  
  
-   Si une application effectue une grande utilité de date, heure et types de données timestamp, l’application peut déclarer soit un ODBC 2. *x* application et utilisez ses existant au lieu d’utiliser le code de conditionnement de code.  
  
 La mise à niveau doit également inclure les étapes suivantes :  
  
-   Appelez **SQLSetEnvAttr** avant d’allouer une connexion pour la valeur de l’attribut d’environnement SQL_ATTR_ODBC_VERSION SQL_OV_ODBC2.  
  
-   Remplacer tous les appels à **SQLAllocEnv**, **SQLAllocConnect**, ou **SQLAllocStmt** avec les appels de **SQLAllocHandle** le *HandleType* argument de SQL_HANDLE_ENV, SQL_HANDLE_DBC ou SQL_HANDLE_STMT.  
  
-   Remplacer tous les appels à **SQLFreeEnv** ou **SQLFreeConnect** avec les appels de **SQLFreeHandle** le *HandleType* argument de SQL_HANDLE_DBC ou SQL_HANDLE_STMT.  
  
-   Remplacer tous les appels à **SQLSetConnectOption** avec les appels de **SQLSetConnectAttr**. Si la définition d’un attribut dont la valeur est une chaîne, définissez la *StringLength* argument correctement. Modification *attribut* argument à partir de SQL_XXXX à SQL_ATTR_XXXX.  
  
-   Remplacer tous les appels à **SQLGetConnectOption** avec les appels de **SQLGetConnectAttr**. Si une chaîne ou un attribut binaire, définissez *BufferLength* à la valeur appropriée et passez un *StringLength* argument. Modification *attribut* argument à partir de SQL_XXXX à SQL_ATTR_XXXX.  
  
-   Remplacer tous les appels à **SQLSetStmtOption** avec les appels de **SQLSetStmtAttr**. Si la définition d’un attribut dont la valeur est une chaîne, définissez la *StringLength* argument correctement. Modification *attribut* argument à partir de SQL_XXXX à SQL_ATTR_XXXX.  
  
-   Remplacer tous les appels à **SQLGetStmtOption** avec les appels de **SQLGetStmtAttr**. Si une chaîne ou un attribut binaire, définissez *BufferLength* à la valeur appropriée et passez un *StringLength* argument. Modification *attribut* argument à partir de SQL_XXXX à SQL_ATTR_XXXX.  
  
-   Remplacer tous les appels à **SQLTransact** avec les appels de **SQLEndTran**. Si le handle valid plus à droite dans le **SQLTransact** appel est un handle d’environnement, un *HandleType* argument de SQL_HANDLE_ENV doit être utilisé dans le **SQLEndTran** appeler le *gérer* argument. Si le handle valid plus à droite dans votre **SQLTransact** appel est un handle de connexion, un *HandleType* argument de SQL_HANDLE_DBC doit être utilisé dans le **SQLEndTran** appeler le *gérer* argument.  
  
-   Remplacer tous les appels à **SQLColAttributes** avec les appels de **SQLColAttribute**. Si le *FieldIdentifier* argument est SQL_COLUMN_PRECISION, SQL_COLUMN_SCALE ou SQL_COLUMN_LENGTH, ne modifiez rien d’autre que le nom de la fonction. Dans le cas contraire, modifiez *FieldIdentifier* de SQL_COLUMN_XXXX à SQL_DESC_XXXX. Si *FieldIdentifier* est SQL_DESC_CONCISE_TYPE et le type de données est un type de données datetime, modifiez le 3 ODBC correspondants *.x* type de données.  
  
-   Si vous utilisez des curseurs de bloc, les curseurs de défilement ou les deux, l’application effectue les opérations suivantes :  
  
    -   Définit la taille de l’ensemble de lignes, d’un type de curseur et d’une concurrence de curseur à l’aide **SQLSetStmtAttr**.  
  
    -   Appels **SQLSetStmtAttr** définir SQL_ATTR_ROW_STATUS_PTR pour pointer vers un tableau d’enregistrements d’état.  
  
    -   Appels **SQLSetStmtAttr** définir SQL_ATTR_ROWS_FETCHED_PTR pour pointer vers un SQLINTEGER.  
  
    -   Effectue les liaisons requises et exécute l’instruction SQL.  
  
    -   Appels **SQLFetchScroll** dans une boucle pour extraire des lignes et de vous déplacer dans le résultat défini.  
  
    -   S’il souhaite extraire par le signet, l’application appelle **SQLSetStmtAttr** définir SQL_ATTR_FETCH_BOOKMARK_PTR à une variable qui contient le signet pour la ligne qu’il souhaite extraire et les appels **SQLFetchScroll** avec un *FetchOrientation* argument de SQL_FETCH_BOOKMARK.  
  
-   Si vous utilisez les tableaux de paramètres, l’application effectue les opérations suivantes :  
  
    -   Appels **SQLSetStmtAttr** pour définir l’attribut SQL_ATTR_PARAMSET_SIZE à la taille du tableau de paramètres.  
  
    -   Appels **SQLSetStmtAttr** définir SQL_ATTR_ROWS_PROCESSED_PTR pour pointer vers une variable UDWORD interne.  
  
    -   Effectue préparer, lier et exécuter des opérations comme il convient.  
  
    -   Si l’exécution s’arrête pour une raison quelconque (par exemple, SQL_NEED_DATA), il peut trouver la ligne « actuelle » des paramètres en inspectant l’emplacement vers lequel pointé SQL_ATTR_ROWS_PROCESSED_PTR.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)  
  
-   [Appel de SQLCloseCursor](../../../odbc/reference/develop-app/calling-sqlclosecursor.md)  
  
-   [Appel de SQLGetDiagField](../../../odbc/reference/develop-app/calling-sqlgetdiagfield.md)  
  
-   [Appel de SQLSetPos](../../../odbc/reference/develop-app/calling-sqlsetpos.md)  
  
-   [Opérations de bibliothèque de curseurs](../../../odbc/reference/develop-app/cursor-library-operations.md)  
  
-   [Mappage des types d’informations Attributes1 du curseur](../../../odbc/reference/develop-app/mapping-the-cursor-attributes1-information-types.md)  
  
-   [SQL_NO_DATA](../../../odbc/reference/develop-app/sql-no-data.md)
