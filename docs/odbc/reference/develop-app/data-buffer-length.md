---
title: Longueur du tampon de données | Documents Microsoft
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
- data buffers [ODBC], length
- buffers [ODBC], data
- length of data buffers [ODBC]
- buffers [ODBC], length
ms.assetid: 7288d143-f9e5-4f90-9b31-2549df79c109
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b40fb1b58a10ddd5db371869584b4d29f2110855
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-buffer-length"></a>Longueur de mémoire tampon de données
L’application transmet la longueur d’octet de la mémoire tampon de données pour le pilote dans un argument nommé *BufferLength* ou un nom similaire. Par exemple, dans l’exemple suivant appel à **SQLBindCol**, l’application spécifie la longueur de la *ValuePtr* tampon (**sizeof (***ValuePtr***)**):  
  
```  
SQLCHAR      ValuePtr[50];  
SQLINTEGER   ValueLenOrInd;  
SQLBindCol(hstmt, 1, SQL_C_CHAR, ValuePtr, sizeof(ValuePtr), &ValueLenOrInd);  
```  
  
 Un pilote retourne toujours le nombre d’octets, pas le nombre de caractères, dans l’argument de longueur de la mémoire tampon de n’importe quelle fonction qui a un argument de chaîne de sortie.  
  
 Longueurs de mémoire tampon de données sont nécessaires uniquement pour les mémoires tampons de sortie ; le pilote utilise les pour éviter d’écrire au-delà de la fin de la mémoire tampon. Toutefois, le pilote vérifie la longueur de mémoire tampon de données uniquement lorsque la mémoire tampon contient des données de longueur variable, telles que binaire ou caractère. Si la mémoire tampon contient des données de longueur fixe, par exemple une structure entier ou date, le pilote ignore la longueur de mémoire tampon de données et suppose que la mémoire tampon est suffisamment grande pour contenir les données ; Autrement dit, il jamais tronque les données de longueur fixe. Il est donc important de l’application pour allouer une mémoire tampon suffisamment grand pour les données de longueur fixe.  
  
 Lorsqu’une troncation des données non-sortie chaînes se produit (par exemple, le nom du curseur renvoyée pour **SQLGetCursorName**), la longueur retournée dans l’argument de longueur de la mémoire tampon est le nombre maximal de caractères possible.  
  
 Longueurs de mémoire tampon de données ne sont pas requis pour les mémoires tampons d’entrée, car le pilote n’écrit pas dans ces mémoires tampons.  
  
 Cette section contient les rubriques suivantes.  
  
-   [À l’aide des valeurs de longueur / d’indicateur](../../../odbc/reference/develop-app/using-length-and-indicator-values.md)  
  
-   [Longueur des données, longueur de la mémoire tampon et troncation](../../../odbc/reference/develop-app/data-length-buffer-length-and-truncation.md)  
  
-   [Données caractères et chaînes C](../../../odbc/reference/develop-app/character-data-and-c-strings.md)
