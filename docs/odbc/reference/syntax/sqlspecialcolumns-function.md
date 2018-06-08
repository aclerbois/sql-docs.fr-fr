---
title: Fonction SQLSpecialColumns | Documents Microsoft
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
- SQLSpecialColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSpecialColumns
helpviewer_keywords:
- SQLSpecialColumns function [ODBC]
ms.assetid: bb2d9f21-bda0-4e50-a8be-f710db660034
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2bae2c5a51d7d16798bc2d7fca0e9d38509a0d6e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlspecialcolumns-function"></a>Fonction SQLSpecialColumns
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ouvrir le groupe  
  
 **Résumé**  
 **SQLSpecialColumns** récupère les informations suivantes sur les colonnes dans une table spécifiée :  
  
-   Le jeu optimal de colonnes qui identifie de façon unique une ligne dans la table.  
  
-   Colonnes qui sont automatiquement mis à jour lorsqu’une valeur dans la ligne est mise à jour par une transaction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLSpecialColumns(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   IdentifierType,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3,  
     SQLSMALLINT   Scope,  
     SQLSMALLINT   Nullable);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *IdentifierType*  
 [Entrée] Type de colonne à retourner. Doit être une des valeurs suivantes :  
  
 SQL_BEST_ROWID : Retourne la colonne ou jeu optimal de colonnes qui, en récupérant des valeurs à partir de l’ou les colonnes, permet à n’importe quelle ligne dans la table spécifiée pour être identifié de manière unique. Une colonne peut être soit une pseudo-colonne conçue spécifiquement pour cet objectif (comme dans Oracle ROWID ou Ingres TID) ou l’ou les colonnes d’un index unique pour la table.  
  
 SQL_ROWVER : Retourne l’ou les colonnes de la table spécifiée, cas échéant, sont automatiquement mises à jour par la source de données lorsqu’une valeur dans la ligne est mise à jour par une transaction (comme dans SQLBase ROWID ou Sybase TIMESTAMP).  
  
 *Nom de catalogue*  
 [Entrée] Nom du catalogue de la table. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de catalogues. *Nom de catalogue* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *CatalogName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *CatalogName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif. Pour plus d’informations, consultez [Arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 [Entrée] Longueur en caractères de **CatalogName*.  
  
 *schemaName*  
 [Entrée] Nom de schéma pour la table. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de schémas. *SchemaName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *SchemaName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *SchemaName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength2*  
 [Entrée] Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 [Entrée] Nom de la table. Cet argument ne peut pas être un pointeur null. *TableName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *TableName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *TableName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength3*  
 [Entrée] Longueur en caractères de **TableName*.  
  
 *Portée*  
 [Entrée] Étendue minimale requise de rowid. Rowid retourné peut être étendue supérieure. Doit prendre l'une des valeurs suivantes :  
  
 SQL_SCOPE_CURROW : Le ROWID n’est garanti valide uniquement quand positionné sur cette ligne. Une version ultérieure sélectionnez de nouveau à l’aide du rowid ne peut pas retourner une ligne si la ligne a été mis à jour ou supprimée par une autre transaction.  
  
 SQL_SCOPE_TRANSACTION : Le ROWID n’est garanti être valide pour la durée de la transaction actuelle.  
  
 SQL_SCOPE_SESSION : Le ROWID n’est garanti pour être valide pour la durée de la session (entre les limites des transactions).  
  
 *Nullable*  
 [Entrée] Détermine s’il faut retourner les colonnes spéciales qui peuvent avoir une valeur NULL. Doit prendre l'une des valeurs suivantes :  
  
 SQL_NO_NULLS : Exclure les colonnes spéciales qui peuvent avoir des valeurs NULL. Certains pilotes ne peut pas prendre en charge SQL_NO_NULLS, et ces pilotes renvoie un résultat vide de définir si SQL_NO_NULLS a été spécifié. Les applications doivent être préparées pour ce cas et la demande SQL_NO_NULLS uniquement s’il est absolument nécessaire.  
  
 SQL_NULLABLE : Retourne les colonnes spéciales, même s’ils peuvent avoir des valeurs NULL.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLSpecialColumns** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLSpecialColumns** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
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
|HY009|Utilisation non valide d’un pointeur null|Le *TableName* argument était un pointeur null.<br /><br /> L’attribut d’instruction SQL_ATTR_METADATA_ID a pris la valeur SQL_TRUE, le *CatalogName* argument était un pointeur null et le SQL_CATALOG_NAME *InfoType* retourne le nom de catalogue est pris en charge.<br /><br /> (DM), l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini avec SQL_TRUE et le *SchemaName* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction était en cours d’exécution lorsque **SQLSpecialColumns** a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur est inférieur à 0 mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur a dépassé la valeur de longueur maximale pour le nom correspondant. La longueur maximale de chaque nom peut être obtenue en appelant **SQLGetInfo** avec la *InfoType* valeurs : SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN ou SQL_MAX_TABLE_NAME_LEN.|  
|HY097|Type de colonne hors limites|(DM) non valide *IdentifierType* valeur a été spécifiée.|  
|HY098|Type d’étendue hors limite|(DM) non valide *étendue* valeur a été spécifiée.|  
|HY099|Type acceptant les valeurs NULL hors limite|(DM) non valide *Nullable* valeur a été spécifiée.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue a été spécifié, et le pilote ou une source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma a été spécifié, et le pilote ou une source de données ne prend pas en charge les schémas.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant la source de données a retourné le jeu de résultat demandé. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Lorsque le *IdentifierType* argument est SQL_BEST_ROWID, **SQLSpecialColumns** retourne l’ou les colonnes qui identifient de façon unique chaque ligne de la table. Ces colonnes peuvent toujours être utilisées dans un *liste de sélection* ou **où** clause. **SQLColumns**, qui est utilisé pour retourner une variété d’informations sur les colonnes d’une table, ne retournent pas nécessairement les colonnes qui identifient de façon unique chaque ligne, ou les colonnes qui sont automatiquement mis à jour lorsqu’une valeur dans la ligne est mise à jour par un transaction. Par exemple, **SQLColumns** peut ne pas retourner Oracle pseudo-colonne ROWID. C’est pourquoi **SQLSpecialColumns** est utilisée pour retourner ces colonnes. Pour plus d’informations, consultez [utilise des données du catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données retournées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Si aucune des colonnes qui identifient de façon unique chaque ligne dans la table, **SQLSpecialColumns** renvoie un ensemble de lignes sans lignes ; un appel ultérieur à **SQLFetch** ou **SQLFetchScroll** sur l’instruction retourne SQL_NO_DATA.  
  
 Si le *IdentifierType*, *étendue*, ou *Nullable* arguments spécifient des caractéristiques qui ne sont pas pris en charge par la source de données **SQLSpecialColumns** renvoie un jeu de résultats vide.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, le *CatalogName*, *SchemaName*, et *TableName* arguments sont traités comme des identificateurs, donc ils ne peuvent pas être attribuées à un pointeur null dans certaines situations. (Pour plus d’informations, consultez [Arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).)  
  
 **SQLSpecialColumns** retourne les résultats sous la forme d’un jeu de résultats standard trié par SCOPE.  
  
 Les colonnes suivantes ont été renommés pour ODBC 3 *.x*. Les changements de nom de colonne n’affectent pas la compatibilité descendante, car les applications lier par numéro de colonne.  
  
|Colonne de ODBC 2.0|ODBC 3 *.x* colonne|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
  
 Pour déterminer la longueur réelle de la colonne COLUMN_NAME, une application peut appeler **SQLGetInfo** avec l’option SQL_MAX_COLUMN_NAME_LEN.  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 8 (PSEUDO_COLUMN) peuvent être définies par le pilote. Une application doit accéder à des colonnes spécifiques aux pilotes à rebours à partir de la fin du jeu de résultats plutôt que de spécifier une position ordinale explicite. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|PORTÉE (ODBC VERSION 1.0)|1|Smallint|Étendue réelle de rowid. Contient l’une des valeurs suivantes :<br /><br /> SQL_SCOPE_CURROW SQL_SCOPE_TRANSACTION SQL_SCOPE_SESSION<br /><br /> NULL est retourné lorsque *IdentifierType* est SQL_ROWVER. Pour obtenir une description de chaque valeur, consultez la description de *étendue* dans « Syntaxe », plus haut dans cette section.|  
|COLUMN_NAME (ODBC VERSION 1.0)|2|Varchar non NULL|Nom de colonne. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|DATA_TYPE (ODBC 1.0)|3|Smallint non NULL|Type de données SQL. Cela peut être un type de données SQL ODBC ou un type de données SQL spécifique au pilote. Pour obtenir la liste des types de données ODBC SQL valides, consultez [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md). Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.|  
|TYPE_NAME (ODBC VERSION 1.0)|4|Varchar non NULL|Nom du type de données de dépend de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINARY » ou « () CHAR pour les données BIT ».|  
|COLUMN_SIZE (ODBC 1.0)|5|Entier|La taille de la colonne de la source de données. Pour plus d’informations concernant la taille de colonne, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|(ODBC 1.0) DE BUFFER_LENGTH|6|Entier|La longueur en octets des données transférées sur un **SQLGetData** ou **SQLFetch** opération si SQL_C_DEFAULT est spécifié. Pour les données numériques, cette taille peut être différente de la taille des données stockées sur la source de données. Cette valeur est identique à la colonne COLUMN_SIZE binaire ou caractère. Pour plus d’informations, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|DECIMAL_DIGITS (ODBC VERSION 1.0)|7|Smallint|Les chiffres décimaux de la colonne sur la source de données. Valeur NULL est retournée pour les types de données où les chiffres décimaux ne sont pas applicables. Pour plus d’informations concernant les chiffres décimaux, voir [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|PSEUDO_COLUMN (ODBC VERSION 2.0)|8|Smallint|Indique si la colonne est une pseudo-colonne, telles que Oracle ROWID :<br /><br /> SQL_PC_UNKNOWN forme SQL_PC_NOT_PSEUDO SQL_PC_PSEUDO **Remarque :** pour assurer une interopérabilité maximale, pseudo-colonnes ne doivent pas être mis entre guillemets avec l’identificateur de guillemet retourné par **SQLGetInfo**.|  
  
 Une fois que l’application récupère les valeurs pour SQL_BEST_ROWID, l’application peut utiliser ces valeurs à nouveau cette ligne dans l’étendue définie. Le **sélectionnez** instruction est garantie pour retourner aucune ligne ou une ligne.  
  
 Si une application spécifie à nouveau une ligne en fonction de l’ou les colonnes rowid et la ligne n’est pas trouvée, l’application peut supposer que la ligne a été supprimée ou les colonnes rowid ont été modifiés. Le contraire n’est pas vrai : même si le ROWID n’a pas changé, les autres colonnes de la ligne peuvent ont été modifiés.  
  
 Colonnes retournées pour le type de colonne SQL_BEST_ROWID sont utiles pour les applications qui ont besoin pour avancer et revenir au sein d’un jeu de résultats à récupérer les données les plus récentes à partir d’un ensemble de lignes. L’ou les colonnes de rowid sont garanti que ne pas modifier tout en étant positionné sur cette ligne.  
  
 L’ou les colonnes de rowid peuvent rester valides même lorsque le curseur n’est pas positionné sur la ligne ; l’application peut déterminer cela en vérifiant la colonne de portée dans le jeu de résultats.  
  
 Colonnes retournées pour le type de colonne SQL_ROWVER sont utiles pour les applications qui ont besoin de pouvoir vérifier si toutes les colonnes dans une ligne donnée ont été mis à jour pendant que la ligne a été nouveau activée à l’aide de l’ID de ligne. Par exemple, après sélectionnant à nouveau une ligne à l’aide du rowid, l’application peut comparer les valeurs précédentes dans les colonnes SQL_ROWVER à celles extraites uniquement. Si la valeur dans une colonne SQL_ROWVER diffère de la valeur précédente, l’application peut avertir l’utilisateur que les données sur l’affichage a changé.  
  
## <a name="code-example"></a>Exemple de code  
 Pour obtenir un exemple de code d’une fonction similaire, consultez [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retourner les colonnes d’une table ou de tables|[SQLColumns, fonction](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|L’extraction d’une seule ligne ou un bloc de données dans une direction avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retourner les colonnes d’une clé primaire|[SQLPrimaryKeys, fonction](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
