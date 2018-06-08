---
title: Les tableaux de paramètres de liaison | Documents Microsoft
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
- binding parameter arrays [ODBC]
- arrays of parameter values [ODBC]
- parameter arrays [ODBC]
ms.assetid: 037afe23-052d-4f3a-8aa7-45302b199ad0
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2fe314ff1db42944ccd37dfa0c336f3ed218b6b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="binding-arrays-of-parameters"></a>Les tableaux de paramètres de liaison
Les applications qui utilisent des tableaux de paramètres de lier les tableaux de paramètres dans l’instruction SQL. Il existe deux styles de liaison :  
  
-   Lier un tableau à chaque paramètre. Chaque structure de données (tableau) contient toutes les données pour un seul paramètre. Il s’agit *liaison selon les colonnes* car elle est liée à une colonne de valeurs pour un seul paramètre.  
  
-   Définissez une structure pour contenir les données de paramètre d’un ensemble de paramètres et de lier un tableau de ces structures. Chaque structure de données contient les données pour une instruction SQL unique. Il s’agit *liaison selon les lignes* car elle est liée à une ligne de paramètres.  
  
 Comme lors de l’application lie des variables uniques aux paramètres, il appelle **SQLBindParameter** pour lier les tableaux de paramètres. La seule différence est que les adresses sont transmises sont des adresses de tableau, des adresses non-variable unique. L’application définit l’attribut d’instruction SQL_ATTR_PARAM_BIND_TYPE pour spécifier s’il s’agit de l’utilisation selon les colonnes (la valeur par défaut) ou la liaison. S’il faut utiliser la liaison selon les colonnes ou lignes est essentiellement une question de préférence de l’application. Selon la façon dont le processeur accède à la mémoire, la liaison selon les lignes peut-être être plus rapide. Toutefois, la différence est susceptible d’être négligeable à l’exception de très grand nombre de lignes de paramètres.  
  
## <a name="column-wise-binding"></a>Liaison selon les colonnes  
 Lorsque vous utilisez la liaison, une application lie une ou deux tableaux pour chaque paramètre pour lequel les données doivent être fournies. Le premier tableau conserve les valeurs de données, et le deuxième tableau conserve les mémoires tampons de longueur / d’indicateur. Chaque tableau contient autant d’éléments qu’il sont a les valeurs pour le paramètre.  
  
 La liaison est la valeur par défaut. L’application également peut modifier à partir de la liaison pour la liaison en définissant l’attribut d’instruction SQL_ATTR_PARAM_BIND_TYPE. L’illustration suivante montre le fonctionnement selon les colonnes de la liaison.  
  
 ![Montre comment colonne&#45;liaison fonctionne au niveau](../../../odbc/reference/develop-app/media/pr31.gif "pr31")  
  
 Par exemple, le code suivant lie des tableaux d’éléments de 10 à des paramètres pour les colonnes PartID, Description et le prix et exécute une instruction pour insérer des lignes de 10. Il utilise la liaison selon les colonnes.  
  
```  
#define DESC_LEN 51  
#define ARRAY_SIZE 10  
  
SQLCHAR *      Statement = "INSERT INTO Parts (PartID, Description,  Price) "  
                                                "VALUES (?, ?, ?)";  
SQLUINTEGER    PartIDArray[ARRAY_SIZE];  
SQLCHAR        DescArray[ARRAY_SIZE][DESC_LEN];  
SQLREAL        PriceArray[ARRAY_SIZE];  
SQLINTEGER     PartIDIndArray[ARRAY_SIZE], DescLenOrIndArray[ARRAY_SIZE],  
               PriceIndArray[ARRAY_SIZE];  
SQLUSMALLINT   i, ParamStatusArray[ARRAY_SIZE];  
SQLULEN ParamsProcessed;  
  
memset(DescLenOrIndArray, 0, sizeof(DescLenOrIndArray));  
memset(PartIDIndArray, 0, sizeof(PartIDIndArray));  
memset(PriceIndArray, 0, sizeof(PriceIndArray));  
  
// Set the SQL_ATTR_PARAM_BIND_TYPE statement attribute to use  
// column-wise binding.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAM_BIND_TYPE, SQL_PARAM_BIND_BY_COLUMN, 0);  
  
// Specify the number of elements in each parameter array.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAMSET_SIZE, ARRAY_SIZE, 0);  
  
// Specify an array in which to return the status of each set of  
// parameters.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAM_STATUS_PTR, ParamStatusArray, 0);  
  
// Specify an SQLUINTEGER value in which to return the number of sets of  
// parameters processed.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAMS_PROCESSED_PTR, &ParamsProcessed, 0);  
  
// Bind the parameters in column-wise fashion.  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_ULONG, SQL_INTEGER, 5, 0,  
                  PartIDArray, 0, PartIDIndArray);  
SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, DESC_LEN - 1, 0,  
                  DescArray, DESC_LEN, DescLenOrIndArray);  
SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_FLOAT, SQL_REAL, 7, 0,  
                  PriceArray, 0, PriceIndArray);  
  
// Set part ID, description, and price.  
for (i = 0; i < ARRAY_SIZE; i++) {  
   GetNewValues(&PartIDArray[i], DescArray[i], &PriceArray[i]);  
   PartIDIndArray[i] = 0;  
   DescLenOrIndArray[i] = SQL_NTS;  
   PriceIndArray[i] = 0;  
}  
  
// Execute the statement.  
SQLExecDirect(hstmt, Statement, SQL_NTS);  
  
// Check to see which sets of parameters were processed successfully.  
for (i = 0; i < ParamsProcessed; i++) {  
   printf("Parameter Set  Status\n");  
   printf("-------------  -------------\n");  
   switch (ParamStatusArray[i]) {  
      case SQL_PARAM_SUCCESS:  
      case SQL_PARAM_SUCCESS_WITH_INFO:  
         printf("%13d  Success\n", i);  
         break;  
  
      case SQL_PARAM_ERROR:  
         printf("%13d  Error\n", i);  
         break;  
  
      case SQL_PARAM_UNUSED:  
         printf("%13d  Not processed\n", i);  
         break;  
  
      case SQL_PARAM_DIAG_UNAVAILABLE:  
         printf("%13d  Unknown\n", i);  
         break;  
  
   }  
}  
```  
  
## <a name="row-wise-binding"></a>Liaison selon les lignes  
 Lorsque vous utilisez la liaison selon les lignes, une application définit une structure pour chaque jeu de paramètres. La structure contient un ou deux éléments pour chaque paramètre. Le premier élément conserve la valeur de paramètre et le deuxième élément conserve la mémoire tampon de longueur / d’indicateur. L’application alloue un tableau de ces structures, qui contient autant d’éléments qu’il sont a les valeurs pour chaque paramètre.  
  
 L’application déclare la taille de la structure du pilote avec l’attribut d’instruction SQL_ATTR_PARAM_BIND_TYPE. L’application lie les adresses des paramètres de la première structure du tableau. Par conséquent, le pilote peut calculer l’adresse des données pour une ligne particulière et une colonne en tant que  
  
```  
Address = Bound Address + ((Row Number - 1) * Structure Size) + Offset  
```  
  
 dans lequel les lignes sont numérotées de 1 à la taille de l’ensemble de paramètres. Le décalage, s’il est défini, est la valeur vers laquelle pointée l’attribut d’instruction SQL_ATTR_PARAM_BIND_OFFSET_PTR. L’illustration suivante montre le fonctionnement selon les lignes de la liaison. Les paramètres peuvent être placés dans la structure dans n’importe quel ordre, mais sont affichées dans un ordre séquentiel par souci de clarté.  
  
 ![Montre comment ligne&#45;liaison fonctionne au niveau](../../../odbc/reference/develop-app/media/pr32.gif "pr32")  
  
 Le code suivant crée une structure avec des éléments pour les valeurs à stocker dans les colonnes PartID, Description et le prix. Ensuite, il alloue un tableau de 10 éléments de ces structures et lie à des paramètres pour les colonnes PartID, Description et le prix, à l’aide de la liaison. Il exécute ensuite une instruction pour insérer des lignes de 10.  
  
```  
#define DESC_LEN 51  
#define ARRAY_SIZE 10  
  
typedef tagPartStruct {  
   SQLREAL       Price;  
   SQLUINTEGER   PartID;  
   SQLCHAR       Desc[DESC_LEN];  
   SQLINTEGER    PriceInd;  
   SQLINTEGER    PartIDInd;  
   SQLINTEGER    DescLenOrInd;  
} PartStruct;  
  
PartStruct PartArray[ARRAY_SIZE];  
SQLCHAR *      Statement = "INSERT INTO Parts (PartID, Description,  
                Price) "  
               "VALUES (?, ?, ?)";  
SQLUSMALLINT   i, ParamStatusArray[ARRAY_SIZE];  
SQLULEN ParamsProcessed;  
  
// Set the SQL_ATTR_PARAM_BIND_TYPE statement attribute to use  
// column-wise binding.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAM_BIND_TYPE, sizeof(PartStruct), 0);  
  
// Specify the number of elements in each parameter array.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAMSET_SIZE, ARRAY_SIZE, 0);  
  
// Specify an array in which to return the status of each set of  
// parameters.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAM_STATUS_PTR, ParamStatusArray, 0);  
  
// Specify an SQLUINTEGER value in which to return the number of sets of  
// parameters processed.  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAMS_PROCESSED_PTR, &ParamsProcessed, 0);  
  
// Bind the parameters in row-wise fashion.  
SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_ULONG, SQL_INTEGER, 5, 0,  
                  &PartArray[0].PartID, 0, &PartArray[0].PartIDInd);  
SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, DESC_LEN - 1, 0,  
                  PartArray[0].Desc, DESC_LEN, &PartArray[0].DescLenOrInd);  
SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_FLOAT, SQL_REAL, 7, 0,  
                  &PartArray[0].Price, 0, &PartArray[0].PriceInd);  
  
// Set part ID, description, and price.  
for (i = 0; i < ARRAY_SIZE; i++) {  
   GetNewValues(&PartArray[i].PartID, PartArray[i].Desc, &PartArray[i].Price);  
   PartArray[0].PartIDInd = 0;  
   PartArray[0].DescLenOrInd = SQL_NTS;  
   PartArray[0].PriceInd = 0;  
}  
  
// Execute the statement.  
SQLExecDirect(hstmt, Statement, SQL_NTS);  
  
// Check to see which sets of parameters were processed successfully.  
for (i = 0; i < ParamsProcessed; i++) {  
   printf("Parameter Set  Status\n");  
   printf("-------------  -------------\n");  
   switch (ParamStatusArray[i]) {  
      case SQL_PARAM_SUCCESS:  
      case SQL_PARAM_SUCCESS_WITH_INFO:  
         printf("%13d  Success\n", i);  
         break;  
  
      case SQL_PARAM_ERROR:  
         printf("%13d  Error\n", i);  
         break;  
  
      case SQL_PARAM_UNUSED:  
         printf("%13d  Not processed\n", i);  
         break;  
  
      case SQL_PARAM_DIAG_UNAVAILABLE:  
         printf("%13d  Unknown\n", i);  
         break;  
  
   }  
```
