---
title: Fonction SQLForeignKeys | Documents Microsoft
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
- SQLForeignKeys
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLForeignKeys
helpviewer_keywords:
- SQLForeignKeys function [ODBC]
ms.assetid: 07f3f645-f643-4d39-9a10-70a72f24e608
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c5e90ce8d272b7345a9cd6f450df6293c63d4151
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlforeignkeys-function"></a>Fonction SQLForeignKeys
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 1.0 ODBC : ODBC  
  
 **Résumé**  
 **SQLForeignKeys** peut retourner :  
  
-   Une liste de clés étrangères dans la table spécifiée (les colonnes de la table spécifiée qui font référence aux clés primaires d’autres tables).  
  
-   Une liste de clés étrangères dans d’autres tables qui font référence à la clé primaire de la table spécifiée.  
  
 Le pilote retourne chaque liste comme un jeu de résultats sur l’instruction spécifiée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLForeignKeys(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      PKCatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      PKSchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      PKTableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      FKCatalogName,  
     SQLSMALLINT    NameLength4,  
     SQLCHAR *      FKSchemaName,  
     SQLSMALLINT    NameLength5,  
     SQLCHAR *      FKTableName,  
     SQLSMALLINT    NameLength6);  
```  
  
## <a name="arguments"></a>Arguments  
 *Au paramètre StatementHandle*  
 [Entrée] Descripteur d’instruction.  
  
 *PKCatalogName*  
 [Entrée] Nom du catalogue de table de clé primaire. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de catalogues. *PKCatalogName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *PKCatalogName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *PKCatalogName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif. Pour plus d’informations, consultez [Arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 [Entrée] Longueur de **PKCatalogName*, en caractères.  
  
 *PKSchemaName*  
 [Entrée] Nom du schéma de table de clé primaire. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de schémas. *PKSchemaName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *PKSchemaName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *PKSchemaName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength2*  
 [Entrée] Longueur de **PKSchemaName*, en caractères.  
  
 *PKTableName*  
 [Entrée] Nom de la table de clé primaire. *PKTableName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *PKTableName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *PKTableName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength3*  
 [Entrée] Longueur de **PKTableName*, en caractères.  
  
 *FKCatalogName*  
 [Entrée] Nom du catalogue de table de clé étrangère. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de catalogues. *FKCatalogName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *FKCatalogName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *FKCatalogName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength4*  
 [Entrée] Longueur de **FKCatalogName*, en caractères.  
  
 *FKSchemaName*  
 [Entrée] Nom du schéma de table de clé étrangère. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, une chaîne vide (« ») indique les tables qui n’ont pas de schémas. *FKSchemaName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *FKSchemaName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *FKSchemaName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength5*  
 [Entrée] Longueur de **FKSchemaName*, en caractères.  
  
 *FKTableName*  
 [Entrée] Nom de la table de clé étrangère. *FKTableName* ne peut pas contenir un modèle de recherche de chaîne.  
  
 Si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_TRUE, *FKTableName* est traité comme un identificateur et ses cas n’est pas significatif. S’il s’agit de SQL_FALSE, *FKTableName* est un argument ordinaire ; elle est traitée littéralement, et ses cas est significatif.  
  
 *NameLength6*  
 [Entrée] Longueur de **FKTableName*, en caractères.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLForeignKeys** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer* de *au paramètre StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLForeignKeys** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
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
|HY009|Utilisation non valide d’un pointeur null|(DM) les arguments *PKTableName* et *FKTableName* ont été les deux pointeurs null.<br /><br /> L’attribut d’instruction SQL_ATTR_METADATA_ID a pris la valeur SQL_TRUE, le *FKCatalogName* ou *PKCatalogName* argument était un pointeur null et le SQL_CATALOG_NAME *InfoType* retourne le nom de catalogue est pris en charge.<br /><br /> (DM), l’attribut d’instruction SQL_ATTR_METADATA_ID a été défini avec SQL_TRUE et le *FKSchemaName*, *PKSchemaName*, *FKTableName*, ou *PKTableName* argument était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution a été appelée pour le handle de connexion qui est associé à la *au paramètre StatementHandle*. Cette fonction asynchrone toujours en cours d’exécution lorsque la fonction SQLForeignKeys a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, ou **SQLMoreResults** a été appelé pour le *au paramètre StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant la récupération des données pour tous les paramètres transmis en continu.<br /><br /> (DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *au paramètre StatementHandle* et toujours en cours d’exécution lorsque cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations**, ou **SQLSetPos** a été appelé pour le *au paramètre StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi de données pour tous les paramètres de data-at-execution ou les colonnes.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur de l’un des arguments de longueur de nom était inférieur à 0 mais n’est pas égale à SQL_NTS.|  
|||La valeur de l’un des arguments de longueur de nom a dépassé la valeur de longueur maximale pour le nom correspondant. (Voir « Commentaires ».)|  
|HY117|Connexion est interrompue en raison de l’état de transaction inconnu. Déconnecter uniquement et les fonctions en lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Un nom de catalogue a été spécifié, et le pilote ou une source de données ne prend pas en charge les catalogues.<br /><br /> Un nom de schéma a été spécifié, et le pilote ou une source de données ne prend pas en charge les schémas.|  
|||La combinaison des paramètres actuels des attributs d’instruction SQL_ATTR_CONCURRENCY et SQL_ATTR_CURSOR_TYPE n’était pas pris en charge par la source de données ou de pilote.<br /><br /> L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a été défini sur SQL_UB_VARIABLE, et l’attribut d’instruction SQL_ATTR_CURSOR_TYPE a été défini pour un type de curseur pour lequel le pilote ne prend pas en charge les signets.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de requête a expiré avant que la source de données a retourné le jeu de résultats. Le délai d’expiration est défini par le **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote associé à la *au paramètre StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Pour plus d’informations sur la façon dont les informations retournées par cette fonction peuvent être utilisées, consultez [utilise des données du catalogue](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Si \* *PKTableName* contient un nom de table, **SQLForeignKeys** renvoie un jeu de résultats qui contient la clé primaire de la table spécifiée et toutes les clés étrangères qui font référence à celui-ci. La liste des clés étrangères dans d’autres tables n’inclut pas de clés étrangères qui pointent vers des contraintes uniques dans la table spécifiée.  
  
 Si \* *FKTableName* contient un nom de table, **SQLForeignKeys** renvoie un jeu de résultats qui contient toutes les clés étrangères dans la table spécifiée qui pointent vers des clés primaires dans d’autres tables et les clés primaires dans les autres tables auxquelles ils font référence. La liste des clés étrangères dans la table spécifiée ne contient pas de clés étrangères qui font référence à des contraintes uniques dans d’autres tables.  
  
 Si les deux \* *PKTableName* et \* *FKTableName* contiennent les noms de table, **SQLForeignKeys** renvoie les clés étrangères dans la table spécifiée dans \* *FKTableName* qui font référence à la clé primaire de la table spécifiée dans **PKTableName*. Ce doit être une clé au maximum.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation générale, les arguments et les données retournées des fonctions de catalogue ODBC, consultez [fonctions de catalogue](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLForeignKeys** retourne les résultats sous la forme d’un jeu de résultats standard. Si les clés étrangères associées à une clé primaire sont demandées, le jeu de résultats est trié par FKTABLE_CAT, FKTABLE_SCHEM, FKTABLE_NAME et KEY_SEQ. Si les clés primaires associés à une clé étrangère sont demandées, le jeu de résultats est trié par PKTABLE_CAT, PKTABLE_SCHEM, nom_de_la_pktable et KEY_SEQ. Le tableau suivant répertorie les colonnes du jeu de résultats.  
  
 Les longueurs des colonnes VARCHAR ne figurent pas dans la table ; les longueurs réelles dépendant de la source de données. Pour déterminer la longueur réelle du PKTABLE_CAT FKTABLE_CAT, PKTABLE_SCHEM ou FKTABLE_SCHEM, colonnes nom_de_la_pktable ou FKTABLE_NAME et PKCOLUMN_NAME ou FKCOLUMN_NAME, une application peut appeler **SQLGetInfo** avec les options SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN et SQL_MAX_COLUMN_NAME_LEN.  
  
 Les colonnes suivantes ont été renommés pour ODBC 3 *. x.* Les changements de nom de colonne n’affectent pas la compatibilité descendante, car les applications lier par numéro de colonne.  
  
|Colonne de ODBC 2.0|ODBC 3 *.x* colonne|  
|---------------------|-----------------------|  
|PKTABLE_QUALIFIER|PKTABLE_CAT|  
|PKTABLE_OWNER|PKTABLE_SCHEM|  
|FKTABLE_QUALIFIER|FK_TABLE_CAT|  
|FKTABLE_OWNER|FKTABLE_SCHEM|  
  
 Le tableau suivant répertorie les colonnes du jeu de résultats. Les colonnes supplémentaires au-delà de la colonne 14 (Remarques) peuvent être définies par le pilote. Une application doit accéder à des colonnes spécifiques aux pilotes à rebours à partir de la fin de l’ensemble au lieu de spécifier une position ordinale explicite de résultats. Pour plus d’informations, consultez [les données renvoyées par les fonctions de catalogue](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nom de colonne|Numéro de colonne|Type de données|Commentaires|  
|-----------------|-------------------|---------------|--------------|  
|PKTABLE_CAT (ODBC VERSION 1.0)|1|Varchar|Nom du catalogue de table de clé primaire ; NULL si non applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de catalogues.|  
|PKTABLE_SCHEM (ODBC VERSION 1.0)|2|Varchar|Nom du schéma de table de clé primaire ; NULL si non applicable à la source de données. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de schémas.|  
|NOM_DE_LA_PKTABLE (ODBC 1.0)|3|Varchar non NULL|Nom de la table de clé primaire.|  
|PKCOLUMN_NAME (ODBC VERSION 1.0)|4|Varchar non NULL|Nom de la colonne de clé primaire. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|FKTABLE_CAT (ODBC VERSION 1.0)|5|Varchar|Nom du catalogue de table de clés étrangères ; NULL si non applicable à la source de données. Si un pilote prend en charge les catalogues pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de catalogues.|  
|FKTABLE_SCHEM (ODBC VERSION 1.0)|6|Varchar|Nom du schéma de table de clés étrangères ; NULL si non applicable à la source de données. Si un pilote prend en charge les schémas pour certaines tables, mais pas pour d’autres, telles que lorsque le pilote récupère les données à partir de différents SGBD, il retourne une chaîne vide (« ») pour les tables qui n’ont pas de schémas.|  
|FKTABLE_NAME (ODBC 1.0)|7|Varchar non NULL|Nom de la table de clé étrangère.|  
|FKCOLUMN_NAME (ODBC VERSION 1.0)|8|Varchar non NULL|Nom de la colonne de clé étrangère. Le pilote retourne une chaîne vide pour une colonne qui n’a pas de nom.|  
|KEY_SEQ (ODBC 1.0)|9|Smallint non NULL|Numéro de séquence de colonne de clé (en commençant par 1).|  
|UPDATE_RULE (ODBC 1.0)|10|Smallint|Action à appliquer à la clé étrangère lorsque l’opération SQL est **mise à jour**. Peut avoir l’une des valeurs suivantes. (La table référencée est la table qui contient la clé primaire ; la table de référence est la table qui contient la clé étrangère).<br /><br /> SQL_CASCADE : Lors de la mise à jour de la clé primaire de la table référencée, la clé étrangère de la table de référence est également mis à jour.<br /><br /> SQL_NO_ACTION : Si une mise à jour de la clé primaire de la table référencée provoquerait une « référence non résolue » dans la table de référence (autrement dit, les lignes dans la table de référence n’aurait aucuns équivalents dans la table référencée), la mise à jour est rejetée. Si une mise à jour de la clé étrangère de la table de référence créerait une valeur qui n’existe pas en tant que valeur de la clé primaire de la table référencée, la mise à jour est rejetée. (Cette action est le même que l’action SQL_RESTRICT dans ODBC 2 *.x*.)<br /><br /> SQL_SET_NULL : Une ou plusieurs lignes dans la table référencée sont mises à jour de manière à ce qu’un ou plusieurs composants de la clé primaire sont modifiées, les composants de la clé étrangère dans la table de référence qui correspondent aux composants modifiés de la clé primaire ont des valeurs NULL dans toutes les lignes correspondantes de la table de référence.<br /><br /> SQL_SET_DEFAULT : Une ou plusieurs lignes dans la table référencée sont mises à jour de manière à ce qu’un ou plusieurs composants de la clé primaire sont modifiées, les composants de la clé étrangère dans la table de référence qui correspondent aux composants modifiés de la clé primaire sont définis aux valeurs par défaut applicables à toutes les lignes correspondantes de la table de référence.<br /><br /> NULL si non applicable à la source de données.|  
|DELETE_RULE (ODBC 1.0)|11|Smallint|Action à appliquer à la clé étrangère lorsque l’opération SQL est **supprimer**. Peut avoir l’une des valeurs suivantes. (La table référencée est la table qui contient la clé primaire ; la table de référence est la table qui contient la clé étrangère).<br /><br /> SQL_CASCADE : Lorsqu’une ligne dans la table référencée est supprimée, toutes les lignes correspondantes dans les tables de référence sont également supprimés.<br /><br /> SQL_NO_ACTION : Si une suppression d’une ligne dans la table référencée provoquerait une « référence non résolue » dans la table de référence (autrement dit, les lignes dans la table de référence n’aurait aucuns équivalents dans la table référencée), la mise à jour est rejetée. (Cette action est le même que l’action SQL_RESTRICT dans ODBC 2 *.x*.)<br /><br /> SQL_SET_NULL : Lorsqu’une ou plusieurs lignes dans la table référencée sont supprimées, chaque composant de la clé étrangère de la table de référence est défini sur NULL dans toutes les lignes correspondantes de la table de référence.<br /><br /> SQL_SET_DEFAULT : Lorsqu’une ou plusieurs lignes dans la table référencée sont supprimées, chaque composant de la clé étrangère de la table de référence est défini sur la valeur par défaut applicable dans toutes les lignes correspondantes de la table de référence.<br /><br /> NULL si non applicable à la source de données.|  
|FK_NAME (ODBC VERSION 2.0)|12|Varchar|Nom de clé étrangère. NULL si non applicable à la source de données.|  
|PK_NAME (ODBC VERSION 2.0)|13|Varchar|Nom de clé primaire. NULL si non applicable à la source de données.|  
|DEFERRABILITY (ODBC 3.0)|14|Smallint|SQL_INITIALLY_DEFERRED, SQL_INITIALLY_IMMEDIATE, SQL_NOT_DEFERRABLE.|  
  
## <a name="code-example"></a>Exemple de code  
 Comme illustré dans le tableau suivant, cet exemple utilise trois tables, nommées ORDERS, les lignes et les clients.  
  
|ORDERS|LIGNES|CLIENTS|  
|------------|-----------|---------------|  
|ORDERID|ORDERID|CUSTID|  
|CUSTID|LIGNES|NAME|  
|OPENDATE|PARTID|ADRESSE|  
|VENDEUR|QUANTITÉ|TÉLÉPHONE|  
|STATUS|||  
  
 Dans la table ORDERS, CUSTID identifie le client auquel la vente a été effectuée. Il s’agit d’une clé étrangère qui fait référence à CUSTID dans la table CUSTOMERS.  
  
 Dans la table de lignes, ORDERID identifie la commande à laquelle la ligne est associée. Il s’agit d’une clé étrangère qui fait référence à l’ORDERID de la table ORDERS.  
  
 Cet exemple appelle **SQLPrimaryKeys** pour obtenir la clé primaire de la table ORDERS. Le jeu de résultats aura une ligne ; les colonnes importantes sont présentées dans le tableau suivant.  
  
|TABLE_NAME|COLUMN_NAME|KEY_SEQ|  
|-----------------|------------------|--------------|  
|ORDERS|ORDERID|1|  
  
 Ensuite, l’exemple appelle **SQLForeignKeys** pour obtenir les clés étrangères dans d’autres tables qui font référence à la clé primaire de la table ORDERS. Le jeu de résultats aura une ligne ; les colonnes importantes sont présentées dans le tableau suivant.  
  
|PKTABLE_NAME|PKCOLUMN_NAME|FKTABLE_NAME|FKCOLUMN_NAME|KEY_SEQ|  
|-------------------|--------------------|-------------------|--------------------|--------------|  
|ORDERS|CUSTID|LIGNES|CUSTID|1|  
  
 Enfin, l’exemple appelle **SQLForeignKeys** pour obtenir les clés étrangères dans la table ORDERS qui font référence aux clés primaires d’autres tables. Le jeu de résultats aura une ligne ; les colonnes importantes sont présentées dans le tableau suivant.  
  
|PKTABLE_NAME|PKCOLUMN_NAME|FKTABLE_NAME|FKCOLUMN_NAME|KEY_SEQ|  
|-------------------|--------------------|-------------------|--------------------|--------------|  
|CLIENTS|CUSTID|ORDERS|CUSTID|1|  
  
```  
#define TAB_LEN SQL_MAX_TABLE_NAME_LEN + 1  
#define COL_LEN SQL_MAX_COLUMN_NAME_LEN + 1  
  
LPSTR   szTable;              /* Table to display */  
  
UCHAR szPkTable[TAB_LEN];   /* Primary key table name */  
UCHAR szFkTable[TAB_LEN];   /* Foreign key table name */  
UCHAR szPkCol[COL_LEN];     /* Primary key column */  
UCHAR szFkCol[COL_LEN];     /* Foreign key column */  
  
SQLHSTMT      hstmt;  
SQLINTEGER    cbPkTable, cbPkCol, cbFkTable, cbFkCol, cbKeySeq;  
SQLSMALLINT   iKeySeq;  
SQLRETURN     retcode;  
  
// Bind the columns that describe the primary and foreign keys.  
// Ignore the table schema, name, and catalog for this example.  
  
SQLBindCol(hstmt, 3, SQL_C_CHAR, szPkTable, TAB_LEN, &cbPkTable);  
SQLBindCol(hstmt, 4, SQL_C_CHAR, szPkCol, COL_LEN, &cbPkCol);  
SQLBindCol(hstmt, 5, SQL_C_SSHORT, &iKeySeq, TAB_LEN, &cbKeySeq);  
SQLBindCol(hstmt, 7, SQL_C_CHAR, szFkTable, TAB_LEN, &cbFkTable);  
SQLBindCol(hstmt, 8, SQL_C_CHAR, szFkCol, COL_LEN, &cbFkCol);  
  
strcpy_s(szTable, sizeof(szTable), "ORDERS");  
  
/* Get the names of the columns in the primary key. */  
  
retcode = SQLPrimaryKeys(hstmt,  
         NULL, 0,             /* Catalog name */  
         NULL, 0,             /* Schema name */  
         szTable, SQL_NTS);   /* Table name */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL SUCCESS_WITH_INFO)) {  
  
   /* Fetch and display the result set. This will be a list of the */  
   /* columns in the primary key of the ORDERS table. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "Table: %s Column: %s Key Seq: %hd \n", szPkTable, szPkCol,  
      iKeySeq);  
}  
  
/* Close the cursor (the hstmt is still allocated). */  
  
SQLFreeStmt(hstmt, SQL_CLOSE);  
  
/* Get all the foreign keys that refer to ORDERS primary key.*/   
  
retcode = SQLForeignKeys(hstmt,  
         NULL, 0,            /* Primary catalog */  
         NULL, 0,            /* Primary schema */  
         szTable, SQL_NTS,   /* Primary table */  
         NULL, 0,            /* Foreign catalog */  
         NULL, 0,            /* Foreign schema */  
         NULL, 0);           /* Foreign table */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) {  
  
/* Fetch and display the result set. This will be all of the */  
/* foreign keys in other tables that refer to the ORDERS */  
/* primary key. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "%-s ( %-s ) <-- %-s ( %-s )\n", szPkTable,  
               szPkCol, szFkTable, szFkCol);  
}  
  
/* Close the cursor (the hstmt is still allocated). */  
  
SQLFreeStmt(hstmt, SQL_CLOSE);  
  
/* Get all the foreign keys in the ORDERS table. */  
  
retcode = SQLForeignKeys(hstmt,  
         NULL, 0,             /* Primary catalog */  
         NULL, 0,             /* Primary schema */  
         NULL, 0,             /* Primary table */  
         NULL, 0,             /* Foreign catalog */  
         NULL, 0,             /* Foreign schema */  
         szTable, SQL_NTS);   /* Foreign table */  
  
while ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) {  
  
/* Fetch and display the result set. This will be all of the */  
/* primary keys in other tables that are referred to by foreign */  
/* keys in the ORDERS table. */  
  
   retcode = SQLFetch(hstmt);  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
      fprintf(out, "%-s ( %-s )--> %-s ( %-s )\n", szFkTable, szFkCol, szPkTable, szPkCol);  
}  
  
/* Free the hstmt. */  
SQLFreeStmt(hstmt, SQL_DROP);  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Une mémoire tampon de la liaison à une colonne dans un jeu de résultats|[SQLBindCol, fonction](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|L’annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|L’extraction d’une seule ligne ou un bloc de données dans une direction avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’un bloc de données ou de défilement d’un résultat défini|[SQLFetchScroll, fonction](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Retourner les colonnes d’une clé primaire|[SQLPrimaryKeys, fonction](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
|Retourner l’index et les statistiques de table|[SQLStatistics, fonction](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
