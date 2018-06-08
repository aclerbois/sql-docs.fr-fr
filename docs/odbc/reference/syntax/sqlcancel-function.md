---
title: Sqlcancel, fonction | Documents Microsoft
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
- SQLCancel
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCancel
helpviewer_keywords:
- SQLCancel function [ODBC]
ms.assetid: ac0b5972-627f-4440-8c5a-0e8da728726d
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 500051efaef93c3f2d4ffbe7f2e8520bfecf70ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcancel-function"></a>Sqlcancel, fonction
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLCancel** annule le traitement sur une instruction.  
  
 Pour annuler le traitement sur une connexion ou une instruction, utilisez [SQLCancelHandle fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLCancel(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLCancel** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLCancel** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) dans l’argument  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLCancel** fonction a été appelée.<br /><br /> (DM) annuler l’opération a échoué car une opération asynchrone est en cours d’exécution sur un handle de connexion qui est associé à *au paramètre StatementHandle*.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY018|Le serveur a rejeté la requête d’annulation|Le serveur a refusé la demande d’annulation.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 **SQLCancel** peut annuler les types suivants de traitement sur une instruction :  
  
-   Une fonction en cours d’exécution asynchrone de l’instruction.  
  
-   Une fonction sur une instruction qui a besoin de données.  
  
-   Une fonction en cours d’exécution sur l’instruction sur un autre thread.  
  
 Dans ODBC 2. *x*, si une application appelle **SQLCancel** lorsque aucun traitement n’est effectuée sur l’instruction, **SQLCancel** a le même effet que **SQLFreeStmt** avec l’option SQL_CLOSE ; ce comportement est défini uniquement par souci d’exhaustivité et les applications doivent appeler **SQLFreeStmt** ou **SQLCloseCursor** de fermeture des curseurs.  
  
 Lorsque **SQLCancel** est appelée pour annuler une fonction en cours d’exécution en mode asynchrone dans une instruction ou une fonction dans une instruction que les besoins de données, les enregistrements de diagnostic validés par la fonction en cours d’annulation sont désactivées, et **SQLCancel** publie ses propres enregistrements de diagnostic ; lorsque **SQLCancel** est appelée pour annuler une fonction en cours d’exécution sur une instruction sur un autre thread, toutefois, il n’efface pas les enregistrements de diagnostic de l’annulation fonction et ne publie pas ses propres enregistrements de diagnostic.  
  
## <a name="canceling-asynchronous-processing"></a>Annuler le traitement asynchrone  
 Une fois une application appelle une fonction de façon asynchrone, il appelle la fonction à plusieurs reprises pour déterminer si elle a terminé le traitement. Si la fonction est encore en cours, elle retourne SQL_STILL_EXECUTING. Si la fonction a terminé le traitement, il retourne un code différent.  
  
 Après chaque appel à la fonction qui retourne SQL_STILL_EXECUTING, une application peut appeler **SQLCancel** pour annuler la fonction. Si la demande d’annulation est réussie, le pilote retourne SQL_SUCCESS. Ce message n’indique pas que la fonction a été réellement annulée ; Il indique que la demande d’annulation a été traitée. Quand ou si la fonction est en fait annulée est dépendant du pilote et la source de données. L’application doit continuer à appeler la fonction d’origine jusqu'à ce que le code de retour n’est pas SQL_STILL_EXECUTING. Si la fonction a été annulée avec succès, le code de retour est SQL_ERROR et SQLSTATE HY008 (opération annulée). Si la fonction terminé son traitement normal, le code de retour est SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO si la fonction a réussi ou SQL_ERROR et un SQLSTATE autre que HY008 (opération annulée) en cas d’échec de la fonction.  
  
> [!NOTE]  
>  Dans ODBC 3.5, un appel à **SQLCancel** lorsque aucun traitement n’est effectuée sur l’instruction n’est pas traitée comme **SQLFreeStmt** avec l’option SQL_CLOSE, mais a n’est sans effet. Pour fermer un curseur, une application doit appeler **SQLCloseCursor**, et non **SQLCancel**.  
  
 Pour plus d’informations sur le traitement asynchrone, consultez [exécution asynchrone](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).  
  
## <a name="canceling-functions-that-need-data"></a>L’annulation des fonctions nécessitant des données  
 Après avoir **SQLExecute** ou **SQLExecDirect** retourne SQL_NEED_DATA et avant l’envoi de données pour tous les paramètres à l’exécution, une application peut appeler **SQLCancel** pour annuler l’exécution de l’instruction. Une fois que l’instruction a été annulée, l’application peut appeler **SQLExecute** ou **SQLExecDirect** à nouveau. Pour plus d’informations, consultez [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 Après avoir **SQLBulkOperations** ou **SQLSetPos** retourne SQL_NEED_DATA et avant l’envoi de données pour toutes les colonnes de données en cours d’exécution, une application peut appeler **SQLCancel** pour annuler l’opération. Une fois que l’opération a été annulée, l’application peut appeler **SQLBulkOperations** ou **SQLSetPos** ; l’annulation n’affecte pas l’état de curseur ou la position actuelle du curseur. Pour plus d’informations, consultez [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) ou [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="canceling-functions-executing-on-another-thread"></a>L’annulation de l’exécution sur un autre Thread de fonctions  
 Dans une application multithread, l’application peut annuler une fonction qui est en cours d’exécution sur un autre thread. Pour annuler la fonction, l’application appelle **SQLCancel** avec le même descripteur d’instruction que celle utilisée par la fonction cible, mais sur un thread différent. Comment la fonction est annulée dépend du pilote et le système d’exploitation. Comme dans l’annulation d’une fonction en cours d’exécution en mode asynchrone, le code de retour de la **SQLCancel** indique uniquement si le pilote a traité la demande avec succès. Seulement SQL_SUCCESS ou SQL_ERROR peuvent être retournées ; Aucune information de diagnostic n’est retournée. Si la fonction d’origine est annulée, elle retourne SQL_ERROR et SQLSTATE HY008 (opération annulée).  
  
 Si une instruction SQL est exécutée lorsque **SQLCancel** est appelée sur un autre thread pour annuler l’exécution de l’instruction, il est possible que l’exécution réussisse et retour SQL_SUCCESS lors de l’annulation réussie. Dans ce cas, le Gestionnaire de pilote part du principe que le curseur ouvert par l’exécution de l’instruction est fermé par l’annulation, l’application sera donc pas en mesure d’utiliser le curseur.  
  
 Pour plus d’informations à propos du threading, consultez [Multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à un paramètre|[SQLBindParameter, fonction](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Exécution en bloc insérer ou mettre à jour des opérations|[SQLBulkOperations, fonction](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Annule une fonction en cours d’exécution en mode asynchrone sur un handle de connexion, en outre les fonctionnalités de **SQLCancel**.|[SQLCancelHandle, fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|La libération d’un descripteur d’instruction|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Obtention d’un champ d’un enregistrement de diagnostic ou d’un champ de l’en-tête de diagnostic|[SQLGetDiagField, fonction](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
|Obtention de plusieurs champs d’une structure de données de diagnostic|[SQLGetDiagRec, fonction](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|  
|Retourner le paramètre suivant pour envoyer des données pour|[SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[SQLPutData, fonction](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Positionnez le curseur dans un ensemble de lignes, l’actualisation des données dans l’ensemble de lignes ou de mise à jour ou de suppression de données dans le jeu de résultats|[SQLSetPos, fonction](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
