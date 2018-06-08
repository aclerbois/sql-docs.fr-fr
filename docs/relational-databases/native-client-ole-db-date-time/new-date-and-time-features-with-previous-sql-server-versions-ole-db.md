---
title: Nouvelles fonctionnalités de Date et heure avec les versions SQL Server antérieures (OLE DB) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-ole-db-date-time
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 96976bac-018c-47cc-b1b2-fa9605eb55e5
caps.latest.revision: 27
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5cb50889a9cb584e5f22d5f2e77960dba567fda1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-date-and-time-features-with-previous-sql-server-versions-ole-db"></a>Nouvelles fonctionnalités de Date et heure avec les versions SQL Server antérieures (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Cette rubrique décrit le comportement attendu lorsqu’une application cliente qui utilise des fonctionnalités améliorées de date et heure communique avec une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]et lorsqu’un client compilé avec une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] envoie des commandes à un serveur qui prend en charge améliorées des fonctions de date et d’heure.  
  
## <a name="down-level-client-behavior"></a>Comportement de client de bas niveau  
 Les applications clientes qui utilisent une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] voir les types date/heure de nouveau en tant que **nvarchar** colonnes. Les contenus des colonnes sont des représentations littérales. Pour plus d’informations, consultez la section « Formats de données : chaînes et littéraux » de [prise en charge du Type de données de Date OLE DB et les améliorations apportées au](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md). La taille de colonne est la longueur littérale maximale pour la précision spécifiée pour la colonne.  
  
 API du catalogue retournent des métadonnées cohérentes avec le code de type de données de bas niveau retourné au client (par exemple, **nvarchar**) et la représentation de bas niveau associée (par exemple, le format littéral approprié). Toutefois, le nom du type de données retourné est le nom de type [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] réel.  
  
 Lorsqu’une application cliente de bas niveau s’exécute sur un [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (ou version ultérieure) sur les modifications de schéma à la date/heure ont été apportées des types de serveur, le comportement attendu est comme suit :  
  
|Type du client OLE DB|Type SQL Server 2005|SQL Server 2008 (ou versions ultérieures)|Conversion de résultat (serveur vers client)|Conversion de paramètre (client vers serveur)|  
|------------------------|--------------------------|---------------------------------------|--------------------------------------------|-----------------------------------------------|  
|DBTYPE_DBDATE|DateTime|Date|OK|OK|  
|DBTYPE_DBTIMESTAMP|||Champs d'heure définis à zéro.|IRowsetChange échoue en raison d’une troncation de chaîne si le champ d’heure est différent de zéro.|  
|DBTYPE_DBTIME||Time(0)|OK|OK|  
|DBTYPE_DBTIMESTAMP|||Champs de date définis à la date actuelle.|IRowsetChange échoue en raison d’une troncation de chaîne si les fractions de seconde sont différentes de zéro.<br /><br /> Date est ignorée.|  
|DBTYPE_DBTIME||Time(7)|Échec : littéral d'heure non valide.|OK|  
|DBTYPE_DBTIMESTAMP|||Échec : littéral d'heure non valide.|OK|  
|DBTYPE_DBTIMESTAMP||Datetime2(3)|OK|OK|  
|DBTYPE_DBTIMESTAMP||Datetime2 (7)|OK|OK|  
|DBTYPE_DBDATE|Smalldatetime|Date|OK|OK|  
|DBTYPE_DBTIMESTAMP|||Champs d'heure définis à zéro.|IRowsetChange échoue en raison d’une troncation de chaîne si le champ d’heure est différent de zéro.|  
|DBTYPE_DBTIME||Time(0)|OK|OK|  
|DBTYPE_DBTIMESTAMP|||Champs de date définis à la date actuelle.|IRowsetChange échoue en raison d’une troncation de chaîne si les fractions de seconde sont différentes de zéro.<br /><br /> Date est ignorée.|  
|DBTYPE_DBTIMESTAMP||Datetime2(0)|OK|OK|  
  
 OK signifie que si cela a fonctionné avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], cela doit continuer à fonctionner avec [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (ou version ultérieure).  
  
 Seules les modifications de schéma communes suivantes ont été considérées :  
  
-   Utilisation d'un nouveau type alors qu'en toute logique une application requiert uniquement une valeur de date ou d'heure. Toutefois, l’application a été obligée d’utiliser **datetime** ou **smalldatetime** car types distincts de date et heure n’étaient pas disponibles.  
  
-   Utilisation d'un nouveau type pour gagner en précision sur les fractions de seconde.  
  
-   Passage à **datetime2** , car c’est le type de données par défaut pour la date et l’heure.  
  
 Les applications qui utilisent des métadonnées du serveur obtenues via ICommandWithParameters::GetParameterInfo ou ensembles de lignes de schéma pour définir les informations de type de paramètre par le biais ICommandWithParameters::SetParameterInfo échouent pendant les conversions clientes où la représentation sous forme de chaîne d’un type de source est supérieure à la représentation sous forme de chaîne du type de destination. Par exemple, si une liaison de client utilise DBTYPE_DBTIMESTAMP et si la colonne de serveur est date, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client convertit la valeur de « aaaa-mm-jj hh:mm:ss.fff », mais les métadonnées du serveur seront retournée comme **nvarchar (10)**. Le dépassement de capacité résultant provoque DBSTATUS_E_CATCONVERTVALUE. Problèmes semblables surviennent avec les conversions de données par IRowsetChange, car les métadonnées de l’ensemble de lignes sont définie à partir des métadonnées du jeu de résultats.  
  
### <a name="parameter-and-rowset-metadata"></a>Métadonnées de paramètre et d'ensemble de lignes  
 Cette section décrit les métadonnées des paramètres, les colonnes de résultats et les ensembles de lignes de schéma pour les clients qui sont compilés avec une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
#### <a name="icommandwithparametersgetparameterinfo"></a>ICommandWithParameters::GetParameterInfo  
 La structure DBPARAMINFO retourne les informations suivantes via le *prgParamInfo* paramètre :  
  
|Type de paramètre|wType|ulParamSize|bPrecision|bScale|  
|--------------------|-----------|-----------------|----------------|------------|  
|date|DBTYPE_WSTR|10|~0|~0|  
|time|DBTYPE_WSTR|8, 10..16|~0|~0|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19,21..27|~0|~0|  
|datetimeoffset|DBTYPE_WSTR|26,28..34|~0|~0|  
  
 Notez que certaines de ces plages de valeurs sont discontinues ; par exemple, 9 est manquant dans 8,10..16. Cela est dû à l'ajout d'une virgule décimale lorsque la précision fractionnaire est supérieure à zéro.  
  
#### <a name="icolumnsrowsetgetcolumnsrowset"></a>IColumnsRowset::GetColumnsRowset  
 Les colonnes suivantes sont retournées :  
  
|Type de colonne|DBCOLUMN_TYPE|DBCOLUMN_COLUMNSIZE|DBCOLUMN_PRECISION|DBCOLUMN_SCALE, DBCOLUMN_DATETIMEPRECISION|  
|-----------------|--------------------|--------------------------|-------------------------|--------------------------------------------------|  
|date|DBTYPE_WSTR|10|NULL|NULL|  
|time|DBTYPE_WSTR|8, 10..16|NULL|NULL|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19,21..27|NULL|NULL|  
|datetimeoffset|DBTYPE_WSTR|26,28..34|NULL|NULL|  
  
#### <a name="columnsinfogetcolumninfo"></a>ColumnsInfo::GetColumnInfo  
 La structure DBCOLUMNINFO retourne les informations suivantes :  
  
|Type de paramètre|wType|ulColumnSize|bPrecision|bScale|  
|--------------------|-----------|------------------|----------------|------------|  
|date|DBTYPE_WSTR|10|~0|~0|  
|time(1..7)|DBTYPE_WSTR|8, 10..16|~0|~0|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19,21..27|~0|~0|  
|datetimeoffset|DBTYPE_WSTR|26,28..34|~0|~0|  
  
### <a name="schema-rowsets"></a>Ensembles de lignes de schéma  
 Cette section décrit les métadonnées des paramètres, des colonnes de résultats et des ensembles de lignes de schéma pour les nouveaux types de données. Ces informations sont utiles vous avez un fournisseur client développé à l’aide d’outils antérieurs à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
#### <a name="columns-rowset"></a>Ensemble de lignes COLUMNS  
 Les valeurs de colonnes suivantes sont retournées pour les types date/heure :  
  
|Type de colonne|DATA_TYPE|CHARACTER_MAXIMUM_LENGTH|CHARACTER_OCTET_LENGTH|DATETIME_PRECISION|  
|-----------------|----------------|--------------------------------|------------------------------|-------------------------|  
|date|DBTYPE_WSTR|10|20|NULL|  
|time|DBTYPE_WSTR|8, 10..16|16,20..32|NULL|  
|smalldatetime|DBTYPE_DBTIMESTAMP|NULL|NULL|0|  
|datetime|DBTYPE_DBTIMESTAMP|NULL|NULL|3|  
|datetime2|DBTYPE_WSTR|19,21..27|38,42..54|NULL|  
|datetimeoffset|DBTYPE_WSTR|26,28..34|52, 56..68|NULL|  
  
#### <a name="procedureparameters-rowset"></a>Ensemble de lignes PROCEDURE_PARAMETERS  
 Les valeurs de colonnes suivantes sont retournées pour les types date/heure :  
  
|Type de colonne|DATA_TYPE|CHARACTER_MAXIMUM_LENGTH|CHARACTER_OCTET_LENGTH|TYPE_NAME<br /><br /> LOCAL_TYPE_NAME|  
|-----------------|----------------|--------------------------------|------------------------------|--------------------------------------|  
|date|DBTYPE_WSTR|10|20|date|  
|time|DBTYPE_WSTR|8, 10..16|16,20..32|time|  
|smalldatetime|DBTYPE_DBTIMESTAMP|NULL|NULL|smalldatetime|  
|datetime|DBTYPE_DBTIMESTAMP|NULL|NULL|datetime|  
|datetime2|DBTYPE_WSTR|19,21..27|38,42..54|datetime2|  
|datetimeoffset|DBTYPE_WSTR|26,28..34|52, 56..68|datetimeoffset|  
  
#### <a name="providertypes-rowset"></a>Ensemble de lignes PROVIDER_TYPES  
 Les lignes suivantes sont retournées pour les types date/heure :  
  
|Type -><br /><br /> Colonne|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|--------------------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|DATA_TYPE|DBTYPE_WSTR|DBTYPE_WSTR|DBTYPE_DBTIMESTAMP|DBTYPE_DBTIMESTAMP|DBTYPE_WSTR|DBTYPE_WSTR|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|‘|‘|‘|‘|‘|‘|  
|LITERAL_SUFFIX|‘|‘|‘|‘|‘|‘|  
|CREATE_PARAMS|NULL|NULL|NULL|NULL|NULL|NULL|  
|IS_NULLABLE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|  
|CASE_SENSITIVE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|NULL|NULL|NULL|NULL|NULL|NULL|  
|FIXED_PREC_SCALE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|AUTO_UNIQUE_VALUE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|LOCAL_TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|MINIMUM_SCALE|NULL|NULL|NULL|NULL|NULL|NULL|  
|MAXIMUM_SCALE|NULL|NULL|NULL|NULL|NULL|NULL|  
|GUID|NULL|NULL|NULL|NULL|NULL|NULL|  
|TYPELIB|NULL|NULL|NULL|NULL|NULL|NULL|  
|VERSION|NULL|NULL|NULL|NULL|NULL|NULL|  
|IS_LONG|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|BEST_MATCH|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_TRUE|VARIANT_FALSE|VARIANT_FALSE|  
|IS_FIXEDLENGTH|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
  
## <a name="down-level-server-behavior"></a>Comportement de serveur de bas niveau  
 Lorsqu’il est connecté à un serveur d’une version antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], toute tentative d’utiliser les nouveaux noms de type de serveur (par exemple, avec ICommandWithParameters::SetParameterInfo ou ITableDefinition::CreateTable) génère db_e_badtypename.  
  
 Si de nouveaux types sont liés pour des paramètres ou des résultats sans l'utilisation d'un nom de type, et si le nouveau type est utilisé pour spécifier le type serveur implicitement ou s'il n'existe pas de conversion valide du type serveur en type client, DB_E_ERRORSOCCURRED est retourné ; par ailleurs, DBBINDSTATUS_UNSUPPORTED_CONVERSION est défini en tant qu'état de liaison pour l'accesseur utilisé au moment de l'exécution.  
  
 S'il existe une conversion cliente prise en charge du type de mémoire tampon en type serveur pour la version du serveur de la connexion, tous les types de mémoires tampons clients peuvent être utilisés. Dans ce contexte, *type de serveur* signifie que le type spécifié par ICommandWithParameters::SetParameterInfo ou impliquée par le type de tampon si ICommandWithParameters::SetParameterInfo n’a pas été appelée. En d'autres termes, DBTYPE_DBTIME2 et DBTYPE_DBTIMESTAMPOFFSET peuvent être utilisés avec des serveurs de bas niveau, ou lorsque DataTypeCompatibility=80, si la conversion cliente vers un type serveur pris en charge réussit. Bien entendu, si le type serveur est incorrect, une erreur peut toujours être signalée par le serveur lorsque ce dernier ne peut pas effectuer de conversion implicite vers le type serveur effectif.  
  
## <a name="sspropinitdatatypecompatibility-behavior"></a>Comportement de SSPROP_INIT_DATATYPECOMPATIBILITY  
 Lorsque SSPROP_INIT_DATATYPECOMPATIBILITY a la valeur sspropval_datatypecompatibility_sql2000, les types date/heure de nouveau et les métadonnées associées apparaissent aux clients tels qu’ils apparaissent pour les clients de bas niveau, comme décrit dans [modifications de la copie en bloc Améliorées des Types de Date et heure &#40;OLE DB et ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  
  
## <a name="comparability-for-irowsetfind"></a>Comparabilité pour IRowsetFind  
 Tous les opérateurs de comparaison sont autorisés pour les nouveaux types date/heure, car ils apparaissent sous forme de types chaîne et non sous forme de types date/heure.  
  
## <a name="see-also"></a>Voir aussi  
 [Date et heure améliorations & #40 ; OLE DB & #41 ;](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
