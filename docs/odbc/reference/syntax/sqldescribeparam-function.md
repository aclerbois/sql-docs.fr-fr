---
title: Fonction SQLDescribeParam | Documents Microsoft
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
- SQLDescribeParam
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeParam
helpviewer_keywords:
- SQLDescribeParam function [ODBC]
ms.assetid: 1f5b63c4-2f3e-44da-b155-876405302281
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 584e24e074a89670f0182fdfc29be1017b0a6ad6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqldescribeparam-function"></a>Fonction SQLDescribeParam
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 1.0 ODBC : ODBC  
  
 **Résumé**  
 **SQLDescribeParam** retourne la description d’un marqueur de paramètre associé à une instruction SQL préparée. Ces informations sont également disponibles dans les champs de l’IPD.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLDescribeParam(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT *   DataTypePtr,  
      SQLULEN *       ParameterSizePtr,  
      SQLSMALLINT *   DecimalDigitsPtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="argument"></a>Argument  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *ParameterNumber*  
 [Entrée] Nombre de marqueur de paramètre classés de manière séquentielle dans l’ordre croissant des paramètres, en commençant à 1.  
  
 *DataTypePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le type de données SQL du paramètre. Cette valeur est lue à partir du champ d’enregistrement SQL_DESC_CONCISE_TYPE de l’IPD. Il s’agit d’une des valeurs dans le [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) section annexe d : les Types de données ou un type de données SQL spécifique au pilote.  
  
 Dans ODBC 3. *x*, SQL_TYPE_DATE SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP s’affichera dans  *\*DataTypePtr* pour date, heure ou données timestamp, respectivement ; dans ODBC 2. *x*, SQL_DATE, SQL_TIME ou SQL_TIMESTAMP sera retourné. Le Gestionnaire de pilotes effectue les mappages requis lorsqu’un ODBC 2. *x* application fonctionne avec un ODBC 3. *x* pilote ou lorsqu’un ODBC 3. *x* application fonctionne avec une API ODBC 2. *x* pilote.  
  
 Lorsque *ColumnNumber* est égal à 0 (pour un signet de colonne), SQL_BINARY est retourné dans  *\*DataTypePtr* de signets de longueur variable. (SQL_INTEGER est retournée si les signets sont utilisés par un ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote ou par un ODBC 2. *x* application utilisant une ODBC 3. *x* pilote.)  
  
 Pour plus d’informations, consultez [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) annexe d : Types de données. Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.  
  
 *ParameterSizePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner la taille, en caractères, de la colonne ou l’expression du marqueur de paramètre correspondant, tel que défini par la source de données. Pour plus d’informations sur la taille de colonne, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *DecimalDigitsPtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le nombre de chiffres décimaux de la colonne ou expression de paramètre correspondant, tel que défini par la source de données. Pour plus d’informations sur les chiffres décimaux, voir [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *NullablePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner une valeur qui indique si le paramètre accepte les valeurs NULL. Cette valeur est lue à partir du champ SQL_DESC_NULLABLE de l’IPD. Il peut s'agir :  
  
-   SQL_NO_NULLS : Le paramètre n’autorise pas les valeurs NULL (il s’agit de la valeur par défaut).  
  
-   SQL_NULLABLE : Le paramètre accepte les valeurs NULL.  
  
-   SQL_NULLABLE_UNKNOWN : Le pilote ne peut pas déterminer si le paramètre accepte les valeurs NULL.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLDescribeParam** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLDescribeParam** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|07009|Index de descripteur non valide|(DM) la valeur spécifiée pour l’argument *ParameterNumber* est inférieur à 1.<br /><br /> La valeur spécifiée pour l’argument *ParameterNumber* était supérieur au nombre de paramètres dans l’instruction SQL associée.<br /><br /> Le marqueur de paramètre fait partie d’une instruction non-DML.<br /><br /> Le marqueur de paramètre faisait partie d’un **sélectionnez** liste.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|21S01|Liste de valeurs d’insertion ne correspond pas à la liste des colonnes|Le nombre de paramètres dans le **insérer** instruction ne correspondait pas le nombre de colonnes dans la table nommée dans l’instruction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire qui est requis pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), la fonction a été appelée avant d’appeler **SQLPrepare** ou **SQLExecDirect** pour le *au paramètre StatementHandle*.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLDescribeParam** fonction a été appelée.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Marqueurs de paramètres sont numérotées dans l’ordre croissant des paramètres, en commençant par 1, dans l’ordre de qu'apparition dans l’instruction SQL.  
  
 **SQLDescribeParam** ne retourne pas le type (entrée, entrée/sortie, ou de sortie) d’un paramètre dans une instruction SQL. Sauf dans les appels aux procédures, tous les paramètres dans les instructions SQL sont des paramètres d’entrée. Pour déterminer le type de chaque paramètre dans un appel à une procédure, une application appelle **SQLProcedureColumns**.  
  
 Pour plus d’informations, consultez [décrivant les paramètres](../../../odbc/reference/develop-app/describing-parameters.md).  
  
## <a name="code-example"></a>Exemple de code  
 L’exemple suivant demande à l’utilisateur d’une instruction SQL et prépare ensuite cette instruction. Ensuite, il appelle **SQLNumParams** pour déterminer si l’instruction contient tous les paramètres. Si l’instruction contient des paramètres, il appelle **SQLDescribeParam** pour décrire ces paramètres et **SQLBindParameter** pour les lier. Enfin, il invite l’utilisateur pour les valeurs des paramètres et puis exécute l’instruction.  
  
```  
SQLCHAR       Statement[100];  
SQLSMALLINT   NumParams, i, DataType, DecimalDigits, Nullable;  
SQLUINTEGER   ParamSize;  
SQLHSTMT      hstmt;  
  
// Prompt the user for an SQL statement and prepare it.  
GetSQLStatement(Statement);  
SQLPrepare(hstmt, Statement, SQL_NTS);  
  
// Check to see if there are any parameters. If so, process them.  
SQLNumParams(hstmt, &NumParams);  
if (NumParams) {  
   // Allocate memory for three arrays. The first holds pointers to buffers in which  
   // each parameter value will be stored in character form. The second contains the  
   // length of each buffer. The third contains the length/indicator value for each  
   // parameter.  
   SQLPOINTER * PtrArray = (SQLPOINTER *) malloc(NumParams * sizeof(SQLPOINTER));  
   SQLINTEGER * BufferLenArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
   SQLINTEGER * LenOrIndArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
  
   for (i = 0; i < NumParams; i++) {  
   // Describe the parameter.  
   SQLDescribeParam(hstmt, i + 1, &DataType, &ParamSize, &DecimalDigits, &Nullable);  
  
   // Call a helper function to allocate a buffer in which to store the parameter  
   // value in character form. The function determines the size of the buffer from  
   // the SQL data type and parameter size returned by SQLDescribeParam and returns  
   // a pointer to the buffer and the length of the buffer.  
   AllocParamBuffer(DataType, ParamSize, &PtrArray[i], &BufferLenArray[i]);  
  
   // Bind the memory to the parameter. Assume that we only have input parameters.  
   SQLBindParameter(hstmt, i + 1, SQL_PARAM_INPUT, SQL_C_CHAR, DataType, ParamSize,  
         DecimalDigits, PtrArray[i], BufferLenArray[i],  
         &LenOrIndArray[i]);  
  
   // Prompt the user for the value of the parameter and store it in the memory  
   // allocated earlier. For simplicity, this function does not check the value  
   // against the information returned by SQLDescribeParam. Instead, the driver does  
   // this when the statement is executed.  
   GetParamValue(PtrArray[i], BufferLenArray[i], &LenOrIndArray[i]);  
   }  
}  
  
// Execute the statement.  
SQLExecute(hstmt);  
  
// Process the statement further, such as retrieving results (if any) and closing the  
// cursor (if any). Code not shown.  
  
// Free the memory allocated for each parameter and the memory allocated for the arrays  
// of pointers, buffer lengths, and length/indicator values.  
for (i = 0; i < NumParams; i++) free(PtrArray[i]);  
free(PtrArray);  
free(BufferLenArray);  
free(LenOrIndArray);  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à un paramètre|[SQLBindParameter, fonction](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Préparation de l’exécution d’une instruction|[SQLPrepare, fonction](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
