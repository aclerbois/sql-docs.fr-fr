---
title: Exécution asynchrone (méthode de Notification) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e509dad9-5263-4a10-9a4e-03b84b66b6b3
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5c2504962a81d9e9e3a5ac8bd4f57f3fe74f9441
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="asynchronous-execution-notification-method"></a>Exécution asynchrone (méthode de notification)
ODBC permet l’exécution asynchrone de connexion et les opérations de l’instruction. Un thread d’application peut appeler une fonction ODBC en mode asynchrone et la fonction peut retourner avant que l’opération est terminée, en autorisant le thread d’application effectuer d’autres tâches. Dans le SDK Windows 7, pour les opérations de connexion, ou l’instruction asynchrone, une application déterminé que l’opération asynchrone a été terminée à l’aide de la méthode d’interrogation. Pour plus d’informations, consultez [exécution asynchrone (méthode d’interrogation)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md). À compter dans le SDK Windows 8, vous pouvez déterminer qu’une opération asynchrone est terminée à l’aide de la méthode de notification.  
  
 Dans la méthode d’interrogation, les applications doivent appeler la fonction asynchrone chaque fois qu’il souhaite que l’état de l’opération. La méthode de notification est similaire à rappel et l’attente dans ADO.NET. ODBC, utilise toutefois, les événements Win32 en tant que l’objet de notification.  
  
 La bibliothèque de curseurs ODBC et la notification asynchrone ODBC ne peut pas être utilisés en même temps. Si les deux attributs retournera une erreur avec SQLSTATE S1119 (bibliothèque de curseurs et Notification asynchrone ne peut pas être activés en même temps).  
  
 Consultez [Notification de fin de fonction asynchrone](../../../odbc/reference/develop-driver/notification-of-asynchronous-function-completion.md) pour plus d’informations pour les développeurs de pilote.  
  
> [!NOTE]  
>  La méthode de notification n’est pas pris en charge avec la bibliothèque de curseurs. Une application message d’erreur si elle tente d’activer la bibliothèque de curseurs via SQLSetConnectAttr, lorsque la méthode de notification est activée.  
  
## <a name="overview"></a>Vue d'ensemble  
 Lorsqu’une fonction ODBC est appelée en mode asynchrone, le contrôle est retourné à l’application appelante immédiatement avec le code de retour SQL_STILL_EXECUTING. L’application doit interroger à plusieurs reprises de la fonction jusqu'à ce qu’elle retourne différent de SQL_STILL_EXECUTING. La boucle d’interrogation augmente l’utilisation du processeur, à l’origine de faibles performances dans de nombreux scénarios asynchrones.  
  
 Chaque fois que le modèle de notification est utilisé, le modèle d’interrogation est désactivé. Les applications ne doivent pas appeler la fonction d’origine à nouveau. Appelez [SQLCompleteAsync fonction](../../../odbc/reference/syntax/sqlcompleteasync-function.md) pour terminer l’opération asynchrone. Si une application appelle la fonction d’origine à nouveau avant l’opération asynchrone est terminée, l’appel retourne SQL_ERROR avec SQLSTATE IM017 (interrogation est désactivée en Mode de Notification asynchrone).  
  
 Lorsque vous utilisez le modèle de notification, l’application peut appeler **SQLCancel** ou **SQLCancelHandle** pour annuler une opération de connexion ou d’instruction. Si la demande d’annulation a réussi, ODBC retourne SQL_SUCCESS. Ce message n’indique pas que la fonction a été réellement annulée ; Il indique que la demande d’annulation a été traitée. Si la fonction est annulée de réellement est dépendant du pilote et la source de données. Lorsqu’une opération est annulée, le Gestionnaire de pilotes signale toujours l’événement. Le Gestionnaire de pilotes retourne SQL_ERROR dans la mémoire tampon de code de retour et de l’état est SQLSTATE HY008 (opération annulée) pour indiquer l’annulation est terminée. Si la fonction terminé son traitement normal, le Gestionnaire de pilotes retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO.  
  
### <a name="downlevel-behavior"></a>Comportement de niveau inférieur  
 La version du Gestionnaire de pilotes ODBC prenant en charge cette notification sur complète est 3.81 d’ODBC.  
  
|Version de l’application ODBC|Version du Gestionnaire de pilotes|Version du pilote|Comportement|  
|------------------------------|----------------------------|--------------------|--------------|  
|Nouvelle application de n’importe quelle version d’ODBC|ODBC 3.81|ODBC 3,80 pilote|Application peut utiliser cette fonctionnalité si le pilote prend en charge cette fonctionnalité, le Gestionnaire de pilotes provoque une erreur, dans le cas contraire.|  
|Nouvelle application de n’importe quelle version d’ODBC|ODBC 3.81|Pilote ODBC de Pre 3,80|Le Gestionnaire de pilotes provoque une erreur, si le pilote ne prend pas en charge cette fonctionnalité.|  
|Nouvelle application de n’importe quelle version d’ODBC|Pre-ODBC 3.81|Tout|Lorsque l’application utilise cette fonctionnalité, un gestionnaire de pilotes ancien considère les nouveaux attributs en tant qu’attributs spécifiques du pilote et le pilote doit d’erreur. Un nouveau gestionnaire de pilote ne passera pas ces attributs pour le pilote.|  
  
 Une application doit vérifier la version du Gestionnaire de pilotes avant d’utiliser cette fonctionnalité. Sinon, si un pilote mal écrit pas d’erreur et la version du Gestionnaire de pilotes est antérieur 3.81 d’ODBC, le comportement est indéfini.  
  
## <a name="use-cases"></a>Cas d'usage  
 Cette section présente des exemples d’utilisation pour l’exécution asynchrone et le mécanisme d’interrogation.  
  
### <a name="integrate-data-from-multiple-odbc-sources"></a>Intégrer des données provenant de plusieurs Sources ODBC  
 En mode asynchrone, une application d’intégration de données extrait les données à partir de plusieurs sources de données. Certaines des données à partir de sources de données distantes et certaines données proviennent de fichiers locaux. L’application ne peut pas continuer jusqu'à ce que les opérations asynchrones sont terminées.  
  
 Au lieu de reprises une opération pour déterminer si elle est terminée, l’application peut créer un objet d’événement et l’associer à un handle de connexion ODBC ou un handle d’instruction ODBC. L’application appelle ensuite API à attendre plusieurs objets d’événements (événements ODBC et autres événements de Windows) ou d’objet d’un événement de synchronisation de système d’exploitation. ODBC signale l’objet d’événement lorsque l’opération asynchrone ODBC correspondante est terminée.  
  
 Sous Windows, les objets d’événement Win32 seront utilisés et qui fournira l’utilisateur un modèle de programmation unifié. Les responsables de pilote sur d’autres plateformes peuvent utiliser l’implémentation d’objet événement spécifique pour ces plateformes.  
  
 L’exemple de code suivant illustre l’utilisation de connexion et de la notification asynchrone d’instruction :  
  
```  
// This function opens NUMBER_OPERATIONS connections and executes one query on statement of each connection.  
// Asynchronous Notification is used  
  
#define NUMBER_OPERATIONS 5  
int AsyncNotificationSample(void)  
{  
    RETCODE     rc;  
  
    SQLHENV     hEnv              = NULL;  
    SQLHDBC     arhDbc[NUMBER_OPERATIONS]         = {NULL};  
    SQLHSTMT    arhStmt[NUMBER_OPERATIONS]        = {NULL};  
  
    HANDLE      arhDBCEvent[NUMBER_OPERATIONS]    = {NULL};  
    RETCODE     arrcDBC[NUMBER_OPERATIONS]        = {0};  
    HANDLE      arhSTMTEvent[NUMBER_OPERATIONS]   = {NULL};  
    RETCODE     arrcSTMT[NUMBER_OPERATIONS]       = {0};  
  
    rc = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &hEnv);  
    if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
  
    rc = SQLSetEnvAttr(hEnv,  
        SQL_ATTR_ODBC_VERSION,  
        (SQLPOINTER) SQL_OV_ODBC3_80,  
        SQL_IS_INTEGER);  
    if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
  
    // Connection operations begin here  
  
    // Alloc NUMBER_OPERATIONS connection handles  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        rc = SQLAllocHandle(SQL_HANDLE_DBC, hEnv, &arhDbc[i]);  
        if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
    }  
  
    // Enable DBC Async on all connection handles  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        rc= SQLSetConnectAttr(arhDbc[i], SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE, (SQLPOINTER)SQL_ASYNC_DBC_ENABLE_ON, SQL_IS_INTEGER);  
        if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
    }  
  
    // Application must create event objects  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        arhDBCEvent[i] = CreateEvent(NULL, FALSE, FALSE, NULL); // Auto-reset, initial state is not-signaled  
        if (!arhDBCEvent[i]) goto Cleanup;  
    }  
  
    // Enable notification on all connection handles  
    // Event  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        rc= SQLSetConnectAttr(arhDbc[i], SQL_ATTR_ASYNC_DBC_EVENT, arhDBCEvent[i], SQL_IS_POINTER);  
        if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
    }  
  
    // Initiate connect establishing  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        SQLDriverConnect(arhDbc[i], NULL, (SQLTCHAR*)TEXT("Driver={ODBC Driver 11 for SQL Server};SERVER=dp-srv-sql2k;DATABASE=pubs;UID=sa;PWD=XYZ;"), SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);  
    }  
  
    // Can do some other staff before calling WaitForMultipleObjects  
    WaitForMultipleObjects(NUMBER_OPERATIONS, arhDBCEvent, TRUE, INFINITE); // Wait All  
  
    // Complete connect API calls  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        SQLCompleteAsync(SQL_HANDLE_DBC, arhDbc[i], & arrcDBC[i]);  
    }  
  
    BOOL fFail = FALSE; // Whether some connection openning fails.  
  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        if ( !SQL_SUCCEEDED(arrcDBC[i]) )   
            fFail = TRUE;  
    }  
  
    // If some SQLDriverConnect() fail, clean up.  
    if (fFail)  
    {  
        for (int i=0; i<NUMBER_OPERATIONS; i++)  
        {  
            if (SQL_SUCCEEDED(arrcDBC[i]) )   
            {  
                SQLDisconnect(arhDbc[i]); // This is also async  
            }  
            else  
            {  
                SetEvent(arhDBCEvent[i]); // Previous SQLDriverConnect() failed. No need to call SQLDisconnect().  
            }  
        }  
        WaitForMultipleObjects(NUMBER_OPERATIONS, arhDBCEvent, TRUE, INFINITE);   
        for (int i=0; i<NUMBER_OPERATIONS; i++)  
        {  
            if (SQL_SUCCEEDED(arrcDBC[i]) )   
            {     
                SQLCompleteAsync(SQL_HANDLE_DBC, arhDbc[i], &arrcDBC[i]);; // To Complete  
            }  
        }  
  
        goto Cleanup;  
    }  
  
    // Statement Operations begin here  
  
    // Alloc statement handle  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        rc = SQLAllocHandle(SQL_HANDLE_STMT, arhDbc[i], &arhStmt[i]);  
        if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
    }  
  
    // Enable STMT Async on all statement handles  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        rc = SQLSetStmtAttr(arhStmt[i], SQL_ATTR_ASYNC_ENABLE, (SQLPOINTER)SQL_ASYNC_ENABLE_ON, SQL_IS_INTEGER);  
        if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
    }  
  
    // Create event objects  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        arhSTMTEvent[i] = CreateEvent(NULL, FALSE, FALSE, NULL); // Auto-reset, initial state is not-signaled  
        if (!arhSTMTEvent[i]) goto Cleanup;  
    }  
  
    // Enable notification on all statement handles  
    // Event  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        rc= SQLSetStmtAttr(arhStmt[i], SQL_ATTR_ASYNC_STMT_EVENT, arhSTMTEvent[i], SQL_IS_POINTER);  
        if ( !SQL_SUCCEEDED(rc) ) goto Cleanup;  
    }  
  
    // Initiate SQLExecDirect() calls  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        SQLExecDirect(arhStmt[i], (SQLTCHAR*)TEXT("select au_lname, au_fname from authors"), SQL_NTS);  
    }  
  
    // Can do some other staff before calling WaitForMultipleObjects  
    WaitForMultipleObjects(NUMBER_OPERATIONS, arhSTMTEvent, TRUE, INFINITE); // Wait All  
  
    // Now, call SQLCompleteAsync to complete the operation and get return code  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        SQLCompleteAsync(SQL_HANDLE_STMT, arhStmt[i], &arrcSTMT[i]);  
    }  
  
    // Check return values  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        if ( !SQL_SUCCEEDED(arrcSTMT[i]) ) goto Cleanup;  
    }  
  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        //Do some binding jobs here, set SQL_ATTR_ROW_ARRAY_SIZE   
    }  
  
    // Now, initiate fetching  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        SQLFetch(arhStmt[i]);  
    }  
  
    // Can do some other staff before calling WaitForMultipleObjects  
    WaitForMultipleObjects(NUMBER_OPERATIONS, arhSTMTEvent, TRUE, INFINITE);   
  
    // Now, to complete the operations and get return code  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        SQLCompleteAsync(SQL_HANDLE_STMT, arhStmt[i], &arrcSTMT[i]);  
    }  
  
    // Check return code  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        if ( !SQL_SUCCEEDED(arrcSTMT[i]) ) goto Cleanup;  
    }  
  
    // USE fetched data here!!  
  
Cleanup:  
  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        if (arhStmt[NUMBER_OPERATIONS])  
        {  
            SQLFreeHandle(SQL_HANDLE_STMT, arhStmt[i]);  
            arhStmt[i] = NULL;  
        }  
    }  
  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        if (arhSTMTEvent[i])  
        {  
            CloseHandle(arhSTMTEvent[i]);  
            arhSTMTEvent[i] = NULL;  
        }  
    }  
  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        if (arhDbc[i])  
        {  
            SQLFreeHandle(SQL_HANDLE_DBC, arhDbc[i]);  
            arhDbc[i] = NULL;  
        }  
    }  
  
    for (int i=0; i<NUMBER_OPERATIONS; i++)  
    {  
        if (arhDBCEvent[i])  
        {  
            CloseHandle(arhDBCEvent[i]);  
            arhDBCEvent[i] = NULL;  
        }  
    }  
  
    if (hEnv)  
    {  
        SQLFreeHandle(SQL_HANDLE_ENV, hEnv);  
        hEnv = NULL;  
    }  
  
    return 0;  
}  
  
```  
  
### <a name="determining-if-a-driver-supports-asynchronous-notification"></a>Déterminer si un pilote prend en charge la Notification asynchrone  
 Une application ODBC peut déterminer si un pilote ODBC prend en charge la notification asynchrone en appelant [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md). Le Gestionnaire de pilotes ODBC appelle en conséquence le **SQLGetInfo** du pilote avec SQL_ASYNC_NOTIFICATION.  
  
```  
SQLUINTEGER InfoValue;  
SQLLEN      cbInfoLength;  
  
SQLRETURN retcode;  
retcode = SQLGetInfo (hDbc,   
                      SQL_ASYNC_NOTIFICATION,   
                      &InfoValue,  
                      sizeof(InfoValue),  
                      NULL);  
if (SQL_SUCCEEDED(retcode))  
{  
if (SQL_ASYNC_NOTIFICATION_CAPABLE == InfoValue)  
      {  
          // The driver supports asynchronous notification  
      }  
      else if (SQL_ASYNC_NOTIFICATION_NOT_CAPABLE == InfoValue)  
      {  
          // The driver does not support asynchronous notification  
      }  
}  
```  
  
### <a name="associating-a-win32-event-handle-with-an-odbc-handle"></a>Association d’un Handle d’événement Win32 avec un descripteur ODBC  
 Les applications sont tenues de création d’objets d’événement Win32 à l’aide de fonctions Win32 correspondantes. Une application peut associer un handle d’événement Win32 un handle de connexion ODBC ou un handle d’instruction ODBC.  
  
 Les attributs de connexion SQL_ATTR_ASYNC_DBC_FUNCTION_ENABLE et SQL_ATTR_ASYNC_DBC_EVENT déterminent que ODBC s’exécute en mode asynchrone et si ODBC permet le mode de notification pour un handle de connexion. Les attributs d’instruction SQL_ATTR_ASYNC_ENABLE et SQL_ATTR_ASYNC_STMT_EVENT déterminent que ODBC s’exécute en mode asynchrone et si ODBC permet le mode de notification pour un descripteur d’instruction.  
  
|SQL_ATTR_ASYNC_ENABLE ou SQL_ATTR_ASYNC_DBC_FUNCTION_ENABLE|SQL_ATTR_ASYNC_STMT_EVENT ou SQL_ATTR_ASYNC_DBC_EVENT|Mode|  
|-------------------------------------------------------------------------|-------------------------------------------------------------------|----------|  
|Activer|non null|Notification asynchrone|  
|Activer|Null|Interrogation asynchrone|  
|Disable|n'importe laquelle|Synchrone|  
  
 Une application peut désactiver temporairement en mode d’opération asynchrone. ODBC ignore les valeurs de SQL_ATTR_ASYNC_DBC_EVENT si l’opération asynchrone au niveau de connexion est désactivée. ODBC ignore les valeurs de SQL_ATTR_ASYNC_STMT_EVENT si l’opération asynchrone d’instruction au niveau est désactivée.  
  
 Appel synchrone de **SQLSetStmtAttr** et **SQLSetConnectAttr**  
 -   **SQLSetConnectAttr** prend en charge les opérations asynchrones, mais l’appel de **SQLSetConnectAttr** à définir SQL_ATTR_ASYNC_DBC_EVENT est toujours synchrone.  
  
-   **SQLSetStmtAttr** ne prend pas en charge l’exécution asynchrone.  
  
 Scénario de sortie d’erreur  
 Lorsque **SQLSetConnectAttr** est appelé avant d’effectuer une connexion, le Gestionnaire de pilotes ne peut pas déterminer le pilote à utiliser. Par conséquent, le Gestionnaire de pilotes retourne la réussite de **SQLSetConnectAttr** , mais l’attribut ne peut pas être prêt à définir dans le pilote. Le Gestionnaire de pilotes définit ces attributs lorsque l’application appelle une fonction de la connexion. Le Gestionnaire de pilotes risque d’erreur à la sortie, car le pilote ne prend pas en charge les opérations asynchrones.  
  
 Héritage des attributs de connexion  
 En règle générale, les instructions d’une connexion hérite les attributs de connexion. Toutefois, l’attribut SQL_ATTR_ASYNC_DBC_EVENT ne peut pas être héritée et affecte uniquement les opérations de connexion.  
  
 Pour associer un gestionnaire d’événements à un handle de connexion ODBC, une application ODBC appelle des API ODBC **SQLSetConnectAttr** et spécifie SQL_ATTR_ASYNC_DBC_EVENT comme l’attribut et l’événement de gérer en tant que la valeur d’attribut. Le nouvel attribut ODBC SQL_ATTR_ASYNC_DBC_EVENT est de type SQL_IS_POINTER.  
  
```  
HANDLE hEvent;  
hEvent = CreateEvent(   
            NULL,                // default security attributes  
            FALSE,               // auto-reset event  
            FALSE,               // initial state is non-signaled  
            NULL                 // no name  
            );  
```  
  
 En règle générale, les applications créent des objets de l’événement de réinitialisation automatique. ODBC ne réinitialise pas l’objet d’événement. Applications doivent vous assurer que l’objet n’est pas dans l’état signalé avant d’appeler toute fonction ODBC asynchrone.  
  
```  
SQLRETURN retcode;  
retcode = SQLSetConnectAttr ( hDBC,  
                              SQL_ATTR_ASYNC_DBC_EVENT, // Attribute name  
                              (SQLPOINTER) hEvent,      // Win32 Event handle  
                              SQL_IS_POINTER);          // Length Indicator  
```  
  
 SQL_ATTR_ASYNC_DBC_EVENT est un attribut uniquement par le Gestionnaire de pilotes qui ne sera pas défini dans le pilote.  
  
 La valeur par défaut de SQL_ATTR_ASYNC_DBC_EVENT est NULL. Si le pilote ne prend pas en charge la notification asynchrone, obtenir ou définir SQL_ATTR_ASYNC_DBC_EVENT retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
 Si la dernière valeur SQL_ATTR_ASYNC_DBC_EVENT définie sur un handle de connexion ODBC n’est pas NULL et l’application activée en mode asynchrone en définissant l’attribut SQL_ATTR_ASYNC_DBC_FUNCTION_ENABLE avec SQL_ASYNC_DBC_ENABLE_ON, en appelant une fonction de connexion ODBC qui prend en charge le mode asynchrone recevez une notification de fin. Si la dernière valeur SQL_ATTR_ASYNC_DBC_EVENT définie sur un handle de connexion ODBC est NULL, ODBC n’envoie pas l’application toute notification, quel que soit si le mode asynchrone est activé.  
  
 Une application peut définir SQL_ATTR_ASYNC_DBC_EVENT avant ou après avoir défini l’attribut SQL_ATTR_ASYNC_DBC_FUNCTION_ENABLE.  
  
 Les applications peuvent définir l’attribut SQL_ATTR_ASYNC_DBC_EVENT sur un handle de connexion ODBC avant d’appeler une fonction de connexion (**SQLConnect**, **SQLBrowseConnect**, ou **SQLDriverConnect**). Étant donné que le Gestionnaire de pilotes ODBC ne connaît pas selon l’application utilise le pilote ODBC, elle retournera SQL_SUCCESS. Lorsque l’application appelle une fonction de connexion, le Gestionnaire de pilotes ODBC vérifie si le pilote prend en charge la notification asynchrone. Si le pilote ne prend pas en charge la notification asynchrone, le Gestionnaire de pilotes ODBC retourne SQL_ERROR avec SQLSTATE S1_118 (pilote ne prend pas en charge la notification asynchrone). Si le pilote prend en charge la notification asynchrone, le Gestionnaire de pilotes ODBC sera appeler le pilote et définir les attributs correspondants SQL_ATTR_ASYNC_DBC_NOTIFICATION_CALLBACK et SQL_ATTR_ASYNC_DBC_NOTIFICATION_CONTEXT.  
  
 De même, une application appelle **SQLSetStmtAttr** sur une instruction ODBC gèrent et spécifie l’attribut SQL_ATTR_ASYNC_STMT_EVENT pour activer ou désactiver la notification asynchrone au niveau instruction. Car une fonction de l’instruction est toujours appelée une fois la connexion est établie, **SQLSetStmtAttr** retourne SQL_ERROR avec SQLSTATE S1_118 (pilote ne prend pas en charge la notification asynchrone) immédiatement si le pilote correspondant ne prend pas en charge les opérations asynchrones ou le pilote prend en charge une opération asynchrone, mais ne prend pas en charge la notification asynchrone.  
  
```  
SQLRETURN retcode;  
retcode = SQLSetStmtAttr ( hSTMT,  
                           SQL_ATTR_ASYNC_STMT_EVENT, // Attribute name   
                           (SQLPOINTER) hEvent,       // Win32 Event handle  
                           SQL_IS_POINTER);           // length Indicator  
```  
  
 SQL_ATTR_ASYNC_STMT_EVENT, ce qui peut être défini avec la valeur NULL, est un attribut uniquement par le Gestionnaire de pilotes qui ne sera pas défini dans le pilote.  
  
 La valeur par défaut de SQL_ATTR_ASYNC_STMT_EVENT est NULL. Si le pilote ne prend pas en charge la notification asynchrone, l’obtention ou la définition de l’attribut SQL_ATTR_ASYNC_ STMT_EVENT retourne SQL_ERROR avec SQLSTATE HY092 (identificateur d’attribut/option non valide).  
  
 Une application ne doit pas associer le même handle d’événement avec plusieurs handles ODBC. Sinon, une notification seront perdue si deux appels de fonction ODBC asynchrones se termine sur deux poignées qui partagent le même handle d’événement. Pour éviter un descripteur d’instruction qui héritent de la même handle d’événement à partir du handle de connexion, ODBC retourne SQL_ERROR avec SQLSTATE les IM016 (Impossible de définir attribut d’instruction dans le handle de connexion) si une application définit SQL_ATTR_ASYNC_STMT_EVENT sur un handle de connexion.  
  
### <a name="calling-asynchronous-odbc-functions"></a>Appel de fonctions ODBC asynchrone  
 Après l’activation de la notification asynchrone et le démarrage d’une opération asynchrone, l’application peut appeler n’importe quelle fonction ODBC. Si la fonction appartient à l’ensemble de fonctions qui prennent en charge une opération asynchrone, l’application obtiendra une notification de fin lors de l’opération terminée, indépendamment de si la fonction a échoué ou a été créée.  La seule exception est que l’application appelle une fonction ODBC avec un handle de connexion ou une instruction non valide. Dans ce cas, ODBC ne sera pas obtenir le handle d’événement et définissez-le sur l’état signalé.  
  
 L’application doit garantir que l’objet d’événement associé est dans un état non signalé avant de commencer une opération asynchrone sur le handle ODBC correspondant. ODBC ne réinitialise pas l’objet d’événement.  
  
### <a name="getting-notification-from-odbc"></a>Mise en route de la Notification à partir d’ODBC  
 Un thread d’application peut appeler **WaitForSingleObject** d’attente sur le handle d’un événement ou appel **WaitForMultipleObjects** pour attendre un tableau de handles d’événement et être suspendu jusqu'à ce qu’un ou tous les objets d’événement soit signalé ou que l’intervalle de délai d’attente est écoulé.  
  
```  
DWORD dwStatus = WaitForSingleObject(  
                        hEvent,  // The event associated with the ODBC handle  
                        5000     // timeout is 5000 millisecond   
);  
  
If (dwStatus == WAIT_TIMEOUT)  
{  
    // time-out interval elapsed before all the events are signaled.   
}  
Else  
{  
    // Call the corresponding Asynchronous ODBC API to complete all processing and retrieve the return code.  
}  
```
