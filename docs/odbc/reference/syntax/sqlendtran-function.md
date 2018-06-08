---
title: Fonction SQLEndTran | Documents Microsoft
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
- SQLEndTran
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLEndTran
helpviewer_keywords:
- SQLEndTran function [ODBC]
ms.assetid: ff375ce1-eb50-4693-b1e6-70181a6dbf9f
caps.latest.revision: 29
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: af0207a5ccaa11728b0ff67a4acad75577acdca7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlendtran-function"></a>Fonction SQLEndTran
**Mise en conformité**  
 Version introduite : Conformité des normes 3.0 de ODBC : ISO 92  
  
 **Résumé**  
 **SQLEndTran** demande une opération de validation ou de restauration pour toutes les opérations actives sur toutes les instructions associées à une connexion. **SQLEndTran** peut également demander de qu’une opération de validation ou de restauration est effectuée pour toutes les connexions associées à un environnement.  
  
> [!NOTE]  
>  Pour plus d’informations sur les le Gestionnaire de pilotes mappe cette fonction lorsqu’un ODBC 3. *x* application fonctionne avec une API ODBC 2. *x* pilote, consultez [mappage des fonctions de remplacement pour la compatibilité descendante des Applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLEndTran(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle,  
     SQLSMALLINT   CompletionType);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 [Entrée] Identificateur de type de handle. Contient deux SQL_HANDLE_ENV (si *gérer* est un handle d’environnement) ou SQL_HANDLE_DBC (si *gérer* est un handle de connexion).  
  
 *Handle*  
 [Entrée] Le handle, du type indiqué par *HandleType*, qui indique la portée de la transaction. Pour plus d’informations, consultez « Commentaires ».  
  
 *CompletionType*  
 [Entrée] Une des deux valeurs suivantes :  
  
 SQL_COMMIT SQL_ROLLBACK  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLEndTran** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** le *HandleType* et *gérer*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLEndTran** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08003|Connexion non ouverte|(DM) le *HandleType* a été SQL_HANDLE_DBC et le *gérer* n’était pas dans un état connecté.|  
|08007|Échec de la connexion au cours de la transaction|Le *HandleType* a été SQL_HANDLE_DBC et associé à la connexion le *gérer* a échoué lors de l’exécution de la fonction, et il ne peut pas être déterminé si demandé **valider** ou **restauration** s’est produite avant la défaillance.|  
|25 S 01|État de transaction inconnu|Un ou plusieurs des connexions dans *gérer* n’a pas pu terminer la transaction avec le résultat spécifié, et le résultat est inconnu.|  
|25S02|Transaction est toujours active|Le pilote n’a pas pu garantit que tout le travail dans la transaction globale pu être effectué de façon atomique, et la transaction est toujours active.|  
|25S03|Annulation de la transaction|Le pilote n’a pas pu garantit que tout le travail dans la transaction globale pu être effectué de façon atomique, et tout le travail dans la transaction active dans *gérer* a été restaurée.|  
|40001|Échec de la sérialisation|La transaction a été annulée en raison d’un blocage de ressource avec une autre transaction.|  
|40002|Violation de contrainte d’intégrité|Le *CompletionType* a été SQL_COMMIT, et l’engagement de modifications a provoqué la violation de contrainte d’intégrité. Par conséquent, la transaction a été annulée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*szMessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *handle de connexion*. La fonction a été appelée, et il fin avant de l’exécution [SQLCancelHandle fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md) a été appelé sur le *handle de connexion*. La fonction a été appelée à nouveau sur le *handle de connexion*.<br /><br /> La fonction a été appelée, et il fin avant de l’exécution **SQLCancelHandle** a été appelé sur le *handle de connexion* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour un descripteur d’instruction associé à la *handle de connexion* et toujours en cours d’exécution lorsque **SQLEndTran** a été appelée.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *handle de connexion* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour un descripteur d’instruction associée le *handle de connexion* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *gérer* avec *HandleType* ayant pour valeur SQL_HANDLE_DBC et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour une des poignées d’instruction associées *gérer* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.|  
|HY012|Code d’opération de transaction non valide|(DM) la valeur spécifiée pour l’argument *CompletionType* n’était ni SQL_COMMIT ni SQL_ROLLBACK.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY092|Identificateur d’attribut/option non valide|(DM) la valeur spécifiée pour l’argument *HandleType* n’était ni SQL_HANDLE_ENV ni SQL_HANDLE_DBC.|  
|HY115|SQLEndTran n’est pas autorisée pour un environnement qui contient une connexion avec l’exécution d’une fonction asynchrone activée|(DM) *HandleType* ne peut pas être défini à SQL_HANDLE_ENV si l’exécution asynchrone de fonctions de connexion est activée pour une connexion dans l’environnement.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, reportez-vous à la section des commentaires de cette rubrique.|  
|HYC00|Fonctionnalité facultative non implémentée|La source de données ou le pilote ne prend pas en charge la **restauration** opération.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *handle de connexion* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Pour une application ODBC 3. *x* pilote, si *HandleType* est SQL_HANDLE_ENV et *gérer* est un handle d’environnement valide, puis appelle le Gestionnaire de pilotes **SQLEndTran** dans chaque pilote associé à l’environnement. Le *gérer* argument pour l’appel à un pilote sera le handle d’environnement du pilote. Pour une application ODBC 2. *x* pilote, si *HandleType* est SQL_HANDLE_ENV et *gérer* est un handle d’environnement valide, et il existe plusieurs connexions dans l’état connecté dans cet environnement, puis appelle le Gestionnaire de pilotes **SQLTransact** dans le pilote une fois pour chaque connexion dans l’état connecté dans cet environnement. Le *gérer* argument dans chaque appel sera le handle de la connexion. Dans les deux cas, le pilote tente de valider ou annuler les transactions, selon la valeur de *CompletionType*, sur toutes les connexions qui se trouvent dans un état connecté sur cet environnement. Les connexions qui ne sont pas actives n’affectent pas la transaction.  
  
> [!NOTE]  
>  **SQLEndTran** ne peut pas être utilisé pour valider ou annuler les transactions sur un environnement partagé. SQLSTATE HY092 (identificateur d’attribut/option non valide) sera retourné si **SQLEndTran** est appelée avec *gérer* la valeur soit le handle d’un environnement partagé ou le handle d’une connexion pour un environnement partagé.  
  
 Le Gestionnaire de pilotes retourne SQL_SUCCESS uniquement s’il ne reçoit SQL_SUCCESS pour chaque connexion. Si le Gestionnaire de pilotes reçoit SQL_ERROR sur une ou plusieurs connexions, il retourne SQL_ERROR à l’application, et les informations de diagnostic sont placées dans la structure de données de diagnostic de l’environnement. Pour déterminer les connexions ou la connexion a échoué pendant l’opération commit ou rollback, l’application peut appeler **SQLGetDiagRec** pour chaque connexion.  
  
> [!NOTE]  
>  Le Gestionnaire de pilotes ne simule pas une transaction globale pour toutes les connexions et par conséquent, n’utilise pas les protocoles de validation en deux phases.  
  
 Si *CompletionType* est SQL_COMMIT, **SQLEndTran** émet une demande de validation pour toutes les opérations actives sur n’importe quelle instruction associée à une connexion affectée. Si *CompletionType* est SQL_ROLLBACK, **SQLEndTran** émet une demande de restauration de toutes les opérations actives sur n’importe quelle instruction associée à une connexion affectée. Si aucune transaction n’est active, **SQLEndTran** retourne SQL_SUCCESS sans aucune incidence sur les sources de données. Pour plus d’informations, consultez [engagement et restaurer les Transactions](../../../odbc/reference/develop-app/committing-and-rolling-back-transactions.md).  
  
 Si le pilote est en mode de validation manuelle (en appelant **SQLSetConnectAttr** avec le SQL_ATTR_AUTOCOMMIT attribut SQL_AUTOCOMMIT_OFF), une nouvelle transaction est démarrée implicitement lorsqu’une instruction SQL qui peut être contenue dans une transaction est exécutée sur la source de données actuelle. Pour plus d’informations, consultez [en Mode de validation](../../../odbc/reference/develop-app/commit-mode.md).  
  
 Pour déterminer les conséquences des opérations de transaction sur les curseurs, une application appelle **SQLGetInfo** avec les options SQL_CURSOR_ROLLBACK_BEHAVIOR et SQL_CURSOR_COMMIT_BEHAVIOR. Pour plus d’informations, consultez les paragraphes suivants et également voir [effet des Transactions sur les curseurs et des instructions préparées](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
 Si la valeur SQL_CURSOR_ROLLBACK_BEHAVIOR ou SQL_CURSOR_COMMIT_BEHAVIOR est égal à SQL_CB_DELETE, **SQLEndTran** ferme et supprime tous les curseurs ouverts sur toutes les instructions associées à la connexion et ignore tous les résultats en attente. **SQLEndTran** laisse n’importe quelle instruction présente dans un état (annulé) alloué ; les réutiliser pour les requêtes SQL suivantes de l’application ou peut appeler **SQLFreeStmt** ou **SQLFreeHandle** avec un *HandleType* de SQL_HANDLE_STMT pour libérer.  
  
 Si la valeur SQL_CURSOR_ROLLBACK_BEHAVIOR ou SQL_CURSOR_COMMIT_BEHAVIOR est égal à SQL_CB_CLOSE, **SQLEndTran** ferme tous les curseurs ouverts sur toutes les instructions associées à la connexion. **SQLEndTran** laisse n’importe quelle instruction présente dans un état préparé ; l’application peut appeler **SQLExecute** pour une instruction associée à la connexion sans appeler d’abord **SQLPrepare**.  
  
 Si la valeur SQL_CURSOR_ROLLBACK_BEHAVIOR ou SQL_CURSOR_COMMIT_BEHAVIOR est égal à SQL_CB_PRESERVE, **SQLEndTran** n’affecte pas les curseurs ouverts associés à la connexion. Les curseurs restent à la ligne qu’ils désignés avant l’appel à **SQLEndTran**.  
  
 Pour les pilotes et sources de données qui prennent en charge des transactions, en appelant **SQLEndTran** avec SQL_COMMIT ou SQL_ROLLBACK lorsque aucune transaction n’est active retourne SQL_SUCCESS (indiquant qu’il n’existe pas de travail pour être validée ou restaurée) et n’a aucun effet sur la source de données.  
  
 Lorsqu’un pilote est en mode de validation automatique, le Gestionnaire de pilotes n’appelle pas **SQLEndTran** dans le pilote. **SQLEndTran** toujours retourne SQL_SUCCESS, quelle que soit la si elle est appelée avec un *CompletionType* de SQL_COMMIT ou SQL_ROLLBACK.  
  
 Sources de données ou les pilotes qui ne prennent pas en charge les transactions (**SQLGetInfo** *option* SQL_TXN_CAPABLE est SQL_TC_NONE) sont en réalité toujours en mode autocommit et par conséquent retourne toujours SQL_SUCCESS pour **SQLEndTran** ou non qu’elles sont appelées avec un *CompletionType* de SQL_COMMIT ou SQL_ROLLBACK. Ces pilotes et les sources de données ne pas réellement restaurer les transactions lors de la demande.  
  
## <a name="suspended-state"></a>État suspendu  
 Dans les gestionnaires de pilotes qui ont été publiés avant Windows 7, une transaction était active si **SQLEndTran** renvoyées SQL_ERROR par le pilote. Toutefois, il était possible que la transaction a été validée avec succès sur le serveur, mais le pilote sur le client n’avait pas été notifié (par exemple, car une erreur réseau s’est produite). Ce qui laisse la connexion dans un état incorrect. À partir de Windows 7, lorsque **SQLEndTran** retourne SQL_ERROR, la connexion peut être dans un état suspendu. Dans un état suspendu, il est possible d’appeler des fonctions en lecture seule. Par la suite, l’application doit appeler **SQLDisconnect** sur une connexion interrompue pour libérer des ressources.  
  
 Si toutes les conditions suivantes sont remplies, la connexion est placée dans un état suspendu :  
  
-   Le pilote retourne SQL_ERROR de **SQLEndTran**.  
  
-   Le pilote est ODBC 3.8, ou version ultérieure.  
  
-   La version de l’application est 3.8 ou version ultérieure ; ou l’application de 2.x ou 3.x ODBC recompilée correctement annule le **SQLEndTran** fonctionner à travers **SQLCancelHandle**.  
  
-   Le pilote n’a pas retourné un des messages suivants, permettant de vérifier que la transaction n’a pas terminé :  
  
    -   25S03 : annulation de la Transaction  
  
    -   40001 : Échec de la sérialisation  
  
    -   40002 : une contrainte d’intégrité  
  
    -   HYC00 : De fonctionnalité facultative non implémentée  
  
 Si **SQLEndTran** a été appelée dans un environnement de handles et une de ses connexions remplies les conditions ci-dessus, toutes les connexions, la connexion à la même pilote sont placées dans l’état suspendu.  
  
 Une fois une application appelle **SQLDisconnect** sur une connexion interrompue, la connexion peut être utilisée pour se reconnecter à une autre source de données ou de la même source de données.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|L’annulation d’une fonction en cours d’exécution en mode asynchrone sur un handle de connexion.|[SQLCancelHandle, fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|Retour d’informations sur un pilote ou d’une source de données|[SQLGetInfo, fonction](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|La libération d’un handle|[SQLFreeHandle, fonction](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|La libération d’un descripteur d’instruction|[SQLFreeStmt, fonction](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exécution asynchrone (méthode d’interrogation)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
