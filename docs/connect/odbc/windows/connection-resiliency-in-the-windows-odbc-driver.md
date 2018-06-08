---
title: Résilience des connexions dans le pilote ODBC de Windows | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 614fa0b4-e9fd-4c68-aab3-183f9b9df143
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e2b27a848773b09d651d748bd321ace69ab2a6b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connection-resiliency-in-the-windows-odbc-driver"></a>Résilience de connexion du pilote ODBC Windows
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

  Pour vous assurer que les applications restent connectées à un [!INCLUDE[ssAzure](../../../includes/ssazure_md.md)], le pilote ODBC sur Windows peut restaurer les connexions inactives.  
  
> [!IMPORTANT]  
>  La fonctionnalité de résilience de connexion est prise en charge sur les versions de serveur SQL Server 2014 (et versions ultérieures) et les bases de données Microsoft Azure SQL Database.  
  
 Pour plus d’informations sur la résilience des connexions inactives, consultez [Article technique – résilience des connexions inactives](http://go.microsoft.com/fwlink/?LinkId=393996).  
  
 Pour contrôler le comportement de reconnexion, ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows propose deux options :  
  
-   Nombre de tentatives de connexion.  
  
     Contrôle le nombre de nouvelles tentatives de connexion en cas d’échec de connexion. Les valeurs valides sont comprises entre 0 et 255. Zéro (0) signifie qu’il ne faut pas essayer de se reconnecter. La valeur par défaut est une (1) nouvelle tentative de connexion.  
  
     Vous pouvez modifier le nombre de tentatives de connexion quand vous :  
  
    -   définissez ou modifiez une source de données qui utilise ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] avec le contrôle **Nombre de tentatives de connexion** .  
  
    -   utilisez le mot clé de chaîne de connexion **ConnectRetryCount** .  
  
     Pour récupérer le nombre de nouvelles tentatives de connexion, utilisez la **SQL_COPT_SS_CONNECT_RETRY_COUNT** (lecture seule) attribut de connexion. Si une application se connecte à un serveur qui ne prend pas en charge la résilience des connexions, **SQL_COPT_SS_CONNECT_RETRY_COUNT** retourne 0.  
  
-   Intervalle avant nouvelle tentative de connexion.  
  
     L’intervalle avant nouvelle tentative de connexion spécifie le nombre de secondes entre chaque nouvelle tentative de connexion. Les valeurs valides sont comprises entre 1 et 60. La durée totale de reconnexion ne peut pas dépasser le délai de maintien de la connexion (SQL_ATTR_QUERY_TIMEOUT dans SQLSetStmtAttr). La valeur par défaut est 10 secondes.  
  
     Vous pouvez modifier l’intervalle de nouvelle tentative de connexion quand vous :  
  
    -   définissez ou modifiez une source de données qui utilise ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] avec le contrôle **Intervalle avant nouvelle tentative de connexion** .  
  
    -   utilisez le mot clé de chaîne de connexion **ConnectRetryInterval** .  
  
     Pour récupérer la longueur de l’intervalle de nouvelle tentative de connexion, utilisez la **sql_copt_ss_connect_retry_count** (lecture seule) attribut de connexion.  
  
 Si une application établit une connexion avec SQL_DRIVER_COMPLETE_REQUIRED et essaie ultérieurement d’exécuter une instruction sur une connexion interrompue, le pilote ODBC ne réaffiche pas la boîte de dialogue. De plus, quand la récupération est en cours :  
  
-   Lors de la récupération, tout appel à **SQLGetConnectAttr (sql_copt_ss_connection_dead)**, doit retourner **SQL_CD_FALSE**.  
  
-   Si la récupération échoue, un appel à **SQLGetConnectAttr (sql_copt_ss_connection_dead)**, doit retourner **SQL_CD_TRUE**.  
  
 Les codes d’état suivants sont retournés par toute fonction qui exécute une commande sur le serveur :  
  
|État|Boîte de|  
|-----------|-------------|  
|IMC01|La connexion est interrompue et la récupération n’est pas possible. Le pilote du client a tenté de rétablir la connexion une fois ou plus et toutes les tentatives ont échoué. Augmentez la valeur de ConnectRetryCount pour augmenter le nombre de tentatives de récupération.|  
|IMC02|Le serveur n’a pas reconnu une tentative de récupération. La récupération de la connexion est impossible.|  
|IMC03|Le serveur n’a pas préservé la version TDS exacte demandée lors d’une tentative de récupération. La récupération de la connexion est impossible.|  
|IMC04|Le serveur n’a pas préservé la version principale du serveur exacte demandée lors d’une tentative de récupération. La récupération de la connexion est impossible.|  
|IMC05|La connexion est interrompue et la récupération n’est pas possible. La connexion est marquée par le serveur comme irrécupérable. Aucune tentative n’a été faite pour rétablir la connexion.|  
|IMC06|La connexion est interrompue et la récupération n’est pas possible. La connexion est marquée par le pilote du client comme irrécupérable. Aucune tentative n’a été faite pour rétablir la connexion.|  
  
## <a name="example"></a>Exemple  
 L’exemple suivant contient deux fonctions. **func1** montre comment vous pouvez vous connecter avec un nom de source de données (DSN) qui utilise le pilote ODBC pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Windows. La source de données utilise l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] et spécifie l’ID utilisateur. **func1** récupère ensuite le nombre de tentatives de connexion avec **SQL_COPT_SS_CONNECT_RETRY_COUNT**.  
  
 **func2** utilise **SQLDriverConnect**, le mot clé de chaîne de connexion **ConnectRetryCount** et les attributs de connexion pour récupérer le paramètre pour les tentatives de connexion et l’intervalle avant nouvelle tentative.  
  
```  
// Connection_resiliency.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <stdio.h>  
#include <sqlext.h>  
#include <msodbcsql.h>  
  
void func1() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
   SQLSMALLINT i = 21;  

   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLCHAR*) "MyDSN", SQL_NTS, (SQLCHAR*) "userID", SQL_NTS, (SQLCHAR*) "password_for_userID", SQL_NTS);  
            retcode = SQLGetConnectAttr(hdbc, SQL_COPT_SS_CONNECT_RETRY_COUNT, &i, SQL_IS_INTEGER, NULL);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}
  
void func2() {  
   SQLHENV henv;  
   SQLHDBC hdbc1;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
   SQLSMALLINT i = 21;  
  
#define MAXBUFLEN 255  
  
   SQLCHAR ConnStrIn[MAXBUFLEN] = "DRIVER={ODBC Driver 13 for SQL Server};SERVER=server_that_supports_connection_resiliency;UID=userID;PWD= password_for_userID;ConnectRetryCount=2";
   SQLCHAR ConnStrOut[MAXBUFLEN];

   SQLSMALLINT cbConnStrOut = 0;  
 
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3_80, SQL_IS_INTEGER);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            // SQLSetConnectAttr(hdbc1, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            retcode = SQLDriverConnect(hdbc1, NULL, ConnStrIn, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);  
         }  
         retcode = SQLGetConnectAttr(hdbc1, SQL_COPT_SS_CONNECT_RETRY_COUNT, &i, SQL_IS_INTEGER, NULL);  
         retcode = SQLGetConnectAttr(hdbc1, SQL_COPT_SS_CONNECT_RETRY_INTERVAL, &i, SQL_IS_INTEGER, NULL);  
      }  
   }  
}  
  
int main() {  
   func1();  
   func2();  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Pilote Microsoft ODBC pour SQL Server sur Windows](../../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)  
  
  
