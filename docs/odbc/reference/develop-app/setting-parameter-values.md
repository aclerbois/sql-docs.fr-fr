---
title: Définition des valeurs de paramètre | Documents Microsoft
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
- parameter values [ODBC]
ms.assetid: 13e5da79-b60c-48d0-b467-773f481ef2a4
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9669f001498f00b79b95bf2dca1457d2bf4aadb9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-parameter-values"></a>Définition des valeurs de paramètre
Pour définir la valeur d’un paramètre, l’application définit simplement la valeur de la variable liée au paramètre. Il n’est pas important lorsque cette valeur est définie, tant qu’elle est définie avant l’exécution de l’instruction. L’application peut définir la valeur avant ou après la variable de liaison, et elle peut modifier la valeur autant de fois qu’il le souhaite. Lorsque l’instruction est exécutée, le pilote récupère simplement la valeur actuelle de la variable. Cela est particulièrement utile lors de l’exécution d’une instruction préparée plusieurs fois ; l’application définit de nouvelles valeurs pour certaines ou toutes les variables chaque fois que l’instruction est exécutée. Pour obtenir un exemple, consultez [exécution préparée](../../../odbc/reference/develop-app/prepared-execution-odbc.md), plus haut dans cette section.  
  
 Si une mémoire tampon de longueur / d’indicateur a été liée dans l’appel à **SQLBindParameter**, elle doit être définie à une des valeurs suivantes avant l’exécution de l’instruction :  
  
-   La longueur d’octet des données dans la variable liée. Le pilote vérifie cette longueur uniquement si la variable est de type caractère ou binaire (*ValueType* est SQL_C_CHAR ou SQL_C_BINARY).  
  
-   SQL_NTS. Les données sont une chaîne se terminant par null.  
  
-   SQL_NULL_DATA. La valeur de données est NULL, et le pilote ignore la valeur de la variable liée.  
  
-   SQL_DATA_AT_EXEC ou le résultat de la macro SQL_LEN_DATA_AT_EXEC. La valeur du paramètre doit être envoyé avec **SQLPutData**. Pour plus d’informations, consultez [envoyer les données de type Long](../../../odbc/reference/develop-app/sending-long-data.md), plus loin dans cette section.  
  
 Le tableau suivant montre les valeurs de la variable liée et de la mémoire tampon de longueur / d’indicateur par l’application pour une variété de valeurs de paramètre.  
  
|Paramètre<br /><br /> valeur|Paramètre<br /><br /> (SQL)<br /><br /> type de données|Variable (C)<br /><br /> type de données|Valeur dans<br /><br /> lié<br /><br /> variable|Valeur dans<br /><br /> longueur / d’indicateur<br /><br /> mémoire tampon [d]|  
|-------------------------|-----------------------------------------|----------------------------------|-------------------------------------|----------------------------------------------------|  
|"ABC"|SQL_CHAR|SQL_C_CHAR|ABC\0 [a]|SQL_NTS ou 3|  
|10|SQL_INTEGER|SQL_C_SLONG|10|--|  
|10|SQL_INTEGER|SQL_C_CHAR|10\0 [a]|SQL_NTS ou 2|  
|À 13 H|SQL_TYPE_TIME|SQL_C_TYPE_TIME|13,0,0 [b]|--|  
|À 13 H|SQL_TYPE_TIME|SQL_C_CHAR|{t 13 : 00:00'} \0 [a], [c]|SQL_NTS ou 14|  
|NULL|SQL_SMALLINT|SQL_C_SSHORT|--|SQL_NULL_DATA|  
  
 [a] « \0 » représente un caractère de fin de la valeur null. Le caractère de fin de la valeur null est requis uniquement si la valeur de la mémoire tampon de longueur / d’indicateur est SQL_NTS.  
  
 [b] les nombres dans cette liste sont les nombres stockés dans les champs de la structure TIME_STRUCT.  
  
 [c] la chaîne utilise la clause d’échappement de date ODBC. Pour plus d’informations, consultez [Date, Time et Timestamp littéraux](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md).  
  
 [d] pilotes doivent toujours vérifier cette valeur pour déterminer s’il est une valeur spéciale, comme SQL_NULL_DATA.  
  
 Ce que fait par un pilote avec une valeur de paramètre au moment de l’exécution dépend du pilote. Si nécessaire, le pilote convertit la valeur de la C codés et type de longueur des données de la variable liée pour le type de données SQL, la précision et l’échelle du paramètre. Dans la plupart des cas, le pilote envoie ensuite la valeur de la source de données. Dans certains cas, il met en forme la valeur sous forme de texte et l’insère dans l’instruction SQL avant d’envoyer l’instruction à la source de données.
