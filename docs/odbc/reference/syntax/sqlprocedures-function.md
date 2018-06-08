---
title: Fonction SQLProcedures | Documents Microsoft
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
- SQLProcedures
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedures
helpviewer_keywords:
- SQLProcedures function [ODBC]
ms.assetid: d0d9ef10-2fd4-44a5-9334-649f186f4ba0
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 486616002b58db8de8de5cd3e54e13c286b2587d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlprocedures-function"></a>Fonction SQLProcedures
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 1.0 ODBC : ODBC  
  
 **Résumé**  
 **SQLProcedures** renvoie la liste des noms de procédures stockées dans une source de données spécifique. *Procédure* est un terme générique utilisé pour décrire un *objet exécutable*, ou une entité nommée qui peut être appelée à l’aide des paramètres d’entrée et de sortie. Pour plus d’informations sur les procédures, consultez le [procédures](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLProcedures(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      ProcName,  
     SQLSMALLINT    NameLength3);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *Nom de catalogue*  
 [Entrée] Catalogue de la procédure. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de catalogues. *Nom de catalogue* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *CatalogName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *CatalogName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif. Pour plus d’informations, consultez [Arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 [Entrée] Longueur en caractères de **CatalogName*.  
  
 *schemaName*  
 [Entrée] Modèle de recherche de chaîne pour les noms de schéma de procédure. Si un pilote prend en charge les schémas pour certaines procédures, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les procédures qui n’ont pas de schémas.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *SchemaName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *SchemaName* est un argument de valeur de modèle ; il est traité littéralement, et ses cas est significatif.  
  
 *NameLength2*  
 [Entrée] Longueur en caractères de **SchemaName*.  
  
 *ProcName*  
 [Entrée] Modèle de recherche de chaîne pour les noms de procédure.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *ProcName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *ProcName* est un argument de valeur de modèle ; il est traité littéralement, et ses cas est significatif.  
  
 *NameLength3*  
 [Entrée] Longueur en caractères de **ProcName*.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLProcedures** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLProcedures** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|24000|État de curseur non valide|Un curseur a été ouvert sur le *au paramètre StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** avait été appelée. Cette erreur est retournée par le Gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **SQLFetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *au paramètre StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’avait pas été appelée.|  
|40001|Échec de la sérialisation|La transaction a été annulée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction, et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut d’instruction SQL_ATTR_METADATA_ID a pris la valeur SQL_TRUE, le *CatalogName* argument était un pointeur null et le SQL_CATALOG_NAME *InfoType* retourne le nom de catalogue est pris en charge.<br /><br /> (DM), l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini avec SQL_TRUE et le *SchemaName* ou *ProcName* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone était toujours l’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieur à 0 mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom a dépassé la valeur de longueur maximale pour le nom correspondant.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue de la procédure a été spécifié, et le pilote ou une source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma de la procédure a été spécifié, et le pilote ou une source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaîne a été spécifié pour le schéma de la procédure ou le nom de la procédure, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant la source de données a retourné le jeu de résultat demandé. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge cette fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLProcedures** répertorie toutes les procédures dans la plage demandée. Un utilisateur peut ou n’êtes pas autorisé à exécuter l’une de ces procédures. Pour vérifier l’accessibilité, une application peut appeler **SQLGetInfo** et vérifiez la valeur des informations SQL_ACCESSIBLE_PROCEDURES. Sinon, l’application doit être en mesure de gérer une situation où l’utilisateur sélectionne une procédure qui il ne peut pas s’exécuter. Pour plus d’informations sur la façon dont ces informations peuvent être utilisées, consultez [procédures](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données retournées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLProcedures** retourne les résultats sous la forme d’un jeu de résultats standard classé par PROCEDURE_CAT, PROCEDURE_SCHEMA et PROCEDURE_NAME.  
  
> [!NOTE]  
>  **SQLProcedures** peut ne pas retourner toutes les procédures. Applications peuvent utiliser toute procédure valide, indépendamment de si elle est retournée par **SQLProcedures**.  
  
 Les colonnes suivantes ont été renommés pour ODBC 3 *.x*. Les changements de nom de colonne n’affectent pas la compatibilité descendante, car les applications lier par numéro de colonne.  
  
|Colonne de ODBC 2.0|ODBC 3 *.x* colonne|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|PROCÉDURE _OWNER|PROCÉDURE _SCHEM|  
  
 Pour déterminer les longueurs réelles des colonnes PROCEDURE_CAT, PROCEDURE_SCHEM et PROCEDURE_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN et SQL_MAX_PROCEDURE_NAME_LEN.  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 8 (PROCEDURE_TYPE) peuvent être définies par le pilote. Une application doit accéder à des colonnes spécifiques aux pilotes à rebours à partir de la fin du jeu de résultats plutôt que de spécifier une position ordinale explicite. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC VERSION 2.0)|1|Varchar|Identificateur du catalogue de procédure ; NULL si non applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines procédures, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les procédures qui n’ont pas de catalogues.|  
|PROCEDURE_SCHEM (ODBC VERSION 2.0)|2|Varchar|Identificateur de schéma de procédure ; NULL si non applicable à la source de données. Si un pilote prend en charge les schémas pour certaines procédures, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les procédures qui n’ont pas de schémas.|  
|NOM_PROCÉDURE (ODBC VERSION 2.0)|3|Varchar non NULL|Identificateur de la procédure.|  
|NUM_INPUT_PARAMS (ODBC VERSION 2.0)|4|Néant|Réservé pour un usage ultérieur. Applications ne doivent pas compter sur les données retournées dans ces colonnes de résultats.|  
|NUM_OUTPUT_PARAMS (ODBC VERSION 2.0)|5|Néant|Réservé pour un usage ultérieur. Applications ne doivent pas compter sur les données retournées dans ces colonnes de résultats.|  
|NUM_RESULT_SETS (ODBC VERSION 2.0)|6|Néant|Réservé pour un usage ultérieur. Applications ne doivent pas compter sur les données retournées dans ces colonnes de résultats.|  
|REMARQUES (ODBC VERSION 2.0)|7|Varchar|Description de la procédure.|  
|PROCEDURE_TYPE (ODBC VERSION 2.0)|8|Smallint|Définit le type de procédure :<br /><br /> SQL_PT_UNKNOWN : Il ne peut pas être déterminé si la procédure retourne une valeur.<br /><br /> SQL_PT_PROCEDURE : L’objet retourné est une procédure ; Autrement dit, il n’a pas une valeur de retour.<br /><br /> SQL_PT_FUNCTION : L’objet retourné est une fonction ; Autrement dit, elle a une valeur de retour.|  
  
 Le *SchemaName* et *ProcName* arguments accepte les modèles de recherche. Pour plus d’informations sur les modèles de recherche valides, consultez [Arguments de valeur de modèle](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [les appels de procédure](../../../odbc/reference/develop-app/procedure-calls.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|L’extraction d’une seule ligne ou un bloc de données dans une direction avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour d’informations sur un pilote ou d’une source de données|[SQLGetInfo, fonction](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Retourner les paramètres et le résultat de définir les colonnes d’une procédure|[SQLProcedureColumns, fonction](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)|  
|Syntaxe pour appeler des procédures stockées|[L’exécution d’instructions](../../../odbc/reference/develop-app/executing-statements-odbc.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
