---
title: Récupérer des données numériques avec SQL_NUMERIC_STRUCT | Documents Microsoft
description: C/C++ à l’aide de ODBC récupère le type de données numérique SQL Server à l’aide de SQL_NUMERIC_STRUCT, liés à SQL_C_NUMERIC.
documentationCenter: ''
authors: MightyPen
manager: craigg
editor: ''
ms.prod: sql
ms.prod_service: connectivity
ms.suite: sql
ms.technology: dbe-data-tier-apps
ms.devlang: C++
ms.topic: conceptual
ms.custom: ''
ms.tgt_pltfrm: NA
ms.date: 07/13/2017
ms.author: genemi
ms.openlocfilehash: 57bd5ffbe1adb9c0ecbefda8d99434767ed6c3e0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieve-numeric-data-with-sqlnumericstruct"></a>Récupérer des données numériques avec SQL\_numérique\_STRUCT

Cet article décrit comment récupérer des données numériques à partir du pilote ODBC de SQL Server dans une structure numérique. Elle décrit également comment obtenir les valeurs correctes à l’aide de la précision spécifique et une échelle de valeurs.

Ce type de données permet aux applications de gérer directement les données numériques. Autour de l’année 2003, ODBC 3.0 a introduit un nouveau type de données ODBC C, identifié par **SQL\_C\_numérique**. Ce type de données est toujours applicables à compter de 2017.

La mémoire tampon C qui est utilisée est la définition du type de **SQL\_numérique\_STRUCT**. Cette structure comporte des champs pour le stockage de la précision, échelle, l’authentification et à la valeur des données numériques. La valeur elle-même est stockée comme un entier à l’échelle avec le moins significatif d’octets commençant à la position la plus à gauche. 

L’article [les Types de données C](c-data-types.md) fournit plus d’informations sur l’utilisation de SQL et le format\_numérique\_STRUCT. En règle générale le [annexe D](appendix-d-data-types.md) décrit les types de données de référence du programmeur de 3.0 de ODBC.


## <a name="sqlnumericstruct-overview"></a>SQL\_numérique\_vue d’ensemble de la structure


L’instruction SQL\_numérique\_STRUCT est défini dans le fichier d’en-tête sqltypes.h comme suit :


``` C
#define SQL_MAX_NUMERIC_LEN    16
typedef struct tagSQL_NUMERIC_STRUCT
{
   SQLCHAR    precision;
   SQLSCHAR   scale;
   SQLCHAR    sign;   /* 1 if positive, 0 if negative */
   SQLCHAR    val[SQL_MAX_NUMERIC_LEN];
} SQL_NUMERIC_STRUCT;
```

            
Les champs de précision et l’échelle de la structure numérique ne sont jamais utilisés pour l’entrée d’une application, uniquement pour la sortie à partir du pilote à l’application.

Le pilote utilise la précision par défaut (définies par le pilote) et l’échelle par défaut (0) chaque fois que le renvoi de données à l’application. Sauf si l’application spécifie des valeurs de précision et l’échelle, le pilote suppose que la valeur par défaut et tronque la partie décimale des données numériques.

## <a name="sqlnumericstruct-code-sample"></a>SQL\_numérique\_exemple de code STRUCT

Cet exemple de code montre comment à :

- Définissez la précision.
- Définissez l’échelle.
- Récupérer les valeurs correctes. 

> [!Note]
> UTILISATION DE DU CODE FOURNI DANS CET ARTICLE EST À VOS PROPRES RISQUES. 
>
> Microsoft fournit ces exemples de code « en l’état » sans garantie d’aucune sorte, expresse ou implicite, y compris de manière non limitative, les garanties implicites de qualité marchande et d’adéquation à un usage particulier.

``` C
#include <stdio.h>
#include <string.h>
#include <conio.h>
#include <stdlib.h>
#include <ctype.h>
#include <windows.h>
#include <sql.h>
#include <sqlext.h>

#define MAXDSN       25
#define MAXUID       25
#define MAXAUTHSTR   25
#define MAXBUFLEN   255

SQLHENV     henv   = SQL_NULL_HENV;
SQLHDBC     hdbc1  = SQL_NULL_HDBC;     
SQLHSTMT    hstmt1 = SQL_NULL_HSTMT;
SQLHDESC    hdesc  = NULL;


SQL_NUMERIC_STRUCT NumStr;

int main()
{
   RETCODE retcode;

//Change the values below as appropriate to make a successful connection.
//szDSN: DataSourceName, szUID=userid, szAuthStr: password

UCHAR szDSN[MAXDSN+1] = "sql33",szUID[MAXUID+1]="sa", szAuthStr[MAXAUTHSTR+1] = "";
SQLINTEGER strlen1;
SQLINTEGER a;
int i,sign =1;
long myvalue, divisor;
float final_val;
   
// Allocate the Environment handle. Set the Env attribute, allocate the
//connection handle, connect to the database and allocate the statement //handle.

retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);
retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION,(SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);
retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);
retcode = SQLConnect(hdbc1, szDSN,SQL_NTS,szUID,SQL_NTS,szAuthStr,SQL_NTS);
retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);

// Execute the select statement. Here it is assumed that numeric_test
//table is created using the following statements:

// Create table numeric_test (col1 numeric(5,3))
//insert into numeric_test values (25.212)

retcode = SQLExecDirect(hstmt1,(UCHAR *)"select * from numeric_test",SQL_NTS);

// Use SQLBindCol to bind the NumStr to the column that is being retrieved.

retcode = SQLBindCol(hstmt1,1,SQL_C_NUMERIC,&NumStr,19,&strlen1);

// Get the application row descriptor for the statement handle using
//SQLGetStmtAttr.

retcode = SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_ROW_DESC,&hdesc, 0, NULL);

// You can either use SQLSetDescRec or SQLSetDescField when using
// SQLBindCol. However, if you prefer to call SQLGetData, you have to
// call SQLSetDescField instead of SQLSetDescRec. For more information on
// descriptors, please refer to the ODBC 3.0 Programmers reference or
// your Online documentation.

//Used when using SQLSetDescRec
//a=b=sizeof(NumStr);

// Set the datatype, precision and scale fields of the descriptor for the 
//numeric column. Otherwise the default precision (driver defined) and 
//scale (0) are returned.

// In this case, the table contains only one column, hence the second 
//parameter contains one. Zero applies to bookmark columns. Please check 
//the programmers guide for more information.

//retcode=SQLSetDescRec(hdesc,1,SQL_NUMERIC,NULL,sizeof(NumStr),5,3,&NumStr,&a,&b);

retcode = SQLSetDescField (hdesc,1,SQL_DESC_TYPE,(VOID*)SQL_C_NUMERIC,0);
retcode = SQLSetDescField (hdesc,1,SQL_DESC_PRECISION,(VOID*) 5,0);
retcode = SQLSetDescField (hdesc,1,SQL_DESC_SCALE,(VOID*) 3,0);
    
// Initialize the val array in the numeric structure.

memset(NumStr.val,0,16);
   
// Call SQLFetch to fetch the first record.

while((retcode =SQLFetch(hstmt1)) != SQL_NO_DATA)
  {
// Notice that the TargetType (3rd Parameter) is SQL_ARD_TYPE, which  
//forces the driver to use the Application Row Descriptor with the 
//specified scale and precision.

   retcode = SQLGetData(hstmt1, 1, SQL_ARD_TYPE, &NumStr, 19, &a); 

// Check for null indicator.

   if ( SQL_NULL_DATA == a )
   {
   printf( "The final value: NULL\n" );
   continue;
   }

// Call to convert the little endian mode data into numeric data.

   myvalue = strtohextoval();

// The returned value in the above code is scaled to the value specified
//in the scale field of the numeric structure. For example 25.212 would
//be returned as 25212. The scale in this case is 3 hence the integer 
//value needs to be divided by 1000.

   divisor = 1;
   if(NumStr.scale > 0)
     {
    for (i=0;i< NumStr.scale; i++)   
         divisor = divisor * 10;
     }
   final_val =  (float) myvalue /(float) divisor;

// Examine the sign value returned in the sign field for the numeric
//structure.
//NOTE: The ODBC 3.0 spec required drivers to return the sign as 
//1 for positive numbers and 2 for negative number. This was changed in the
//ODBC 3.5 spec to return 0 for negative instead of 2.

      if(!NumStr.sign) sign = -1;
      else sign  =1;

   final_val *= sign;
   printf("The final value: %f\n",final_val);
    }

   while ( ( retcode = SQLMoreResults(hstmt1) ) != SQL_NO_DATA_FOUND);

   /* clean up */ 
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);
   SQLDisconnect(hdbc1);
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);
   SQLFreeHandle(SQL_HANDLE_ENV, henv);
   return(0);
}
```


### <a name="interim-results"></a>Résultats intermédiaires :


```
//  C  ==> 12 * 1    =     12
//  7  ==> 07 * 16   =    112
//  2  ==> 02 * 256  =    512
//  6  ==> 06 * 4096 =  24576
===============================
                 Sum =  25212
```


Dans la structure numérique, le champ val est un tableau de caractères de 16 éléments. Par exemple, 25.212 est mis à l’échelle pour 25212 et l’échelle est 3. Au format hexadécimal, ce nombre serait 627 C.

Le pilote retourne les éléments suivants :

- Le caractère équivalent de 7C, qui est ' |' (diriger) dans le premier élément du tableau de caractères.
- L’équivalent de 62, qui est « b » dans le deuxième élément.
- Les restes des éléments du tableau contiennent des zéros, par conséquent, la mémoire tampon contient ' | b\0'.

Désormais, le défi consiste à construire l’entier à l’échelle en dehors de ce tableau de chaînes. Chaque caractère de la chaîne correspond à deux chiffres hexadécimaux, par exemple le chiffre le moins significatif (LSD) et chiffre le plus significatif (MSD). La valeur entière à l’échelle peut être générée en multipliant chaque chiffre (LSD & MSD) avec un multiple de 16, en commençant par 1.

Code qui implémente la conversion du mode de primauté peu entier à l’échelle. Il incombe au développeur de l’application pour implémenter cette fonctionnalité. L’exemple de code suivant est simplement l’une des nombreuses façons.


``` C
long strtohextoval()
{
    long val=0,value=0;
    int i=1,last=1,current;
    int a=0,b=0;

        for(i=0;i<=15;i++)
        {
         current = (int) NumStr.val[i];
         a= current % 16; //Obtain LSD
         b= current / 16; //Obtain MSD
            
         value += last* a;   
         last = last * 16;   
         value += last* b;
         last = last * 16;   
        }
     return value;
}
```


### <a name="applies-to-versions"></a>S’applique aux versions


Les informations précédentes sur SQL\_numérique\_STRUCT s’applique aux versions de produit suivants :

- Pilote Microsoft ODBC pour Microsoft SQL Server 3.7
- Microsoft Data Access Components 2.1
- Microsoft Data Access Components 2.5
- Microsoft Data Access Components 2.6
- Microsoft Data Access Components 2.7


## <a name="sqlcnumeric-overview"></a>SQL\_C\_vue d’ensemble numérique


L’exemple de programme suivant illustre l’utilisation de SQL\_C\_numérique, en insérant 123,45 dans une table. Dans la table, la colonne est définie comme une valeur numérique ou une valeur décimale, avec une précision de 5 et avec une échelle de 2.

Le pilote ODBC que vous utilisez pour exécuter ce programme doit prendre en charge la fonctionnalité ODBC 3.0.


``` C
#include <windows.h>
#include <sql.h>
#include <sqlext.h>

void main() {

   SQLHENV    henv  = NULL;
   SQLHDBC    hdbc  = NULL;
   SQLHSTMT   hstmt = NULL;

   SQL_NUMERIC_STRUCT   NumStr;
   SQLINTEGER           cbNumStr = sizeof (NumStr);

   SQLAllocHandle(SQL_HANDLE_ENV, NULL, &henv);

   /* Set the ODBC behavior version. */ 
   SQLSetEnvAttr(henv,
         SQL_ATTR_ODBC_VERSION,
         (SQLPOINTER) SQL_OV_ODBC3,
         SQL_IS_INTEGER);

   SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);

   /* Substitute your own connection information */ 
   SQLConnect(hdbc,
      (SQLCHAR *) "MyDSN", 5,
      (SQLCHAR *) "UserID", 6,
      (SQLCHAR *) "Password", 8);

   SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);

   /*
      Set up the SQL_NUMERIC_STRUCT, NumStr, to hold "123.45".

      First, we need to scale 123.45 to an integer: 12345
      One way to switch the bytes is to convert 12345 to Hex:  0x3039
      Since the least significant byte will be stored starting from the
      leftmost byte, "0x3039" will be stored as "0x3930".

      The precision and scale fields are not used for input to the driver,
      only for output from the driver. The precision and scale will be set
      in the application parameter descriptor later.
   */ 

   NumStr.sign = 1;   /* 1 if positive, 2 if negative */ 

   memset (NumStr.val, 0, 16);
   NumStr.val [0] = 0x39;
   NumStr.val [1] = 0x30;

   /* SQLBindParameter needs to be called before SQLSetDescField */ 
   SQLBindParameter(hstmt,
          1,
          SQL_PARAM_INPUT,
          SQL_C_NUMERIC,
          SQL_NUMERIC,
          5,
          2,
          &NumStr,
          0,
          (SQLINTEGER *) &cbNumStr);

   /* Modify the fields in the implicit application parameter descriptor */ 
   SQLHDESC   hdesc = NULL;

   SQLGetStmtAttr(hstmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
   SQLSetDescField(hdesc, 1, SQL_DESC_TYPE, (SQLPOINTER) SQL_C_NUMERIC, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_PRECISION, (SQLPOINTER) 5, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_SCALE, (SQLPOINTER) 2, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_DATA_PTR, (SQLPOINTER) &NumStr, 0);

   SQLExecDirect(hstmt,
         (SQLCHAR *) "INSERT INTO table (numeric_column) VALUES (?)",
         SQL_NTS);

   SQLFreeHandle(SQL_HANDLE_STMT, hstmt);

   SQLDisconnect (hdbc);

   SQLFreeHandle(SQL_HANDLE_DBC, hdbc);
   SQLFreeHandle(SQL_HANDLE_ENV, henv);
}
```


<!--
GeneMi historical notes, 2017/July/12. Per Jason.C

http://go.microsoft.com/fwlink/?LinkId=147596

https://support.microsoft.com/en-us/help/222831

http://web.archive.org/web/20140319133434/http:/support.microsoft.com:80/kb/222831

http://web.archive.org/web/20080505073901/http:/support.microsoft.com:80/kb/181254

https://docs.microsoft.com/en-us/sql/odbc/reference/appendixes/c-data-types
-->

