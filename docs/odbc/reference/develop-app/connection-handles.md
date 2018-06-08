---
title: Handles de connexion | Documents Microsoft
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
- connection handles [ODBC]
- handles [ODBC], connection
ms.assetid: 12222653-f04d-46d6-bdee-61348f5d550f
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2b2f8168384c4636bab98cd64105f4c9d0884155
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-handles"></a>Handles de connexion
A *connexion* se compose d’un pilote et une source de données. Un handle de connexion identifie chaque connexion. Le handle de connexion définit non seulement le pilote à utiliser, mais la source de données à utiliser avec ce pilote. Dans un segment de code qui implémente ODBC (le Gestionnaire de pilotes ou un pilote), le handle de connexion identifie une structure qui contient les informations de connexion, tels que les éléments suivants :  
  
-   L’état de la connexion  
  
-   Les diagnostics au niveau de la connexion en cours  
  
-   Les handles d’instructions et les descripteurs actuellement allouées sur la connexion  
  
-   Les paramètres actuels de chaque attribut de connexion  
  
 ODBC n’empêche pas plusieurs connexions simultanées, si le pilote prend en charge les. Par conséquent, dans un environnement ODBC particulier, plusieurs descripteurs de connexion peuvent indiquer une variété de pilotes et les sources de données, sur le même pilote et une variété de sources de données, ou même plusieurs connexions à la même pilote et de la source de données. Certains pilotes de limitent le nombre de connexions actives, qu'ils prennent en charge ; option le SQL_MAX_DRIVER_CONNECTIONS **SQLGetInfo** Spécifie le nombre de connexions actif prend en charge par un pilote spécifique.  
  
 Handles de connexion sont principalement utilisées lors de la connexion à la source de données (**SQLConnect**, **SQLDriverConnect**, ou **SQLBrowseConnect**), déconnexion à partir de la source de données (**SQLDisconnect**), obtention d’informations sur la source de données et de pilote (**SQLGetInfo**), la récupération de diagnostics (**SQLGetDiagField** et **SQLGetDiagRec**) et l’exécution de transactions (**SQLEndTran**). Ils sont également utilisées lors de la définition et l’obtention des attributs de connexion (**SQLSetConnectAttr** et **SQLGetConnectAttr**) et lors de l’obtention du format natif d’une instruction SQL (**SQLNativeSql**).  
  
 Handles de connexion sont allouées avec **SQLAllocHandle** et libérée avec **SQLFreeHandle**.
