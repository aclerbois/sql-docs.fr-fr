---
title: Allouer des Handles et se connecter à SQL Server (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-how-to
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- handles [ODBC]
- handles [ODBC], connection
- handles [ODBC], about handles
ms.assetid: 6172cd52-9c9a-467d-992f-def07f3f3bb1
caps.latest.revision: 29
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 58eac1839471709926630e5fe256ba21704887c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="allocate-handles-and-connect-to-sql-server-odbc"></a>Allouer des handles et se connecter à SQL Server (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

    
### <a name="to-allocate-handles-and-connect-to-sql-server"></a>Pour allouer les handles et se connecter à SQL Server  
  
1.  Incluez les fichiers d'en-tête ODBC Sql.h, Sqlext.h, Sqltypes.h.  
  
2.  Incluez le fichier d'en-tête spécifique au pilote [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Odbcss.h.  
  
3.  Appelez [SQLAllocHandle](http://go.microsoft.com/fwlink/?LinkId=58396) avec un **HandleType** de SQL_HANDLE_ENV pour initialiser ODBC et allouer un handle d’environnement.  
  
4.  Appelez [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) avec **attribut** défini à SQL_ATTR_ODBC_VERSION et **ValuePtr** défini à SQL_OV_ODBC3 pour indiquer l’application utilisera les appels de fonction au format 3.x ODBC.  
  
5.  Le cas échéant, appelez [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) pour définir l’autre environnement options ou appelez [cas](http://go.microsoft.com/fwlink/?LinkId=58403) pour obtenir des options d’environnement.  
  
6.  Appelez [SQLAllocHandle](http://go.microsoft.com/fwlink/?LinkId=58396) avec un **HandleType** de SQL_HANDLE_DBC pour allouer un handle de connexion.  
  
7.  Le cas échéant, appelez [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) pour définir les options de connexion ou appelez [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) pour obtenir des options de connexion.  
  
8.  Appelez SQLConnect pour utiliser une source de données existante pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     ou  
  
     Appelez [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) à utiliser une chaîne de connexion pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Une chaîne de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] minimale complète revêt l'une des deux formes :  
  
    ```  
    DSN=dsn_name;Trusted_connection=yes;  
    DRIVER={SQL Server Native Client 10.0};SERVER=server;Trusted_connection=yes;  
    ```  
  
     Si la chaîne de connexion n’est pas terminée, **SQLDriverConnect** peut demander les informations nécessaires. Ceci est contrôlé par la valeur spécifiée pour le *DriverCompletion* paramètre.  
  
     \- ou -  
  
     Appelez [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) plusieurs fois de manière itérative pour générer la chaîne de connexion et de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
9. Le cas échéant, appelez [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md) pour obtenir les attributs de pilote et le comportement de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données.  
  
10. Allouez et utilisez les instructions.  
  
11. Appelez SQLDisconnect pour vous déconnecter de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et établir la connexion gérer disponible pour une nouvelle connexion.  
  
12. Appelez [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) avec un **HandleType** de SQL_HANDLE_DBC pour libérer le handle de connexion.  
  
13. Appelez **SQLFreeHandle** avec un **HandleType** de SQL_HANDLE_ENV pour libérer le handle d’environnement.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, utilisez l'authentification Windows. Si l'authentification Windows n'est pas disponible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Évitez de stocker ces informations dans un fichier. Si vous devez rendre les informations d'identification persistantes, chiffrez-les avec l' [API de chiffrement Win32](http://go.microsoft.com/fwlink/?LinkId=64532).  
  
## <a name="example"></a>Exemple  
 Cet exemple montre un appel à **SQLDriverConnect** pour se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans nécessiter une source de données ODBC existante. En passant une chaîne de connexion incomplète à **SQLDriverConnect**, le pilote ODBC inviter l’utilisateur à entrer les informations manquantes.  
  
```  
#define MAXBUFLEN   255  
  
SQLHENV      henv = SQL_NULL_HENV;  
SQLHDBC      hdbc1 = SQL_NULL_HDBC;  
SQLHSTMT      hstmt1 = SQL_NULL_HSTMT;  
  
SQLCHAR      ConnStrIn[MAXBUFLEN] =  
         "DRIVER={SQL Server Native Client 10.0};SERVER=MyServer";  
  
SQLCHAR      ConnStrOut[MAXBUFLEN];  
SQLSMALLINT   cbConnStrOut = 0;  
  
// Make connection without data source. Ask that driver   
// prompt if insufficient information. Driver returns  
// SQL_ERROR and application prompts user  
// for missing information. Window handle not needed for  
// SQL_DRIVER_NOPROMPT.  
retcode = SQLDriverConnect(hdbc1,      // Connection handle  
                  NULL,         // Window handle  
                  ConnStrIn,      // Input connect string  
                  SQL_NTS,         // Null-terminated string  
                  ConnStrOut,      // Address of output buffer  
                  MAXBUFLEN,      // Size of output buffer  
                  &cbConnStrOut,   // Address of output length  
                  SQL_DRIVER_PROMPT);  
```  
  
  
