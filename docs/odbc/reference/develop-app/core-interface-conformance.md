---
title: Mise en conformité de l’Interface de base | Documents Microsoft
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
- interface conformance levels [ODBC]
- conformance levels [ODBC], interface
- core-level interface conformance levels [ODBC]
ms.assetid: aaaa864a-6477-45ff-a50a-96d8db66a252
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7c45fdfe2b01ddfd34e7db391b799b95ce696da8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="core-interface-conformance"></a>Conformité d’Interface de base
Tous les pilotes ODBC doivent comporter au moins au niveau du noyau mise en conformité de l’interface. Les fonctionnalités du niveau de base sont celles qui sont requises par les applications interopérables plus génériques, le pilote peut travailler avec ces applications. Les fonctionnalités du niveau de base correspondent également pour les fonctionnalités définies dans la spécification ISO CLI et les fonctionnalités obligatoires définies dans la spécification CLI de groupe ouvert. Un pilote ODBC d’interface conforme au niveau du noyau permet à l’application effectuer toutes les opérations suivantes :  
  
-   Allouer et libérer tous les types de handles, en appelant **SQLAllocHandle** et **SQLFreeHandle**.  
  
-   Utiliser toutes les formes de la **SQLFreeStmt** (fonction).  
  
-   Liez les colonnes du jeu de résultats, en appelant **SQLBindCol**.  
  
-   Gérer des paramètres dynamiques, y compris les tableaux de paramètres, dans la direction d’entrée uniquement, en appelant **SQLBindParameter** et **SQLNumParams**. (Dans le sens de la sortie, les paramètres sont des fonctionnalités 203 dans [mise en conformité de niveau 2 Interface](../../../odbc/reference/develop-app/level-2-interface-conformance.md).)  
  
-   Spécifiez un décalage de la liaison.  
  
-   Utilisez la boîte de dialogue data-at-execution, impliquant des appels à **SQLParamData** et **SQLPutData**.  
  
-   Gérer les curseurs et les noms de curseur en appelant **SQLCloseCursor**, **SQLGetCursorName**, et **SQLSetCursorName**.  
  
-   Accéder à la description (métadonnées) des jeux de résultats, en appelant **SQLColAttribute**, **SQLDescribeCol**, **SQLNumResultCols**, et **SQLRowCount**. (Utilisation de ces fonctions sur la colonne numéro 0 pour récupérer des métadonnées de signet est fonctionnalité 204 dans [mise en conformité de niveau 2 Interface](../../../odbc/reference/develop-app/level-2-interface-conformance.md).)  
  
-   Le dictionnaire de données, de la requête en appelant les fonctions de catalogue **SQLColumns**, **SQLGetTypeInfo**, **SQLStatistics**, et **SQLTables**.  
  
     Le pilote n’est pas requis pour prendre en charge des noms en plusieurs parties de tables de base de données et de vues. (Pour plus d’informations, consultez fonctionnalité 101 [mise en conformité de niveau 1 Interface](../../../odbc/reference/develop-app/level-1-interface-conformance.md) et d’une fonctionnalité 201 dans [mise en conformité de niveau 2 Interface](../../../odbc/reference/develop-app/level-2-interface-conformance.md).) Toutefois, certaines fonctionnalités de la spécification de SQL-92, telles que les noms d’index et de qualification de colonne sont syntaxiquement comparables à un nom en plusieurs parties. La présente liste de fonctions ODBC n’est pas destinée à introduire de nouvelles options dans ces aspects de SQL-92.  
  
-   Gérer les sources de données et les connexions, en appelant **SQLConnect**, **SQLDataSources**, **SQLDisconnect**, et **SQLDriverConnect**. Obtenir des informations sur les pilotes, quel que soit le ODBC niveau pris en charge, en appelant **SQLDrivers**.  
  
-   Préparer et exécuter des instructions SQL, en appelant **SQLExecDirect**, **SQLExecute**, et **SQLPrepare**.  
  
-   Extraire une ligne d’un jeu de résultats ou de plusieurs lignes, dans la direction vers l’avant uniquement, en appelant **SQLFetch** ou en appelant **SQLFetchScroll** avec la *FetchOrientation* argument a la valeur SQL_FETCH_NEXT.  
  
-   Obtenir une colonne indépendante dans les composants, en appelant **SQLGetData**.  
  
-   Obtenir les valeurs actuelles de tous les attributs, en appelant **SQLGetConnectAttr**, **cas**, et **SQLGetStmtAttr**et tous les attributs à leurs valeurs par défaut et définir certains attributs pour les valeurs par défaut en appelant **SQLSetConnectAttr**, **SQLSetEnvAttr**, et **SQLSetStmtAttr**.  
  
-   Manipuler certains champs de descripteurs, en appelant **SQLCopyDesc**, **SQLGetDescField**, **SQLGetDescRec**, **SQLSetDescField**, et **SQLSetDescRec**.  
  
-   Obtenir des informations de diagnostic, en appelant **SQLGetDiagField** et **SQLGetDiagRec**.  
  
-   Détecter les fonctionnalités du pilote, en appelant **SQLGetFunctions** et **SQLGetInfo**. En outre, détecter le résultat de toutes les substitutions de texte effectuées dans une instruction SQL avant d’être envoyée à la source de données, en appelant **SQLNativeSql**.  
  
-   Utilisez la syntaxe de **SQLEndTran** pour valider une transaction. Un pilote au niveau du noyau devez prend pas en charge les transactions trues ; Par conséquent, l’application ne peut pas spécifier SQL_ROLLBACK ni SQL_AUTOCOMMIT_OFF pour l’attribut de connexion SQL_ATTR_AUTOCOMMIT. (Pour plus d’informations, consultez fonctionnalité 109 [mise en conformité de niveau 2 Interface](../../../odbc/reference/develop-app/level-2-interface-conformance.md).)  
  
-   Appelez **SQLCancel** pour annuler la boîte de dialogue data-at-execution et, dans un environnement multithread, pour annuler une ODBC en cours d’exécution dans un autre thread. Mise en conformité de l’interface d’au niveau du noyau n’impose pas de prise en charge de l’exécution asynchrone de fonctions, ni l’utilisation de **SQLCancel** pour annuler une fonction ODBC exécuter de façon asynchrone. La plateforme, ni le pilote ODBC doivent être multithread pour le pilote gérer les activités indépendantes en même temps. Toutefois, dans un environnement multithread, le pilote ODBC doit être thread-safe. Sérialisation des requêtes à partir de l’application est un moyen de conforme pour implémenter cette spécification, même si elle peut créer de graves problèmes de performances.  
  
-   Obtenir la colonne d’identification de ligne SQL_BEST_ROWID de tables, en appelant **SQLSpecialColumns**. (Prise en charge de SQL_ROWVER est fonctionnalité 208 de [mise en conformité de niveau 2 Interface](../../../odbc/reference/develop-app/level-2-interface-conformance.md).)  
  
    > [!IMPORTANT]  
    >  ODBC (pilotes) doit implémenter les fonctions dans le niveau de conformité de l’interface Core.
