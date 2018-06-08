---
title: Mappage de SQLGetStmtOption | Documents Microsoft
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
- SQLGetStmtOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLGetStmtOption
ms.assetid: fa599517-3f3e-4dad-a65a-b8596ae3f330
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 02eaacbe3503b5d93677633aa0e98326b14e304f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgetstmtoption-mapping"></a>Mappage de SQLGetStmtOption
Lorsqu’une application appelle **SQLGetStmtOption** à un ODBC 3 *.x* pilote qui ne prend pas en charge il, l’appel à  
  
```  
SQLGetStmtOption(hstmt, fOption, pvParam)  
```  
  
 suite, comme suit :  
  
-   Si *fOption* indique une option ODBC définie par l’instruction qui retourne une chaîne, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
-   Si *fOption* indique une option ODBC définie par l’instruction qui retourne une valeur d’entier 32 bits, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, 0, NULL)  
    ```  
  
-   Si *fOption* indique une option d’instruction définie par le pilote, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
 Dans trois cas précédents, les *au paramètre StatementHandle* argument a la valeur de la valeur de *hstmt*, le *attribut* argument a la valeur de la valeur de *fOption*et le *ValuePtr* argument est défini sur la même valeur que *pvParam*.  
  
 Pour les options de connexion de chaîne définie par ODBC, le Gestionnaire de pilotes définit le *BufferLength* argument dans l’appel à **SQLGetConnectAttr** à la longueur maximale prédéfinie (SQL_MAX_OPTION_STRING_LENGTH) ; pour l’option de connexion sans chaînes, *BufferLength* est définie sur 0.  
  
 L’option d’instruction SQL_GET_BOOKMARK a été déconseillée dans ODBC 3 *.x*. Pour un ODBC 3 *.x* pilote pour fonctionner avec ODBC 2. *x* les applications qui utilisent SQL_GET_BOOKMARK, il doit prendre en charge SQL_GET_BOOKMARK. Pour un ODBC 3 *.x* pilote pour fonctionner avec ODBC 2. *x* applications, il doit prendre en charge affectant SQL_USE_BOOKMARKS SQL_UB_ON et doit exposer des signets de longueur fixe. Si un ODBC 3 *.x* pilote prend en charge uniquement des signets de longueur variable, les signets pas à longueur fixe, elle doit retourner la valeur SQLSTATE HYC00 (fonctionnalité facultative non implémentée) si un ODBC 2. *x* application tente de la valeur SQL_USE_BOOKMARKS SQL_UB_ON.  
  
 Pour un ODBC 3 *.x* pilote, le Gestionnaire de pilotes ne sont plus vérifie si *Option* est entre SQL_STMT_OPT_MIN et SQL_STMT_OPT_MAX, ou est supérieure à SQL_CONNECT_OPT_DRVR_START. Le pilote doit vérifier.
