---
title: Fonction SQLPrepare | Documents Microsoft
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
- SQLPrepare
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPrepare
helpviewer_keywords:
- SQLPrepare function [ODBC]
ms.assetid: 332e1b4b-b0ed-4e7a-aa4d-4f35f4f4476b
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e2134b972d8cceebb5928f856f8a4361aa4199fd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlprepare-function"></a>Fonction SQLPrepare
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLPrepare** prépare une chaîne SQL pour l’exécution.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLPrepare(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     StatementText,  
     SQLINTEGER    TextLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *StatementText*  
 [Entrée] Chaîne de texte SQL.  
  
 *TextLength*  
 [Entrée] Longueur de **StatementText* en caractères.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLPrepare** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLPrepare** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01S02|Valeur de l’option modifiée|Un attribut d’instruction spécifiée non valide en raison de conditions de travail de mise en œuvre, donc une valeur similaire a été remplacée temporairement. (**SQLGetStmtAttr** peut être appelée pour déterminer quelle est la valeur substituée temporairement.) La valeur de remplacement n’est valide pour le *au paramètre StatementHandle* jusqu'à ce que le curseur est fermé. Les attributs d’instruction qui peuvent être modifiés sont : SQL_ATTR_CONCURRENCY SQL_ATTR_CURSOR_TYPE SQL_ATTR_KEYSET_SIZE SQL_ATTR_MAX_LENGTH SQL_ATTR_MAX_ROWS SQL_ATTR_QUERY_TIMEOUT SQL_ATTR_SIMULATE_CURSOR<br /><br /> (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|21S01|Liste de valeurs d’insertion ne correspond pas à la liste des colonnes|\**StatementText* contenait un **insérer** instruction et le nombre de valeurs à insérer ne correspond pas au degré de la table dérivée.|  
|21S02|Degré de la table dérivée ne correspond pas à la liste des colonnes|\**StatementText* contenait un **CREATE VIEW** instruction et le nombre de noms spécifié n’est pas le même niveau que la table dérivée défini par la spécification de requête.|  
|22018|Valeur de caractère non valide pour la spécification de la casse|**StatementText* contenait une instruction SQL qui contenait un littéral ou un paramètre et la valeur n’est pas compatible avec le type de données de la colonne de table associée.|  
|22019|Caractère d’échappement non valide|L’argument *StatementText* contenus un **comme** prédicat avec une **échappement** dans les **où** clause et la longueur de ces caractères d’échappement **échappement** n’est pas égale à 1.|  
|22025|Séquence d’échappement non valide|L’argument *StatementText* contenus «**comme** *valeur de modèle* **échappement** *caractère d’échappement*» dans le **où** clause et le caractère suivant le caractère d’échappement dans la valeur de modèle qui a été « % », ni « _ ».|  
|24000|État de curseur non valide|(DM) un curseur a été ouvert sur le *au paramètre StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** avait été appelée.<br /><br /> Un curseur a été ouvert sur le *au paramètre StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’avait pas été appelée.|  
|34000|Nom de curseur non valide|\**StatementText* contenait un positionnées **supprimer** ou une position **mise à jour**, et le curseur référencé par l’instruction préparée n’est pas ouvert.|  
|3D000|Nom de catalogue non valide|Le nom de catalogue spécifié dans *StatementText* n’était pas valide.|  
|3F000|Nom de schéma non valide|Le nom de schéma spécifié dans *StatementText* n’était pas valide.|  
|42000|Syntaxe ou violation d’accès|\**StatementText* contient une instruction SQL qui n’a pas été pouvant être préparé ou contenait une erreur de syntaxe.<br /><br /> **StatementText* contient une instruction pour laquelle l’utilisateur ne disposait pas des privilèges requis.|  
|42S01|Table de base ou la vue existe déjà|\**StatementText* contenait un **CREATE TABLE** ou **CREATE VIEW** instruction et que le nom de la table ou vue nom spécifié existe déjà.|  
|42 S 02|Table de base ou de vue introuvable|\**StatementText* contenait un **DROP TABLE** ou un **DROP VIEW** instruction et le nom de la table spécifiée ou le nom n’existe pas de vue.<br /><br /> \**StatementText* contenait un **ALTER TABLE** instruction et le nom de la table spécifiée n’existent pas.<br /><br /> \**StatementText* contenait un **CREATE VIEW** instruction et un nom de table ou la vue nom défini par la spécification de requête n’existe pas.<br /><br /> \**StatementText* contenait un **CREATE INDEX** instruction et le nom de la table spécifiée n’existent pas.<br /><br /> \**StatementText* contenait un **GRANT** ou **RÉVOQUER** instruction et le nom de la table spécifiée ou le nom n’existe pas de vue.<br /><br /> \**StatementText* contenait un **sélectionnez** instruction et une table spécifiée ou vue de nom n’existe pas.<br /><br /> \**StatementText* contenait un **supprimer**, **insérer**, ou **mise à jour** instruction et le nom de la table spécifiée n’existent pas.<br /><br /> \**StatementText* contenait un **CREATE TABLE** instruction et une table spécifiée dans une contrainte (faisant référence à une table autre que celui en cours de création) n’existent pas.|  
|42S11|L’index existe déjà|\**StatementText* contenait un **CREATE INDEX** instruction et le nom de l’index spécifié existaient déjà.|  
|42S12|Index non trouvé|\**StatementText* contenait un **DROP INDEX** instruction et le nom de l’index spécifié n’existent pas.|  
|42S21|La colonne existe déjà|\**StatementText* contenait un **ALTER TABLE** instruction et la colonne spécifiée dans le **ajouter** clause n’est pas unique ou identifie une colonne existante dans la table de base.|  
|42S22|Colonne introuvable|\**StatementText* contenait un **CREATE INDEX** instruction et la valeur d’un ou plusieurs de la colonne spécifiées dans la liste des colonnes de noms n’existent pas.<br /><br /> \**StatementText* contenait un **GRANT** ou **RÉVOQUER** instruction et un nom de colonne spécifié n’existent pas.<br /><br /> \**StatementText* contenait un **sélectionnez**, **supprimer**, **insérer**, ou **mise à jour** instruction et un nom de colonne spécifiée n’existe pas.<br /><br /> \**StatementText* contenait un **CREATE TABLE** instruction et une colonne spécifiée dans une contrainte (faisant référence à une table autre que celui en cours de création) n’existent pas.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*, et la fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) *StatementText* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLPrepare** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) l’argument *TextLength* était inférieur ou égal à 0, mais pas égale à SQL_NTS.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le paramètre d’accès concurrentiel n’est pas valide pour le type de curseur défini.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’attente a expiré avant que la source de données a retourné le jeu de résultats. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 L’application appelle **SQLPrepare** pour envoyer une instruction SQL à la source de données de préparation. Pour plus d’informations sur l’exécution préparée, consultez [exécution préparée](../../../odbc/reference/develop-app/prepared-execution-odbc.md). L’application peut inclure un ou plusieurs des marqueurs de paramètre dans l’instruction SQL. Pour inclure un marqueur de paramètre, l’application incorpore un point d’interrogation ( ?) dans la chaîne SQL à la position appropriée. Pour plus d’informations sur les paramètres, consultez [paramètres de l’instruction](../../../odbc/reference/develop-app/statement-parameters.md).  
  
> [!NOTE]  
>  Si une application utilise **SQLPrepare** pour préparer et **SQLExecute** pour envoyer un **valider** ou **restauration** instruction, il ne sera pas interopérable entre les produits de SGBD. Pour valider ou restaurer une transaction, appelez **SQLEndTran**.  
  
 Le pilote peut modifier l’instruction pour utiliser la forme du SQL utilisée par la source de données puis l’envoyer à la source de données de préparation. En particulier, le pilote modifie les séquences d’échappement utilisées pour définir la syntaxe SQL pour certaines fonctionnalités. (Pour obtenir une description de la grammaire d’instruction SQL, consultez [les séquences d’échappement dans ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md) et [annexe c : SQL grammaire](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).) Pour le pilote, un descripteur d’instruction est similaire à un identificateur de l’instruction du code SQL incorporé. Si la source de données prend en charge les identificateurs de l’instruction, le pilote peut envoyer un identificateur de l’instruction et les valeurs de paramètre à la source de données.  
  
 Une fois une instruction est préparée, l’application utilise le handle d’instruction pour faire référence à l’instruction dans les appels de fonction ultérieurs. L’instruction préparée associée au handle d’instruction peut être réexécutée en appelant **SQLExecute** jusqu'à ce que l’application libère l’instruction avec un appel à **SQLFreeStmt** avec l’option SQL_DROP ou jusqu'à ce que l’instruction handle est utilisé dans un appel à **SQLPrepare**, **SQLExecDirect**, ou l’une des fonctions de catalogue (**SQLColumns**, **SQLTables**, et ainsi de suite). Une fois que l’application prépare une instruction, elle peut demander des informations sur le format du jeu de résultats. Pour certaines implémentations, appelant **SQLDescribeCol** ou **SQLDescribeParam** après **SQLPrepare** peut ne pas être aussi efficace que l’appel de la fonction après **SQLExecute** ou **SQLExecDirect**.  
  
 Certains pilotes ne peut pas retourner des erreurs de syntaxe ou les violations d’accès lorsque l’application appelle **SQLPrepare**. Un pilote peut gérer les erreurs de syntaxe et les violations d’accès, uniquement les erreurs de syntaxe ou des erreurs de syntaxe ni les violations d’accès. Par conséquent, une application doit être en mesure de gérer ces conditions lors de l’appel suivant des fonctions associées telles que **SQLNumResultCols**, **SQLDescribeCol**, **SQLColAttribute**, et **SQLExecute**.  
  
 Selon les fonctionnalités du pilote et de la source de données, les informations de paramètre (par exemple, les types de données) peuvent être vérifiées lorsque l’instruction est préparée (si tous les paramètres ont été liés) ou lorsqu’il est exécuté (si tous les paramètres n’ont pas été liés). Pour une interopérabilité maximale, une application doit dissocier tous les paramètres appliquaient à une instruction SQL ancien avant de préparer une nouvelle instruction SQL sur la même instruction. Cela évite les erreurs dues à des anciennes informations de paramètre qui est appliquées à la nouvelle instruction.  
  
> [!IMPORTANT]  
>  Validation d’une transaction en appelant explicitement **SQLEndTran** ou en travaillant en mode autocommit, peut provoquer la source de données supprimer les plans d’accès pour toutes les instructions sur une connexion. Pour plus d’informations, consultez les types d’informations SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR dans [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) et [effet des Transactions sur les curseurs et des instructions préparées](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md), et [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur d’instruction|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Liaison d’une mémoire tampon à un paramètre|[SQLBindParameter, fonction](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécuter une opération de validation ou de restauration|[SQLEndTran, fonction](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|L’exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécuter une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Renvoi du nombre de lignes affectées par une instruction|[SQLRowCount, fonction](../../../odbc/reference/syntax/sqlrowcount-function.md)|  
|Définition d’un nom de curseur|[SQLSetCursorName, fonction](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
