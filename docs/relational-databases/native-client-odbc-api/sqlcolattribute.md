---
title: SQLColAttribute | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLColAttribute function
ms.assetid: a5387d9e-a243-4cfe-b786-7fad5842b1d6
caps.latest.revision: 52
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6a846851ac4558c3b39bd821f32f32c20a0993f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcolattribute"></a>SQLColAttribute
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Vous pouvez utiliser **SQLColAttribute** pour extraire un attribut d’une colonne de jeu de résultats pour les instructions ODBC préparées ou exécutées. Appel de **SQLColAttribute** sur des instructions préparées génère un aller-retour vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client reçoit des données de colonne de jeu de résultats dans le cadre de l’exécution des instructions, par conséquent, l’appel **SQLColAttribute** après l’exécution de **SQLExecute** ou **SQLExecDirect** n’implique pas un aller-retour sur le serveur.  
  
> [!NOTE]  
>  Les attributs de l'identificateur de colonne ODBC ne sont pas disponibles dans tous les jeux de résultats [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Identificateur de champ| Description|  
|----------------------|-----------------|  
|SQL_COLUMN_TABLE_NAME|Disponible dans des jeux de résultats extraits d'instructions générant des curseurs côté serveur ou sur des instructions SELECT exécutées qui contiennent une clause FOR BROWSE.|  
|SQL_DESC_BASE_COLUMN_NAME|Disponible dans des jeux de résultats extraits d'instructions générant des curseurs côté serveur ou sur des instructions SELECT exécutées qui contiennent une clause FOR BROWSE.|  
|SQL_DESC_BASE_TABLE_NAME|Disponible dans des jeux de résultats extraits d'instructions générant des curseurs côté serveur ou sur des instructions SELECT exécutées qui contiennent une clause FOR BROWSE.|  
|SQL_DESC_CATALOG_NAME|Nom de la base de données. Disponible dans des jeux de résultats extraits d'instructions générant des curseurs côté serveur ou sur des instructions SELECT exécutées qui contiennent une clause FOR BROWSE.|  
|SQL_DESC_LABEL|Disponible dans tous les jeux de résultats. La valeur est identique à la valeur du champ SQL_DESC_NAME.<br /><br /> La longueur du champ est nulle uniquement si une colonne est le résultat d'une expression et si l'expression ne comporte aucune étiquette affectée.|  
|SQL_DESC_NAME|Disponible dans tous les jeux de résultats. La valeur est identique à celle du champ SQL_DESC_LABEL_NAME.<br /><br /> La longueur du champ est nulle uniquement si une colonne est le résultat d'une expression et si l'expression ne comporte aucune étiquette affectée.|  
|SQL_DESC_SCHEMA_NAME|Nom du propriétaire. Disponible dans des jeux de résultats extraits d'instructions générant des curseurs côté serveur ou sur des instructions SELECT exécutées qui contiennent une clause FOR BROWSE.<br /><br /> Disponible uniquement si le nom du propriétaire est spécifié pour la colonne dans l'instruction SELECT.|  
|SQL_DESC_TABLE_NAME|Disponible dans des jeux de résultats extraits d'instructions générant des curseurs côté serveur ou sur des instructions SELECT exécutées qui contiennent une clause FOR BROWSE.|  
|SQL_DESC_UNNAMED|Valeur SQL_NAMED de toutes les colonnes dans un jeu de résultats sauf si une colonne est le résultat d'une expression qui ne contient aucune étiquette affectée dans le cadre de l'expression. Lorsque SQL_DESC_UNNAMED retourne la valeur SQL_UNNAMED, tous les attributs d'identificateur de colonne ODBC contiennent des chaînes de longueur nulle pour la colonne.|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Pilote ODBC Native Client utilise l’instruction SET FMTONLY pour réduire la charge sur le serveur lorsque **SQLColAttribute** est appelée pour préparées mais non exécutées.  
  
 Pour les types de valeur élevée, **SQLColAttribute** retourne les valeurs suivantes :  
  
|Identificateur de champ|Description de la modification|  
|----------------------|---------------------------|  
|SQL_DESC_DISPLAY_SIZE|Nombre maximal de caractères requis pour l'affichage des données à partir de la colonne. Pour les colonnes de type de valeur élevée, la valeur retournée est SQL_SS_LENGTH_UNLIMITED.|  
|SQL_DESC_LENGTH|Retourne la longueur réelle de la colonne dans le jeu de résultats. Pour les colonnes de type de valeur élevée, la valeur retournée est SQL_SS_LENGTH_UNLIMITED.|  
|SQL_DESC_OCTET_LENGTH|Retourne la longueur maximale d'une colonne de type de valeur élevée. SQL_SS_LENGTH_UNLIMITED est utilisé pour indiquer une taille illimitée.|  
|SQL_DESC_PRECISION|Retourne la valeur SQL_SS_LENGTH_UNLIMITED pour les colonnes de type de valeur élevée.|  
|SQL_DESC_TYPE|Retourne SQL_VARCHAR, SQL_WVARCHAR et SQL_VARBINARY pour les types de valeur élevée.|  
|SQL_DESC_TYPE_NAME|Retourne « varchar », « varbinary » et « nvarchar » pour les types de valeur élevée.|  
  
 Pour toutes les versions, les attributs de colonne sont signalés uniquement pour le premier jeu de résultats lorsque plusieurs jeux de résultats sont générés par un lot préparé d'instructions SQL.  
  
 Les attributs de colonne suivants sont des extensions proposées par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client retourne toutes les valeurs dans le *NumericAttrPtr* paramètre. Les valeurs sont retournées en tant qu'éléments SDWORD (long signé) sauf SQL_CA_SS_COMPUTE_BYLIST qui désigne un pointeur vers un tableau WORD.  
  
|Identificateur de champ|Valeur retournée|  
|----------------------|--------------------|  
|SQL_CA_SS_COLUMN_HIDDEN*|TRUE si la colonne référencée appartient à une clé primaire masquée et créée afin de prendre en charge une instruction Transact-SQL SELECT contenant la clause FOR BROWSE.|  
|SQL_CA_SS_COLUMN_ID|Position ordinale d'une colonne de résultats d'une clause COMPUTE dans l'instruction Transact-SQL SELECT actuelle.|  
|SQL_CA_SS_COLUMN_KEY*|TRUE si la colonne référencée appartient à une clé primaire de la ligne et si l'instruction Transact-SQL SELECT contient la clause FOR BROWSE.|  
|SQL_CA_SS_COLUMN_OP|Entier spécifiant l'opérateur d'agrégation responsable de la valeur d'une colonne de la clause COMPUTE. Les définitions des valeurs entières sont disponibles dans sqlncli.h.|  
|SQL_CA_SS_COLUMN_ORDER|Position ordinale de la colonne dans une clause ORDER BY d'une instruction ODBC ou Transact-SQL SELECT.|  
|SQL_CA_SS_COLUMN_SIZE|Longueur maximale (en octets) requise pour lier une valeur de données extraite de la colonne à une variable SQL_C_BINARY.|  
|SQL_CA_SS_COLUMN_SSTYPE|Type de données natif des données stockées dans la colonne SQL Server. Les définitions des valeurs de type sont disponibles dans sqlncli.h.|  
|SQL_CA_SS_COLUMN_UTYPE|Type de données de base du type de données défini par l'utilisateur de la colonne SQL Server. Les définitions des valeurs de type sont disponibles dans sqlncli.h.|  
|SQL_CA_SS_COLUMN_VARYLEN|TRUE si la longueur des données de la colonne est variable, FALSE sinon.|  
|SQL_CA_SS_COMPUTE_BYLIST|Pointeur vers un tableau WORD (court non signé) spécifiant les colonnes employées dans l'expression BY d'une clause COMPUTE. Si la clause COMPUTE ne spécifie aucune expression BY, un pointeur NULL est retourné.<br /><br /> Le premier élément du tableau contient le nombre de colonnes dans la liste BY. Les éléments supplémentaires correspondent aux nombres de colonnes.|  
|SQL_CA_SS_COMPUTE_ID|*computeid* d’une ligne qui est le résultat d’une clause COMPUTE dans l’instruction Transact-SQL SELECT actuelle.|  
|SQL_CA_SS_NUM_COMPUTES|Nombre de clauses COMPUTE spécifiées dans l'instruction SELECT Transact-SQL actuelle.|  
|SQL_CA_SS_NUM_ORDERS|Nombre de colonnes spécifiées dans une clause ORDER BY d'une instruction Transact-SQL SELECT.|  
  
 \*   Disponible si l’attribut d’instruction SQL_SOPT_SS_HIDDEN_COLUMNS est défini sur SQL_HC_ON.  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] inclut les champs de descripteur spécifiques au pilote pour fournir des informations supplémentaires pour indiquer le nom de collection de schémas XML, le nom de schéma et le nom du catalogue, de respectivement. Ces propriétés ne nécessitent pas l'usage de guillemets ou d'un caractère d'échappement si elles contiennent des caractères non alphanumériques. Le tableau suivant répertorie ces nouveaux champs de descripteur :  
  
|Nom de colonne|Type| Description|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_CATALOG_NAME|CharacterAttributePtr|Nom du catalogue dans lequel un nom de collection de schémas XML est défini. Si le nom du catalogue est introuvable, cette variable contient une chaîne vide.<br /><br /> Ces informations sont retournées à partir du champ d'enregistrement SQL_DESC_SS_XML_SCHEMACOLLECTION_CATALOG_NAME de l'IRD qui est un champ en lecture-écriture.|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_SCHEMA_NAM E|CharacterAttributePtr|Nom du schéma dans lequel un nom de collection de schémas XML est défini. Si le nom du schéma est introuvable, cette variable contient une chaîne vide.<br /><br /> Ces informations sont retournées à partir du champ d'enregistrement SQL_DESC_SS_XML_SCHEMACOLLECTION_SCHEMA_NAME du champ de descripteur de ligne d'implémentation (IRD) en lecture-écriture.|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_NAME|CharacterAttributePtr|Nom d'une collection de schémas XML. Si le nom est introuvable, cette variable contient une chaîne vide.<br /><br /> Ces informations sont retournées à partir du champ d'enregistrement SQL_DESC_SS_XML_SCHEMACOLLECTION_NAME du champ de descripteur de ligne d'implémentation (IRD) en lecture-écriture.|  
  
 De même, [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] inclut des champs de descripteur inhérents au pilote qui fournissent des informations supplémentaires pour une colonne de type défini par l'utilisateur (UDT) d'un jeu de résultats ou un paramètre UDT d'une procédure stockée ou d'une requête paramétrable. Ces propriétés ne nécessitent pas l'usage de guillemets ou d'un caractère d'échappement si elles contiennent des caractères non alphanumériques. Le tableau suivant répertorie ces nouveaux champs de descripteur :  
  
|Nom de la colonne|Type| Description|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_UDT_CATALOG_NAME|CharacterAttributePtr|Nom du catalogue contenant le type défini par l'utilisateur (UDT).|  
|SQL_CA_SS_UDT_SCHEMA_NAME|CharacterAttributePtr|Le nom du schéma contenant l’UDT.|  
|SQL_CA_SS_UDT_TYPE_NAME|CharacterAttributePtr|Le nom de l’UDT.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|CharacterAttributePtr|Le nom qualifié d’assembly de l’UDT.|  
  
 L'identificateur du champ de descripteur SQL_DESC_TYPE_NAME existant est utilisé pour indiquer le nom de l'UDT. Le champ SQL_DESC_TYPE pour une colonne de type défini par l'utilisateur (UDT) est SQL_SS_UDT.  
  
## <a name="sqlcolattribute-support-for-enhanced-date-and-time-features"></a>Prise en charge de la fonction SQLColAttribute pour les fonctionnalités de date et heure améliorées  
 Pour les valeurs retournées pour les types de date/heure, consultez la section « Informations retournées dans les champs IRD » dans [paramètre et les métadonnées de résultat](../../relational-databases/native-client-odbc-date-time/metadata-parameter-and-result.md).  
  
 Pour plus d’informations, consultez [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlcolattribute-support-for-large-clr-udts"></a>Prise en charge SQLColAttribute pour les types CLR volumineux définis par l'utilisateur  
 **SQLColAttribute** prend en charge les types CLR volumineux définis par l’utilisateur (UDT). Pour plus d’informations, consultez [Large CLR User-Defined Types & #40 ; ODBC & #41 ;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlcolattribute-support-for-sparse-columns"></a>Prise en charge SQLColAttribute pour les colonnes éparses  
 SQLColAttribute interroge le champ de descripteur (IRD) de ligne implémentation nouveau, SQL_CA_SS_IS_COLUMN_SET, afin de déterminer si une colonne est un **column_set** colonne.  
  
 Pour plus d’informations, consultez [prise en charge des colonnes éparses &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction SQLColAttribute](http://go.microsoft.com/fwlink/?LinkId=59334)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)  
  
  
