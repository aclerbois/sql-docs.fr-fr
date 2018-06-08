---
title: Adresse du tampon de données | Documents Microsoft
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
- address of data buffers [ODBC]
- buffers [ODBC], data
- data buffers [ODBC], address
ms.assetid: f2426d68-71bc-4ef7-a5cb-ee9d6c1c9671
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 46b961c1820714e79a0d362c187272105e90f656
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-buffer-address"></a>Adresse de mémoire tampon de données
L’application transmet l’adresse de la mémoire tampon de données pour le pilote dans un argument nommé souvent *ValuePtr* ou un nom similaire. Par exemple, dans l’exemple suivant appel à **SQLBindCol**, l’application spécifie l’adresse de la *Date* variable :  
  
```  
SQL_DATE_STRUCT Date;  
SQLINTEGER DateInd;  
SQLBindCol(hstmt, 1, SQL_C_TYPE_DATE, &dsDate, 0, &DateInd);  
```  
  
 Comme indiqué dans le [affectation et la libération de mémoires tampons](../../../odbc/reference/develop-app/allocating-and-freeing-buffers.md) section, l’adresse d’une mémoire tampon différée doit rester valide jusqu'à ce que la mémoire tampon est indépendante.  
  
 Sauf si elle est interdite spécifiquement, l’adresse d’une mémoire tampon de données peut être un pointeur null. Pour les mémoires tampons utilisées pour envoyer des données au pilote, cela provoque le pilote ignorer les informations généralement contenues dans la mémoire tampon. Pour les mémoires tampons utilisées pour récupérer des données à partir du pilote, cela entraîne le pilote à ne pas retourner une valeur. Dans les deux cas, le pilote ignore l’argument de longueur de mémoire tampon données correspondant.
