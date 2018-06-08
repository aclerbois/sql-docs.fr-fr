---
title: Fonction SQLStatistics | Documents Microsoft
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
- SQLStatistics
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLStatistics
helpviewer_keywords:
- SQLStatistics function [ODBC]
ms.assetid: 45210682-cfea-4e5d-9951-bcf1cbe10f41
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b5d7396e972d2c43798ca6a993c96355595e5e9c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlstatistics-function"></a>Fonction SQLStatistics
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLStatistics** récupère une liste des statistiques sur une seule table et les index associés à la table. Le pilote retourne les informations comme jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLStatistics(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CatalogName,  
     SQLSMALLINT     NameLength1,  
     SQLCHAR *       SchemaName,  
     SQLSMALLINT     NameLength2,  
     SQLCHAR *       TableName,  
     SQLSMALLINT     NameLength3,  
     SQLUSMALLINT    Unique,  
     SQLUSMALLINT    Reserved);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *Nom de catalogue*  
 [Entrée] Nom du catalogue. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de catalogues. *Nom de catalogue* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *CatalogName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *CatalogName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif. Pour plus d’informations, consultez [Arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 [Entrée] Longueur en caractères de **CatalogName*.  
  
 *schemaName*  
 [Entrée] Nom du schéma. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de schémas. *SchemaName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *SchemaName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *SchemaName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength2*  
 [Entrée] Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 [Entrée] Nom de la table. Cet argument ne peut pas être un pointeur null. *SchemaName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *TableName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *TableName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength3*  
 [Entrée] Longueur en caractères de **TableName*.  
  
 *Unique*  
 [Entrée] Type d’index : SQL_INDEX_UNIQUE ou SQL_INDEX_ALL.  
  
 *Réservé*  
 [Entrée] Indique l’importance des colonnes cardinalité et des PAGES dans le jeu de résultats. Les options suivantes affectent le retour des cardinalité et des PAGES uniquement des colonnes ; informations d’index sont retournées même si la cardinalité et des PAGES ne sont pas retournés.  
  
 SQL_ENSURE demande que le pilote de récupérer les statistiques. (Les pilotes qui sont conformes uniquement à la norme Open Group et ne prennent pas en charge les extensions ODBC pas sera en mesure de prendre en charge SQL_ENSURE.)  
  
 SQL_QUICK demande que le pilote de récupérer la cardinalité et des PAGES uniquement si elles sont disponibles à partir du serveur. Dans ce cas, le pilote ne s'assure pas que les valeurs sont actuelles. (Les applications qui sont écrites dans la norme Open Group obtenez toujours un comportement SQL_QUICK à partir d’ODBC 3 *.x*-pilotes compatibles.)  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLStatistics** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLStatistics** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|24000|État de curseur non valide|Un curseur a été ouvert sur le *au paramètre StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** avait été appelée. Cette erreur est retournée par le Gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **SQLFetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *au paramètre StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’avait pas été appelée.|  
|40001|Échec de la sérialisation|La transaction a été annulée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction, et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire qui est requis pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*, et la fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|Le *TableName* argument était un pointeur null.<br /><br /> L’attribut d’instruction SQL_ATTR_METADATA_ID a pris la valeur SQL_TRUE, le *CatalogName* argument était un pointeur null et le SQL_CATALOG_NAME *InfoType* retourne le nom de catalogue est pris en charge.<br /><br /> (DM), l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini avec SQL_TRUE et le *SchemaName* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLStatistics** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieur à 0 mais n’est pas égale à SQL_NTS.<br /><br /> La valeur de l’un des arguments de longueur de nom a dépassé la valeur de longueur maximale pour le nom correspondant.|  
|HY100|Type d’option unique hors limite|(DM) non valide *Unique* valeur a été spécifiée.|  
|HY101|Type d’option de précision hors limite|(DM) non valide *réservé* valeur a été spécifiée.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un catalogue a été spécifié, et le pilote ou une source de données ne prend pas en charge les catalogues.<br /><br /> Un schéma a été spécifié, et le pilote ou une source de données ne prend pas en charge les schémas.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant la source de données a retourné le jeu de résultat demandé. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLStatistics** retourne des informations sur une table unique comme un jeu de résultats standard, classé par NON_UNIQUE, TYPE, INDEX_QUALIFIER, INDEX_NAME et position ordinale. Le jeu de résultats combine les informations statistiques (dans les colonnes de cardinalité et des PAGES du jeu de résultats) pour la table avec des informations sur chaque index. Pour plus d’informations sur la façon dont ces informations peuvent être utilisées, consultez [utilise des données du catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Pour déterminer les longueurs réelles des colonnes TABLE_CAT, TABLE_SCHEM, TABLE_NAME et COLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données retournées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Les colonnes suivantes ont été renommés pour ODBC 3 *.x*. Les changements de nom de colonne n’affectent pas la compatibilité descendante, car les applications lier par numéro de colonne.  
  
|Colonne de ODBC 2.0|ODBC 3 *.x* colonne|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|SEQ_IN_INDEX|ORDINAL_POSITION|  
|COLLATION|ASC_OR_DESC|  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 13 (FILTER_CONDITION) peuvent être définies par le pilote. Une application doit accéder à des colonnes spécifiques aux pilotes à rebours à partir de la fin de l’ensemble au lieu de spécifier une position ordinale explicite de résultats. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC VERSION 1.0)|1|Varchar|Nom du catalogue de la table à laquelle l’index ou statistique s’applique ; NULL si non applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de catalogues.|  
|TABLE_SCHEM (ODBC 1.0)|2|Varchar|Nom du schéma de la table à laquelle l’index ou statistique s’applique ; NULL si non applicable à la source de données. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de schémas.|  
|TABLE_NAME (ODBC 1.0)|3|Varchar non NULL|Nom de la table de la table à laquelle s’applique l’index ou statistiques.|  
|NON_UNIQUE (ODBC 1.0)|4|Smallint|Indique si l’index n’autorise pas les valeurs en double :<br /><br /> SQL_TRUE si les valeurs d’index peuvent être non uniques.<br /><br /> SQL_FALSE si les valeurs d’index doivent être uniques.<br /><br /> Valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|INDEX_QUALIFIER (ODBC VERSION 1.0)|5|Varchar|Nom de l’identificateur qui est utilisé pour qualifier l’index de cette opération une **DROP INDEX**; Valeur NULL est retournée si l’identificateur d’index n’est pas pris en charge par la source de données ou si le TYPE est SQL_TABLE_STAT. Si une valeur non null est retournée dans cette colonne, il doit être utilisé pour qualifier le nom d’index sur une **DROP INDEX** instruction ; sinon, le TABLE_SCHEM doit être utilisé pour qualifier le nom de l’index.|  
|INDEX_NAME (ODBC 1.0)|6|Varchar|Nom de l’index ; Valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|TYPE (ODBC 1.0)|7|Smallint non NULL|Type d’informations renvoyées :<br /><br /> SQL_TABLE_STAT indique une statistique pour la table (dans la colonne de cardinalité ou de PAGES).<br /><br /> SQL_INDEX_BTREE indique un index B-Tree.<br /><br /> SQL_INDEX_CLUSTERED indique un index cluster.<br /><br /> SQL_INDEX_CONTENT indique un index de contenu.<br /><br /> SQL_INDEX_HASHED indique un index haché.<br /><br /> SQL_INDEX_OTHER indique un autre type d’index.|  
|POSITION ORDINALE (ODBC VERSION 1.0)|8|Smallint|Numéro de séquence de colonne dans l’index (à partir de 1) ; Valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|COLUMN_NAME (ODBC VERSION 1.0)|9|Varchar|Nom de colonne. Si la colonne est basée sur une expression, telles que le salaire + avantages, l’expression est retournée ; Si l’expression ne peut pas être déterminée, une chaîne vide est retournée. Valeur NULL est retournée si le TYPE est SQL_TABLE_STAT.|  
|ASC_OR_DESC (ODBC VERSION 1.0)|10|Char (1)|Séquence pour la colonne de tri : « A » pour l’ordre croissant ; « D » pour un tri décroissant ; Valeur NULL est retournée si la séquence de tri de la colonne n’est pas pris en charge par la source de données ou si le TYPE est SQL_TABLE_STAT.|  
|CARDINALITÉ (ODBC 1.0)|11|Entier|Cardinalité de la table ou l’index ; nombre de lignes dans la table si le TYPE est SQL_TABLE_STAT ; nombre de valeurs uniques dans l’index si TYPE n’est pas SQL_TABLE_STAT ; Valeur NULL est retournée si la valeur n’est pas disponible à partir de la source de données.|  
|PAGES (ODBC 1.0)|12|Entier|Nombre de pages utilisées pour stocker l’index ou la table ; nombre de pages pour la table, si le TYPE est SQL_TABLE_STAT ; nombre de pages de l’index si TYPE n’est pas SQL_TABLE_STAT ; Valeur NULL est retournée si la valeur n’est pas disponible à partir de la source de données ou si non applicable à la source de données.|  
|FILTER_CONDITION (ODBC VERSION 2.0)|13|Varchar|Si l’index est un index filtré, il s’agit de la condition de filtre, tels que les salaires > 30000 ; Si la condition de filtre ne peut pas être déterminée, il s’agit d’une chaîne vide.<br /><br /> NULL si l’index n’est pas un index filtré, il ne peut pas être déterminé si l’index est un index filtré ou le TYPE est SQL_TABLE_STAT.|  
  
 Si la ligne du jeu de résultats correspond à une table, le pilote définit le TYPE à SQL_TABLE_STAT et définit NON_UNIQUE, INDEX_QUALIFIER, INDEX_NAME, ORDINAL_POSITION, COLUMN_NAME et ASC_OR_DESC avec la valeur NULL. Si la cardinalité ou PAGES ne sont pas disponibles à partir de la source de données, le pilote les affecte à NULL.  
  
## <a name="code-example"></a>Exemple de code  
 Pour obtenir un exemple de code d’une fonction similaire, consultez [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’une seule ligne ou un bloc de données dans une direction avant uniquement.|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retourner les colonnes de clés étrangères|[SQLForeignKeys, fonction](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|  
|Retourner les colonnes d’une clé primaire|[SQLPrimaryKeys, fonction](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
