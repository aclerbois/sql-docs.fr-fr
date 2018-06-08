---
title: Mappage de SQLSetConnectOption | Documents Microsoft
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
- SQLSetConnectOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetConnectOption
ms.assetid: a1b325cf-0c42-41c1-b141-b5a4fee7e708
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fcb6465f499686f37bc6fb62a6311d41798024ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsetconnectoption-mapping"></a>Mappage de SQLSetConnectOption
Lorsqu’une application ODBC 2. *x* application appelle **SQLSetConnectOption** via un ODBC 3 *.x* pilote, l’appel à  
  
```  
SQLSetConnectOption(hdbc, fOption, vParam)  
```  
  
 suite, comme suit :  
  
-   Si *fOption* indique un attribut défini par ODBC de connexion qui nécessite une chaîne, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, SQL_NTS)  
    ```  
  
-   Si *fOption* indique un attribut de connexion de définie par ODBC qui retourne une valeur d’entier 32 bits, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0)  
    ```  
  
-   Si *fOption* indique un attribut définies par le pilote de connexion, les appels du Gestionnaire de pilotes  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength)  
    ```  
  
 Dans trois cas précédents, les *handle de connexion* argument a la valeur de la valeur de *pas*, le *attribut* argument a la valeur de la valeur de *fOption*et le *ValuePtr* argument est défini sur la même valeur que *vParam*.  
  
 Étant donné que le Gestionnaire de pilotes ne sait pas si l’attribut de connexion de définies par le pilote a besoin d’une chaîne ou une valeur d’entier 32 bits, il doit passer une valeur valide pour le *BufferLength* argument de **SQLSetConnectAttr**. Si le pilote a défini une sémantique spéciale pour définies par le pilote connecter doit être appelée à l’aide et des attributs **SQLSetConnectOption**, il doit prendre en charge **SQLSetConnectOption**.  
  
 If un ODBC 2. *x* application appelle **SQLSetConnectOption** pour définir une option d’instruction spécifiques au pilote dans un ODBC 3 *.x* pilote et l’option a été définie dans une API ODBC 2. *x* version du pilote, une nouvelle constante de manifeste doit être définie pour l’option dans ODBC 3 *.x* pilote. Si la constante de manifeste ancien est utilisée dans l’appel à **SQLSetConnectOption**, appelle le Gestionnaire de pilotes **SQLSetConnectAttr** avec la **StringLength** argument défini à 0.  
  
 Pour un ODBC 3 *.x* pilote, le Gestionnaire de pilotes ne sont plus vérifie si *fOption* est entre SQL_CONN_OPT_MIN et SQL_CONN_OPT_MAX, ou est supérieure à SQL_CONNECT_OPT_DRVR_START.  
  
## <a name="setting-statement-options-on-the-connection-level"></a>Définition des Options d’instruction sur le niveau de connexion  
 Dans ODBC 2. *x*, une application peut appeler **SQLSetConnectOption** pour définir une option d’instruction. Lorsque que vous avez terminé, le pilote établit l’option d’instruction comme une valeur par défaut pour toute instruction plus tard allouée pour cette connexion. Il est définies par le pilote si le pilote définit l’option d’instruction pour toute instruction existante associé à la connexion spécifiée.  
  
 Cette fonctionnalité a été déconseillée dans ODBC 3 *.x*. ODBC 3 *.x* pilotes doivent prennent uniquement en charge le paramètre ODBC 2. *x* les attributs d’instruction au niveau de la connexion s’ils veulent travailler avec ODBC 2. *x* applications cela. ODBC 3 *.x* applications ne doivent jamais définir les attributs d’instruction au niveau de la connexion. ODBC 3 *.x* les attributs d’instruction ne peut pas être définies au niveau de la connexion, à l’exception des attributs SQL_ATTR_METADATA_ID et SQL_ATTR_ASYNC_ENABLE, qui sont des attributs de connexion et les attributs d’instruction et peut être définie au niveau de la connexion ou de niveau de l’instruction.
