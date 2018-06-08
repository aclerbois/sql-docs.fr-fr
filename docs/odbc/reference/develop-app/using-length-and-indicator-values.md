---
title: À l’aide de la longueur et les valeurs d’indicateur | Documents Microsoft
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
- length/indicator buffers [ODBC]
- length of data buffers [ODBC]
- buffers [ODBC], length
ms.assetid: 849792f1-cb1e-4bc2-b568-c0aff0b66199
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c1a59b47c8394187f094b8d7c5e1f15611a8f8ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-length-and-indicator-values"></a>À l’aide de la longueur et les valeurs d’indicateur
La mémoire tampon de longueur / d’indicateur est utilisée pour passer la longueur en octets des données dans le tampon de données ou un indicateur spécial tel que SQL_NULL_DATA, ce qui indique que les données sont NULL. Selon la fonction dans laquelle il est utilisé, une mémoire tampon de longueur / d’indicateur est définie comme un SQLINTEGER ou un SQLSMALLINT. Par conséquent, un seul argument est nécessaire pour le décrire. Si la mémoire tampon de données est un tampon d’entrée nondeferred, cet argument contient la longueur en octets des données elles-mêmes ou une valeur de l’indicateur. Il est souvent appelée *StrLen_or_Ind* ou un nom similaire. Par exemple, les éléments suivants code appelle **SQLPutData** à passer une mémoire tampon complète de données ; la longueur d’octet (*ValueLen*) est passée directement, car le tampon de données (*ValuePtr*) est un mémoire tampon d’entrée.  
  
```  
SQLCHAR      ValuePtr[50];  
SQLINTEGER   ValueLen;  
  
// Call local function to place data in ValuePtr. In ValueLen, return the  
// number of bytes of data placed in ValuePtr. If there is not enough  
// data, this will be less than 50.  
FillBuffer(ValuePtr, sizeof(ValuePtr), &ValueLen);  
  
// Call SQLPutData to send the data to the driver.  
SQLPutData(hstmt, ValuePtr, ValueLen);  
```  
  
 Si la mémoire tampon de données est un tampon d’entrée différée, une mémoire tampon de sortie nondeferred ou un mémoire tampon de sortie, l’argument contient l’adresse de la mémoire tampon de longueur / d’indicateur. Il est souvent appelée *StrLen_or_IndPtr* ou un nom similaire. Par exemple, les éléments suivants code appelle **SQLGetData** pour récupérer une mémoire tampon de données ; la longueur d’octet est retournée à l’application dans la mémoire tampon de longueur / d’indicateur (*ValueLenOrInd*), dont l’adresse est passée à **SQLGetData** , car le tampon de données correspondante (*ValuePtr*) est une mémoire tampon de sortie nondeferred.  
  
```  
SQLCHAR      ValuePtr[50];  
SQLINTEGER   ValueLenOrInd;  
SQLGetData(hstmt, 1, SQL_C_CHAR, ValuePtr, sizeof(ValuePtr), &ValueLenOrInd);  
```  
  
 Sauf si elle est interdite spécifiquement, un argument de mémoire tampon de longueur / d’indicateur peut être 0 (si entrée nondeferred) ou un pointeur null (si sortie ou entrée différée). Pour les tampons d’entrée, cela provoque le pilote ignorer la longueur en octets des données. Cela retourne une erreur lors du passage des données de longueur variable, mais est courant lors de la transmission des données non null, de longueur fixe, car la longueur, ni une valeur de l’indicateur est nécessaire. Pour les mémoires tampons de sortie, cela entraîne le pilote à ne pas retourner la longueur en octets des données ou une valeur de l’indicateur. Il s’agit d’une erreur si les données retournées par le pilote a la valeur NULL, mais sont courantes lors de la récupération des données de longueur fixe, non nullable, car une longueur, ni une valeur de l’indicateur est nécessaire.  
  
 Comme lors de l’adresse d’une mémoire tampon de données différée est passée au pilote, l’adresse d’une mémoire tampon de longueur / d’indicateur différée doit rester valide jusqu'à ce que la mémoire tampon est indépendante.  
  
 Les longueurs suivantes sont valides en tant que valeurs de longueur / d’indicateur :  
  
-   *n*, où *n* > 0.  
  
-   0.  
  
-   SQL_NTS. Une chaîne envoyée au pilote dans le tampon de données correspondante est terminée par null ; Il s’agit d’un moyen pratique pour les programmeurs en langage C passer des chaînes sans avoir à calculer leur longueur en octets. Cette valeur est autorisée uniquement lorsque l’application envoie des données au pilote. Lorsque le pilote retourne des données à l’application, elle retourne toujours la longueur d’octet réelle des données.  
  
 Les valeurs suivantes sont valides en tant que valeurs de longueur / d’indicateur. SQL_NULL_DATA est stocké dans le champ de descripteur SQL_DESC_INDICATOR_PTR ; toutes les autres valeurs sont stockées dans le champ de descripteur SQL_DESC_OCTET_LENGTH_PTR.  
  
-   SQL_NULL_DATA. Les données sont une valeur de données de valeur NULL, et la valeur dans la mémoire tampon de données correspondante est ignorée. Cette valeur est valide uniquement pour les données SQL ou récupérées à partir du pilote.  
  
-   SQL_DATA_AT_EXEC. Le tampon de données ne contient pas de données. Au lieu de cela, les données sont envoyées avec **SQLPutData** lorsque l’instruction est exécutée ou lorsque **SQLBulkOperations** ou **SQLSetPos** est appelée. Cette valeur est valide uniquement pour les données SQL envoyées au pilote. Pour plus d’informations, consultez [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), et [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
-   Résultat de la SQL_LEN_DATA_AT_EXEC (*longueur*) (macro). Cette valeur est similaire à SQL_DATA_AT_EXEC. Pour plus d’informations, consultez [envoyer les données de type Long](../../../odbc/reference/develop-app/sending-long-data.md).  
  
-   SQL_NO_TOTAL. Le pilote ne peut pas déterminer le nombre d’octets de données de type long toujours disponibles à renvoyer dans un mémoire tampon de sortie. Cette valeur est valide uniquement pour les données SQL récupérées à partir du pilote.  
  
-   SQL_DEFAULT_PARAM. Une procédure consiste à utiliser la valeur par défaut d’un paramètre d’entrée dans une procédure au lieu de la valeur dans la mémoire tampon de données correspondante.  
  
-   SQL_COLUMN_IGNORE. **SQLBulkOperations** ou **SQLSetPos** consiste à ignorer la valeur dans la mémoire tampon de données. Lors de la mise à jour d’une ligne de données par un appel à **SQLBulkOperations** ou **SQLSetPos,** la valeur de la colonne n’est pas modifiée. Lors de l’insertion d’une nouvelle ligne de données par un appel à **SQLBulkOperations**, la valeur de colonne est définie à sa valeur par défaut ou, si la colonne n’a pas de valeur par défaut, null.
