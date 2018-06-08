---
title: Fonction SQLPutData | Documents Microsoft
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
- SQLPutData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPutData
helpviewer_keywords:
- SQLPutData function [ODBC]
ms.assetid: 9a60f004-1477-4c54-a20c-7378e1116713
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0e612ac7a6df03349f7cf00eb83433a86556cf8b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlputdata-function"></a>Fonction SQLPutData
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLPutData** permet à une application envoyer des données d’un paramètre ou une colonne pour le pilote au moment de l’exécution d’instruction. Cette fonction peut être utilisée pour envoyer des valeurs de caractères ou des données binaires dans les parties à une colonne avec un type spécifique à la source données caractère, binaire ou des données (par exemple, les paramètres de type SQL_LONGVARBINARY ou SQL_LONGVARCHAR). **SQLPutData** prend en charge la liaison à un type de données Unicode C, même si le pilote sous-jacent ne prend pas en charge les données Unicode.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLPutData(  
      SQLHSTMT     StatementHandle,  
      SQLPOINTER   DataPtr,  
      SQLLEN       StrLen_or_Ind);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *DataPtr*  
 [Entrée] Pointeur vers une mémoire tampon contenant les données réelles pour le paramètre ou la colonne. Les données doivent être dans le type de données C spécifié dans le *ValueType* argument de **SQLBindParameter** (pour les données de paramètre) ou le *TargetType* argument de **SQLBindCol** (pour les données de la colonne).  
  
 *StrLen_or_Ind*  
 [Entrée] Longueur de \* *DataPtr*. Spécifie la quantité de données envoyées dans un appel à **SQLPutData**. La quantité de données peut varier à chaque appel pour une colonne ou un paramètre donné. *StrLen_or_Ind* est ignorée sauf si elle répond à l’une des conditions suivantes :  
  
-   *StrLen_or_Ind* est SQL_NTS, SQL_NULL_DATA ou SQL_DEFAULT_PARAM.  
  
-   Le type de données C spécifié dans **SQLBindParameter** ou **SQLBindCol** est SQL_C_CHAR ou SQL_C_BINARY.  
  
-   Le type de données C est SQL_C_DEFAULT, et le type de données C par défaut pour le type de données SQL spécifié est SQL_C_CHAR ou SQL_C_BINARY.  
  
 Pour tous les autres types de données C, si *StrLen_or_Ind* n’est pas SQL_NULL_DATA ou SQL_DEFAULT_PARAM, le pilote part du principe que la taille de la \* *DataPtr* mémoire tampon correspond à la taille du type de données C spécifiée avec *ValueType* ou *TargetType* et envoie la valeur de données entière. Pour plus d’informations, consultez [conversion des données à partir de C en Types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) annexe d : Types de données.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLPutData** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLPutData** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|Chaîne ou des données binaires retournées pour un paramètre de sortie a entraîné la troncation des caractères non vides ou les données binaires non NULL. S’il s’agissait d’une valeur de chaîne, il a été tronqué à la droite. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07006|Violation de l’attribut de type de données restreint|La valeur de données identifiée par le *ValueType* argument dans **SQLBindParameter** pour le paramètre dépendant n’a pas pu être converti au type de données identifié par le *ParameterType* argument dans **SQLBindParameter**.|  
|07 S 01|Utilisation non valide du paramètre par défaut|Un paramètre défini avec **SQLBindParameter**, a été SQL_DEFAULT_PARAM, et le paramètre correspondant n’avait pas de valeur par défaut.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|22001|Chaîne de données sont tronquées à droite|L’affectation d’un caractère ou d’une valeur binaire à une colonne a entraîné la troncation de non vides (caractère) ou des caractères non-null (binaires) ou d’octets.<br /><br /> Le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** a été « Y », et plus de données a été envoyés pour un paramètre de type long (type de données a été SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un type de données de spécifique à la source de données de type long) qu’a été spécifié avec la *StrLen_or_IndPtr* argument dans **SQLBindParameter**.<br /><br /> Le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** a été « Y », et plus de données a été envoyées pour une colonne de type long (type de données a été SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un type de données de spécifique à la source de données de type long), celui qui est spécifié dans la mémoire tampon de longueur correspondant à une colonne dans une ligne de données qui a été ajoutées ou mis à jour avec **SQLBulkOperations** ou mis à jour avec **SQLSetPos**.|  
|22003|Valeur numérique hors limites|Les données envoyées pour un paramètre numérique lié ou colonne à la partie entière (par opposition aux fractions de seconde) le nombre à tronquer lorsque affecté à la colonne de table associée.<br /><br /> Renvoi d’une valeur numérique (comme numérique ou chaîne) pour un ou plusieurs paramètres d’entrée/sortie ou de sortie aurait entraîné la partie entière (par opposition aux fractions de seconde) le nombre à tronquer.|  
|22007|Format datetime non valide|Les données envoyées pour un paramètre ou une colonne qui a été liée à une date, heure ou une structure d’horodatage a été, respectivement, une date non valide, heure ou timestamp.<br /><br /> Un paramètre d’entrée/sortie ou de sortie a été lié à une date, heure ou une structure d’horodateur C, et une valeur dans le paramètre retournée était, respectivement, une date non valide, heure ou timestamp. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|22008|Dépassement du champ DateTime|Une expression datetime calculée pour une entrée/sortie ou paramètre de sortie a entraîné une date, heure ou une structure d’horodatage C n’est pas valide.|  
|22012|Division par zéro|Une expression arithmétique calculée pour une entrée/sortie ou paramètre de sortie a entraîné de division par zéro.|  
|22015|Dépassement du champ Intervalle|Les données envoyées pour une colonne numérique ou intervalle exacte ou paramètre de type de données SQL d’intervalle a entraîné une perte de chiffres significatifs.<br /><br /> A été envoyé pour un paramètre avec plus d’un champ ou une colonne de l’intervalle a été converti en type de données numérique et n’ont aucune représentation dans le type de données numérique.<br /><br /> Les données envoyées pour la colonne ou les données de paramètre a été affectées à un intervalle de type SQL, et il n’a pas de représentation de la valeur de type C dans l’intervalle de type SQL.<br /><br /> Les données envoyées pour une valeur numérique exacte ou colonne de l’intervalle C ou du paramètre à un type d’intervalle C a entraîné une perte de chiffres significatifs.<br /><br /> Les données envoyées pour la colonne ou les données de paramètre a été attribuées à une structure d’intervalle C, et il n’a pas de représentation des données dans la structure de données d’intervalle.|  
|22018|Valeur de caractère non valide pour la spécification de la casse|Le type C a été un numérique exact ou approximatif, une valeur datetime ou un type de données intervalle ; le type SQL de la colonne a un type de données caractère. et la valeur dans la colonne ou le paramètre n’est pas un littéral valid du type C dépendant.<br /><br /> Le type SQL a été un numérique exact ou approximatif, une valeur datetime ou un type de données intervalle ; le type C a été SQL_C_CHAR ; et la valeur dans la colonne ou le paramètre n’est pas un littéral valid du type SQL lié.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) l’argument *DataPtr* était un pointeur null et que l’argument *StrLen_or_Ind* n’était pas 0, SQL_DEFAULT_PARAM ou SQL_NULL_DATA.|  
|HY010|Erreur de séquence de fonction|(DM) l’appel de fonction précédente n’était pas un appel à **SQLPutData** ou **SQLParamData**.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lors de l’appel de la fonction SQLPutData.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY019|Données non caractères et non binaires envoyées en fragments|**SQLPutData** a été appelé plusieurs fois pour un paramètre ou une colonne, et il n’était pas utilisé pour envoyer des données de caractères C à une colonne avec un type de données de spécifique à la source de données, binaire ou caractère ou pour envoyer des données binaires de C à une colonne avec un caractère , binaire ou type de données de spécifique à la source de données.|  
|HY020|Tentative de concaténation d’une valeur null|**SQLPutData** a été appelé plusieurs fois depuis l’appel retourné SQL_NEED_DATA et dans un de ces appels, le *StrLen_or_Ind* argument contenus SQL_NULL_DATA ou SQL_DEFAULT_PARAM.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|L’argument *DataPtr* n’était pas un pointeur null et que l’argument *StrLen_or_Ind* était inférieur à 0, mais pas égale à SQL_NTS ou SQL_NULL_DATA.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
 Si **SQLPutData** est appelé lors de l’envoi des données pour un paramètre dans une instruction SQL, il peut retourner tout SQLSTATE qui peut être retournée par la fonction appelée pour exécuter l’instruction (**SQLExecute** ou **SQLExecDirect**). Si elle est appelée lors de l’envoi de données pour une colonne en cours de mise à jour ou ajoutés avec **SQLBulkOperations** ou mise à jour **SQLSetPos**, elle peut retourner tout SQLSTATE qui peut être retournée par **SQLBulkOperations** ou **SQLSetPos**.  
  
## <a name="comments"></a>Commentaires  
 **SQLPutData** peut être appelée pour fournir des données de data-at-execution de deux utilisations : données du paramètre à utiliser dans un appel à **SQLExecute** ou **SQLExecDirect**, ou les données de la colonne à utiliser lorsqu’une ligne est mise à jour ou ajouté par un appel à **SQLBulkOperations** ou est mis à jour par un appel à **SQLSetPos**.  
  
 Lorsqu’une application appelle **SQLParamData** pour déterminer les données d’envoyer, le pilote retourne un indicateur que l’application peut utiliser pour déterminer les données de paramètre pour envoyer ou où se trouvent les données de la colonne. Il retourne également SQL_NEED_DATA, ce qui est un indicateur à l’application qu’il doit appeler **SQLPutData** pour envoyer les données. Dans le *DataPtr* argument **SQLPutData**, l’application passe un pointeur vers la mémoire tampon qui contient les données réelles pour le paramètre ou la colonne.  
  
 Lorsque le pilote retourne SQL_SUCCESS pour **SQLPutData**, l’application appelle **SQLParamData** à nouveau. **SQLParamData** retourne SQL_NEED_DATA si davantage de données doit être envoyé, auquel cas l’application appelle **SQLPutData** à nouveau. Il retourne SQL_SUCCESS, que si toutes les données de data-at-execution a été envoyé. L’application appelle ensuite **SQLParamData** à nouveau. Si le pilote retourne SQL_NEED_DATA et un autre indicateur dans  *\*ValuePtrPtr*, il requiert des données pour un autre paramètre ou une colonne et **SQLPutData** est appelée à nouveau. Si le pilote retourne SQL_SUCCESS, toutes les données d’exécution les données ont été envoyées et l’instruction SQL peut être exécutée ou **SQLBulkOperations** ou **SQLSetPos** appel peut être traité.  
  
 Pour plus d’informations sur les données de paramètre comment data-at-execution est passée au moment de l’exécution d’instruction, consultez « Passage des valeurs de paramètre » dans [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) et [envoyer les données de type Long](../../../odbc/reference/develop-app/sending-long-data.md). Pour plus d’informations sur les données de la colonne comment data-at-execution est mis à jour ou ajouté, consultez la section « SQLSetPos à l’aide de » [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md), « Exécution en bloc les mises à jour à l’aide de signets » dans [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), et [données de type Long et SQLSetPos et SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
> [!NOTE]  
>  Une application peut utiliser **SQLPutData** pour envoyer des données dans les parties uniquement lors de l’envoi des données de caractères C à une colonne avec un type de données de spécifique à la source de données, binaire ou caractère, ou lors de l’envoi des données binaires de C à une colonne avec un caractère, binaire ou données spécifique à la source de données. Si **SQLPutData** est appelée plusieurs fois dans toutes les autres conditions, il retourne SQL_ERROR et SQLSTATE HY019 (données Non caractères et non binaires envoyées en fragments).  
  
## <a name="example"></a>Exemple  
 L’exemple suivant suppose un nom de source de données appelé Test. La base de données associée doit avoir une table que vous pouvez créer, comme suit :  
  
```  
CREATE TABLE emp4 (NAME char(30), AGE int, BIRTHDAY datetime, Memo1 text)  
```  
  
```  
// SQLPutData.cpp  
// compile with: odbc32.lib user32.lib  
#include <stdio.h>  
#include <windows.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define TEXTSIZE  12000  
#define MAXBUFLEN 256  
  
SQLHENV henv = SQL_NULL_HENV;  
SQLHDBC hdbc1 = SQL_NULL_HDBC;       
SQLHSTMT hstmt1 = SQL_NULL_HSTMT;  
  
void Cleanup() {  
   if (hstmt1 != SQL_NULL_HSTMT)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
int main() {  
   RETCODE retcode;  
  
   // SQLBindParameter variables.  
   SQLLEN cbTextSize, lbytes;  
  
   // SQLParamData variable.  
   PTR pParmID;  
  
   // SQLPutData variables.  
   UCHAR  Data[] =   
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyz";  
  
   SDWORD cbBatch = (SDWORD)sizeof(Data) - 1;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle and connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample uses Integrated Security, create SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"Test", SQL_NTS, (UCHAR*)"",SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Allocate statement handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLAllocHandle(hstmt1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Set parameters based on total data to send.  
   lbytes = (SDWORD)TEXTSIZE;  
   cbTextSize = SQL_LEN_DATA_AT_EXEC(lbytes);  
  
   // Bind the parameter marker.  
   retcode = SQLBindParameter (hstmt1,           // hstmt  
                               1,                // ipar  
                               SQL_PARAM_INPUT,  // fParamType  
                               SQL_C_CHAR,       // fCType  
                               SQL_LONGVARCHAR,  // FSqlType  
                               lbytes,           // cbColDef  
                               0,                // ibScale  
                               (VOID *)1,        // rgbValue  
                               0,                // cbValueMax  
                               &cbTextSize);     // pcbValue  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLBindParameter Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Execute the command.  
   retcode =   
      SQLExecDirect(hstmt1, (UCHAR*)"INSERT INTO emp4 VALUES('Paul Borm', 46,'1950-11-12 00:00:00', ?)", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_NEED_DATA) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLExecDirect Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Check to see if NEED_DATA; if yes, use SQLPutData.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if (retcode == SQL_NEED_DATA) {  
      while (lbytes > cbBatch) {  
         SQLPutData(hstmt1, Data, cbBatch);  
         lbytes -= cbBatch;  
      }  
      // Put final batch.  
      retcode = SQLPutData(hstmt1, Data, lbytes);   
   }  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Make final SQLParamData call.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("Final SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Clean up.  
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à un paramètre|[SQLBindParameter, fonction](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Retourner le paramètre suivant pour envoyer des données pour|[SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
