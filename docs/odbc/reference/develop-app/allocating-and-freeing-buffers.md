---
title: Allocation et la libération des mémoires tampons | Documents Microsoft
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
- buffers [ODBC], allocating and freeing
- allocating buffers [ODBC]
- freeing buffers [ODBC]
ms.assetid: 886bc9ed-39d4-43d2-82ff-aebc35b14d39
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e85c2ab4ad25501637ccba2206f85c7895440ade
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="allocating-and-freeing-buffers"></a>Allocation et la libération des mémoires tampons
Toutes les mémoires tampons sont alloués et libérés par l’application. Si une mémoire tampon n’est pas reportée, il doit exister seulement pendant la durée de l’appel à une fonction. Par exemple, **SQLGetInfo** retourne la valeur associée à une option spécifique dans la mémoire tampon vers laquelle pointée le *InfoValuePtr* argument. Cette mémoire tampon peut être libéré immédiatement après l’appel à **SQLGetInfo**, comme illustré dans l’exemple de code suivant :  
  
```  
SQLSMALLINT   InfoValueLen;  
SQLCHAR *     InfoValuePtr = malloc(50);   // Allocate InfoValuePtr.  
  
SQLGetInfo(hdbc, SQL_DBMS_NAME, (SQLPOINTER)InfoValuePtr, 50,  
            &InfoValueLen);  
  
free(InfoValuePtr);                        // OK to free InfoValuePtr.  
```  
  
 Mémoires tampons différées sont spécifiés dans une fonction et utilisés dans un autre, il est une erreur de programmation d’application pour libérer une mémoire tampon différée, tandis que le pilote attend toujours qu’elle existe. Par exemple, l’adresse de la \* *ValuePtr* tampon est transmis à **SQLBindCol** pour une utilisation ultérieure par **SQLFetch**. Impossible de libérer cette mémoire tampon jusqu'à ce que la colonne est indépendante, comme avec un appel à **SQLBindCol** ou **SQLFreeStmt** comme indiqué dans l’exemple de code suivant :  
  
```  
SQLRETURN    rc;  
SQLINTEGER   ValueLenOrInd;  
SQLHSTMT     hstmt;  
  
// Allocate ValuePtr  
SQLCHAR * ValuePtr = malloc(50);  
  
// Bind ValuePtr to column 1. It is an error to free ValuePtr here.  
SQLBindCol(hstmt, 1, SQL_C_CHAR, ValuePtr, 50, &ValueLenOrInd);  
  
// Fetch each row of data and place the value for column 1 in *ValuePtr.  
// Code to check if rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO   
// not shown.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {  
   // It is an error to free ValuePtr here.  
}  
  
// Unbind ValuePtr from column 1.  It is now OK to free ValuePtr.  
SQLFreeStmt(hstmt, SQL_UNBIND);  
free(ValuePtr);  
```  
  
 Cette erreur est facilement effectuée en déclarant de la mémoire tampon localement dans une fonction ; la mémoire tampon est libérée lorsque l’application quitte la fonction. Par exemple, le code suivant génère le comportement non défini et probablement irrécupérable dans le pilote :  
  
```  
SQLRETURN   rc;  
SQLHSTMT    hstmt;  
  
BindAColumn(hstmt);  
  
// Fetch each row of data and try to place the value for column 1 in  
// *ValuePtr. Because ValuePtr has been freed, the behavior is undefined  
// and probably fatal. Code to check if rc equals SQL_ERROR or   
// SQL_SUCCESS_WITH_INFO not shown.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {}  
  
   .  
   .  
   .  
  
void BindAColumn(SQLHSTMT hstmt)  // WARNING! This function won't work!  
{  
   // Declare ValuePtr locally.  
   SQLCHAR      ValuePtr[50];  
   SQLINTEGER   ValueLenOrInd;  
  
   // Bind rgbValue to column.  
   SQLBindCol(hstmt, 1, SQL_C_CHAR, ValuePtr, sizeof(ValuePtr),  
               &ValueLenOrInd);  
  
   // ValuePtr is freed when BindAColumn exits.  
}  
```
