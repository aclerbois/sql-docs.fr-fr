---
title: Fonction SQLProcedureColumns | Documents Microsoft
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
- SQLProcedureColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedureColumns
helpviewer_keywords:
- SQLProcedureColumns function [ODBC]
ms.assetid: 4ca37b28-a6df-465b-8988-d422d37fc025
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3077585de15ac3ceb42ce1b1d258c67178fe2e0a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlprocedurecolumns-function"></a>Fonction SQLProcedureColumns
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 1.0 ODBC : ODBC  
  
 **Résumé**  
 **SQLProcedureColumns** renvoie la liste des paramètres d’entrée et de sortie, ainsi que les colonnes qui composent le jeu de résultats pour les procédures spécifiées. Le pilote retourne les informations dans un jeu de résultats sur l’instruction spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLProcedureColumns(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     ProcName,  
     SQLSMALLINT   NameLength3,  
     SQLCHAR *     ColumnName,  
     SQLSMALLINT   NameLength4);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *Nom de catalogue*  
 [Entrée] Nom du catalogue de procédure. Si un pilote prend en charge les catalogues pour certaines procédures, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les procédures qui n’ont pas de catalogues. *Nom de catalogue* ne peut pas contenir un modèle de recherche de chaîne.  
  
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
  
 *ColumnName*  
 [Entrée] Modèle de recherche de chaîne pour les noms de colonne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *ColumnName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *ColumnName* est un argument de valeur de modèle ; il est traité littéralement, et ses cas est significatif.  
  
 *NameLength4*  
 [Entrée] Longueur en caractères de **ColumnName*.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLProcedureColumns** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLProcedureColumns** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|24000|État de curseur non valide|Un curseur a été ouvert sur le *au paramètre StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** avait été appelée. Cette erreur est retournée par le Gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **SQLFetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *au paramètre StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’avait pas été appelée.|  
|40001|Échec de la sérialisation|La transaction a été annulée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction, et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLError** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut d’instruction SQL_ATTR_METADATA_ID a pris la valeur SQL_TRUE, le *CatalogName* argument était un pointeur null et le SQL_CATALOG_NAME *InfoType* retourne le nom de catalogue est pris en charge.<br /><br /> (DM), l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini avec SQL_TRUE et le *SchemaName*, *ProcName*, ou *ColumnName* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction aynschronous toujours en cours d’exécution lorsque la fonction SQLProcedureColumns a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieur à 0 mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom a dépassé la valeur de longueur maximale pour le catalogue correspondant, un schéma, une procédure ou un nom de colonne.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue de la procédure a été spécifié, et le pilote ou une source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma de la procédure a été spécifié, et le pilote ou une source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaîne a été spécifié pour le schéma de la procédure, le nom de la procédure ou le nom de colonne, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’attente a expiré avant que la source de données a retourné le jeu de résultats. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Cette fonction est généralement utilisée avant l’exécution d’instruction pour récupérer des informations sur les paramètres de procédure et les colonnes qui composent le jeu de résultats ou les jeux retournés par la procédure, le cas échéant. Pour plus d’informations, consultez [procédures](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
> [!NOTE]  
>  **SQLProcedureColumns** peut ne pas retourner toutes les colonnes utilisées par une procédure. Par exemple, un pilote peut retourner des informations uniquement sur les paramètres utilisés par une procédure et pas les colonnes dans un jeu de résultats qu’il génère.  
  
 Le *SchemaName*, *ProcName*, et *ColumnName* arguments accepte les modèles de recherche. Pour plus d’informations sur les modèles de recherche valides, consultez [Arguments de valeur de modèle](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données retournées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLProcedureColumns** retourne les résultats sous la forme d’un jeu de résultats standard classé par PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME et COLUMN_TYPE. Noms de colonnes sont renvoyées pour chaque procédure dans l’ordre suivant : le nom de la valeur de retour, les noms de chaque paramètre dans l’appel de procédure (dans l’ordre d’appel), puis les noms de chaque colonne du jeu de résultats retourné par la procédure (dans l’ordre des colonnes).  
  
 Les applications doivent lier les colonnes spécifiques au pilote par rapport à la fin du jeu de résultats. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
 Pour déterminer les longueurs réelles des colonnes PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME et COLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_PROCEDURE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
 Les colonnes suivantes ont été renommés pour ODBC 3. *x*. Les changements de nom de colonne n’affectent pas la compatibilité descendante, car les applications lier par numéro de colonne.  
  
|Colonne de ODBC 2.0|ODBC 3. *x* colonne|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|PROCÉDURE _OWNER|PROCEDURE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Les colonnes suivantes ont été ajoutées au jeu de résultats retourné par **SQLProcedureColumns** pour ODBC 3. *x*:  
  
-   COLUMN_DEF  
  
-   DATETIME_CODE  
  
-   CHAR_OCTET_LENGTH  
  
-   ORDINAL_POSITION  
  
-   IS_NULLABLE  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 19 (IS_NULLABLE) peuvent être définies par le pilote. Une application doit accéder à des colonnes spécifiques aux pilotes à rebours à partir de la fin du jeu de résultats plutôt que de spécifier une position ordinale explicite. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC VERSION 2.0)|1|Varchar|Nom du catalogue de procédure ; NULL si non applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines procédures, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les procédures qui n’ont pas de catalogues.|  
|PROCEDURE_SCHEM (ODBC VERSION 2.0)|2|Varchar|Nom de schéma de procédure ; NULL si non applicable à la source de données. Si un pilote prend en charge les schémas pour certaines procédures, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les procédures qui n’ont pas de schémas.|  
|NOM_PROCÉDURE (ODBC VERSION 2.0)|3|Varchar non NULL|Nom de la procédure. Une chaîne vide est retournée pour une procédure qui n’a pas de nom.|  
|COLUMN_NAME (ODBC VERSION 2.0)|4|Varchar non NULL|Nom de colonne de procédure. Le pilote retourne une chaîne vide pour une colonne de procédure qui n’a pas de nom.|  
|COLUMN_TYPE (ODBC VERSION 2.0)|5|Smallint non NULL|Définit la colonne de la procédure en tant que paramètre ou une colonne de jeu de résultats :<br /><br /> SQL_PARAM_TYPE_UNKNOWN : La colonne de la procédure est un paramètre dont le type est inconnu. ODBC (1.0)<br /><br /> SQL_PARAM_INPUT : La colonne de la procédure est un paramètre d’entrée. ODBC (1.0)<br /><br /> SQL_PARAM_INPUT_OUTPUT : La colonne de la procédure est un paramètre d’entrée/sortie. ODBC (1.0)<br /><br /> SQL_PARAM_OUTPUT : La colonne de la procédure est un paramètre de sortie. (ODBC VERSION 2.0)<br /><br /> SQL_RETURN_VALUE : La colonne de la procédure est la valeur de retour de la procédure. (ODBC VERSION 2.0)<br /><br /> SQL_RESULT_COL : La colonne de la procédure est une colonne de jeu de résultats. ODBC (1.0)|  
|DATA_TYPE (ODBC VERSION 2.0)|6|Smallint non NULL|Type de données SQL. Cela peut être un type de données SQL ODBC ou un type de données SQL spécifique au pilote. Pour les types de données datetime et interval, cette colonne renvoie les types de données concis (par exemple, SQL_TYPE_TIME ou SQL_INTERVAL_YEAR_TO_MONTH). Pour obtenir la liste des types de données ODBC SQL valides, consultez [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) annexe d : Types de données. Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.|  
|TYPE_NAME (ODBC VERSION 2.0)|7|Varchar non NULL|Nom du type de données de dépend de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINARY » ou « () CHAR pour les données BIT ».|  
|COLUMN_SIZE (ODBC VERSION 2.0)|8|Entier|La taille de la colonne de la colonne de procédure sur la source de données. Valeur NULL est retournée pour les types de données où la taille de la colonne n’est pas applicable. Pour plus d’informations sur la précision, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|BUFFER_LENGTH (ODBC VERSION 2.0)|9|Entier|La longueur en octets des données transférées sur un **SQLGetData** ou **SQLFetch** opération si SQL_C_DEFAULT est spécifié. Pour les données numériques, cette taille peut être différente de la taille des données stockées sur la source de données. Pour plus d’informations, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md), annexe d : Types de données.|  
|DECIMAL_DIGITS (ODBC VERSION 2.0)|10|Smallint|Les chiffres décimaux de la colonne de procédure sur la source de données. Valeur NULL est retournée pour les types de données où chiffres décimaux ne sont pas applicables. Pour plus d’informations concernant les chiffres décimaux, voir [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md), annexe d : Types de données.|  
|NUM_PREC_RADIX (ODBC VERSION 2.0)|11|Smallint|Pour les types de données numériques, 10 ou 2.<br /><br /> S’il y a 10, les valeurs COLUMN_SIZE et DECIMAL_DIGITS donnent le nombre de chiffres autorisé pour la colonne. Par exemple, une colonne DECIMAL(12,5) renvoie un NUM_PREC_RADIX de 10, un COLUMN_SIZE 12 et un DECIMAL_DIGITS 5 ; une colonne de type FLOAT peut retourner un NUM_PREC_RADIX de 10, un COLUMN_SIZE 15 et DECIMAL_DIGITS est NULL.<br /><br /> S’il y a 2, les valeurs COLUMN_SIZE et DECIMAL_DIGITS donnent le nombre de bits autorisées dans la colonne. Par exemple, une colonne de type FLOAT peut retourner un NUM_PREC_RADIX de 2, un COLUMN_SIZE 53 et DECIMAL_DIGITS est NULL.<br /><br /> Valeur NULL est retournée pour les types de données où NUM_PREC_RADIX n’est pas applicable.|  
|NULLABLE (ODBC VERSION 2.0)|12|Smallint non NULL|Si la colonne de la procédure accepte une valeur NULL :<br /><br /> SQL_NO_NULLS : La colonne de la procédure n’accepte pas les valeurs NULL.<br /><br /> SQL_NULLABLE : La colonne de la procédure accepte les valeurs NULL.<br /><br /> SQL_NULLABLE_UNKNOWN : Il ne connaît pas si la colonne de la procédure accepte les valeurs NULL.|  
|REMARQUES (ODBC VERSION 2.0)|13|Varchar|Description de la colonne de la procédure.|  
|COLUMN_DEF (ODBC 3.0)|14|Varchar|La valeur par défaut de la colonne.<br /><br /> Si la valeur NULL a été spécifiée en tant que la valeur par défaut, cette colonne est le mot NULL, ne pas entourée guillemets. Si la valeur par défaut ne peut pas être représentée sans troncation, cette colonne contient tronquées, sans guillemets englobante. Si aucune valeur par défaut a été spécifié, cette colonne est NULL.<br /><br /> La valeur du COLUMN_DEF peut être utilisée pour générer une nouvelle définition de colonne, sauf lorsqu’elle contient la valeur tronquées.|  
|SQL_DATA_TYPE (ODBC 3.0)|15|Smallint non NULL|La valeur du type de données SQL telle qu’elle apparaît dans le champ SQL_DESC_TYPE du descripteur. Cette colonne est la même que la colonne DATA_TYPE, à l’exception des types de données datetime et interval.<br /><br /> Pour les types de données datetime et interval, le champ SQL_DATA_TYPE dans le jeu de résultats renvoie SQL_INTERVAL ou SQL_DATETIME, et le champ SQL_DATETIME_SUB retournera le sous-code pour le type de données spécifique de l’intervalle ou datetime. (Consultez [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)|  
|SQL_DATETIME_SUB (ODBC 3.0)|16|Smallint|Code de sous-type pour les types de données datetime et interval. Pour les autres types de données, cette colonne renvoie une valeur NULL.|  
|CHAR_OCTET_LENGTH (ODBC 3.0)|17|Entier|Colonne de type de la longueur maximale en octets de données binaire ou caractère. Pour tous les autres types de données, cette colonne retourne une valeur NULL.|  
|POSITION ORDINALE (ODBC 3.0)|18|Entier non NULL|Pour les paramètres d’entrée et de sortie, la position ordinale du paramètre dans la définition de procédure (par ordre croissant paramètre, en commençant à 1). Pour une valeur de retour (le cas échéant), 0 est retourné. Pour les colonnes du jeu de résultats, définissez la position ordinale de la colonne dans le résultat avec la première colonne dans le jeu de résultats numéro 1. S’il existe plusieurs jeux de résultats, les positions ordinales des colonnes sont retournées d’une manière spécifique au pilote.|  
|IS_NULLABLE (ODBC 3.0)|19|Varchar|« NON » si la colonne n’inclut pas les valeurs NULL.<br /><br /> « YES » si la colonne peut inclure des valeurs NULL.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible ISO SQL ne peut pas retourner une chaîne vide.<br /><br /> La valeur renvoyée pour cette colonne est différente de celle renvoyée pour la colonne NULLABLE. (Consultez la description de la colonne accepte les valeurs NULL.)|  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [les appels de procédure](../../../odbc/reference/develop-app/procedure-calls.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|L’extraction d’une seule ligne ou un bloc de données dans une direction avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retour d’une liste des procédures dans une source de données|[SQLProcedures, fonction](../../../odbc/reference/syntax/sqlprocedures-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
