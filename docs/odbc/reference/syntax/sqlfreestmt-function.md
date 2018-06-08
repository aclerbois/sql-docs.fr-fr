---
title: Fonction SQLFreeStmt | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLFreeStmt
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLFreeStmt
helpviewer_keywords:
- SQLFreeStmt function [ODBC]
ms.assetid: 03408162-8b63-4470-90c4-e6c7d8d33892
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d20a486a644a5608accef46a9558c1754efd975d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlfreestmt-function"></a>Fonction SQLFreeStmt
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLFreeStmt** arrête le traitement associé à une instruction spécifique, ferme tous les curseurs ouverts associés à l’instruction, ignore les résultats, en attente ou, si vous le souhaitez, libère toutes les ressources associées au handle d’instruction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLFreeStmt(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Option);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction  
  
 *Option*  
 [Entrée] Une des options suivantes :  
  
 SQL_ fermer : Ferme le curseur associé *au paramètre StatementHandle* (si elle a été définie) et ignore tous les résultats en attente. L’application peut rouvrir ce curseur ultérieurement en exécutant un **sélectionnez** instruction avec les valeurs de paramètre identiques ou différents. Si aucun curseur n’est ouverte, cette option n’a aucun effet sur l’application. **SQLCloseCursor** peut également être appelée pour fermer un curseur. Pour plus d’informations, consultez [fermeture du curseur](../../../odbc/reference/develop-app/closing-the-cursor.md).  
  
 SQL_DROP : Cette option est déconseillée. Un appel à **SQLFreeStmt** avec un *Option* de SQL_DROP est mappé dans le Gestionnaire de pilotes à [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
 SQL_UNBIND : Définit le champ SQL_DESC_COUNT de la ARD 0, libérant toutes les mémoires tampons de colonne liée par **SQLBindCol** pour la donnée *au paramètre StatementHandle*. Cela ne dissocie le signet de colonne ; Pour ce faire, le champ SQL_DESC_DATA_PTR de la ARD pour le signet de colonne est défini avec la valeur NULL. Notez que si cette opération est effectuée sur un descripteur explicitement alloué qui est partagé par plusieurs instructions, l’opération affecte les liaisons de toutes les instructions qui partagent le descripteur. Pour plus d’informations, consultez [vue d’ensemble de la récupération des résultats (Basic)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 SQL_RESET_PARAMS : Définit le champ SQL_DESC_COUNT de l’APD 0, libérant toutes les mémoires tampons de paramètres définis par **SQLBindParameter** pour la donnée *au paramètre StatementHandle*. Si cette opération est effectuée sur un descripteur explicitement alloué qui est partagé par plusieurs instructions, cette opération affecte les liaisons de toutes les instructions qui partagent le descripteur. Pour plus d’informations, consultez [liaison de paramètres](../../../odbc/reference/develop-app/binding-parameters-odbc.md).  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLFreeStmt** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLFreeStmt** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque **SQLFreeStmt** a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avec *Option* la valeur SQL_RESET_PARAMS avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY092|Type d’option hors limites|(DM) la valeur spécifiée pour l’argument *Option* n’était pas :<br /><br /> SQL_CLOSE SQL_DROP SQL_UNBIND SQL_RESET_PARAMS|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Appel de **SQLFreeStmt** avec la SQL_CLOSE option revient à appeler la méthode **SQLCloseCursor**, sauf que **SQLFreeStmt** avec SQL_CLOSE n’affecte pas l’application si aucun curseur n’est ouverte sur l’instruction. Si aucun curseur n’est ouvrir, un appel à **SQLCloseCursor** retourne SQLSTATE 24000 (état de curseur non valide).  
  
 Une application ne doit pas utiliser un descripteur d’instruction après que qu’elle a été libérée ; le Gestionnaire de pilotes ne vérifie pas la validité d’un handle dans un appel de fonction.  
  
## <a name="example"></a>Exemple  
 Il est une bonne pratique de programmation pour libérer les handles. Toutefois, par souci de simplicité, l’exemple suivant n’inclut pas de code qui libère alloué des handles. Pour obtenir un exemple montrant comment libérer les handles, consultez [SQLFreeHandle, fonction](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
```  
// SQLFreeStmt.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   // declare and initialize the environment, connection, statement handles  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1024 + 1, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
   retCode = SQLFreeStmt(hstmt, SQL_UNBIND);  
   retCode = SQLFreeStmt(hstmt, SQL_RESET_PARAMS);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Fermeture d’un curseur|[SQLCloseCursor, fonction](../../../odbc/reference/syntax/sqlclosecursor-function.md)|  
|La libération d’un handle|[SQLFreeHandle, fonction](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
