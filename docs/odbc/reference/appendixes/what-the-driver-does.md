---
title: Ce que fait le pilote | Documents Microsoft
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
- scrollable cursors [ODBC]
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: 75dcdea6-ff6b-4ac8-aa11-a1f9edbeb8e6
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fa114af7390d3ac4f971c507f508d8f747e134c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="what-the-driver-does"></a>Ce que fait le pilote
Le tableau suivant résume les fonctions attributs et d’instruction un ODBC 3 *.x* pilote doit implémenter pour le blocage et les curseurs de défilement.  
  
|Fonction ou<br /><br /> attribut d'instruction|Commentaires|  
|-----------------------------------------|--------------|  
|SQL_ATTR_ROW_STATUS_PTR|Définit l’adresse du tableau d’état de ligne remplie par **SQLFetch** et **SQLFetchScroll**. Ce tableau est également rempli par **SQLSetPos** si **SQLSetPos** instruction état S6 est appelée. Si **SQLSetPos** est appelée dans un état S7, ce tableau n’est pas renseigné, mais le tableau vers lequel pointe le *RowStatusArray* argument de **SQLExtendedFetch** est remplie. Pour plus d’informations, consultez [Transitions de l’instruction](../../../odbc/reference/appendixes/statement-transitions.md) dans les Tables de Transition d’état annexe b : ODBC.|  
|SQL_ATTR_ROWS_FETCHED_PTR|Définit l’adresse de la mémoire tampon dans laquelle **SQLFetch** et **SQLFetchScroll** retourner le nombre de lignes extraites. Si **SQLExtendedFetch** est appelée, cette mémoire tampon n’est pas remplie, mais le *RowCountPtr* argument pointe vers le nombre de lignes extraites.|  
|SQL_ATTR_ROW_ARRAY_SIZE|Définit la taille de l’ensemble de lignes utilisée par **SQLFetch** et **SQLFetchScroll**.|  
|SQL_ROWSET_SIZE|Définit la taille de l’ensemble de lignes utilisée par **SQLExtendedFetch**. ODBC 3 *.x* pilotes implémentent cela s’ils veulent travailler avec ODBC 2. *x* les applications qui appellent **SQLExtendedFetch** ou **SQLSetPos**.|  
|**SQLBulkOperations**|Si un ODBC 3 *.x* pilote doit fonctionner avec ODBC 2. *x* les applications qui utilisent **SQLSetPos** avec un *opération* de SQL_ADD, le pilote doit prendre en charge **SQLSetPos** avec un *opération* de SQL_ADD à **SQLBulkOperations** avec un *opération* de SQL_ADD.|  
|**SQLExtendedFetch**|Retourne l’ensemble de lignes spécifié. ODBC 3 *.x* pilotes implémentent cela s’ils veulent travailler avec ODBC 2. *x* les applications qui appellent **SQLExtendedFetch** ou **SQLSetPos**. Détails d’implémentation sont les suivantes :<br /><br /> -Le pilote récupère la taille de l’ensemble de lignes à partir de la valeur de l’attribut d’instruction SQL_ROWSET_SIZE.<br />-Le pilote récupère l’adresse du tableau d’état de ligne à partir de la *RowStatusArray* argument, pas l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR. Le *RowStatusArray* argument dans un appel à **SQLExtendedFetch** ne doit pas être un pointeur null. (Notez que dans ODBC 3 *.x*, l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR peut être un pointeur null.)<br />-Le pilote récupère l’adresse de la mémoire tampon de lignes lues à partir de la *RowCountPtr* argument, pas l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.<br />-Le pilote retourne 01 s 01 SQLSTATE (erreur de ligne) pour indiquer qu’une erreur s’est produite alors que les lignes ont été extraites par un appel à **SQLExtendedFetch**. Un ODBC 3 *.x* pilote doit retourner 01 s 01 SQLSTATE (erreur de ligne) uniquement lorsque **SQLExtendedFetch** est appelée, pas lorsque **SQLFetch** ou **SQLFetchScroll** est appelée. Pour préserver la compatibilité descendante, lorsque 01 s 01 SQLSTATE (erreur de ligne) est retourné par **SQLExtendedFetch**, le Gestionnaire de pilotes ne trie pas les enregistrements d’état dans la file d’attente de l’erreur selon les règles indiquées dans la section « Séquence d’enregistrements d’état » de [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).|  
|**SQLFetch**|Retourne l’ensemble de lignes suivant. Détails d’implémentation sont les suivantes :<br /><br /> -Le pilote récupère la taille de l’ensemble de lignes à partir de la valeur de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.<br />-Le pilote récupère l’adresse du tableau d’état de ligne à partir de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.<br />-Le pilote récupère l’adresse de lignes extraites de mémoire tampon à partir de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.<br />-L’application peut combiner des appels entre **SQLFetchScroll** et **SQLFetch**.<br />-   **SQLFetch** renvoie des signets, si la colonne 0 est liée.<br />-   **SQLFetch** peut être appelée pour retourner plusieurs lignes.<br />-Le pilote ne retourne pas de 01 s 01 SQLSTATE (erreur de ligne) pour indiquer qu’une erreur s’est produite alors que les lignes ont été extraites par un appel à **SQLFetch**.|  
|**SQLFetchScroll**|Retourne l’ensemble de lignes spécifié. Détails d’implémentation sont les suivantes :<br /><br /> -Le pilote récupère la taille de l’ensemble de lignes à partir de l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE.<br />-Le pilote récupère l’adresse du tableau d’état de ligne à partir de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.<br />-Le pilote récupère l’adresse de lignes extraites de mémoire tampon à partir de l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR.<br />-L’application peut combiner des appels entre **SQLFetchScroll** et **SQLFetch**.<br />-Le pilote ne retourne pas de 01 s 01 SQLSTATE (erreur de ligne) pour indiquer qu’une erreur s’est produite alors que les lignes ont été extraites par un appel à **SQLFetchScroll**.|  
|**SQLSetPos**|Effectue des opérations positionnées différents. Détails d’implémentation sont les suivantes :<br /><br /> -Il peut être appelé dans les États d’instruction S6 ou S7. Pour plus d’informations, consultez [Transitions de l’instruction](../../../odbc/reference/appendixes/statement-transitions.md) dans les Tables de Transition d’état annexe b : ODBC.<br />-Si cela est appelée dans un état d’instruction S5 ou S6, le pilote récupère la taille de l’ensemble de lignes à partir de l’attribut SQL_ATTR_ROWS_FETCHED_PTR d’instruction et l’adresse du tableau d’état de ligne à partir de l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR.<br />-Si cela est appelée dans l’état de l’instruction S7, le pilote récupère la taille de l’ensemble de lignes à partir de l’attribut d’instruction SQL_ROWSET_SIZE et l’adresse du tableau d’état de ligne à partir de la *RowStatusArray* argument de **SQLExtendedFetch**.<br />-Le pilote retourne 01 s 01 SQLSTATE (erreur de ligne) uniquement pour indiquer qu’une erreur s’est produite alors que les lignes ont été extraites par un appel à **SQLSetPos** pour effectuer une opération en bloc lorsque la fonction est appelée dans un état S7. Pour conserver la compatibilité descendante, si 01 s 01 SQLSTATE (erreur de ligne) est retourné par **SQLSetPos**, le Gestionnaire de pilotes ne trie pas les enregistrements d’état dans la file d’attente de l’erreur selon les règles indiquées dans la section « Séquence d’enregistrements d’état » de [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).<br />-Si le pilote doit fonctionner avec ODBC 2. *x* les applications qui appellent **SQLSetPos** avec un *opération* argument de SQL_ADD, le pilote doit prendre en charge **SQLSetPos** avec un *opération* argument de SQL_ADD.|
