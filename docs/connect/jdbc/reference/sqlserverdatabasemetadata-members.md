---
title: Membres de SQLServerDatabaseMetaData | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 327ba0bc-438a-494c-b119-1cd4a096bb58
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 99e11d827cfc8e81f00a471b06521f33e872d64e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlserverdatabasemetadata-members"></a>Membres de SQLServerDatabaseMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants répertorient les membres qui sont exposées par le [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) classe.  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Nom| Description|  
|----------|-----------------|  
|java.sql.DatabaseMetaData|attributeNoNulls, attributeNullable, attributeNullableUnknown, bestRowNotPseudo, bestRowPseudo, bestRowSession, bestRowTemporary, bestRowTransaction, bestRowUnknown, columnNoNulls, columnNullable, columnNullableUnknown, importedKeyCascade, importedKeyInitiallyDeferred, importedKeyInitiallyImmediate, importedKeyNoAction, importedKeyNotDeferrable, importedKeyRestrict, importedKeySetDefault, importedKeySetNull, procedureColumnIn, procedureColumnInOut, procedureColumnOut, procedureColumnResult, procedureColumnReturn, procedureColumnUnknown, procedureNoNulls, procedureNoResult, procedureNullable, procedureNullableUnknown, procedureResultUnknown, procedureReturnsResult, sqlStateSQL, sqlStateSQL99, sqlStateXOpen, tableIndexClustered, tableIndexHashed, tableIndexOther, tableIndexStatistic, typeNoNulls, typeNullable, typeNullableUnknown, typePredBasic, typePredChar, typePredNone, typeSearchable, versionColumnNotPseudo, versionColumnPseudo, versionColumnUnknown|  
  
## <a name="methods"></a>Méthodes  
  
|Nom| Description|  
|----------|-----------------|  
|[allProceduresAreCallable](../../../connect/jdbc/reference/allproceduresarecallable-method-sqlserverdatabasemetadata.md)|Récupère si l’utilisateur actuel dispose d’autorisations pour appeler toutes les procédures retournées par la [getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md) (méthode).|  
|[allTablesAreSelectable](../../../connect/jdbc/reference/alltablesareselectable-method-sqlserverdatabasemetadata.md)|Récupère si l’utilisateur actuel dispose d’autorisations pour toutes les tables retournées par la [getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md) méthode dans une instruction SELECT.|  
|[autoCommitFailureClosesAllResultSets](../../../connect/jdbc/reference/autocommitfailureclosesallresultsets-method-sqlserverdatabasemetadata.md)|Indique si le pilote JDBC ferme tous les jeux de résultats ouverts, notamment ceux pouvant être mis en attente, lorsqu'une validation automatique est activée et une exception levée.|  
|[dataDefinitionCausesTransactionCommit](../../../connect/jdbc/reference/datadefinitioncausestransactioncommit-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si une instruction de définition des données dans une transaction force la transaction à valider.|  
|[dataDefinitionIgnoredInTransactions](../../../connect/jdbc/reference/datadefinitionignoredintransactions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données ignore une instruction de définition des données dans une transaction.|  
|[deletesAreDetected](../../../connect/jdbc/reference/deletesaredetected-method-sqlserverdatabasemetadata.md)|Récupère, supprimer ou non d’une ligne visible peut être détectée en appelant le [rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) classe.|  
|[doesMaxRowSizeIncludeBlobs](../../../connect/jdbc/reference/doesmaxrowsizeincludeblobs-method-sqlserverdatabasemetadata.md)|Récupère si la valeur de retour pour la [getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md) méthode inclut les types de données SQL LONGVARCHAR et LONGVARBINARY.|  
|[GetAttributes](../../../connect/jdbc/reference/getattributes-method-sqlserverdatabasemetadata.md)|Récupère une description de l'attribut donné du type donné pour un type défini par l'utilisateur disponible dans le schéma et le catalogue donnés.|  
|[getBestRowIdentifier](../../../connect/jdbc/reference/getbestrowidentifier-method-sqlserverdatabasemetadata.md)|Récupère une description du jeu optimal de colonnes d'une table qui identifie une ligne de façon unique.|  
|[getCatalogs](../../../connect/jdbc/reference/getcatalogs-method-sqlserverdatabasemetadata.md)|Récupère les noms de catalogues disponibles sur le serveur connecté.|  
|[getCatalogSeparator](../../../connect/jdbc/reference/getcatalogseparator-method-sqlserverdatabasemetadata.md)|Récupère le **chaîne** que cette base de données utilise en tant que séparateur entre un nom de table et de catalogue.|  
|[getCatalogTerm](../../../connect/jdbc/reference/getcatalogterm-method-sqlserverdatabasemetadata.md)|Récupère le terme favori du fournisseur de base de données pour « catalog ».|  
|[getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md)|Récupère une liste de propriétés d'informations clientes prises en charge par le pilote.|  
|[getColumnPrivileges](../../../connect/jdbc/reference/getcolumnprivileges-method-sqlserverdatabasemetadata.md)|Récupère une description des droits d'accès aux colonnes d'une table.|  
|[getColumns](../../../connect/jdbc/reference/getcolumns-method-sqlserverdatabasemetadata.md)|Récupère une description des colonnes d'une table qui sont disponibles dans le catalogue spécifié.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatabasemetadata.md)|Récupère la connexion qui a produit cet objet de métadonnées.|  
|[getCrossReference](../../../connect/jdbc/reference/getcrossreference-method-sqlserverdatabasemetadata.md)|Récupère une description des colonnes de clés étrangères dans la table de clés étrangères donnée qui référence les colonnes de clés primaires de la table de clés primaires donnée.|  
|[getDatabaseMajorVersion](../../../connect/jdbc/reference/getdatabasemajorversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version majeure de la base de données sous-jacente.|  
|[getDatabaseMinorVersion](../../../connect/jdbc/reference/getdatabaseminorversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version mineure de la base de données sous-jacente.|  
|[getDatabaseProductName](../../../connect/jdbc/reference/getdatabaseproductname-method-sqlserverdatabasemetadata.md)|Récupère le nom de ce produit de base de données.|  
|[getDatabaseProductVersion](../../../connect/jdbc/reference/getdatabaseproductversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version de ce produit de base de données.|  
|[getDefaultTransactionIsolation](../../../connect/jdbc/reference/getdefaulttransactionisolation-method-sqlserverdatabasemetadata.md)|Récupère le niveau d'isolation de la transaction par défaut pour cette base de données.|  
|[getDriverMajorVersion](../../../connect/jdbc/reference/getdrivermajorversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version majeure de ce pilote JDBC.|  
|[getDriverMinorVersion](../../../connect/jdbc/reference/getdriverminorversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version mineure de ce pilote JDBC.|  
|[getDriverName](../../../connect/jdbc/reference/getdrivername-method-sqlserverdatabasemetadata.md)|Récupère le nom de ce pilote JDBC.|  
|[getDriverVersion](../../../connect/jdbc/reference/getdriverversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version de ce pilote JDBC.|  
|[getExportedKeys](../../../connect/jdbc/reference/getexportedkeys-method-sqlserverdatabasemetadata.md)|Récupère une description des colonnes de clés étrangères qui référencent les colonnes de clé primaire de la table donnée.|  
|[getExtraNameCharacters](../../../connect/jdbc/reference/getextranamecharacters-method-sqlserverdatabasemetadata.md)|Récupère tous les caractères supplémentaires qui peuvent être utilisés dans les noms d’identificateur sans guillemets, par exemple, ceux au-delà d’a à z, A-Z, 0-9 et _.|  
|[getFunctions](../../../connect/jdbc/reference/getfunctions-method-sqlserverdatabasemetadata.md)|Récupère une description des fonctions système et utilisateur.|  
|[getFunctionColumns](../../../connect/jdbc/reference/getfunctioncolumns-method-sqlserverdatabasemetadata.md)|Récupère une description des paramètres de fonctions système ou utilisateur du catalogue spécifié et un type de retour.|  
|[getIdentifierQuoteString](../../../connect/jdbc/reference/getidentifierquotestring-method-sqlserverdatabasemetadata.md)|Récupère le **chaîne** qui est utilisé pour encadrer les identificateurs SQL.|  
|[getImportedKeys](../../../connect/jdbc/reference/getimportedkeys-method-sqlserverdatabasemetadata.md)|Récupère une description des colonnes de clés primaires référencées par les colonnes de clés étrangères d'une table.|  
|[getIndexInfo](../../../connect/jdbc/reference/getindexinfo-method-sqlserverdatabasemetadata.md)|Récupère une description des index et statistiques de la table donnée.|  
|[getJDBCMajorVersion](../../../connect/jdbc/reference/getjdbcmajorversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version majeure JDBC pour ce pilote.|  
|[getJDBCMinorVersion](../../../connect/jdbc/reference/getjdbcminorversion-method-sqlserverdatabasemetadata.md)|Récupère le numéro de version mineure JDBC pour ce pilote.|  
|[getMaxBinaryLiteralLength](../../../connect/jdbc/reference/getmaxbinaryliterallength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères hexadécimaux autorisé par cette base de données dans un littéral binaire inséré.|  
|[getMaxCatalogNameLength](../../../connect/jdbc/reference/getmaxcatalognamelength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données dans un nom de catalogue.|  
|[getMaxCharLiteralLength](../../../connect/jdbc/reference/getmaxcharliterallength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données pour un littéral de caractère.|  
|[getMaxColumnNameLength](../../../connect/jdbc/reference/getmaxcolumnnamelength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données pour un nom de colonne.|  
|[getMaxColumnsInGroupBy](../../../connect/jdbc/reference/getmaxcolumnsingroupby-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de colonnes autorisé par cette base de données dans une clause GROUP BY.|  
|[getMaxColumnsInIndex](../../../connect/jdbc/reference/getmaxcolumnsinindex-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de colonnes autorisé par cette base de données dans un index.|  
|[getMaxColumnsInOrderBy](../../../connect/jdbc/reference/getmaxcolumnsinorderby-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de colonnes autorisé par cette base de données dans une clause ORDER BY.|  
|[getMaxColumnsInSelect](../../../connect/jdbc/reference/getmaxcolumnsinselect-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de colonnes autorisé par cette base de données dans une liste SELECT.|  
|[getMaxColumnsInTable](../../../connect/jdbc/reference/getmaxcolumnsintable-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de colonnes autorisé par cette base de données dans une table.|  
|[getMaxConnections](../../../connect/jdbc/reference/getmaxconnections-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de connexions simultanées possibles à cette base de données.|  
|[getMaxCursorNameLength](../../../connect/jdbc/reference/getmaxcursornamelength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données dans un nom de curseur.|  
|[getMaxIndexLength](../../../connect/jdbc/reference/getmaxindexlength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal d’octets qui permet à cette base de données pour un index, y compris toutes les parties de l’index.|  
|[getMaxProcedureNameLength](../../../connect/jdbc/reference/getmaxprocedurenamelength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données dans un nom de procédure.|  
|[getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal d'octets autorisé par cette base de données dans une ligne unique.|  
|[getMaxSchemaNameLength](../../../connect/jdbc/reference/getmaxschemanamelength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données dans un nom de schéma.|  
|[getMaxStatementLength](../../../connect/jdbc/reference/getmaxstatementlength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères qui permet à cette base de données dans une instruction SQL.|  
|[getMaxStatements](../../../connect/jdbc/reference/getmaxstatements-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal d'instructions actives autorisé pour cette base de données qui peuvent être ouvertes en même temps.|  
|[getMaxTableNameLength](../../../connect/jdbc/reference/getmaxtablenamelength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données dans un nom de table.|  
|[getMaxTablesInSelect](../../../connect/jdbc/reference/getmaxtablesinselect-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de tables qui permet à cette base de données dans une instruction SELECT.|  
|[getMaxUserNameLength](../../../connect/jdbc/reference/getmaxusernamelength-method-sqlserverdatabasemetadata.md)|Récupère le nombre maximal de caractères autorisé par cette base de données dans un nom d'utilisateur.|  
|[getNumericFunctions](../../../connect/jdbc/reference/getnumericfunctions-method-sqlserverdatabasemetadata.md)|Récupère une liste séparée par des virgules des fonctions mathématiques disponibles avec cette base de données.|  
|[getPrimaryKeys](../../../connect/jdbc/reference/getprimarykeys-method-sqlserverdatabasemetadata.md)|Récupère une description des colonnes de clés primaires de la table donnée.|  
|[getProcedureColumns](../../../connect/jdbc/reference/getprocedurecolumns-method-sqlserverdatabasemetadata.md)|Récupère une description des paramètres de procédure stockée et les colonnes de résultats.|  
|[getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md)|Récupère une description des procédures stockées disponibles dans le modèle de nom de catalogue, de schéma ou de procédure stockée donné.|  
|[getProcedureTerm](../../../connect/jdbc/reference/getprocedureterm-method-sqlserverdatabasemetadata.md)|Récupère le terme favori pour « procedure » dans cette base de données.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverdatabasemetadata.md)|Récupère la fonctionnalité par défaut de mise en attente des jeux de résultats pour cette base de données.|  
|[getRowIdLifetime](../../../connect/jdbc/reference/getrowidlifetime-method-sqlserverdatabasemetadata.md)|Retourne un statut indiquant si le type de données SQL RowId est pris en charge ou non. S'il l'est, il retourne la durée de validité d'un objet RowId.|  
|[getSchemas](../../../connect/jdbc/reference/getschemas-method.md)|Récupère les noms de schémas disponibles dans la base de données actuelle.|  
|[getSchemaTerm](../../../connect/jdbc/reference/getschematerm-method-sqlserverdatabasemetadata.md)|Récupère le terme favori pour « schema » dans cette base de données.|  
|[getSearchStringEscape](../../../connect/jdbc/reference/getsearchstringescape-method-sqlserverdatabasemetadata.md)|Récupère le **chaîne** qui peut être utilisé pour échapper les caractères génériques.|  
|[getSQLKeywords](../../../connect/jdbc/reference/getsqlkeywords-method-sqlserverdatabasemetadata.md)|Récupère une liste séparée par des virgules des mots clés SQL de la totalité de cette base de données qui ne sont pas également des mots clés SQL92.|  
|[getSQLStateType](../../../connect/jdbc/reference/getsqlstatetype-method-sqlserverdatabasemetadata.md)|Indique si la valeur SQLSTATE retournée par la méthode SQLException.getSQLState est X / Open (maintenant appelé Open Group), SQL CLI, SQL99 (JDBC 3.0), ou SQL : 2003 (JDBC 4.0).|  
|[getStringFunctions](../../../connect/jdbc/reference/getstringfunctions-method-sqlserverdatabasemetadata.md)|Récupère une liste séparée par des virgules de **chaîne** fonctions qui sont disponibles avec cette base de données.|  
|[getSuperTables](../../../connect/jdbc/reference/getsupertables-method-sqlserverdatabasemetadata.md)|Récupère une description des hiérarchies de table définies dans un schéma particulier de cette base de données.|  
|[getSuperTypes](../../../connect/jdbc/reference/getsupertypes-method-sqlserverdatabasemetadata.md)|Récupère une description des hiérarchies de type définies par l'utilisateur dans un schéma particulier de cette base de données.|  
|[getSystemFunctions](../../../connect/jdbc/reference/getsystemfunctions-method-sqlserverdatabasemetadata.md)|Récupère une liste séparée par des virgules des fonctions système disponibles avec cette base de données.|  
|[getTablePrivileges](../../../connect/jdbc/reference/gettableprivileges-method-sqlserverdatabasemetadata.md)|Récupère une description des droits d'accès pour chaque table disponible dans le modèle de nom de catalogue, de schéma ou de table donné.|  
|[getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md)|Récupère une description des tables disponibles dans le modèle de nom de catalogue, de schéma ou de table donné.|  
|[getTableTypes](../../../connect/jdbc/reference/gettabletypes-method-sqlserverdatabasemetadata.md)|Récupère les types de tables disponibles dans la base de données actuelle.|  
|[getTimeDateFunctions](../../../connect/jdbc/reference/gettimedatefunctions-method-sqlserverdatabasemetadata.md)|Récupère une liste séparée par des virgules des fonctions de date et heure disponibles avec cette base de données.|  
|[GetTypeInfo](../../../connect/jdbc/reference/gettypeinfo-method-sqlserverdatabasemetadata.md)|Récupère une description de tous les types SQL standard pris en charge par la base de données actuelle.|  
|[getUDTs](../../../connect/jdbc/reference/getudts-method-sqlserverdatabasemetadata.md)|Récupère une description des types définis par l'utilisateur dans un schéma particulier.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatabasemetadata.md)|Récupère l'URL pour cette base de données.|  
|[GetUserName](../../../connect/jdbc/reference/getusername-method-sqlserverdatabasemetadata.md)|Récupère le nom d'utilisateur tel qu'il est connu dans cette base de données.|  
|[getVersionColumns](../../../connect/jdbc/reference/getversioncolumns-method-sqlserverdatabasemetadata.md)|Récupère une description des colonnes d'une table qui reflète automatiquement la mise à jour d'une valeur d'une ligne.|  
|[insertsAreDetected](../../../connect/jdbc/reference/insertsaredetected-method-sqlserverdatabasemetadata.md)|Récupère ou non une insertion de ligne visible peut être détectée en appelant la méthode [rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) classe.|  
|[isCatalogAtStart](../../../connect/jdbc/reference/iscatalogatstart-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un catalogue s'affiche au début d'un nom de table complet.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données est en mode lecture seule.|  
|[locatorsUpdateCopy](../../../connect/jdbc/reference/locatorsupdatecopy-method-sqlserverdatabasemetadata.md)|Indique si les mises à jour d'un LOB sont effectuées sur une copie ou dans le LOB lui-même.|  
|[nullPlusNonNullIsNull](../../../connect/jdbc/reference/nullplusnonnullisnull-method-sqlserverdatabasemetadata.md)|Indique si cette base de données prend en charge les concaténations entre les valeurs NULL et non NULL étant NULL.|  
|[nullsAreSortedAtEnd](../../../connect/jdbc/reference/nullsaresortedatend-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les valeurs NULL sont triées à la fin indépendamment de l'ordre de tri.|  
|[nullsAreSortedAtStart](../../../connect/jdbc/reference/nullsaresortedatstart-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les valeurs NULL sont triées au début indépendamment de l'ordre de tri.|  
|[nullsAreSortedHigh](../../../connect/jdbc/reference/nullsaresortedhigh-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les valeurs Null sont triées par ordre croissant.|  
|[nullsAreSortedLow](../../../connect/jdbc/reference/nullsaresortedlow-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les valeurs Null sont triées par ordre décroissant.|  
|[othersDeletesAreVisible](../../../connect/jdbc/reference/othersdeletesarevisible-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les suppressions effectuées par les autres sont visibles.|  
|[othersInsertsAreVisible](../../../connect/jdbc/reference/othersinsertsarevisible-method-sqlserverdatabasemetadata.md)|Récupère si les insertions effectuées par d’autres sont visibles.|  
|[othersUpdatesAreVisible](../../../connect/jdbc/reference/othersupdatesarevisible-method-sqlserverdatabasemetadata.md)|Récupère si les mises à jour qui sont effectuées par d’autres sont visibles.|  
|[ownDeletesAreVisible](../../../connect/jdbc/reference/owndeletesarevisible-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les propres suppressions d'un jeu de résultats sont visibles.|  
|[ownInsertsAreVisible](../../../connect/jdbc/reference/owninsertsarevisible-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les propres insertions d'un jeu de résultats sont visibles.|  
|[ownUpdatesAreVisible](../../../connect/jdbc/reference/ownupdatesarevisible-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les propres mises à jour d'un jeu de résultats sont visibles.|  
|[storesLowerCaseIdentifiers](../../../connect/jdbc/reference/storeslowercaseidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui ne se trouvent pas entre guillemets comme non sensibles à la casse et les stocke en minuscules.|  
|[storesLowerCaseQuotedIdentifiers](../../../connect/jdbc/reference/storeslowercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui se trouvent entre guillemets comme non sensibles à la casse et les stocke en minuscules.|  
|[storesMixedCaseIdentifiers](../../../connect/jdbc/reference/storesmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui ne se trouvent pas entre guillemets comme non sensibles à la casse et les stocke en casse mixte.|  
|[storesMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui se trouvent entre guillemets comme non sensibles à la casse et les stocke en casse mixte.|  
|[storesUpperCaseIdentifiers](../../../connect/jdbc/reference/storesuppercaseidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui ne se trouvent pas entre guillemets comme non sensibles à la casse et les stocke en majuscules.|  
|[storesUpperCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesuppercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui se trouvent entre guillemets comme non sensibles à la casse et les stocke en majuscules.|  
|[supportsAlterTableWithAddColumn](../../../connect/jdbc/reference/supportsaltertablewithaddcolumn-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge ALTER TABLE avec AddColumn.|  
|[supportsAlterTableWithDropColumn](../../../connect/jdbc/reference/supportsaltertablewithdropcolumn-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge ALTER TABLE avec la colonne DropColumn.|  
|[supportsANSI92EntryLevelSQL](../../../connect/jdbc/reference/supportsansi92entrylevelsql-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la grammaire SQL de niveau d'entrée ANSI92.|  
|[supportsANSI92FullSQL](../../../connect/jdbc/reference/supportsansi92fullsql-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la grammaire SQL entière ANSI92.|  
|[supportsANSI92IntermediateSQL](../../../connect/jdbc/reference/supportsansi92intermediatesql-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la grammaire SQL intermédiaire ANSI92.|  
|[supportsBatchUpdates](../../../connect/jdbc/reference/supportsbatchupdates-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les mises à jour par lot.|  
|[supportsCatalogsInDataManipulation](../../../connect/jdbc/reference/supportscatalogsindatamanipulation-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de catalogue peut être utilisé dans une instruction de manipulation de données.|  
|[supportsCatalogsInIndexDefinitions](../../../connect/jdbc/reference/supportscatalogsinindexdefinitions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de catalogue peut être utilisé dans une instruction de définition d'index.|  
|[supportsCatalogsInPrivilegeDefinitions](../../../connect/jdbc/reference/supportscatalogsinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de catalogue peut être utilisé dans une instruction de définition de privilège.|  
|[supportsCatalogsInProcedureCalls](../../../connect/jdbc/reference/supportscatalogsinprocedurecalls-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de catalogue peut être utilisé dans une instruction d'appel de procédure.|  
|[supportsCatalogsInTableDefinitions](../../../connect/jdbc/reference/supportscatalogsintabledefinitions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de catalogue peut être utilisé dans une instruction de définition de table.|  
|[supportsColumnAliasing](../../../connect/jdbc/reference/supportscolumnaliasing-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la création d'alias de colonne.|  
|[supportsConvert](../../../connect/jdbc/reference/supportsconvert-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la fonction CONVERT entre les types SQL.|  
|[supportsCoreSQLGrammar](../../../connect/jdbc/reference/supportscoresqlgrammar-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la grammaire SQL principale ODBC.|  
|[supportsCorrelatedSubqueries](../../../connect/jdbc/reference/supportscorrelatedsubqueries-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les sous-requêtes corrélées.|  
|[supportsDataDefinitionAndDataManipulationTransactions](../../../connect/jdbc/reference/supportsdatadefinitionanddatamanipulationtransactions-method.md)|Récupère les informations déterminant si cette base de données prend en charge à la fois les instructions de définition et de manipulation des données dans une transaction.|  
|[supportsDataManipulationTransactionsOnly](../../../connect/jdbc/reference/supportsdatamanipulationtransactionsonly-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend uniquement en charge les instructions de manipulation des données dans une transaction.|  
|[supportsDifferentTableCorrelationNames](../../../connect/jdbc/reference/supportsdifferenttablecorrelationnames-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les noms de corrélation de tables, lorsqu'ils sont pris en charge, doivent être différents des noms des tables.|  
|[supportsExpressionsInOrderBy](../../../connect/jdbc/reference/supportsexpressionsinorderby-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les expressions dans les listes ORDER BY.|  
|[supportsExtendedSQLGrammar](../../../connect/jdbc/reference/supportsextendedsqlgrammar-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la grammaire SQL étendue ODBC.|  
|[supportsFullOuterJoins](../../../connect/jdbc/reference/supportsfullouterjoins-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les jointures externes imbriquées.|  
|[supportsGetGeneratedKeys](../../../connect/jdbc/reference/supportsgetgeneratedkeys-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les clés générées automatiquement peuvent être récupérées après l'exécution d'une instruction.|  
|[supportsGroupBy](../../../connect/jdbc/reference/supportsgroupby-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge certaines formes de la clause GROUP BY.|  
|[supportsGroupByBeyondSelect](../../../connect/jdbc/reference/supportsgroupbybeyondselect-method-sqlserverdatabasemetadata.md)|Récupère si cette base de données prend en charge à l’aide de colonnes non incluses dans l’instruction SELECT dans une clause GROUP BY autant que toutes les colonnes dans l’instruction SELECT sont inclus dans la clause GROUP BY.|  
|[supportsGroupByUnrelated](../../../connect/jdbc/reference/supportsgroupbyunrelated-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge l'utilisation d'une colonne ne figurant pas dans l'instruction SELECT d'une clause GROUP BY.|  
|[supportsIntegrityEnhancementFacility](../../../connect/jdbc/reference/supportsintegrityenhancementfacility-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge SQL Integrity Enhancement Facility.|  
|[supportsLikeEscapeClause](../../../connect/jdbc/reference/supportslikeescapeclause-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la spécification d'une clause d'échappement LIKE.|  
|[supportsLimitedOuterJoins](../../../connect/jdbc/reference/supportslimitedouterjoins-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données fournit une prise en charge limitée des jointures externes.|  
|[supportsMinimumSQLGrammar](../../../connect/jdbc/reference/supportsminimumsqlgrammar-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la grammaire SQL minimale ODBC.|  
|[supportsMixedCaseIdentifiers](../../../connect/jdbc/reference/supportsmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui ne se trouvent pas entre guillemets comme non sensibles à la casse et les stocke en casse mixte.|  
|[supportsMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/supportsmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données traite les identificateurs SQL à casse mixte qui se trouvent entre guillemets comme non sensibles à la casse et les stocke en casse mixte.|  
|[supportsMultipleOpenResults](../../../connect/jdbc/reference/supportsmultipleopenresults-method-sqlserverdatabasemetadata.md)|Récupère s’il est possible que plusieurs [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) les objets retournés par une [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) objet simultanément.|  
|[supportsMultipleResultSets](../../../connect/jdbc/reference/supportsmultipleresultsets-method-sqlserverdatabasemetadata.md)|Récupère si cette base de données prend en charge la mise en route de plusieurs [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) objets à partir d’un seul appel à la [exécuter](../../../connect/jdbc/reference/execute-method.md) méthode de la [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)classe.|  
|[supportsMultipleTransactions](../../../connect/jdbc/reference/supportsmultipletransactions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données permet d'ouvrir plusieurs transactions en même temps sur différentes connexions.|  
|[supportsNamedParameters](../../../connect/jdbc/reference/supportsnamedparameters-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les paramètres nommés dans les instructions pouvant être appelées.|  
|[supportsNonNullableColumns](../../../connect/jdbc/reference/supportsnonnullablecolumns-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si les colonnes dans cette base de données peuvent être définies comme n'acceptant pas la valeur Null.|  
|[supportsOpenCursorsAcrossCommit](../../../connect/jdbc/reference/supportsopencursorsacrosscommit-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la possibilité de garder les curseurs ouverts dans les différentes validations.|  
|[supportsOpenCursorsAcrossRollback](../../../connect/jdbc/reference/supportsopencursorsacrossrollback-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données permet de garder les curseurs ouverts dans les différentes restaurations.|  
|[supportsOpenStatementsAcrossCommit](../../../connect/jdbc/reference/supportsopenstatementsacrosscommit-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la possibilité de garder les instructions ouvertes dans les différentes validations.|  
|[supportsOpenStatementsAcrossRollback](../../../connect/jdbc/reference/supportsopenstatementsacrossrollback-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données permet de garder les instructions ouvertes dans les différentes récupérations.|  
|[supportsOrderByUnrelated](../../../connect/jdbc/reference/supportsorderbyunrelated-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge l'utilisation d'une colonne ne figurant pas dans l'instruction SELECT d'une clause ORDER BY.|  
|[supportsOuterJoins](../../../connect/jdbc/reference/supportsouterjoins-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge certaines formes de jointures externes.|  
|[supportsPositionedDelete](../../../connect/jdbc/reference/supportspositioneddelete-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les instructions DELETE positionnées.|  
|[supportsPositionedUpdate](../../../connect/jdbc/reference/supportspositionedupdate-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les instructions UPDATE positionnées.|  
|[supportsResultSetConcurrency](../../../connect/jdbc/reference/supportsresultsetconcurrency-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge le type de concurrence donné conjointement avec le type de jeu de résultats donné.|  
|[supportsResultSetHoldability](../../../connect/jdbc/reference/supportsresultsetholdability-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge la fonctionnalité de mise en attente du jeu de résultats donné.|  
|[supportsResultSetType](../../../connect/jdbc/reference/supportsresultsettype-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge le type de jeu de résultats donné.|  
|[supportsSavepoints](../../../connect/jdbc/reference/supportssavepoints-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les points de sauvegarde.|  
|[supportsSchemasInDataManipulation](../../../connect/jdbc/reference/supportsschemasindatamanipulation-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de schéma peut être utilisé dans une instruction de manipulation de données.|  
|[supportsSchemasInIndexDefinitions](../../../connect/jdbc/reference/supportsschemasinindexdefinitions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de schéma peut être utilisé dans une instruction de définition d'index.|  
|[supportsSchemasInPrivilegeDefinitions](../../../connect/jdbc/reference/supportsschemasinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de schéma peut être utilisé dans une instruction de définition de privilège.|  
|[supportsSchemasInProcedureCalls](../../../connect/jdbc/reference/supportsschemasinprocedurecalls-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de schéma peut être utilisé dans une instruction d'appel de procédure.|  
|[supportsSchemasInTableDefinitions](../../../connect/jdbc/reference/supportsschemasintabledefinitions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si un nom de schéma peut être utilisé dans une instruction de définition de table.|  
|[supportsSelectForUpdate](../../../connect/jdbc/reference/supportsselectforupdate-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les instructions SELECT FOR UPDATE.|  
|[supportsStatementPooling](../../../connect/jdbc/reference/supportsstatementpooling-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge le regroupement d'instructions.|  
|[supportsStoredFunctionsUsingCallSyntax](../../../connect/jdbc/reference/supportsstoredfunctionsusingcallsyntax-method-sqlserverdatabasemetadata.md)|Indique si la base de données actuelle prend en charge l'appel de fonctions définies par l'utilisateur ou le fournisseur à l'aide de la syntaxe d'échappement de procédure stockée.|  
|[supportsStoredProcedures](../../../connect/jdbc/reference/supportsstoredprocedures-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les appels de procédures stockées qui utilisent la syntaxe d'échappement de procédure stockée.|  
|[supportsSubqueriesInComparisons](../../../connect/jdbc/reference/supportssubqueriesincomparisons-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les sous-requêtes dans les expressions de comparaison.|  
|[supportsSubqueriesInExists](../../../connect/jdbc/reference/supportssubqueriesinexists-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les sous-requêtes dans les expressions EXISTS.|  
|[supportsSubqueriesInIns](../../../connect/jdbc/reference/supportssubqueriesinins-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les sous-requêtes dans les expressions IN.|  
|[supportsSubqueriesInQuantifieds](../../../connect/jdbc/reference/supportssubqueriesinquantifieds-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les sous-requêtes dans les expressions quantifiées.|  
|[supportsTableCorrelationNames](../../../connect/jdbc/reference/supportstablecorrelationnames-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les noms de corrélation de tables.|  
|[supportsTransactionIsolationLevel](../../../connect/jdbc/reference/supportstransactionisolationlevel-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge le niveau d'isolation de la transaction donné.|  
|[supportsTransactions](../../../connect/jdbc/reference/supportstransactions-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge les transactions.|  
|[supportsUnion](../../../connect/jdbc/reference/supportsunion-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge SQL UNION.|  
|[supportsUnionAll](../../../connect/jdbc/reference/supportsunionall-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données prend en charge SQL UNION ALL.|  
|[updatesAreDetected](../../../connect/jdbc/reference/updatesaredetected-method-sqlserverdatabasemetadata.md)|Récupère ou non une mise à jour de ligne visible peut être détectée en appelant le [rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) classe.|  
|[usesLocalFilePerTable](../../../connect/jdbc/reference/useslocalfilepertable-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données utilise un fichier pour chaque table.|  
|[usesLocalFiles](../../../connect/jdbc/reference/useslocalfiles-method-sqlserverdatabasemetadata.md)|Récupère les informations déterminant si cette base de données stocke les tables dans un fichier local.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
