---
title: SQLAllocHandle, fonction | Documents Microsoft
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
- SQLAllocHandle
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLAllocHandle
helpviewer_keywords:
- SQLAllocHandle function [ODBC]
ms.assetid: 6e7fe420-8cf4-4e72-8dad-212affaff317
caps.latest.revision: 43
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 22135dd429d9f1a42f4ee694b5730f38d636cb64
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlallochandle-function"></a>SQLAllocHandle, fonction
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLAllocHandle** alloue un handle d’environnement, connexion, instruction ou descripteur.  
  
> [!NOTE]  
>  Cette fonction est une fonction générique pour allouer des handles qui remplace les fonctions ODBC 2.0 **SQLAllocConnect**, **SQLAllocEnv**, et **SQLAllocStmt**. Pour autoriser les applications qui appellent **SQLAllocHandle** pour travailler avec ODBC 2. *x* pilotes, un appel à **SQLAllocHandle** est mappé dans le Gestionnaire de pilotes à **SQLAllocConnect**, **SQLAllocEnv**, ou **SQLAllocStmt**, le cas échéant. Pour plus d’informations, consultez « Commentaires ». Pour plus d’informations sur les le Gestionnaire de pilotes mappe cette fonction lorsqu’un ODBC 3. *x* application fonctionne avec une API ODBC 2. *x* pilote, consultez [mappage des fonctions de remplacement pour la compatibilité descendante des Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLAllocHandle(  
      SQLSMALLINT   HandleType,  
      SQLHANDLE     InputHandle,  
      SQLHANDLE *   OutputHandlePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 [Entrée] Le type de handle à être alloués par **SQLAllocHandle**. Doit être une des valeurs suivantes :  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 Handle SQL_HANDLE_DBC_INFO_TOKEN est utilisé uniquement par le Gestionnaire de pilotes et le pilote. Applications ne doivent pas utiliser ce type de handle. Pour plus d’informations sur SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement reconnaissance du Pool de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *InputHandle*  
 [Entrée] Le handle d’entrée dans le contexte duquel le nouveau handle doit être allouée. Si *HandleType* est SQL_HANDLE_ENV, il s’agit de SQL_NULL_HANDLE. Si *HandleType* est SQL_HANDLE_DBC, cela doit être un handle d’environnement, et si elle est SQL_HANDLE_STMT ou SQL_HANDLE_DESC, il doit être un handle de connexion.  
  
 *OutputHandlePtr*  
 [Sortie] Pointeur vers une mémoire tampon dans lequel retourner le handle à la structure de données nouvellement alloué.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_INVALID_HANDLE ou SQL_ERROR.  
  
 Lors de l’allocation d’un handle autre qu’un handle d’environnement, si **SQLAllocHandle** retourne SQL_ERROR, elle définit *OutputHandlePtr* SQL_NULL_HDBC, SQL_NULL_HSTMT ou SQL_NULL_HDESC, selon la valeur de *HandleType*, sauf si l’argument de sortie est un pointeur null. L’application peut obtenir ensuite des informations supplémentaires à partir de la structure de données de diagnostic associée au handle dans le *InputHandle* argument.  
  
## <a name="environment-handle-allocation-errors"></a>Erreurs d’Allocation de Handle d’environnement  
 Allocation de l’environnement se produit dans le Gestionnaire de pilotes et au sein de chaque pilote. L’erreur renvoyée par **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_ENV varie selon le niveau dans lequel l’erreur s’est produite.  
  
 Si le Gestionnaire de pilotes ne peut pas allouer de mémoire pour  *\*OutputHandlePtr* lorsque **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_ENV est appelée, ou l’application fournit un pointeur null pour *OutputHandlePtr*, **SQLAllocHandle** retourne SQL_ERROR. Le Gestionnaire de pilotes définit **OutputHandlePtr* à SQL_NULL_HENV (sauf si l’application fourni un pointeur null, ce qui retourne SQL_ERROR). Il n’existe aucun handle auquel associer les informations de diagnostic supplémentaires.  
  
 Le Gestionnaire de pilotes n’appelle pas la fonction d’allocation de handle d’environnement au niveau du pilote jusqu'à ce que l’application appelle **SQLConnect**, **SQLBrowseConnect**, ou **SQLDriverConnect**. Si une erreur se produit au niveau pilote **SQLAllocHandle** (fonction), puis le Gestionnaire de pilotes – niveau **SQLConnect**, **SQLBrowseConnect**, ou **SQLDriverConnect** fonction retourne SQL_ERROR. La structure de données de diagnostic contient SQLSTATE IM004 (le pilote **SQLAllocHandle** échec). L’erreur est retournée sur un handle de connexion.  
  
 Pour plus d’informations sur le flux des appels de fonction entre le Gestionnaire de pilote et un pilote, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLAllocHandle** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** le *HandleType* et *gérer* la valeur de *InputHandle*. SQL_SUCCESS_WITH_INFO (mais pas SQL_ERROR) peut être retourné pour le *OutputHandle* argument. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLAllocHandle** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08003|Connexion non ouverte|(DM) le *HandleType* argument SQL_HANDLE_STMT ou SQL_HANDLE_DESC était, mais la connexion spécifiée par la *InputHandle* argument n’était pas ouvert. Le processus de connexion doit être effectué (et la connexion doit être ouverte) pour le pilote à allouer une instruction ou un descripteur de handle.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans le **MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le Gestionnaire de pilotes (DM) n’a pas pu allouer de la mémoire pour le handle spécifié.<br /><br /> Le pilote n’a pas pu allouer de la mémoire pour le handle spécifié.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) le *OutputHandlePtr* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) le *HandleType* argument était SQL_HANDLE_DBC, et **SQLSetEnvAttr** n’a pas été appelé pour définir l’attribut d’environnement SQL_ODBC_VERSION.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le **InputHandle** et toujours en cours d’exécution lorsque le **SQLAllocHandle** fonction a été appelée avec **HandleType** défini à SQL_HANDLE_STMT ou SQL_HANDLE_DESC.|  
|HY013|Erreur de gestion de mémoire|Le *HandleType* argument était SQL_HANDLE_DBC, SQL_HANDLE_STMT ou SQL_HANDLE_DESC ; et l’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY014|Le nombre maximal de handles a été atteint|La limite définie par le pilote pour le nombre de handles qui peuvent être alloués pour le type de handle indiqué par le *HandleType* argument a été atteint.|  
|HY092|Identificateur d’attribut/option non valide|(DM) le *HandleType* argument n’était pas : SQL_HANDLE_ENV, SQL_HANDLE_DBC, SQL_HANDLE_STMT ou SQL_HANDLE_DESC.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le *HandleType* argument SQL_HANDLE_DESC et le pilote n’était un ODBC 2. *x* pilote.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le *HandleType* argument était SQL_HANDLE_STMT et le pilote n’a pas un pilote ODBC.<br /><br /> (DM) le *HandleType* argument a été SQL_HANDLE_DESC, et le pilote ne prend pas en charge l’allocation d’un handle de descripteur.|  
  
## <a name="comments"></a>Commentaires  
 **SQLAllocHandle** est utilisée pour allouer des handles pour les environnements, les connexions, les instructions et les descripteurs, comme décrit dans les sections suivantes. Pour obtenir des informations générales sur les handles, consultez [Handles](../../../odbc/reference/develop-app/handles.md).  
  
 Plus d’un handle d’environnement, de connexion ou de l’instruction peut être affecté à une application à la fois si plusieurs allocations sont pris en charge par le pilote. Dans ODBC, aucune limite n’est définie sur le nombre d’environnement, connexion, instruction ou les poignées de descripteur qui peuvent être allouées à tout moment. Pilotes peuvent imposer une limite sur le nombre d’un certain type de handle pouvant être allouée à la fois. Pour plus d’informations, consultez la documentation du pilote.  
  
 Si l’application appelle **SQLAllocHandle** avec  *\*OutputHandlePtr* définie pour un environnement, un connexion, une instruction ou un handle de descripteur qui existe déjà, le pilote remplace les informations associées à la *gérer*, sauf si l’application est à l’aide de connexion regroupement (voir « Allouer un environnement attribut pour le regroupement de connexions » plus loin dans cette section). Le Gestionnaire de pilotes ne vérifie pas si le *gérer* entré dans  *\*OutputHandlePtr* est déjà utilisé, ni aucune vérifier le contenu précédent du handle avant de les remplacer.  
  
> [!NOTE]  
>  Il s’agit de la programmation d’applications ODBC incorrecte pour appeler **SQLAllocHandle** deux fois avec la même variable d’application définie pour  *\*OutputHandlePtr* sans appeler **SQLFreeHandle** pour libérer le handle avant de réallouer. Remplacement ODBC gère de manière peut entraîner un comportement incohérent ou erreur de la part des pilotes ODBC.  
  
 Sur les systèmes d’exploitation qui prennent en charge de plusieurs threads, les applications peuvent utiliser le même handle d’environnement, connexion, instruction ou descripteur sur des threads différents. Les pilotes doivent prendre donc en charge un accès multithread-safe à ces informations ; Pour effectuer cette opération, par exemple, consiste à l’aide d’un sémaphore ou une section critique. Pour plus d’informations à propos du threading, consultez [Multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
 **SQLAllocHandle** ne définit pas l’attribut d’environnement SQL_ATTR_ODBC_VERSION lorsqu’elle est appelée pour allouer un handle d’environnement ; l’attribut de l’environnement doit être défini par l’application, ou SQLSTATE HY010 (erreur de séquence de fonction) sera retourné lorsque **SQLAllocHandle** est appelée pour allouer un handle de connexion.  
  
 Pour les applications conformes aux normes, **SQLAllocHandle** est mappé à **SQLAllocHandleStd** au moment de la compilation. La différence entre ces deux fonctions est que **SQLAllocHandleStd** définit l’attribut d’environnement SQL_ATTR_ODBC_VERSION à SQL_OV_ODBC3 lorsqu’elle est appelée avec le *HandleType* argument défini à SQL_HANDLE_ENV. Pour cela, car il est conforme aux normes applications est toujours ODBC 3. *x* applications. En outre, les normes ne nécessitent pas la version de l’application à inscrire. Ceci est la seule différence entre ces deux fonctions. Sinon, elles sont identiques. **SQLAllocHandleStd** est mappé à **SQLAllocHandle** dans le Gestionnaire de pilotes. Par conséquent, les pilotes tiers n’ont pas à implémenter **SQLAllocHandleStd**.  
  
 Les applications ODBC 3.8 doivent utiliser :  
  
-   **SQLAllocHandle et pas SQLAllocHandleStd** pour allouer un handle d’environnement.  
  
-   **SQLSetEnvAttr** à la valeur de l’attribut d’environnement SQL_ATTR_ODBC_VERSION SQL_OV_ODBC3_80.  
  
## <a name="allocating-an-environment-handle"></a>Allocation d'un handle d'environnement  
 Un handle d’environnement fournit l’accès aux informations globales telles que les handles de connexion valide et connexion active. Pour obtenir des informations générales sur les handles d’environnement, consultez [Handles d’environnement](../../../odbc/reference/develop-app/environment-handles.md).  
  
 Pour demander un handle d’environnement, une application appelle **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_ENV et un *InputHandle* de SQL_NULL_HANDLE. Le pilote alloue de la mémoire pour les informations sur l’environnement et les transmet la valeur du handle associé de nouveau la  *\*OutputHandlePtr* argument. L’application transmet le  *\*OutputHandle* valeur dans tous les appels qui requièrent un argument de handle d’environnement. Pour plus d’informations, consultez [allouer le Handle d’environnement](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 Sous le handle d’environnement d’un gestionnaire de pilotes, s’il existe handle d’environnement d’un pilote, puis **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_ENV n'est pas appelée dans ce pilote lorsqu’une connexion est établie uniquement **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_DBC. Si le handle d’environnement d’un pilote n’existe pas sous le handle d’environnement du Gestionnaire de pilotes, le SQLAllocHandle avec un HandleType égal à SQL_HANDLE_ENV et SQLAllocHandle avec un HandleType de SQL_HANDLE_DBC sont appelés dans le pilote lorsque le handle de la première connexion de l’environnement est connecté au pilote.  
  
 Lorsque le Gestionnaire de pilotes traite le **SQLAllocHandle** fonctionne avec un *HandleType* de SQL_HANDLE_ENV, il vérifie le **Trace** mot clé dans la section [ODBC] des informations système. Si elle est définie sur 1, le Gestionnaire de pilotes permet le suivi de l’application actuelle. Si l’indicateur de trace est définie, le suivi commence lorsque le handle d’environnement est alloué et se termine lorsque le dernier handle d’environnement est libéré. Pour plus d’informations, consultez [configuration des Sources de données](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Après avoir alloué un handle d’environnement, une application doit appeler **SQLSetEnvAttr** sur le handle d’environnement pour définir l’attribut d’environnement SQL_ATTR_ODBC_VERSION. Si cet attribut n’est pas défini avant **SQLAllocHandle** est appelée pour allouer un handle de connexion sur l’environnement, l’appel à allouer la connexion retourne SQLSTATE HY010 (erreur de séquence de fonction). Pour plus d’informations, consultez [déclarant ODBC Version l’Application](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md).  
  
## <a name="allocating-shared-environments-for-connection-pooling"></a>Allocation des environnements partagés pour le regroupement de connexions  
 Les environnements peuvent être partagées entre plusieurs composants sur un seul processus. Un environnement partagé peut être utilisé par plusieurs composants en même temps. Lorsqu’un composant utilise un environnement partagé, il peut utiliser des connexions regroupées, ce qui lui permettent d’allouer et d’utiliser une connexion existante sans recréer sur cette connexion.  
  
 Avant de l’allocation d’un environnement partagé qui peut être utilisé pour le regroupement de connexions, une application doit appeler **SQLSetEnvAttr** pour définir l’attribut d’environnement SQL_ATTR_CONNECTION_POOLING SQL_CP_ONE_PER_DRIVER ou SQL_CP_ONE_PER_HENV. **SQLSetEnvAttr** dans ce cas est appelée avec *EnvironmentHandle* la valeur null, ce qui rend l’attribut à un attribut au niveau du processus.  
  
 Une fois que le regroupement de connexions a été activé, une application appelle **SQLAllocHandle** avec la *HandleType* argument défini à SQL_HANDLE_ENV. L’environnement allouée par cet appel sera un environnement partagé implicit, car le regroupement de connexions a été activé.  
  
 Quand un environnement partagé est alloué, l’environnement qui sera utilisé n’est pas déterminé jusqu'à **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_DBC est appelée. À ce stade, le Gestionnaire de pilotes tente de trouver un environnement existant qui correspond aux attributs d’environnement demandés par l’application. Si aucun environnement n’existe, est créé comme un environnement partagé. Le Gestionnaire de pilotes conserve un décompte de références pour chaque environnement partagé ; le nombre est défini à 1 lors de la création de l’environnement. Si un environnement correspondant est trouvé, le handle de cet environnement est retourné à l’application et le nombre de références est incrémenté. Un handle d’environnement alloué de cette manière peut être utilisé dans toute fonction ODBC qui accepte un handle d’environnement comme argument d’entrée.  
  
## <a name="allocating-a-connection-handle"></a>Allocation d'un handle de connexion  
 Un handle de connexion permet d’accéder à des informations telles que l’instruction valide et ouvrir des descripteurs de descripteur sur la connexion et indique si une transaction est actuellement. Pour obtenir des informations générales sur les handles de connexion, consultez [Handles de connexion](../../../odbc/reference/develop-app/connection-handles.md).  
  
 Pour demander un handle de connexion, une application appelle **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_DBC. Le *InputHandle* est défini pour le handle d’environnement qui a été retourné par l’appel à **SQLAllocHandle** attribuée ce handle. Le pilote alloue de la mémoire pour les informations de connexion et la passe de nouveau la valeur du handle associé  *\*OutputHandlePtr*. L’application transmet le  *\*OutputHandlePtr* valeur dans tous les appels qui requièrent un handle de connexion. Pour plus d’informations, consultez [allouer un Handle de connexion](../../../odbc/reference/develop-app/allocating-a-connection-handle-odbc.md).  
  
 Le Gestionnaire de pilotes traite le **SQLAllocHandle** la fonction et appelle du pilote **SQLAllocHandle** fonctionner lorsque l’application appelle **SQLConnect**, **SQLBrowseConnect**, ou **SQLDriverConnect**. (Pour plus d’informations, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).)  
  
 Si l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’est pas définie avant **SQLAllocHandle** est appelée pour allouer un handle de connexion sur l’environnement, l’appel à allouer la connexion retourne SQLSTATE HY010 (erreur de séquence de fonction).  
  
 Lorsqu’une application appelle **SQLAllocHandle** avec la *InputHandle* argument ayant pour valeur SQL_HANDLE_DBC et également définie à un handle d’environnement partagé, le Gestionnaire de pilotes tente de trouver une partagée existante environnement qui correspond aux attributs de l’environnement défini par l’application. Si aucun environnement n’existe, est créé, avec un nombre de référence (géré par le Gestionnaire de pilotes) de 1. Si une correspondance partagé environnement est trouvé, ce handle est retourné à l’application et son décompte de références est incrémenté.  
  
 La connexion réelle qui sera utilisée n’est pas déterminée par le Gestionnaire de pilotes en tant que **SQLConnect** ou **SQLDriverConnect** est appelée. Le Gestionnaire de pilote utilise les options de connexion dans l’appel à **SQLConnect** (ou les mots clés de connexion dans l’appel à **SQLDriverConnect**) et les attributs de connexion définie après l’allocation de connexion pour déterminer quelle connexion dans le pool doit être utilisée. Pour plus d’informations, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="allocating-a-statement-handle"></a>Allocation d'un descripteur d'instruction  
 Un descripteur d’instruction permet d’accéder aux informations de déclaration, telles que les messages d’erreur, le nom du curseur et les informations d’état pour le traitement des instructions SQL. Pour obtenir des informations générales sur les descripteurs d’instruction, consultez [descripteurs d’instruction](../../../odbc/reference/develop-app/statement-handles.md).  
  
 Pour demander un descripteur d’instruction, une application se connecte à une source de données, puis appelle **SQLAllocHandle** avant d’envoyer des instructions SQL. Dans cet appel, *HandleType* doit être défini à SQL_HANDLE_STMT et *InputHandle* doit être défini sur le handle de connexion qui a été retourné par l’appel à **SQLAllocHandle** attribuée ce handle. Le pilote alloue de la mémoire pour les informations de l’instruction, associe le handle d’instruction avec la connexion spécifiée et passe de nouveau la valeur du handle associé  *\*OutputHandlePtr*. L’application transmet le  *\*OutputHandlePtr* valeur dans tous les appels qui requièrent un descripteur d’instruction. Pour plus d’informations, consultez [allouer un Handle d’instruction](../../../odbc/reference/develop-app/allocating-a-statement-handle-odbc.md).  
  
 Quand le handle d’instruction est alloué, le pilote alloue un ensemble de descripteurs et assigne les poignées pour ces descripteurs pour les attributs d’instruction SQL_ATTR_APP_ROW_DESC, SQL_ATTR_APP_PARAM_DESC, SQL_ATTR_IMP_ROW_DESC et SQL_ATTR_IMP_PARAM_DESC automatiquement. Ils sont désignés en tant que *implicitement* allouée descripteurs. Pour allouer un descripteur d’application explicitement, consultez la section suivante, « Allouer un Handle de descripteur. »  
  
## <a name="allocating-a-descriptor-handle"></a>Allouer un Handle de descripteur  
 Lorsqu’une application appelle **SQLAllocHandle** avec un *HandleType* de SQL_HANDLE_DESC, le pilote alloue un descripteur de l’application. Ils sont désignés en tant que *explicitement* allouée descripteurs. L’application demande un pilote à utiliser un descripteur de l’application explicitement alloué à la place un alloué automatiquement pour un descripteur d’instruction donné en appelant le **SQLSetStmtAttr** fonction avec l’attribut SQL_ATTR_APP_ROW_DESC ou SQL_ATTR_APP_PARAM_DESC. Impossible d’allouer un descripteur d’implémentation explicitement, ni si un descripteur d’implémentation peut être spécifié dans une **SQLSetStmtAttr** l’appel de fonction.  
  
 Descripteurs explicitement alloués sont associés à un handle de connexion au lieu d’un descripteur d’instruction (comme les descripteurs alloués automatiquement). Descripteurs demeurent allouées uniquement quand une application est réellement connectée à la base de données. Descripteurs explicitement alloués étant associés à un handle de connexion, une application peut associer un descripteur explicitement alloué plusieurs instructions dans une connexion. Un descripteur d’application implicitement alloué, en revanche, ne peut pas être associé à plus d’un descripteur d’instruction. (Il ne peut pas être associé à un descripteur d’instruction autre que celui qui il a été allouée à.) Handles de descripteur alloué de manière explicite peuvent être libérées explicitement par l’application ou en appelant **SQLFreeHandle** avec un *HandleType* de SQL_HANDLE_DESC, ou implicitement lors de la connexion est fermée.  
  
 Lorsque le descripteur explicitement alloué est libéré, le descripteur implicitement alloué est à nouveau associé à l’instruction. (L’attribut SQL_ATTR_APP_ROW_DESC ou SQL_ATTR_APP_PARAM_DESC pour cette instruction est défini à nouveau pour le handle de descripteur alloué de manière implicite). Cela est vrai pour toutes les instructions qui ont été associées dans le descripteur alloué de manière explicite sur la connexion.  
  
 Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md), [fonction SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md), [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md), et [fonction SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|La libération d’un handle d’environnement, connexion, instruction ou descripteur|[SQLFreeHandle, fonction](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Préparation de l’exécution d’une instruction|[SQLPrepare, fonction](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Définition d’un attribut de connexion|[SQLSetConnectAttr, fonction](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un champ de descripteur|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Définition d’un attribut d’environnement|[SQLSetEnvAttr, fonction](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Définition d’un attribut d’instruction|[SQLSetStmtAttr, fonction](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
