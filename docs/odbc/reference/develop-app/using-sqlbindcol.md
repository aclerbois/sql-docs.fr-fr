---
title: À l’aide de SQLBindCol | Documents Microsoft
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
- result sets [ODBC], binding columns
- binding columns [ODBC]
- SQLBindCol function [ODBC], using
ms.assetid: 17277ab3-33ad-44d3-a81c-a26b5e338512
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d4ccd4607e16b244279e0910fe32047f19e2e6d0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-sqlbindcol"></a>À l’aide de SQLBindCol
L’application lie les colonnes en appelant **SQLBindCol**. Cette fonction est liée à une colonne à la fois. Avec elle, l’application spécifie les éléments suivants :  
  
-   Numéro de la colonne. 0 est la colonne de signet ; Cette colonne n’est pas incluse dans certains jeux de résultats. Toutes les autres colonnes sont numérotées en commençant par le numéro 1. Il s’agit d’une erreur pour lier une colonne élevé qu’il existe de colonnes du jeu de résultats ; Cette erreur ne peut pas être détectée jusqu'à ce que le jeu de résultats a été créé, donc il est renvoyé par **SQLFetch**, et non **SQLBindCol**.  
  
-   Le C octets, l’adresse et type de longueur des données de la variable liée à la colonne. Il s’agit d’une erreur pour spécifier un type de données C à laquelle le type de données SQL de la colonne ne peut pas être converti ; Cette erreur peut ne pas être détectée avant le jeu de résultats a été créé, afin qu’il est retourné par **SQLFetch**, et non **SQLBindCol**. Pour une liste des conversions prises en charge, consultez [conversion des données à partir de SQL pour Types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) annexe d : Types de données. Pour plus d’informations sur la longueur en octets, consultez [longueur de mémoire tampon de données](../../../odbc/reference/develop-app/data-buffer-length.md).  
  
-   L’adresse d’une mémoire tampon de longueur / d’indicateur. La mémoire tampon de longueur / d’indicateur est facultative. Il est utilisé pour retourner la longueur en octets de binaire ou caractère ou retour SQL_NULL_DATA si les données sont NULL. Pour plus d’informations, consultez [à l’aide des valeurs de longueur / d’indicateur](../../../odbc/reference/develop-app/using-length-and-indicator-values.md).  
  
 Lorsque **SQLBindCol** est appelée, le pilote associe ces informations à l’instruction. Lorsque chaque ligne de données est atteinte, il utilise les informations pour placer les données de chaque colonne dans les variables d’application liée.  
  
 Par exemple, le code suivant lie des variables pour les colonnes vendeur et CustID. Données pour les colonnes seront retournées dans *vendeur* et *CustID*. Étant donné que *vendeur* est une mémoire tampon de caractères, l’application spécifie sa longueur en octets (11) afin que le pilote peut déterminer s’il faut tronquer les données. La longueur d’octet de retourné de titre, ou si sa valeur est NULL, seront retournées dans *SalesPersonLenOrInd*.  
  
 Étant donné que *CustID* est une variable de type entier et a résolu longueur, il est inutile de spécifier sa longueur en octets ; le pilote part du principe qu’il est **sizeof (** SQLUINTEGER **)**. La longueur en octets du client retourné ID de données, ou si sa valeur est NULL, seront retournées dans *CustIDInd*. Notez que l’application s’intéresse uniquement si le salaire est NULL, car la longueur d’octet est toujours **sizeof (** SQLUINTEGER **)**.  
  
```  
SQLCHAR       SalesPerson[11];  
SQLUINTEGER   CustID;  
SQLINTEGER    SalesPersonLenOrInd, CustIDInd;  
SQLRETURN     rc;  
SQLHSTMT      hstmt;  
  
// Bind SalesPerson to the SalesPerson column and CustID to the   
// CustID column.  
SQLBindCol(hstmt, 1, SQL_C_CHAR, SalesPerson, sizeof(SalesPerson),  
            &SalesPersonLenOrInd);  
SQLBindCol(hstmt, 2, SQL_C_ULONG, &CustID, 0, &CustIDInd);  
  
// Execute a statement to get the sales person/customer of all orders.  
SQLExecDirect(hstmt, "SELECT SalesPerson, CustID FROM Orders ORDER BY SalesPerson",  
               SQL_NTS);  
  
// Fetch and print the data. Print "NULL" if the data is NULL. Code to   
// check if rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {  
   if (SalesPersonLenOrInd == SQL_NULL_DATA)   
            printf("NULL                     ");  
   else   
            printf("%10s   ", SalesPerson);  
   if (CustIDInd == SQL_NULL_DATA)   
         printf("NULL\n");  
   else   
            printf("%d\n", CustID);  
}  
  
// Close the cursor.  
SQLCloseCursor(hstmt);  
```  
  
 Le code suivant exécute un **sélectionnez** instruction entré par l’utilisateur et imprime chaque ligne de données dans le jeu de résultats. Étant donné que l’application ne peut pas prédire la forme du résultat définir créé par le **sélectionnez** instruction, il ne peut pas lier codé en dur les variables au résultat défini comme dans l’exemple précédent. Au lieu de cela, l’application alloue une mémoire tampon qui conserve les données et une mémoire tampon de longueur / d’indicateur pour chaque colonne de la ligne. Pour chaque colonne, il calcule l’offset au début de la mémoire pour la colonne et ajuste ce décalage, afin que les tampons de données et l’indicateur/longueur de la colonne Démarrer sur les limites d’alignement. Il lie ensuite la mémoire en commençant à l’offset de la colonne. Du point de vue du pilote, l’adresse de cette mémoire est identique à l’adresse d’une variable liée dans l’exemple précédent. Pour plus d’informations sur l’alignement, consultez [alignement](../../../odbc/reference/develop-app/alignment.md).  
  
```  
// This application allocates a buffer at run time. For each column, this   
// buffer contains memory for the column's data and length/indicator.   
// For example:  
//      column 1         column 2      column 3      column 4  
// <------------><---------------><-----><------------>  
//      db1   li1   db2   li2   db3   li3   db4   li4  
//      |      |      |      |      |      |      |         |  
//      _____V_____V________V_______V___V___V______V_____V_  
// |__________|__|_____________|__|___|__|__________|__|  
//  
// dbn = data buffer for column n  
// lin = length/indicator buffer for column n  
  
// Define a macro to increase the size of a buffer so that it is a   
// multiple of the alignment size. Thus, if a buffer starts on an   
// alignment boundary, it will end just before the next alignment   
// boundary. In this example, an alignment size of 4 is used because   
// this is the size of the largest data type used in the application's   
// buffer--the size of an SDWORD and of the largest default C data type   
// are both 4. If a larger data type (such as _int64) was used, it would   
// be necessary to align for that size.  
#define ALIGNSIZE 4  
#define ALIGNBUF(Length) Length % ALIGNSIZE ? \  
                  Length + ALIGNSIZE - (Length % ALIGNSIZE) : Length  
  
SQLCHAR        SelectStmt[100];  
SQLSMALLINT    NumCols, *CTypeArray, i;  
SQLINTEGER *   ColLenArray, *OffsetArray, SQLType, *DataPtr;  
SQLRETURN      rc;   
SQLHSTMT       hstmt;  
  
// Get a SELECT statement from the user and execute it.  
GetSelectStmt(SelectStmt, 100);  
SQLExecDirect(hstmt, SelectStmt, SQL_NTS);  
  
// Determine the number of result set columns. Allocate arrays to hold   
// the C type, byte length, and buffer offset to the data.  
SQLNumResultCols(hstmt, &NumCols);  
CTypeArray = (SQLSMALLINT *) malloc(NumCols * sizeof(SQLSMALLINT));  
ColLenArray = (SQLINTEGER *) malloc(NumCols * sizeof(SQLINTEGER));  
OffsetArray = (SQLINTEGER *) malloc(NumCols * sizeof(SQLINTEGER));  
  
OffsetArray[0] = 0;  
for (i = 0; i < NumCols; i++) {  
   // Determine the column's SQL type. GetDefaultCType contains a switch   
   // statement that returns the default C type for each SQL type.  
   SQLColAttribute(hstmt, ((SQLUSMALLINT) i) + 1, SQL_DESC_TYPE, NULL, 0, NULL, (SQLPOINTER) &SQLType);  
   CTypeArray[i] = GetDefaultCType(SQLType);  
  
   // Determine the column's byte length. Calculate the offset in the   
   // buffer to the data as the offset to the previous column, plus the   
   // byte length of the previous column, plus the byte length of the   
   // previous column's length/indicator buffer. Note that the byte   
   // length of the column and the length/indicator buffer are increased   
   // so that, assuming they start on an alignment boundary, they will  
   // end on the byte before the next alignment boundary. Although this   
   // might leave some holes in the buffer, it is a relatively   
   // inexpensive way to guarantee alignment.  
   SQLColAttribute(hstmt, ((SQLUSMALLINT) i)+1, SQL_DESC_OCTET_LENGTH, NULL, 0, NULL, &ColLenArray[i]);  
   ColLenArray[i] = ALIGNBUF(ColLenArray[i]);  
   if (i)  
      OffsetArray[i] = OffsetArray[i-1]+ColLenArray[i-1]+ALIGNBUF(sizeof(SQLINTEGER));  
}  
  
// Allocate the data buffer. The size of the buffer is equal to the   
// offset to the data buffer for the final column, plus the byte length   
// of the data buffer and length/indicator buffer for the last column.  
void *DataPtr = malloc(OffsetArray[NumCols - 1] +  
               ColLenArray[NumCols - 1] + ALIGNBUF(sizeof(SQLINTEGER)));  
  
// For each column, bind the address in the buffer at the start of the   
// memory allocated for that column's data and the address at the start   
// of the memory allocated for that column's length/indicator buffer.  
for (i = 0; i < NumCols; i++)  
   SQLBindCol(hstmt,  
            ((SQLUSMALLINT) i) + 1,  
            CTypeArray[i],  
            (SQLPOINTER)((SQLCHAR *)DataPtr + OffsetArray[i]),  
            ColLenArray[i],  
            (SQLINTEGER *)((SQLCHAR *)DataPtr + OffsetArray[i] + ColLenArray[i]));  
  
// Retrieve and print each row. PrintData accepts a pointer to the data,   
// its C type, and its byte length/indicator. It contains a switch   
// statement that casts and prints the data according to its type. Code   
// to check if rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {  
   for (i = 0; i < NumCols; i++) {  
      PrintData((SQLCHAR *)DataPtr[OffsetArray[i]], CTypeArray[i],  
               (SQLINTEGER *)((SQLCHAR *)DataPtr[OffsetArray[i] + ColLenArray[i]]));  
   }  
}  
  
// Close the cursor.  
SQLCloseCursor(hstmt);  
```
