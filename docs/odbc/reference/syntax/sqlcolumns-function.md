---
title: Fonction SQLColumns | Documents Microsoft
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
- SQLColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColumns
helpviewer_keywords:
- SQLColumns function [ODBC]
ms.assetid: 4a3618b7-d2b8-43c6-a1fd-7a4e6fa8c7d0
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d48fad8c874a9edda7da1afbe2f006726c39ee5b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcolumns-function"></a>Fonction SQLColumns
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ouvrir le groupe  
  
 **Résumé**  
 **SQLColumns** renvoie la liste des noms de colonnes dans les tables spécifiées. Le pilote retourne ces informations comme jeu de résultats spécifié *au paramètre StatementHandle*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLColumns(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      TableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      ColumnName,  
     SQLSMALLINT    NameLength4);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *Nom de catalogue*  
 [Entrée] Nom du catalogue. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de catalogues. *Nom de catalogue* ne peut pas contenir un modèle de recherche de chaîne.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *CatalogName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *CatalogName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif. Pour plus d’informations, consultez [Arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 [Entrée] Longueur en caractères de **CatalogName*.  
  
 *schemaName*  
 [Entrée] Modèle de recherche de chaîne pour les noms de schéma. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de schémas.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *SchemaName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *SchemaName* est un argument de valeur de modèle ; il est traité littéralement, et ses cas est significatif.  
  
 *NameLength2*  
 [Entrée] Longueur en caractères de **SchemaName*.  
  
 *TableName*  
 [Entrée] Modèle de recherche de chaîne pour les noms de table.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *TableName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *TableName* est un argument de valeur de modèle ; il est traité littéralement, et ses cas est significatif.  
  
 *NameLength3*  
 [Entrée] Longueur en caractères de **TableName*.  
  
 *ColumnName*  
 [Entrée] Modèle de recherche de chaîne pour les noms de colonne.  
  
> [!NOTE]  
>  Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *ColumnName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *ColumnName* est un argument de valeur de modèle ; il est traité littéralement, et ses cas est significatif.  
  
 *NameLength4*  
 [Entrée] Longueur en caractères de **ColumnName*.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLColumns** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLColumns** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|24000|État de curseur non valide|Un curseur a été ouvert sur le *au paramètre StatementHandle*, et **SQLFetch** ou **SQLFetchScroll** avait été appelée. Cette erreur est retournée par le Gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **SQLFetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un curseur a été ouvert sur le *au paramètre StatementHandle* mais **SQLFetch** ou **SQLFetchScroll** n’avait pas été appelée.|  
|40001|Échec de la sérialisation|La transaction a été annulée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction, et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire qui est requis pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*. La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’attribut d’instruction SQL_ATTR_METADATA_ID a pris la valeur SQL_TRUE, le *CatalogName* argument était un pointeur null et le SQL_CATALOG_NAME *InfoType* retourne le nom de catalogue est pris en charge.<br /><br /> (DM), l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini avec SQL_TRUE et le *SchemaName*, *TableName*, ou *ColumnName* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLColumns** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieur à 0 mais n’est pas égale à SQL_NTS.|  
|||La valeur de l’un des arguments de longueur de nom a dépassé la valeur de longueur maximale pour le nom ou le catalogue correspondant. La longueur maximale de chaque catalogue ou le nom peut être obtenue en appelant **SQLGetInfo** avec la *InfoType* valeurs. (Voir « Commentaires ».)|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un nom de catalogue a été spécifié, et le pilote ou une source de données ne prend pas en charge les catalogues.<br /><br /> Un nom de schéma a été spécifié, et le pilote ou une source de données ne prend pas en charge les schémas.<br /><br /> Un modèle de recherche de chaîne a été spécifié pour le nom de schéma, le nom de la table ou le nom de colonne, et la source de données ne prend pas en charge les modèles de recherche pour un ou plusieurs de ces arguments.<br /><br /> La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant que la source de données a retourné le jeu de résultats. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Cette fonction est généralement utilisée avant l’exécution d’instruction pour récupérer des informations sur les colonnes pour une table ou les tables du catalogue de la source de données. **SQLColumns** peut être utilisé pour récupérer des données pour tous les types d’éléments retournés par **SQLTables**. En plus des tables de base, cela peut inclure (mais n’est pas limité à) des vues, synonymes, tables système et ainsi de suite. En revanche, les fonctions **SQLColAttribute** et **SQLDescribeCol** décrivent les colonnes dans un jeu de résultats et la fonction **SQLNumResultCols** retourne le nombre de colonnes dans un jeu de résultats. Pour plus d’informations, consultez [utilise des données du catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données retournées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLColumns** retourne les résultats sous la forme d’un jeu de résultats standard classé par TABLE_CAT, TABLE_SCHEM, TABLE_NAME et position ordinale.  
  
> [!NOTE]  
>  Quand une application fonctionne avec une API ODBC 2. *x* pilote, aucune colonne ORDINAL_POSITION n’est retourné dans le jeu de résultats. Par conséquent, lorsque vous travaillez avec ODBC 2. *x* pilotes, l’ordre des colonnes dans la liste des colonnes retournées par **SQLColumns** n'est pas nécessairement le même que l’ordre des colonnes retourné lorsque l’application exécute une instruction SELECT sur toutes les colonnes de cette table.  
  
> [!NOTE]  
>  **SQLColumns** peut ne pas retourner toutes les colonnes. Par exemple, un pilote peut ne pas retourne des informations sur les pseudo-colonnes, telles que Oracle ROWID. Applications peuvent utiliser n’importe quelle colonne valide, si elle est retournée par **SQLColumns**.  
>   
>  Certaines colonnes peuvent être retournées par **SQLStatistics** ne sont pas retournés par **SQLColumns**. Par exemple, **SQLColumns** ne retourne pas les colonnes dans un index créé sur une expression ou d’un filtre, par exemple le salaire + avantages ou DEPT = 0012.  
  
 Les longueurs des colonnes VARCHAR ne figurent pas dans la table ; les longueurs réelles dépendant de la source de données. Pour déterminer les longueurs réelles des colonnes TABLE_CAT, TABLE_SCHEM, TABLE_NAME et COLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
 Les colonnes suivantes ont été renommés pour ODBC 3. *x*. Les changements de nom de colonne n’affectent pas la compatibilité descendante, car les applications lier par numéro de colonne.  
  
|Colonne de ODBC 2.0|ODBC 3. *x* colonne|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Les colonnes suivantes ont été ajoutées au jeu de résultats retourné par **SQLColumns** pour ODBC 3. *x*:  
  
|||  
|-|-|  
|CHAR_OCTET_LENGTH|ORDINAL_POSITION|  
|COLUMN_DEF|SQL_DATA_TYPE|  
|IS_NULLABLE|SQL_DATETIME_SUB|  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 18 (IS_NULLABLE) peuvent être définies par le pilote. Une application doit accéder à des colonnes spécifiques aux pilotes à rebours à partir de la fin de l’ensemble au lieu de spécifier une position ordinale explicite de résultats. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de colonne|Colonne<br /><br /> nombre|Type de données|Commentaires|  
|-----------------|-----------------------|---------------|--------------|  
|TABLE_CAT (ODBC VERSION 1.0)|1|Varchar|Nom de catalogue ; NULL si non applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de catalogues.|  
|TABLE_SCHEM (ODBC 1.0)|2|Varchar|Nom du schéma ; NULL si non applicable à la source de données. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de schémas.|  
|TABLE_NAME (ODBC 1.0)|3|Varchar non NULL|Nom de la table.|  
|COLUMN_NAME (ODBC VERSION 1.0)|4|Varchar non NULL|Nom de colonne. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|DATA_TYPE (ODBC 1.0)|5|Smallint non NULL|Type de données SQL. Cela peut être un type de données SQL ODBC ou un type de données SQL spécifique au pilote. Pour les types de données datetime et interval, cette colonne renvoie le type de données concis (par exemple, SQL_TYPE_DATE ou SQL_INTERVAL_YEAR_TO_MONTH, au lieu du type de données nonconcise comme SQL_DATETIME ou SQL_INTERVAL). Pour obtenir la liste des types de données ODBC SQL valides, consultez [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) annexe d : Types de données. Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.<br /><br /> Les types de données retournées pour ODBC 3. *x* et ODBC 2. *x* applications peuvent être différentes. Pour plus d’informations, consultez [la compatibilité descendante et la conformité aux normes](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|TYPE_NAME (ODBC VERSION 1.0)|6|Varchar non NULL|Nom du type de données de dépend de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINAR » ou « () CHAR pour les données BIT ».|  
|COLUMN_SIZE (ODBC 1.0)|7|Entier|Si la valeur DATA_TYPE est SQL_CHAR ou SQL_VARCHAR, cette colonne contient la longueur maximale en caractères de la colonne. Pour les types de données datetime, il s’agit du nombre total de caractères requis pour afficher la valeur lorsqu’elle est convertie en caractères. Pour les types de données numériques, il s’agit du nombre total de chiffres ou le nombre total de bits autorisées dans la colonne, en fonction de la colonne NUM_PREC_RADIX. Pour les types de données d’intervalle, il s’agit du nombre de caractères dans la représentation sous forme de caractère de l’intervalle de littéral (tel que défini par l’intervalle de précision d’en-tête, consultez [longueur de Type de données de l’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md) annexe d : Types de données). Pour plus d’informations, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|(ODBC 1.0) DE BUFFER_LENGTH|8|Entier|La longueur en octets des données transférées sur une opération de SQLGetData, SQLFetch ou SQLFetchScroll si SQL_C_DEFAULT est spécifié. Pour les données numériques, cette taille peut différer de la taille des données stockées sur la source de données. Cette valeur peut différer de la colonne COLUMN_SIZE pour les données caractères. Pour plus d’informations sur la longueur, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|DECIMAL_DIGITS (ODBC VERSION 1.0)|9|Smallint|Nombre total de chiffres significatifs à droite de la virgule décimale. Pour SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP, cette colonne contient le nombre de chiffres dans le composant fractions de seconde. Pour les autres types de données, il s’agit des chiffres décimaux de la colonne sur la source de données. Pour les types de données d’intervalle qui contiennent un composant d’heure, cette colonne contient le nombre de chiffres à droite du séparateur décimal (en fractions de seconde). Pour les types de données d’intervalle qui ne contiennent pas d’un composant d’heure, cette colonne est 0. Pour plus d’informations sur les chiffres décimaux, voir [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données. Valeur NULL est retournée pour les types de données où DECIMAL_DIGITS n’est pas applicable.|  
|NUM_PREC_RADIX (ODBC VERSION 1.0)|10|Smallint|Pour les types de données numériques, 10 ou 2. Si elle est de 10, les valeurs COLUMN_SIZE et DECIMAL_DIGITS obtenir le nombre de chiffres autorisé pour la colonne. Par exemple, une colonne DECIMAL(12,5) renvoie un NUM_PREC_RADIX de 10, un COLUMN_SIZE 12 et un DECIMAL_DIGITS 5 ; une colonne de type FLOAT peut retourner un NUM_PREC_RADIX de 10, un COLUMN_SIZE 15 et DECIMAL_DIGITS est NULL.<br /><br /> S’il est 2, les valeurs COLUMN_SIZE et DECIMAL_DIGITS donnent le nombre de bits autorisées dans la colonne. Par exemple, une colonne de type FLOAT peut retourner une base de 2, un COLUMN_SIZE 53 et DECIMAL_DIGITS est NULL.<br /><br /> Valeur NULL est retournée pour les types de données où NUM_PREC_RADIX n’est pas applicable.|  
|NULLABLE (ODBC VERSION 1.0)|11|Smallint non NULL|SQL_NO_NULLS si la colonne ne peut pas inclure de valeurs NULL.<br /><br /> SQL_NULLABLE si la colonne accepte les valeurs NULL.<br /><br /> SQL_NULLABLE_UNKNOWN si on ne sait pas si la colonne accepte les valeurs NULL.<br /><br /> La valeur renvoyée pour cette colonne diffère de celle renvoyée pour la colonne IS_NULLABLE. La colonne accepte les valeurs NULL indique avec certitude qu’une colonne peut accepter les valeurs NULL, mais ne peut pas indiquer avec certitude qu’une colonne n’accepte pas les valeurs NULL. La colonne IS_NULLABLE indique avec certitude qu’une colonne ne peut pas accepter les valeurs NULL, mais ne peut pas indiquer avec certitude qu’une colonne accepte les valeurs NULL.|  
|SECTION NOTES (ODBC 1.0)|12|Varchar|Description de la colonne.|  
|COLUMN_DEF (ODBC 3.0)|13|Varchar|La valeur par défaut de la colonne. La valeur de cette colonne doit être interprétée comme une chaîne si elle est placée entre guillemets.<br /><br /> Si la valeur NULL a été spécifiée en tant que la valeur par défaut, cette colonne est le mot NULL, ne pas entourée guillemets. Si la valeur par défaut ne peut pas être représentée sans troncation, cette colonne contient tronquées, sans placer entre guillemets simples. Si aucune valeur par défaut a été spécifié, cette colonne est NULL.<br /><br /> La valeur du COLUMN_DEF peut être utilisée pour générer une nouvelle définition de colonne, sauf lorsqu’elle contient la valeur tronquées.|  
|SQL_DATA_TYPE (ODBC 3.0)|14|Smallint non NULL|Type de données SQL, tel qu’il apparaît dans le champ d’enregistrement SQL_DESC_TYPE dans l’IRD. Cela peut être un type de données SQL ODBC ou un type de données SQL spécifique au pilote. Cette colonne est la même que la colonne DATA_TYPE, à l’exception des types de données datetime et interval. Cette colonne renvoie le type de données nonconcise (par exemple, SQL_DATETIME ou SQL_INTERVAL), au lieu du type de données concis (comme SQL_TYPE_DATE ou SQL_INTERVAL_YEAR_TO_MONTH) pour datetime et les types de données interval. Si cette colonne retourne SQL_DATETIME ou SQL_INTERVAL, le type de données spécifique peut être déterminé à partir de la colonne SQL_DATETIME_SUB. Pour obtenir la liste des types de données ODBC SQL valides, consultez [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) annexe d : Types de données. Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.<br /><br /> Les types de données retournées pour ODBC 3. *x* et ODBC 2. *x* applications peuvent être différentes. Pour plus d’informations, consultez [la compatibilité descendante et la conformité aux normes](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|SQL_DATETIME_SUB (ODBC 3.0)|15|Smallint|Code de sous-type pour les types de données datetime et interval. Pour les autres types de données, cette colonne renvoie une valeur NULL. Pour plus d’informations sur les codes subordonnés datetime et interval, consultez « SQL_DESC_DATETIME_INTERVAL_CODE » dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).|  
|CHAR_OCTET_LENGTH (ODBC 3.0)|16|Entier|Colonne de type de la longueur maximale en octets de données binaire ou caractère. Pour tous les autres types de données, cette colonne retourne une valeur NULL.|  
|POSITION ORDINALE (ODBC 3.0)|17|Entier non NULL|Position ordinale de la colonne dans la table. La première colonne dans la table est le numérique 1.|  
|IS_NULLABLE (ODBC 3.0)|18|Varchar|« NON » si la colonne n’inclut pas les valeurs NULL.<br /><br /> « YES » si la colonne peut inclure des valeurs NULL.<br /><br /> Cette colonne renvoie une chaîne de longueur zéro si la possibilité de valeurs Null n'est pas connue.<br /><br /> Les règles ISO sont utilisées pour déterminer la possibilité de valeur Null. Un SGBD compatible ISO SQL ne peut pas retourner une chaîne vide.<br /><br /> La valeur renvoyée pour cette colonne diffère de celle renvoyée pour la colonne NULLABLE. (Consultez la description de la colonne accepte les valeurs NULL.)|  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application déclare les mémoires tampons pour le jeu de résultats retourné par **SQLColumns**. Il appelle **SQLColumns** pour retourner un jeu de résultats qui décrit chaque colonne dans la table EMPLOYEE. Il appelle ensuite **SQLBindCol** pour lier les colonnes dans le jeu de résultats pour les mémoires tampons. Enfin, l’application extrait chaque ligne de données avec **SQLFetch** et le traite.  
  
```  
// SQLColumns_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#define STR_LEN 128 + 1  
#define REM_LEN 254 + 1  
  
// Declare buffers for result set data  
SQLCHAR szSchema[STR_LEN];  
SQLCHAR szCatalog[STR_LEN];  
SQLCHAR szColumnName[STR_LEN];  
SQLCHAR szTableName[STR_LEN];  
SQLCHAR szTypeName[STR_LEN];  
SQLCHAR szRemarks[REM_LEN];  
SQLCHAR szColumnDefault[STR_LEN];  
SQLCHAR szIsNullable[STR_LEN];  
  
SQLINTEGER ColumnSize;  
SQLINTEGER BufferLength;  
SQLINTEGER CharOctetLength;  
SQLINTEGER OrdinalPosition;  
  
SQLSMALLINT DataType;  
SQLSMALLINT DecimalDigits;  
SQLSMALLINT NumPrecRadix;  
SQLSMALLINT Nullable;  
SQLSMALLINT SQLDataType;  
SQLSMALLINT DatetimeSubtypeCode;  
  
SQLHSTMT hstmt = NULL;  
  
// Declare buffers for bytes available to return  
SQLINTEGER cbCatalog;  
SQLINTEGER cbSchema;  
SQLINTEGER cbTableName;  
SQLINTEGER cbColumnName;  
SQLINTEGER cbDataType;  
SQLINTEGER cbTypeName;  
SQLINTEGER cbColumnSize;  
SQLLEN cbBufferLength;  
SQLINTEGER cbDecimalDigits;  
SQLINTEGER cbNumPrecRadix;  
SQLINTEGER cbNullable;  
SQLINTEGER cbRemarks;  
SQLINTEGER cbColumnDefault;  
SQLINTEGER cbSQLDataType;  
SQLINTEGER cbDatetimeSubtypeCode;  
SQLINTEGER cbCharOctetLength;  
SQLINTEGER cbOrdinalPosition;  
SQLINTEGER cbIsNullable;  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   retcode = SQLColumns(hstmt, NULL, 0, NULL, 0, (SQLCHAR*)"CUSTOMERS", SQL_NTS, NULL, 0);  
  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      // Bind columns in result set to buffers  
      SQLBindCol(hstmt, 1, SQL_C_CHAR, szCatalog, STR_LEN,&cbCatalog);  
      SQLBindCol(hstmt, 2, SQL_C_CHAR, szSchema, STR_LEN, &cbSchema);  
      SQLBindCol(hstmt, 3, SQL_C_CHAR, szTableName, STR_LEN,&cbTableName);  
      SQLBindCol(hstmt, 4, SQL_C_CHAR, szColumnName, STR_LEN, &cbColumnName);  
      SQLBindCol(hstmt, 5, SQL_C_SSHORT, &DataType, 0, &cbDataType);  
      SQLBindCol(hstmt, 6, SQL_C_CHAR, szTypeName, STR_LEN, &cbTypeName);  
      SQLBindCol(hstmt, 7, SQL_C_SLONG, &ColumnSize, 0, &cbColumnSize);  
      SQLBindCol(hstmt, 8, SQL_C_SLONG, &BufferLength, 0, &cbBufferLength);  
      SQLBindCol(hstmt, 9, SQL_C_SSHORT, &DecimalDigits, 0, &cbDecimalDigits);  
      SQLBindCol(hstmt, 10, SQL_C_SSHORT, &NumPrecRadix, 0, &cbNumPrecRadix);  
      SQLBindCol(hstmt, 11, SQL_C_SSHORT, &Nullable, 0, &cbNullable);  
      SQLBindCol(hstmt, 12, SQL_C_CHAR, szRemarks, REM_LEN, &cbRemarks);  
      SQLBindCol(hstmt, 13, SQL_C_CHAR, szColumnDefault, STR_LEN, &cbColumnDefault);  
      SQLBindCol(hstmt, 14, SQL_C_SSHORT, &SQLDataType, 0, &cbSQLDataType);  
      SQLBindCol(hstmt, 15, SQL_C_SSHORT, &DatetimeSubtypeCode, 0, &cbDatetimeSubtypeCode);  
      SQLBindCol(hstmt, 16, SQL_C_SLONG, &CharOctetLength, 0, &cbCharOctetLength);  
      SQLBindCol(hstmt, 17, SQL_C_SLONG, &OrdinalPosition, 0, &cbOrdinalPosition);  
      SQLBindCol(hstmt, 18, SQL_C_CHAR, szIsNullable, STR_LEN, &cbIsNullable);  
  
      while (SQL_SUCCESS == retcode) {  
         retcode = SQLFetch(hstmt);  
         /*  
         if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // show_error();  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // Process fetched data  
         else  
            break;  
        */  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour des privilèges pour une ou plusieurs colonnes|[SQLColumnPrivileges, fonction](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Retourner des colonnes qui identifient de manière unique une ligne, ou des colonnes automatiquement mises à jour par une transaction|[SQLSpecialColumns, fonction](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|  
|Retourner l’index et les statistiques de table|[SQLStatistics, fonction](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|Retour d’une liste de tables dans une source de données|[SQLTables, fonction](../../../odbc/reference/syntax/sqltables-function.md)|  
|Retour de privilèges pour une table ou de tables|[SQLTablePrivileges, fonction](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
