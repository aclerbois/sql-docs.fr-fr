---
title: Nouvelles fonctionnalités | Documents Microsoft
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
- backward compatibility [ODBC], new features in release
- ODBC drivers [ODBC], backward compatibility
- new features [ODBC]
- compatibility [ODBC], new features in release
- ODBC [ODBC], new features
ms.assetid: a8fcdd00-6cb3-4871-9489-6018b3d0d65f
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ed343aadff0390858230a4b8c3512245ed338062
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-features"></a>Nouvelles fonctionnalités
Les nouvelles fonctionnalités suivantes a été introduite dans ODBC 3. *x*. Une application ODBC 3. *x* application utilisant une API ODBC 2 *.x* pilote ne sera pas en mesure d’utiliser cette fonctionnalité. ODBC 3. *x* du Gestionnaire de pilotes ne mappe pas ces fonctionnalités lorsque vous travaillez avec une API ODBC 2 *.x* pilote.  
  
-   Gérer les fonctions qui acceptent un descripteur en tant qu’argument : **SQLSetDescField**, **SQLGetDescField**, **SQLSetDescRec**, **SQLGetDescRec**, et **SQLCopyDesc**.  
  
-   Les fonctions **SQLSetEnvAttr** et **cas**.  
  
-   L’utilisation de **SQLAllocHandle** pour allouer un handle de descripteur. (L’utilisation de **SQLAllocHandle** pour allouer des handles d’environnement, de connexion et d’instruction est en double, de nouvelles fonctionnalités.)  
  
-   L’utilisation de **SQLGetConnectAttr** pour obtenir les attributs de connexion SQL_ATTR_AUTO_IPD. (L’utilisation de **SQLSetConnectAttr** à définir, et **SQLGetConnectAttr** pour obtenir, les autres attributs de connexion est en double, de nouvelles fonctionnalités.)  
  
-   L’utilisation de **SQLSetStmtAttr** à définir, et **SQLGetStmtAttr** à obtenir, les attributs de l’instruction suivante. (L’utilisation de **SQLSetStmtAttr** à définir, et **SQLGetStmtAttr** pour obtenir, les autres attributs de l’instruction est en double, de nouvelles fonctionnalités.)  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_ENABLE_AUTO_IPD  
  
     SQL_ATTR_FETCH_BOOKMARK_PTR  
  
     SQL_ATTR_BIND_OFFSET  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     SQL_DESC_PARAM_STATUS_PTR  
  
     SQL_ATTR_PARAMS_PROCESSED_PTR  
  
     SQL_ATTR_PARAMSET_SIZE  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_ROW_ARRAY_SIZE  
  
-   L’utilisation de **SQLGetStmtAttr** pour obtenir les attributs de l’instruction suivante. (L’utilisation de **SQLGetStmtAttr** pour obtenir des instructions d’attributs est la fonctionnalité dupliquée, pas de nouvelles fonctionnalités.)  
  
     SQL_ATTR_IMP_ROW_DESC SQL_ATTR_IMP_PARAM_DESC  
  
-   Utilisation de l’intervalle C type de données, les types de données SQL intervalle, les types de données BIGINT C et la structure de données SQL_C_NUMERIC.  
  
-   Liaison selon les lignes de paramètres.  
  
-   Basé sur le décalage de signet extrait, telles que l’appel **SQLFetchScroll** avec un *FetchOrientation* argument de SQL_FETCH_BOOKMARK et en spécifiant un offset différent de 0.  
  
-   **SQLFetch** qui retourne le tableau d’état de ligne, nombre de lignes extraites, l’extraction de plusieurs lignes, les appels mélanger avec **SQLFetchScroll**et mélanger des appels avec **SQLBulkOperations** ou **SQLSetPos**. Pour plus d’informations, consultez la section suivante, [curseurs de bloc, les curseurs permettant le défilement et la compatibilité descendante pour les Applications ODBC 3.x](../../../odbc/reference/develop-app/block-cursors-scrollable-backward-compatibility-odbc-3-x-applications.md).  
  
-   Paramètres nommés.  
  
-   Une des ODBC 3. *x*– spécifique **SQLGetInfo** options. (Le cas une ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote appelle les types d’informations SQL_XXX_CURSOR_ATTRIBUTES1, qui ont remplacé plusieurs ODBC 2. *x* types d’informations, certaines informations peuvent être fiable, mais certaines peuvent être non fiable. Pour plus d’informations, consultez [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).)  
  
-   Lier les décalages.  
  
-   Mise à jour, l’actualisation et suppression de signets (via un appel à **SQLBulkOperations**).  
  
-   Appel de **SQLBulkOperations** ou **SQLSetPos** dans l’état S5.  
  
-   Les champs ROW_NUMBER et numéro_colonne dans l’enregistrement de diagnostic (qui doivent être extraits par les fonctions de remplacement **SQLGetDiagField** ou **SQLGetDiagRec**).  
  
-   Le nombre de lignes approximatif.  
  
-   Informations d’avertissement (SQL_ROW_SUCCESS_WITH_INFO de **SQLFetchScroll**).  
  
-   Signets de longueur variable.  
  
-   Informations d’erreur étendues pour les tableaux de paramètres.  
  
-   Toutes les nouvelles colonnes dans les jeux de résultats retournés par les fonctions de catalogue.  
  
-   Utilisation de **SQLDescribeCol** et **SQLColAttribute** sur la colonne 0.  
  
-   Utilisation de n’importe quel ODBC 3. *x*: les attributs de colonne spécifique dans un appel à **SQLColAttribute**.  
  
-   Utilisation de plusieurs descripteurs d’environnement.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Curseurs de bloc, curseurs avec défilement et compatibilité descendante pour les applications ODBC 3.x](../../../odbc/reference/develop-app/block-cursors-scrollable-backward-compatibility-odbc-3-x-applications.md)
