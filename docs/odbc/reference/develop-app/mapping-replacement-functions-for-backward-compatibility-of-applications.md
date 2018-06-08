---
title: Mappage des fonctions de remplacement pour la compatibilité des applications - ODBC | Documents Microsoft
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
- mapping replacement functions [ODBC]
- upgrading applications [ODBC], mapping replacement functions
- compatibility [ODBC], mapping replacement functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], mapping replacement functions
- application upgrades [ODBC], mapping replacement functions
- backward compatibility [ODBC], mapping replacement functions
ms.assetid: f5e6d9da-76ef-42cb-b3f5-f640857df732
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 14ca73aefb033580c2770da05189e3de04a424e3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="mapping-replacement-functions-for-backward-compatibility-of-applications"></a>Mappage des fonctions de remplacement pour la compatibilité descendante des Applications
Un ODBC 3 *.x* application parcourt le ODBC 3 *.x* du Gestionnaire de pilotes fonctionnera sur une API ODBC 2. *x* pilote tant qu’aucune nouvelle fonctionnalité n’est utilisées. Les deux dupliqué fonctionnalités et changements de comportement, toutefois, affectent-elles la façon qui le ODBC 3. *x* application fonctionne sur une API ODBC 2. *x* pilote. Lorsque vous travaillez avec une API ODBC 2. *x* pilote, le Gestionnaire de pilotes mappe le suivantes ODBC 3. *x* fonctions qui ont remplacé un ou plusieurs ODBC 2. *x* fonctions, dans le correspondant ODBC 2. *x* fonctions.  
  
|ODBC 3. *x* (fonction)|ODBC 2. *x* (fonction)|  
|-------------------------|-------------------------|  
|**SQLAllocHandle**|**SQLAllocEnv**, **SQLAllocConnect**, ou **SQLAllocStmt**|  
|**SQLBulkOperations**|**SQLSetPos**|  
|**SQLColAttribute**|**SQLColAttributes**|  
|**SQLEndTran**|**SQLTransact**|  
|**SQLFetch**|**SQLExtendedFetch**|  
|**SQLFetchScroll**|**SQLExtendedFetch**|  
|**SQLFreeHandle**|**SQLFreeEnv**, **SQLFreeConnect**, ou **SQLFreeStmt**|  
|**SQLGetConnectAttr**|**SQLGetConnectOption**|  
|**SQLGetDiagRec**|**SQLError**|  
|**SQLGetStmtAttr**|**SQLGetStmtOption**[1]|  
|**SQLSetConnectAttr**|**SQLSetConnectOption**|  
|**SQLSetStmtAttr**|**SQLSetStmtOption**[1]|  
  
 [1] d’autres actions peuvent également être prises, selon l’attribut demandé.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
 Le Gestionnaire de pilotes mappe à **SQLAllocEnv**, **SQLAllocConnect**, ou **SQLAllocStmt**, le cas échéant. L’appel suivant à **SQLAllocHandle**:  
  
```  
SQLAllocHandle(HandleType, InputHandle, OutputHandlePtr);  
```  
  
 génère dans le Gestionnaire de pilotes exécutant les opérations suivantes (conceptuel, aucune vérification des erreurs) mappage :  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLAllocEnv(OutputHandlePtr));  
   case SQL_HANDLE_DBC: return (SQLAllocConnect (InputHandle, OutputHandlePtr));  
   case SQL_HANDLE_STMT: return (SQLAllocStmt (InputHandle, OutputHandlePtr));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
 Le Gestionnaire de pilotes mappe à **SQLSetPos**. L’appel suivant à **SQLBulkOperations**:  
  
```  
SQLBulkOperations(hstmt, Operation);  
```  
  
 provoque la séquence d’étapes suivante :  
  
1.  Si l’argument de l’opération est SQL_ADD, le Gestionnaire de pilotes appelle **SQLSetPos** comme suit :  
  
    ```  
    SQLSetPos (hstmt, 0, SQL_ADD, SQL_LOCK_NO_CHANGE);  
    ```  
  
2.  Si l’argument de l’opération n’est pas SQL_ADD, le pilote retourne SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
3.  Si l’application tente de modifier le SQL_ATTR_ROW_STATUS_PTR entre les appels à **SQLFetch** ou **SQLFetchScroll** et **SQLBulkOperations**, le Gestionnaire de pilotes retourne SQLSTATE HY011 (attribut ne peut pas être défini maintenant).  
  
4.  Si l’argument de l’opération est SQL_ADD, l’application doit appeler **SQLBindCol** pour lier les données à insérer. Il ne peut pas appeler **SQLSetDescField** ou **SQLSetDescRec** pour lier les données à insérer.  
  
5.  Si l’argument de l’opération est SQL_ADD et le nombre de lignes à insérer n’est pas identique à la taille actuelle de l’ensemble de lignes, **SQLSetStmtAttr** doit être appelé pour définir l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE au nombre de lignes à insérer avant d’appeler **SQLBulkOperations**. Pour rétablir la taille d’ensemble de lignes précédentes, l’application doit définir l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE avant **SQLFetch**, **SQLFetchScroll**, ou **SQLSetPos** est appelée.  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
 Le Gestionnaire de pilotes mappe à **SQLColAttributes**. L’appel suivant à **SQLColAttribute**:  
  
```  
SQLColAttribute(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
```  
  
 provoque la séquence d’étapes suivante :  
  
1.  Si *FieldIdentifier* est une des opérations suivantes :  
  
     SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH, SQL_DESC_OCTET_LENGTH, SQL_DESC_UNNAMED, SQL_DESC_BASE_COLUMN_NAME, SQL_DESC_LITERAL_PREFIX, SQL_DESC_LITERAL_SUFFIX ou SQL_DESC_LOCAL_TYPE_NAME  
  
     le Gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY091 (identificateur de champ de descripteur non valide). Aucune autre règle de cette section s’appliquent.  
  
2.  Le Gestionnaire de pilotes mappe SQL_COLUMN_COUNT, SQL_COLUMN_NAME ou SQL_COLUMN_NULLABLE SQL_DESC_COUNT, SQL_DESC_NAME ou SQL_DESC_NULLABLE, respectivement. (Un ODBC 2 *.x* pilote doive prennent uniquement en charge SQL_COLUMN_COUNT SQL_COLUMN_NAME et SQL_COLUMN_NULLABLE, SQL_DESC_COUNT, SQL_DESC_NAME et non SQL_DESC_NULLABLE.) L’appel de SQLColAttribute est mappé à :  
  
    ```  
    SQLColAttributes(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
    ```  
  
3.  Tous les autres *FieldIdentifier* valeurs sont transmises au pilote, avec **SQLColAttribute** mappé à **SQLColAttributes** comme indiqué précédemment.  
  
4.  Si *BufferLength* est inférieur à 0, le Gestionnaire de pilotes renvoie SQL_ERROR avec SQLSTATE HY090 (longueur de chaîne ou une mémoire tampon non valide). Aucune autre règle de cette section s’appliquent.  
  
5.  Si *FieldIdentifier* est SQL_DESC_CONCISE_TYPE et le type retourné est un type de données datetime concise, les mappages de gestionnaire de pilotes le retour des valeurs pour les codes de date, time et timestamp.  
  
6.  Le Gestionnaire de pilotes effectue des contrôles nécessaires pour voir si SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Si, par conséquent, le Gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction). Aucune autre règle de cette section s’appliquent.  
  
## <a name="sqlendtran"></a>SQLEndTran  
 Le Gestionnaire de pilotes mappe à **SQLTransact**. L’appel suivant à **SQLEndTran**:  
  
```  
SQLEndTran(HandleType, Handle, CompletionType);  
```  
  
 génère dans le Gestionnaire de pilotes exécutant les opérations suivantes (conceptuel, aucune vérification des erreurs) mappage :  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV:return(SQLTransact(Handle, SQL_NULL_HDBC, CompletionType));  
   case SQL_HANDLE_DBC:return(SQLTransact(SQL_NULL_HENV, Handle, CompletionType);  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlfetch"></a>SQLFetch  
 Le Gestionnaire de pilotes mappe à **SQLExtendedFetch** avec un *FetchOrientation* argument de SQL_FETCH_NEXT. L’appel suivant à **SQLFetch**:  
  
```  
SQLFetch (StatementHandle);  
```  
  
 entraîne l’appel du Gestionnaire de pilotes **SQLExtendedFetch**, comme suit :  
  
```  
rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, &RowCount, RowStatusArray);  
```  
  
 Dans cet appel, le *pcRow* argument est défini à la valeur de l’application définit l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR à via un appel à **SQLSetStmtAttr**.  
  
> [!NOTE]  
>  Lorsque l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROW_STATUS_PTR pour pointer vers un tableau d’état, le Gestionnaire de pilotes met en cache le pointeur. *RowStatusArray* peut être égal à un pointeur null.  
  
 Si le pilote ne prend pas en charge **SQLExtendedFetch** et la bibliothèque de curseurs est chargée, le Gestionnaire de pilotes utilise la bibliothèque de curseurs **SQLExtendedFetch** pour mapper **SQLFetch** à **SQLExtendedFetch**. Si le pilote ne prend pas en charge **SQLExtendedFetch** et la bibliothèque de curseurs n’est pas chargée, le Gestionnaire de pilotes passe l’appel à **SQLFetch** par le biais du pilote. Si l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROW_STATUS_PTR, le Gestionnaire de pilotes permet de s’assurer que le tableau est rempli. Si l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROWS_FETCHED_PTR, le Gestionnaire de pilotes définit ce champ sur 1.  
  
## <a name="sqlfetchscroll"></a>SQLFetchScroll  
 Le Gestionnaire de pilotes mappe à **SQLExtendedFetch**. L’appel suivant à **SQLFetchScroll**:  
  
```  
SQLFetchScroll(StatementHandle, FetchOrientation, FetchOffset);  
```  
  
 provoque la séquence d’étapes suivante :  
  
1.  Lorsque l’application appelle **SQLSetStmtAttr** pour définir SQL_ATTR_ROW_STATUS_PTR (qui définit le champ SQL_DESC_ARRAY_STATUS_PTR dans l’IRD) pointe vers un tableau d’état, le Gestionnaire de pilotes met en cache de ce pointeur. Soit ce pointeur *RowStatusArray*; sinon, laissez *RowStatusArray* être égale à un pointeur null. Si le *RowStatusArray* argument est un pointeur null, le Gestionnaire de pilotes génère un tableau de l’état de ligne.  
  
2.  Si *FetchOrientation* ne fait pas partie de SQL_FETCH_NEXT, SQL_FETCH_PRIOR, SQL_FETCH_ABSOLUTE, SQL_FETCH_RELATIVE, SQL_FETCH_FIRST, SQL_FETCH_LAST ou SQL_FETCH_BOOKMARK, le Gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY106 (type de récupération hors plage). Aucune autre règle de cette section s’appliquent.  
  
3.  Cas :  
  
-   Si *FetchOrientation* est égal à SQL_FETCH_BOOKMARK, puis :  
  
    -   Si **SQLSetStmtAttr** a été appelé précédemment pour définir la valeur de SQL_ATTR_FETCH_BOOKMARK_PTR, puis laissez *Bmk* la valeur obtenue par le pointeur SQL_DESC_FETCH_BOOKMARK_PTR.  
  
    -   Sinon, retourne SQL_ERROR avec SQLSTATE HY111 (valeur de signet non valide). Aucune autre règle de cette section s’appliquent.  
  
     Le Gestionnaire de pilotes appelle maintenant **SQLExtendedFetch**, comme suit :  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, Bmk, pcRow, RowStatusArray);  
    ```  
  
-   Sinon, le Gestionnaire de pilotes appelle **SQLExtendedFetch**, comme suit :  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, pcRow, RowStatusArray);  
    ```  
  
     Dans ces appels, le *pcRow* argument est défini à la valeur de l’application définit l’attribut d’instruction SQL_ATTR_ROWS_FETCHED_PTR à via un appel à **SQLSetStmtAttr**.  
  
-   SQL_ATTR_ROW_ARRAY_SIZE est mappé à SQL_ROWSET_SIZE.  
  
-   Si *rc* est égal à SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO et si *FetchOrientation* est égal à SQL_FETCH_BOOKMARK et *FetchOffset* n’est pas égal à 0, puis le Gestionnaire de pilotes publie un avertissement, la valeur SQLSTATE 01S10 (tentative extraire un décalage de signet, la valeur du décalage ignorée) et retourne SQL_SUCCESS_WITH_INFO.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 Le Gestionnaire de pilotes mappe à **SQLFreeEnv**, **SQLFreeConnect**, ou **SQLFreeStmt** selon le cas. L’appel suivant à **SQLFreeHandle**:  
  
```  
SQLFreeHandle(HandleType, Handle);  
```  
  
 génère dans le Gestionnaire de pilotes exécutant les opérations suivantes (conceptuel, aucune vérification des erreurs) mappage :  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLFreeEnv(Handle));  
   case SQL_HANDLE_DBC: return (SQLFreeConnect(Handle));  
   case SQL_HANDLE_STMT: return (SQLFreeStmt(Handle, SQL_DROP));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
 Le Gestionnaire de pilotes mappe à **SQLGetConnectOption**. L’appel suivant à **SQLGetConnectAttr**:  
  
```  
SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 provoque la séquence d’étapes suivante :  
  
1.  Si *attribut* n’est pas un attribut de connexion ou une instruction définie par le pilote et n’est pas un attribut défini dans ODBC 2. *x*, le Gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle dans cette section s’appliquent.  
  
2.  Si *attribut* est égal à SQL_ATTR_AUTO_IPD ou SQL_ATTR_METADATA_ID, les retours de gestionnaire de pilotes SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
3.  Le Gestionnaire de pilotes effectue des contrôles nécessaires pour voir si SQLSTATE 08003 (connexion non ouverte) ou SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Dans ce cas, le Gestionnaire de pilotes retourne SQL_ERROR et publie le message d’erreur approprié. Aucune autre règle de cette section s’appliquent.  
  
4.  Les appels du Gestionnaire de pilotes **SQLGetConnectOption** comme suit :  
  
    ```  
    SQLGetConnectOption (ConnectionHandle, Attribute, ValuePtr);  
    ```  
  
     Notez que la *BufferLength* et *StringLengthPtr* sont ignorés.  
  
## <a name="sqlgetdata"></a>SQLGetData  
 Lorsqu’une application ODBC 3. *x* application utilisant une API ODBC 2 *.x* pilote appelle **SQLGetData** avec la *ColumnNumber* argument égal à 0, la version 3 ODBC *.x* du Gestionnaire de pilotes le mappe à un appel à **SQLGetStmtOption** avec la *Option* attribut la valeur SQL_GET_BOOKMARK.  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
 Le Gestionnaire de pilotes mappe à **SQLGetStmtOption**. L’appel suivant à **SQLGetStmtAttr**:  
  
```  
SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 provoque la séquence d’étapes suivante :  
  
1.  Si *attribut* n’est pas un attribut de connexion ou une instruction définie par le pilote et n’est pas un attribut défini dans ODBC 2. *x*, le Gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle dans cette section s’appliquent.  
  
2.  Si *attribut* est une des opérations suivantes :  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     le Gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle de cette section s’appliquent.  
  
3.  Le Gestionnaire de pilotes effectue des contrôles nécessaires pour voir si SQLSTATE HY010 (erreur de séquence de fonction) doit être déclenché. Si, par conséquent, le Gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction). Aucune autre règle de cette section s’appliquent.  
  
4.  Si *attribut* est égal à SQL_ATTR_ROWS_FETCHED_PTR, le Gestionnaire de pilotes retourne un pointeur à la variable du Gestionnaire de pilotes interne *a*, qu’il a utilisé ou que vous utiliserez dans un appel à **SQLExtendedFetch**. Aucune autre règle de cette section s’appliquent.  
  
5.  Si *attribut* est égal à SQL_DESC_FETCH_BOOKMARK_PTR, le Gestionnaire de pilotes retourne le pointeur d’approprié qui il a mis en cache pendant un appel à **SQLSetStmtAttr**.  
  
6.  Si *attribut* est égal à SQL_ATTR_ROW_STATUS_PTR, le Gestionnaire de pilotes retourne le pointeur d’approprié qui il a mis en cache pendant un appel à **SQLSetStmtAttr**.  
  
7.  Les appels du Gestionnaire de pilotes **SQLGetStmtOption** comme suit :  
  
    ```  
    SQLGetStmtOption (hstmt, fOption, pvParam);  
    ```  
  
     où *hstmt*, *fOption*, et *pvParam* définira les valeurs de *au paramètre StatementHandle*, *attribut*, et *ValuePtr*, respectivement. Le *BufferLength* et *StringLengthPtr* sont ignorés.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
 Le Gestionnaire de pilotes mappe à **SQLSetConnectOption**. L’appel suivant à **SQLSetConnectAttr**:  
  
```  
SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, StringLength);  
```  
  
 provoque la séquence d’étapes suivante :  
  
1.  Si *attribut* n’est pas un attribut de connexion ou une instruction définie par le pilote et n’est pas un attribut défini dans ODBC 2. *x*, le Gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle dans cette section s’appliquent.  
  
2.  Si *attribut* est égal à SQL_ATTR_AUTO_IPD, les retours de gestionnaire de pilotes SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
3.  Le Gestionnaire de pilotes effectue des contrôles nécessaires pour voir si SQLSTATE 08003 (connexion non ouverte) ou SQLSTATE HY010 (erreur de séquence de fonction) devoir être déclenché. Si une de ces erreurs doit être déclenché, le Gestionnaire de pilotes retourne SQL_ERROR et publie le message d’erreur approprié. Aucune autre règle de cette section s’appliquent.  
  
4.  Les appels du Gestionnaire de pilotes **SQLSetConnectOption** comme suit :  
  
    ```  
    SQLSetConnectOption (hdbc, fOption, vParam);  
    ```  
  
     où *pas*, *fOption*, et *vParam* définira les valeurs de *handle de connexion*, *attribut*, et *ValuePtr*, respectivement. *StringLengthPtr* est ignoré.  
  
> [!NOTE]  
>  La capacité à définir les attributs d’instruction sur le niveau de la connexion a été déconseillée. Attributs de l’instruction ne doivent jamais être définis sur le niveau de connexion par un ODBC 3. *x* application.  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Le Gestionnaire de pilotes mappe à **SQLSetStmtOption**. L’appel suivant à **SQLSetStmtAttr**:  
  
```  
SQLSetStmtAttr(StatementHandle, Attribute, ValuePtr, StringLength);  
```  
  
 provoque la séquence d’étapes suivante :  
  
1.  Si *attribut* n’est pas un attribut de connexion ou une instruction définie par le pilote et n’est pas un attribut défini dans ODBC 2. *x*, le Gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle dans cette section s’appliquent.  
  
2.  Si *attribut* est une des opérations suivantes :  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     le Gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide). Aucune autre règle de cette section s’appliquent.  
  
3.  Le Gestionnaire de pilotes effectue les contrôles nécessaires pour voir si nécessaire (erreur de séquence de fonction) de SQLSTATE HY010 à signaler. Si, par conséquent, le Gestionnaire de pilotes retourne SQL_ERROR et SQLSTATE HY010 (erreur de séquence de fonction). Aucune autre règle de cette section s’appliquent.  
  
4.  Si *attribut* est égal à SQL_ATTR_PARAMSET_SIZE ou SQL_ATTR_PARAMS_PROCESSED_PTR, consultez la section « Mappages pour la gestion des tableaux de paramètres, » plus loin dans cette rubrique. Aucune autre règle de cette section s’appliquent.  
  
5.  Si *attribut* est égal à SQL_ATTR_ROWS_FETCHED_PTR, le met en cache du Gestionnaire de pilotes que le pointeur de la valeur pour une utilisation ultérieure avec **SQLFetchScroll**.  
  
6.  Si *attribut* est égal à SQL_ATTR_ROW_STATUS_PTR, le met en cache du Gestionnaire de pilotes que le pointeur de la valeur pour une utilisation ultérieure avec **SQLFetchScroll** ou **SQLSetPos**. Aucune autre règle de cette section s’appliquent.  
  
7.  Si *attribut* est égal à SQL_ATTR_FETCH_BOOKMARK_PTR, le met en cache du Gestionnaire de pilotes *ValuePtr* et utilise la valeur mise en cache ultérieurement dans un appel à **SQLFetchScroll**. Aucune autre règle de cette section s’appliquent.  
  
8.  Les appels du Gestionnaire de pilotes **SQLSetStmtOption** comme suit :  
  
    ```  
    SQLSetStmtOption (hstmt, fOption, vParam);  
    ```  
  
     où *hstmt*, *fOption*, et *vParam* définira les valeurs de *au paramètre StatementHandle*, *attribut*, et *ValuePtr*, respectivement. Le *StringLength* argument est ignoré.  
  
     If un ODBC 2. *x* pilote prend en charge les options de l’instruction de chaîne de caractères, spécifiques au pilote, un ODBC 3. *x* application doit appeler **SQLSetStmtOption** pour définir ces options.  
  
## <a name="mappings-for-handling-parameter-arrays"></a>Mappages pour la gestion des tableaux de paramètres  
 Lorsque l’application appelle la méthode :  
  
```  
SQLSetStmtAttr (StatementHandle, SQL_ATTR_PARAMSET_SIZE, Size, StringLength);  
```  
  
 les appels du Gestionnaire de pilotes :  
  
```  
SQLParamOptions (StatementHandle, Size, &RowCount);  
```  
  
 Le Gestionnaire de pilotes une version ultérieure retourne un pointeur à cette variable lorsque l’application appelle **SQLGetStmtAttr** SQL_ATTR_PARAMS_PROCESSED_PTR de récupération. Le Gestionnaire de pilotes ne pouvez pas modifier cette variable interne jusqu'à ce que le handle d’instruction est retourné à l’état prêt ou alloué.  
  
 Une application ODBC 3. *x* application peut appeler **SQLGetStmtAttr** pour obtenir la valeur de SQL_ATTR_PARAMS_PROCESSED_PTR bien qu’il n’a pas explicitement défini le champ SQL_DESC_ARRAY_SIZE dans le descripteur APD. Cette situation peut se produire, par exemple, si l’application dispose d’une routine générique qui vérifie la « ligne » des paramètres actuel en cours de traitement lorsque **SQLExecute** retourne SQL_NEED_DATA. Cette routine est appelée ou non le SQL_DESC_ARRAY_SIZE est 1 ou supérieur à 1. Pour prendre en compte, le Gestionnaire de pilotes devez définir cette variable interne ou non de l’application a appelé **SQLSetStmtAttr** pour définir le champ SQL_DESC_ARRAY_SIZE dans APD. Si SQL_DESC_ARRAY_SIZE n’a pas été défini, le Gestionnaire de pilotes a pour vous assurer que cette variable contient la valeur 1 avant de retourner à partir de **SQLExecDirect** ou **SQLExecute**.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Dans ODBC 3. *x*, l’appel **SQLFetch** ou **SQLFetchScroll** remplit le SQL_DESC_ARRAY_STATUS_PTR dans l’IRD, et le champ SQL_DIAG_ROW_NUMBER d’un enregistrement de diagnostic donné contient le numéro de la ligne dans l’ensemble de lignes appartenant à cet enregistrement. Vous utilisez ce paramètre, l’application peut mettre en corrélation un message d’erreur avec une position de ligne donnée.  
  
 Une application ODBC 2. *x* pilote ne pourra pas fournir cette fonctionnalité. Toutefois, elle fournira la délimitation d’erreur avec 01 s 01 SQLSTATE (erreur de ligne). Une application ODBC 3. *x* application qui utilise **SQLFetch** ou **SQLFetchScroll** en allant sur une API ODBC 2. *x* pilote doit être informé de ce fait. Notez également qu’une telle application sera impossible d’appeler **SQLGetDiagField** réellement obtenir le champ SQL_DIAG_ROW_NUMBER quand même. Une application ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote sera en mesure d’appeler **SQLGetDiagField** uniquement avec un *DiagIdentifier* argument de SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_RETURNCODE ou SQL_DIAG_SQLSTATE. La version 3 ODBC *.x* du Gestionnaire de pilotes conserve la structure de données de diagnostic lorsque vous travaillez avec une API ODBC 2. *x* pilote, mais ODBC 2. *x* pilote retourne uniquement ces quatre champs.  
  
 Lorsqu’une application ODBC 2. *x* application fonctionne avec une API ODBC 2. *x* pilote, si une opération peut entraîner plusieurs erreurs renvoyées par le Gestionnaire de pilotes, différentes erreurs peuvent être renvoyés par la version 3 ODBC *.x* du Gestionnaire de pilotes que par l’API ODBC 2. *x* du Gestionnaire de pilotes.  
  
## <a name="mappings-for-bookmark-operations"></a>Mappages pour les opérations de signet  
 La version 3 ODBC *.x* du Gestionnaire de pilotes effectue les mappages suivants lorsqu’un ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote effectue les opérations de signet.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 Lorsqu’une application ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote appelle **SQLBindCol** à lier à la colonne 0 à *fCType* égale à SQL_C_VARBOOKMARK, la version 3 ODBC *.x* du Gestionnaire de pilotes vérifie si le *BufferLength* argument est inférieur à 4 ou supérieure à 4 et si tel est le cas, retourne SQLSTATE HY090 (longueur de chaîne ou une mémoire tampon non valide). Si le *BufferLength* argument est égal à 4, le Gestionnaire de pilotes appelle **SQLBindCol** dans le pilote, après le remplacement *fCType* avec SQL_C_BOOKMARK.  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Lorsqu’une application ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote appelle **SQLColAttribute** avec la *ColumnNumber* argument défini à 0, le Gestionnaire de pilote retourne la *FieldIdentifier* valeurs répertoriées dans le tableau suivant.  
  
|*FieldIdentifier*|Valeur|  
|-----------------------|-----------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|  
|SQL_DESC_CATALOG_NAME|"" (chaîne vide)|  
|SQL_DESC_CONCISE_TYPE|SQL_BINARY|  
|SQL_DESC_COUNT|La valeur retournée par **SQLNumResultCols**|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|  
|SQL_DESC_DISPLAY_SIZE|8|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|  
|SQL_DESC_LABEL|"" (chaîne vide)|  
|SQL_DESC_LENGTH|0|  
|SQL_DESC_LITERAL_PREFIX|"" (chaîne vide)|  
|SQL_DESC_LITERAL_SUFFIX|"" (chaîne vide)|  
|SQL_DESC_LOCAL_TYPE_NAME|"" (chaîne vide)|  
|SQL_DESC_NAME|"" (chaîne vide)|  
|SQL_DESC_NULLABLE|SQL_NO_NULLS|  
|SQL_DESC_OCTET_LENGTH|4|  
|SQL_DESC_PRECISION|4|  
|SQL_DESC_SCALE|0|  
|SQL_DESC_SCHEMA_NAME|"" (chaîne vide)|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|  
|SQL_DESC_TABLE_NAME|"" (chaîne vide)|  
|SQL_DESC_TYPE|SQL_BINARY|  
|SQL_DESC_TYPE_NAME|"" (chaîne vide)|  
|SQL_DESC_UNNAMED|VALEUR SQL_UNNAMED|  
|SQL_DESC_UNSIGNED|SQL_FALSE|  
|SQL_DESC_UPDATEABLE|SQL_ATTR_READ_ONLY|  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Lorsqu’une application ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote appelle **SQLDescribeCol** avec la *ColumnNumber* argument a la valeur 0, le Gestionnaire de pilotes retourne les valeurs répertoriées dans le tableau suivant.  
  
|Buffer|Valeur|  
|------------|-----------|  
|ColumnName|"" (chaîne vide)|  
|* NameLengthPtr|0|  
|* DataTypePtr|SQL_BINARY|  
|* ColumnSizePtr|4|  
|* DecimalDigitsPtr|0|  
|* NullablePtr|SQL_NO_NULLS|  
  
### <a name="sqlgetdata"></a>SQLGetData  
 Lorsqu’une application ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote effectue l’appel suivant à **SQLGetData** pour récupérer un signet :  
  
```  
SQLGetData(StatementHandle, 0, SQL_C_VARBOOKMARK, TargetValuePtr, BufferLength, StrLen_or_IndPtr)  
```  
  
 l’appel est mappé à **SQLGetStmtOption** avec un *fOption* de SQL_GET_BOOKMARK, comme suit :  
  
```  
SQLGetStmtOption(hstmt, SQL_GET_BOOKMARK, TargetValuePtr)  
```  
  
 où *hstmt* et *pvParam* sont définies sur les valeurs de *au paramètre StatementHandle* et *TargetValuePtr*, respectivement. Le signet est retourné dans la mémoire tampon vers laquelle pointée le *pvParam* (*TargetValuePtr*) argument. La valeur dans la mémoire tampon vers laquelle pointe le *StrLen_or_IndPtr* argument dans l’appel à **SQLGetData** a la valeur 4.  
  
 Ce mappage est nécessaire pour prendre en compte pour le cas dans lequel **SQLFetch** a été appelé avant l’appel à **SQLGetData** et ODBC 2. *x* pilote ne prenait pas en charge **SQLExtendedFetch**. Dans ce cas, **SQLFetch** peuvent être transmis par le biais de l’API ODBC 2. *x* pilote, dans laquelle extraction de cas de signet n’est pas pris en charge.  
  
 **SQLGetData** ne peut pas être appelée plusieurs fois dans une API ODBC 2. *x* pilote de récupérer un signet dans les composants, par conséquent, l’appel **SQLGetData** avec la *BufferLength* argument défini à une valeur inférieure à 4 et la *ColumnNumber*argument a la valeur 0 retourne SQLSTATE HY090 (longueur de chaîne ou une mémoire tampon non valide). **SQLGetData** peut, cependant, être appelée plusieurs fois pour récupérer le même signet.  
  
### <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Lorsqu’une application ODBC 3. *x* application utilisant une API ODBC 2. *x* pilote appelle **SQLSetStmtAttr** pour définir l’attribut SQL_ATTR_USE_BOOKMARKS à SQL_UB_VARIABLE, le Gestionnaire de pilotes affecte l’attribut SQL_UB_ON dans sous-jacent ODBC 2. *x* pilote.
