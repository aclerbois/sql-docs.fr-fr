---
title: "Allouer le Handle d’environnement | Documents Microsoft"
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
- ODBC drivers [ODBC], environment handles
- allocating environment handles [ODBC]
- connecting to driver [ODBC], environment handles
- environment handles [ODBC]
- data sources [ODBC], environment handles
- connecting to data source [ODBC], environment handles
- handles [ODBC], environment
ms.assetid: 77b5d1d6-7eb7-428d-bf75-a5c5a325d25c
caps.latest.revision: "6"
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 6ac926c39f1390431b35b49b27e7302fe789ca4a
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="allocating-the-environment-handle"></a>Allouer le Handle d’environnement
La première tâche pour une application ODBC consiste à charger le Gestionnaire de pilotes ; Cette opération dépend du système d’exploitation. Par exemple, sur un ordinateur exécutant Microsoft Windows® 95/98, Windows NT Workstation et Windows 2000 Professionnel ou Microsoft® Windows NT® Server et Windows 2000 Server, l’application soit liée à la bibliothèque du Gestionnaire de pilotes ou les appels **LoadLibrary** de charger la DLL du Gestionnaire de pilotes.  
  
 La tâche suivante, qui doit être effectuée avant qu’une application puisse appeler toute autre fonction ODBC, consiste à initialiser l’environnement ODBC et allouer un handle d’environnement, comme suit :  
  
1.  L’application déclare une variable de type SQLHENV. Il appelle ensuite **SQLAllocHandle** et transmet l’adresse de cette variable et l’option de SQL_HANDLE_ENV. Exemple :  
  
    ```  
    SQLHENV henv1;  
  
    SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv1);  
    ```  
  
2.  Le Gestionnaire de pilotes alloue une structure dans lequel stocker les informations sur l’environnement et retourne le handle d’environnement dans la variable.  
  
 Le Gestionnaire de pilotes n’appelle pas **SQLAllocHandle** dans le pilote à cet instant, car il ne connaît pas le pilote à appeler. Il diffère de l’appel **SQLAllocHandle** dans le pilote jusqu'à ce que l’application appelle une fonction pour se connecter à une source de données. Pour plus d’informations, consultez [rôle du Gestionnaire de pilotes dans le processus de connexion](../../../odbc/reference/develop-app/driver-manager-s-role-in-the-connection-process.md), plus loin dans cette section.  
  
 Lorsque l’application a terminé à l’aide d’ODBC, elle libère le handle d’environnement avec **SQLFreeHandle**. Après la libération de l’environnement, il s’agit d’une erreur de programmation d’application à utiliser le handle de l’environnement dans un appel à une fonction ODBC ; Cette opération donc des conséquences non défini mais probablement irrécupérable.  
  
 Lorsque **SQLFreeHandle** est appelée, les versions de pilote la structure utilisée pour stocker des informations sur l’environnement. Notez que **SQLFreeHandle** ne peut pas être appelée pour un handle d’environnement jusqu'à ce qu’une fois que tous les handles de connexion sur le handle d’environnement ont été libérées.  
  
 Pour plus d’informations sur le handle d’environnement, consultez [environnement gère](../../../odbc/reference/develop-app/environment-handles.md).
