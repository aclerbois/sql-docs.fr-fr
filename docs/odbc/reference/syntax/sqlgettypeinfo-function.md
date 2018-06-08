---
title: Fonction SQLGetTypeInfo | Documents Microsoft
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
- SQLGetTypeInfo
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTypeInfo
helpviewer_keywords:
- SQLGetTypeInfo function [ODBC]
ms.assetid: bdedb044-8924-4ca4-85f3-8b37578e0257
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ac27290ce265a673e113d0cb59195708a34bebd7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlgettypeinfo-function"></a>Fonction SQLGetTypeInfo
**Mise en conformité**  
 Version introduite : Conformité de normes 1.0 ODBC : ISO 92  
  
 **Résumé**  
 **SQLGetTypeInfo** retourne des informations sur les types de données pris en charge par la source de données. Le pilote retourne les informations sous la forme d’un jeu de résultats SQL. Les types de données sont destinées à être utilisés dans les instructions de langage de définition de données (DDL).  
  
> [!IMPORTANT]  
>  Les applications doivent utiliser les noms de type retournés dans la colonne TYPE_NAME de le **SQLGetTypeInfo** jeu de résultats **ALTER TABLE** et **CREATE TABLE** instructions. **SQLGetTypeInfo** peut retourner plusieurs lignes ayant la même valeur dans la colonne DATA_TYPE.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLGetTypeInfo(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   DataType);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Handle d’instruction pour le jeu de résultats.  
  
 *DataType*  
 [Entrée] Le type de données SQL. Cela doit être une des valeurs dans le [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) section annexe d : les Types de données ou un type de données SQL spécifique au pilote. SQL_ALL_TYPES Spécifie que les informations sur tous les types de données doivent être renvoyées.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLGetTypeInfo** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLGetTypeInfo** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01S02|Valeur de l’option modifiée|Un attribut d’instruction spécifiée non valide en raison de conditions de travail de mise en œuvre, donc une valeur similaire a été remplacée temporairement. (Appeler **SQLGetStmtAttr** pour déterminer la valeur temporairement substituée.) La valeur de remplacement n’est valide pour le *au paramètre StatementHandle* jusqu'à ce que le curseur est fermé. Les attributs d’instruction qui peuvent être modifiés sont : SQL_ATTR_CONCURRENCY SQL_ATTR_CURSOR_TYPE, SQL_ATTR_KEYSET_SIZE, SQL_ATTR_MAX_LENGTH, SQL_ATTR_MAX_ROWS, SQL_ATTR_QUERY_TIMEOUT et SQL_ATTR_SIMULATE_CURSOR. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a été connecté avant le traitement de la fonction a été exécutée.|  
|24000|État de curseur non valide|Un curseur a été ouvert sur le *au paramètre StatementHandle,* et **SQLFetch** ou **SQLFetchScroll** avait été appelée. Cette erreur est retournée par le Gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **SQLFetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.<br /><br /> Un jeu de résultats a été ouvert sur le *au paramètre StatementHandle*, mais **SQLFetch** ou **SQLFetchScroll** n’avait pas été appelée.|  
|40001|Échec de la sérialisation|La transaction a été annulée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|Échec de la connexion associée lors de l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY004|Type de données SQL non valide|La valeur spécifiée pour l’argument *DataType* n’est ni un identificateur de type de données ODBC SQL valide ni un identificateur de type de données spécifique au pilote pris en charge par le pilote.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour le *au paramètre StatementHandle*, puis la fonction a été appelée et, avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle*. La fonction a été appelée à nouveau sur le *au paramètre StatementHandle*.<br /><br /> La fonction a été appelée et, avant qu’elle terminée l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *au paramètre StatementHandle* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque le **SQLGetTypeInfo** fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant que la source de données a retourné le jeu de résultats. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 **SQLGetTypeInfo** retourne les résultats sous la forme d’un jeu de résultats standard trié par DATA_TYPE, puis par le degré de parenté le type de données est mappé au type de données ODBC SQL correspondant. Types de données définis par la source de données sont prioritaires sur les types de données définis par l’utilisateur. Par conséquent, l’ordre de tri n’est pas nécessairement cohérent mais peut être généralisé que DATA_TYPE tout d’abord, suivi TYPE_NAME, à la fois par ordre croissant. Par exemple, supposons qu’une source de données défini par les types de données entier et le compteur, selon laquelle le compteur est auto-incrémentée, et qu’un type de données défini par l’utilisateur WHOLENUM a également été défini. Ces serait retournée dans l’ordre entier, WHOLENUM et le compteur, car mappe WHOLENUM étroitement aux données SQL ODBC type SQL_INTEGER, tandis que le type de données à incrémentation automatique, même si la prise en charge par la source de données ne mappe pas étroitement à un type de données ODBC SQL. Pour plus d’informations sur la façon dont ces informations peuvent être utilisées, consultez [instructions DDL](../../../odbc/reference/develop-app/ddl-statements.md).  
  
 Si le *DataType* argument spécifie un type de données qui est valide pour la version d’ODBC pris en charge par le pilote, mais n’est pas pris en charge par le pilote, puis il retourne un jeu de résultats vide.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données retournées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Les colonnes suivantes ont été renommés pour ODBC 3. *x*. Les changements de nom de colonne n’affectent pas la compatibilité descendante, car les applications lier par numéro de colonne.  
  
|Colonne de ODBC 2.0|ODBC 3. *x* colonne|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|MONEY|FIXED_PREC_SCALE|  
|AUTO_INCREMENT|AUTO_UNIQUE_VALUE|  
  
 Les colonnes suivantes ont été ajoutées au jeu de résultats retourné par **SQLGetTypeInfo** pour ODBC 3. *x*:  
  
-   SQL_DATA_TYPE  
  
-   INTERVAL_PRECISION  
  
-   SQL_DATETIME_SUB  
  
-   NUM_PREC_RADIX  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 19 (INTERVAL_PRECISION) peuvent être définies par le pilote. Une application doit accéder à des colonnes spécifiques aux pilotes à rebours à partir de la fin du jeu de résultats plutôt que de spécifier une position ordinale explicite. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
> [!NOTE]  
>  **SQLGetTypeInfo** peut ne pas retourner tous les types de données. Par exemple, un pilote peut ne pas retourne des types de données définis par l’utilisateur. Les applications peuvent utiliser n’importe quel type de données valide, indépendamment de si elle est retournée par **SQLGetTypeInfo**. Les types de données retournés par **SQLGetTypeInfo** sont celles prises en charge par la source de données. Elles sont prévues pour une utilisation dans les instructions de langage de définition de données (DDL). Pilotes peuvent retourner des données de jeu de résultats à l’aide des types de données autres que les types retournés par **SQLGetTypeInfo**. Pour créer le jeu de résultats pour une fonction de catalogue, le pilote peut utiliser un type de données qui n’est pas pris en charge par la source de données.  
  
|Nom de colonne|Colonne<br /><br /> nombre|Type de données|Commentaires|  
|-----------------|-----------------------|---------------|--------------|  
|TYPE_NAME (ODBC VERSION 2.0)|1|Varchar non NULL|Nom du type de données dépend de la source de données ; par exemple, « CHAR() », « VARCHAR() », « MONEY », « LONG VARBINARY » ou « () CHAR pour les données BIT ». Les applications doivent utiliser ce nom dans **CREATE TABLE** et **ALTER TABLE** instructions.|  
|DATA_TYPE (ODBC VERSION 2.0)|2|Smallint non NULL|Type de données SQL. Cela peut être un type de données SQL ODBC ou un type de données SQL spécifique au pilote. Pour les types de données datetime ou interval, cette colonne renvoie le type de données concis (par exemple, SQL_TYPE_TIME ou SQL_INTERVAL_YEAR_TO_MONTH). Pour obtenir la liste des types de données ODBC SQL valides, consultez [les Types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) annexe d : Types de données. Pour plus d’informations sur les types de données spécifiques au pilote SQL, consultez la documentation du pilote.|  
|COLUMN_SIZE (ODBC VERSION 2.0)|3|Entier|La taille de colonne maximale que le serveur prend en charge pour ce type de données. Pour les données numériques, il s’agit de la précision maximale. Pour les données de chaîne, il s’agit de la longueur en caractères. Pour les types de données datetime, il s’agit de la longueur en caractères de la représentation sous forme de chaîne (en supposant la précision maximale autorisée du composant en fractions de seconde). Valeur NULL est retournée pour les types de données où la taille de la colonne n’est pas applicable. Pour les types de données d’intervalle, il s’agit du nombre de caractères dans la représentation sous forme de caractère de l’intervalle de littéral (tel que défini par l’intervalle de début de précision ; consultez [longueur de Type de données de l’intervalle](../../../odbc/reference/appendixes/interval-data-type-length.md) annexe d : Types de données).<br /><br /> Pour plus d’informations sur la taille de colonne, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|LITERAL_PREFIX (ODBC VERSION 2.0)|4|Varchar|Ou les caractères utilisés comme préfixe pour un littéral ; par exemple, un guillemet simple (') pour les types de données caractère ou 0 x pour les types de données binaires. Valeur NULL est retournée pour les types de données où un préfixe n’est pas applicable.|  
|LITERAL_SUFFIX (ODBC VERSION 2.0)|5|Varchar|Ou les caractères utilisés pour mettre fin à un littéral ; par exemple, un guillemet simple (') pour les types de données caractère. Valeur NULL est retournée pour les types de données où un suffixe littéral n’est pas applicable.|  
|CREATE_PARAMS (ODBC VERSION 2.0)|6|Varchar|Une liste de mots-clés, séparés par des virgules, correspondant à chaque paramètre que l’application peut spécifier entre parenthèses lorsque vous utilisez le nom qui est retourné dans le champ TYPE_NAME. Les mots clés dans la liste peuvent être une des opérations suivantes : la longueur, la précision ou l’échelle. Ils apparaissent dans l’ordre que la syntaxe en a besoin pour être utilisé. Par exemple, CREATE_PARAMS de DECIMAL serait « précision, échelle » ; CREATE_PARAMS pour VARCHAR est égal à « longueur ». Valeur NULL est retournée si aucun paramètre pour la définition de type de données ; par exemple, d’entier.<br /><br /> Le pilote fournit le texte CREATE_PARAMS dans la langue du pays/région où il est utilisé.|  
|NULLABLE (ODBC VERSION 2.0)|7|Smallint non NULL|Si le type de données accepte une valeur NULL :<br /><br /> SQL_NO_NULLS si le type de données n’accepte pas les valeurs NULL.<br /><br /> SQL_NULLABLE si le type de données accepte les valeurs NULL.<br /><br /> SQL_NULLABLE_UNKNOWN si on ne sait pas si la colonne accepte les valeurs NULL.|  
|CASE_SENSITIVE (ODBC VERSION 2.0)|8|Smallint non NULL|Si les données de type caractère respecte la casse dans les comparaisons et les classements :<br /><br /> SQL_TRUE si le type de données est un type de données de caractères et respecte la casse.<br /><br /> SQL_FALSE si le type de données n’est pas un type de données caractère ou ne respecte pas la casse.|  
|RECHERCHE (ODBC VERSION 2.0)|9|Smallint non NULL|Comment le type de données est utilisé dans un **où** clause :<br /><br /> SQL_PRED_NONE si la colonne ne peut pas être utilisée dans un **où** clause. (Cela est identique à la valeur SQL_UNSEARCHABLE dans ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR si la colonne peut être utilisée dans un **où** clause, mais uniquement avec les **comme** prédicat. (Cela est identique à la valeur SQL_LIKE_ONLY dans ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC si la colonne peut être utilisée dans un **où** clause avec tous les opérateurs de comparaison sauf **comme** (comparaison, de comparaison, **BETWEEN**, **DISTINCT**, **IN**, **correspondance**, et **UNIQUE**). (Cela est identique à la valeur SQL_ALL_EXCEPT_LIKE dans ODBC 2. *x*.)<br /><br /> SQL_SEARCHABLE si la colonne peut être utilisée dans un **où** clause avec n’importe quel opérateur de comparaison.|  
|UNSIGNED_ATTRIBUTE (ODBC VERSION 2.0)|10|Smallint|Indique si le type de données n’est pas signé :<br /><br /> SQL_TRUE si le type de données n’est pas signé.<br /><br /> SQL_FALSE si le type de données est signé.<br /><br /> Valeur NULL est retournée si l’attribut n’est pas applicable au type de données ou le type de données n’est pas numérique.|  
|FIXED_PREC_SCALE (ODBC VERSION 2.0)|11|Smallint non NULL|Indique si le type de données a prédéfinies fixe la précision et l’échelle (qui sont spécifiques à la source de données), par exemple un type de données money :<br /><br /> Précision et échelle fixes SQL_TRUE si elle a prédéfinies.<br /><br /> SQL_FALSE si elle n’a pas de mise à l’échelle et une précision fixe prédéfinie.|  
|AUTO_UNIQUE_VALUE (ODBC VERSION 2.0)|12|Smallint|Indique si le type de données est incrémentée automatiquement :<br /><br /> SQL_TRUE si le type de données est à auto-incrémentation.<br /><br /> SQL_FALSE si le type de données n’est pas incrémentée automatiquement.<br /><br /> Valeur NULL est retournée si l’attribut n’est pas applicable au type de données ou le type de données n’est pas numérique.<br /><br /> Une application peut insérer des valeurs dans une colonne possédant cet attribut, mais en général, ne peut pas mettre à jour les valeurs dans la colonne.<br /><br /> Lorsqu’une instruction insert est effectué dans une colonne à incrémentation automatique, une valeur unique est insérée dans la colonne au moment de l’insertion. L’incrément n’est pas défini, mais il est spécifique à la source de données. Une application ne doit pas supposer qu’une colonne à incrémentation automatique au démarrage incréments à tout moment donné en une valeur particulière.|  
|LOCAL_TYPE_NAME (ODBC VERSION 2.0)|13|Varchar|Version localisée du nom de dépend de la source de données du type de données. La valeur NULL est retournée si un nom localisé n'est pas pris en charge par la source de données. Ce nom est conçu pour l’affichage uniquement, par exemple, dans les boîtes de dialogue.|  
|MINIMUM_SCALE (ODBC VERSION 2.0)|14|Smallint|Échelle minimale du type de données sur la source de données. Si un type de données possède une échelle fixe, les colonnes MINIMUM_SCALE et MAXIMUM_SCALE contiennent toutes les deux cette valeur. Par exemple, une colonne SQL_TYPE_TIMESTAMP peut-être une échelle fixe pour les fractions de seconde. La valeur NULL est retournée lorsque l'échelle n'est pas applicable. Pour plus d’informations, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|MAXIMUM_SCALE (ODBC VERSION 2.0)|15|Smallint|L’échelle maximale du type de données sur la source de données. La valeur NULL est retournée lorsque l'échelle n'est pas applicable. Si l’échelle maximale n’est pas définie séparément sur la source de données, mais il est défini à la place pour être le même que la précision maximale, cette colonne contient la même valeur que la colonne COLUMN_SIZE. Pour plus d’informations, consultez [taille de colonne, des chiffres décimaux, transférer la longueur en octets et la taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) annexe d : Types de données.|  
|SQL_DATA_TYPE (ODBC 3.0)|16|Smallint non NULL|La valeur du type de données SQL telle qu’elle apparaît dans le champ SQL_DESC_TYPE du descripteur. Cette colonne est la même que la colonne DATA_TYPE, à l’exception des types de données datetime et l’intervalle.<br /><br /> Pour les types de données datetime et l’intervalle, le champ SQL_DATA_TYPE dans le jeu de résultats renvoie SQL_INTERVAL ou SQL_DATETIME, et le champ SQL_DATETIME_SUB retournera le sous-code pour le type de données spécifique de l’intervalle ou datetime. (Consultez [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)|  
|SQL_DATETIME_SUB (ODBC 3.0)|17|Smallint|Lorsque la valeur de SQL_DATA_TYPE est SQL_DATETIME ou SQL_INTERVAL, cette colonne contient le sous-code/intervalle datetime. Pour les types de données autres que datetime et interval, ce champ est NULL.<br /><br /> Pour les types de données de l’intervalle ou datetime, le champ SQL_DATA_TYPE dans le jeu de résultats renvoie SQL_INTERVAL ou SQL_DATETIME, et le champ SQL_DATETIME_SUB retournera le sous-code pour le type de données spécifique de l’intervalle ou datetime. (Consultez [annexe d : les Types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)|  
|NUM_PREC_RADIX (ODBC 3.0)|18|Entier|Si le type de données est un type numérique approché, cette colonne contient la valeur 2 pour indiquer que COLUMN_SIZE spécifie un nombre de bits. Pour les types numériques exacts, cette colonne contient la valeur 10 pour indiquer que COLUMN_SIZE spécifie un nombre de chiffres décimaux. Sinon, cette colonne est NULL.|  
|INTERVAL_PRECISION (ODBC 3.0)|19|Smallint|Si le type de données est un type de données d’intervalle, cette colonne contient la valeur de la précision de début d’intervalle. (Consultez [précision du Type de données de l’intervalle](../../../odbc/reference/appendixes/interval-data-type-precision.md) annexe d : Types de données.) Sinon, cette colonne est NULL.|  
  
 Informations sur les attributs peuvent appliquer aux types de données ou à des colonnes spécifiques dans un jeu de résultats. **SQLGetTypeInfo** retourne des informations sur les attributs associés aux types de données. **SQLColAttribute** renvoie des informations sur les attributs associés aux colonnes dans un jeu de résultats.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[SQLColAttribute, fonction](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|L’extraction d’une seule ligne ou un bloc de données dans une direction avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Retour d’informations sur un pilote ou d’une source de données|[SQLGetInfo, fonction](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
