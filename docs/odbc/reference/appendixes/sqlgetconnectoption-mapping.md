---
title: Mappage de SQLGetConnectOption | Documents Microsoft
ms.custom: 
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: odbc
ms.reviewer: 
ms.suite: sql
ms.technology: drivers
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLGetConnectOption
- SQLGetConnectOption function [ODBC], mapping
ms.assetid: e3792fe4-a955-473a-a297-c1b2403660c4
caps.latest.revision: "5"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: faf67d9649911122b0b3fc19905c50e94af684ab
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="sqlgetconnectoption-mapping"></a>Mappage de SQLGetConnectOption
Lorsqu’une application appelle **SQLGetConnectOption** via un ODBC 3*.x* pilote, l’appel à  
  
```  
SQLGetConnectOption(hdbc, fOption, pvParam)   
```  
  
 est mappé comme suit :  
  
-   Si *fOption* indique une option de connexion de définie par ODBC qui retourne une chaîne, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
-   Si *fOption* indique une option de connexion de définie par ODBC qui retourne une valeur d’entier 32 bits, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0, NULL)  
    ```  
  
-   Si *fOption* indique une option d’instruction définie par le pilote, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
 Dans trois cas précédents, les *handle de connexion* argument a la valeur de la valeur de *pas*, le *attribut* argument a la valeur de la valeur de *fOption*et le *ValuePtr* argument est défini sur la même valeur que *pvParam*.  
  
 Pour les options de connexion de chaîne définie par ODBC, le Gestionnaire de pilotes définit le *BufferLength* argument dans l’appel à **SQLGetConnectAttr** à la longueur maximale prédéfinie (SQL_MAX_OPTION_STRING_LENGTH) ; pour l’option de connexion sans chaînes, *BufferLength* est définie sur 0.  
  
 Pour un ODBC 3*.x* pilote, le Gestionnaire de pilotes ne sont plus vérifie si *Option* est entre SQL_CONN_OPT_MIN et SQL_CONN_OPT_MAX, ou est supérieure à SQL_CONNECT_OPT_DRVR_START. Le pilote doit vérifier la validité des valeurs d’option.
