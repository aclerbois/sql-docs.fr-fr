---
title: Les Types définis par l’utilisateur CLR volumineux (ODBC) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|ODBC
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- ODBC, large user-defined types
- large user-defined types [ODBC]
ms.assetid: ddce337e-bb6e-4a30-b7cc-4969bb1520a9
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ec957ad4bc1ea32c885b51a940a793f84dbc6b73
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="large-clr-user-defined-types-odbc"></a>Types CLR volumineux définis par l’utilisateur (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Cette rubrique traite des modifications apportées à ODBC dans SQL Server Native Client pour prendre en charge les types CLR volumineux définis par l'utilisateur.  
  
 Pour voir un exemple montrant la prise en charge ODBC pour les UDT volumineux du CLR, [prend en charge pour les UDT volumineux](../../../relational-databases/native-client-odbc-how-to/support-for-large-udts.md).  
  
 Pour plus d’informations sur la prise en charge pour les UDT volumineux du CLR dans SQL Server Native Client, consultez [Large CLR User-Defined Types](../../../relational-databases/native-client/features/large-clr-user-defined-types.md).  
  
## <a name="data-format"></a>Format de données  
 SQL Server Native Client utilise SQL_SS_LENGTH_UNLIMITED pour indiquer que la taille d'une colonne est supérieure à 8 000 octets pour les types d'objets LOB. Depuis SQL Server 2008, la même valeur est utilisée pour les types CLR définis par l'utilisateur lorsque leur taille est supérieure à 8 000 octets.  
  
 Les valeurs UDT sont représentées sous la forme de tableaux d'octets. Les conversions en chaînes hexadécimales de même que les conversions depuis des chaînes hexadécimales sont prises en charge. Les valeurs littérales sont représentées sous forme de chaînes hexadécimales avec le préfixe « 0x ».  
  
 Le tableau suivant montre le mappage des types de données dans les paramètres et les jeux de résultats :  
  
|Type de données SQL Server|Type de données SQL|Valeur|  
|--------------------------|-------------------|-----------|  
|UDT CLR|SQL_SS_UDT|-151 (sqlncli.h)|  
  
 Le tableau suivant indique la correspondance entre la structure et le type ODBC C. Essentiellement, le CLR UDT est un **varbinary** type avec des métadonnées supplémentaires.  
  
|Type de données SQL|Disposition en mémoire|Type de données C|Valeur (sqlext.h)|  
|-------------------|-------------------|-----------------|------------------------|  
|SQL_SS_UDT|SQLCHAR * (unsigned char \*)|SQL_C_BINARY|SQL_BINARY (-2)|  
  
## <a name="descriptor-fields-for-parameters"></a>Champs de descripteur pour les paramètres  
 Les informations suivantes sont retournées dans les champs IPD :  
  
|Champ de descripteur|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|  
|----------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_LOCAL_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_SCALE|0|0|  
|SQL_DESC_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_UNSIGNED|SQL_TRUE|SQL_TRUE|  
|SQL_CA_SS_UDT_CATALOG_NAME|Le nom du catalogue contenant l’UDT.|Le nom du catalogue contenant l’UDT.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|Nom du schéma contenant le type défini par l'utilisateur (UDT).|Le nom du schéma du contient l’UDT.|  
|SQL_CA_SS_UDT_TYPE_NAME|Le nom de l’UDT.|Le nom de l’UDT.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|Nom complet du type défini par l'utilisateur (UDT).|Nom complet du type défini par l'utilisateur (UDT).|  
  
 Pour les paramètres UDT, SQL_CA_SS_UDT_TYPE_NAME doit toujours être défini via **SQLSetDescField**. SQL_CA_SS_UDT_CATALOG_NAME et SQL_CA_SS_UDT_SCHEMA_NAME sont facultatifs.  
  
 Si l'UDT est défini dans la même base de données avec un schéma différent de celui de la table, SQL_CA_SS_UDT_SCHEMA_NAME doit être défini.  
  
 Si l'UDT est défini dans une base de données différente de celle de la table, SQL_CA_SS_UDT_CATALOG_NAME et SQL_CA_SS_UDT_SCHEMA_NAME doivent être définis.  
  
 En cas d'erreurs ou d'omissions dans les paramètres de SQL_CA_SS_UDT_TYPE_NAME, SQL_CA_SS_UDT_CATALOG_NAME ou SQL_CA_SS_UDT_SCHEMA_NAME, un enregistrement de diagnostic est généré avec SQLSTATE HY000 et un message spécifique au serveur.  
  
## <a name="descriptor-fields-for-results"></a>Champs de descripteur pour les résultats  
 Les informations retournées dans les champs IRD sont comme suit :  
  
|Champ de descripteur|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|  
|----------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_DISPLAY_SIZE|2*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_LITERAL_PREFIX|"0x"|"0x"|  
|SQL_DESC_LITERAL_SUFFIX|""|""|  
|SQL_DESC_LOCAL_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_SCALE|0|0|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|SQL_PRED_NONE|  
|SQL_DESC_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_UNSIGNED|SQL_TRUE|SQL_TRUE|  
|SQL_CA_SS_UDT_CATALOG_NAME|Le nom du catalogue contenant l’UDT.|Le nom du catalogue contenant l’UDT.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|Nom du schéma contenant le type défini par l'utilisateur (UDT).|Nom du schéma contenant le type défini par l'utilisateur (UDT).|  
|SQL_CA_SS_UDT_TYPE_NAME|Le nom de l’UDT.|Le nom de l’UDT.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|Nom complet du type défini par l'utilisateur (UDT).|Nom complet du type défini par l'utilisateur (UDT).|  
  
## <a name="column-metadata-returned-by-sqlcolumns-and-sqlprocedurecolumns-catalog-metadata"></a>Métadonnées de colonne retournées par SQLColumns et SQLProcedureColumns (métadonnées de catalogue)  
 Les valeurs de colonne suivantes sont retournées pour les types définis par l'utilisateur (UDT) :  
  
|Nom de colonne|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|  
|-----------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|DATA_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|TYPE_NAME|Le nom de l’UDT.|Le nom de l’UDT.|  
|COLUMN_SIZE|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|BUFFER_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|DECIMAL_DIGITS|NULL|NULL|  
|SQL_DATA_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DATETIME_SUB|NULL|NULL|  
|CHAR_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SS_UDT_CATALOG_NAME|Le nom du catalogue contenant l’UDT.|Le nom du catalogue contenant l’UDT.|  
|SS_UDT_SCHEMA_NAME|Nom du schéma contenant le type défini par l'utilisateur (UDT).|Nom du schéma contenant le type défini par l'utilisateur (UDT).|  
|SS_UDT_ASSEMBLY_TYPE_NAME|Nom complet du type défini par l'utilisateur (UDT).|Nom complet du type défini par l'utilisateur (UDT).|  
  
 Les trois dernières colonnes sont spécifiques au pilote. Ils sont ajoutés après les colonnes définies par ODBC, mais avant toutes les colonnes spécifiques au pilote existantes du jeu de résultats de SQLColumns ou SQLProcedureColumns.  
  
 Aucuns lignes ne sont retournées par SQLGetTypeInfo, pour les UDT individuels ou pour le type générique « udt ».  
  
## <a name="bindings-and-conversions"></a>Liaisons et conversions  
 Les conversions suivantes sont prises en charge entre les types de données SQL et C :  
  
|Conversion vers et depuis :|SQL_SS_UDT|  
|-----------------------------|------------------|  
|SQL_C_WCHAR|Prise en charge *|  
|SQL_C_BINARY|Pris en charge|  
|SQL_C_CHAR|Prise en charge *|  
  
 \* Données binaires sont converties en chaîne hexadécimale.  
  
 Les conversions suivantes sont prises en charge entre les types de données C et SQL :  
  
|Conversion vers et depuis :|SQL_SS_UDT|  
|-----------------------------|------------------|  
|SQL_C_WCHAR|Prise en charge *|  
|SQL_C_BINARY|Pris en charge|  
|SQL_C_CHAR|Prise en charge *|  
  
 \* Chaîne hexadécimale à une conversion de données binaires se produit.  
  
## <a name="sqlvariant-support-for-udts"></a>Prise en charge des UDT par SQL_VARIANT  
 Les UDT ne sont pas pris en charge dans les colonnes SQL_VARIANT.  
  
## <a name="bcp-support-for-udts"></a>Prise en charge des UDT par BCP  
 Les valeurs UDT ne peuvent être importées et exportées qu'en tant que valeurs de type character ou binary.  
  
## <a name="downlevel-client-behavior-for-udts"></a>Comportement des clients de niveau inférieur pour les UDT  
 Les UDT sont mappés aux clients de niveau inférieur de la manière suivante :  
  
|Version du serveur|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|  
|--------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL Server 2005|**UDT**|**varbinary(max)**|  
|SQL Server 2008 et versions ultérieur|**UDT**|**UDT**|  
  
## <a name="odbc-functions-supporting-large-clr-udts"></a>Fonctions ODBC prenant en charge les types CLR volumineux définis par l'utilisateur  
 Cette section indique les modifications apportées aux fonctions ODBC SQL Server Native Client au niveau des types CLR volumineux définis par l'utilisateur.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 Les valeurs de colonnes de résultats de l'UDT sont converties du type de données SQL en type de données C comme décrit dans la section « Liaisons et conversions » de cette rubrique.  
  
### <a name="sqlbindparameter"></a>SQLBindParameter  
 Les valeurs requises pour les UDT sont les suivantes :  
  
|Type de données SQL|*Type de paramètre*|*ColumnSizePtr*|*DecimalDigitsPtr*|  
|-------------------|---------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Les valeurs retournées pour les UDT sont celles décrites dans la section « Champs de descripteur pour les résultats », plus haut dans cette rubrique.  
  
### <a name="sqlcolumns"></a>SQLColumns  
 Les valeurs retournées pour les UDT sont comme décrit dans la section « Colonne de métadonnées retourné par SQLColumns et SQLProcedureColumns (métadonnées de catalogue) », plus haut dans cette rubrique.  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Les valeurs retournées pour les UDT sont les suivantes :  
  
|Type de données SQL|*DataTypePtr*|*ColumnSizePtr*|*DecimalDigitsPtr*|  
|-------------------|-------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqldescribeparam"></a>SQLDescribeParam  
 Les valeurs retournées pour les UDT sont les suivantes :  
  
|Type de données SQL|*DataTypePtr*|*ColumnSizePtr*|*DecimalDigitsPtr*|  
|-------------------|-------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlfetch"></a>SQLFetch  
 Les valeurs de colonnes de résultats de l'UDT sont converties du type de données SQL en type de données C comme décrit dans la section « Liaisons et conversions » de cette rubrique.  
  
### <a name="sqlfetchscroll"></a>SQLFetchScroll  
 Les valeurs de colonnes de résultats de l'UDT sont converties du type de données SQL en type de données C comme décrit dans la section « Liaisons et conversions » de cette rubrique.  
  
### <a name="sqlgetdata"></a>SQLGetData  
 Les valeurs de colonnes de résultats de l'UDT sont converties du type de données SQL en type de données C comme décrit dans la section « Liaisons et conversions » de cette rubrique.  
  
### <a name="sqlgetdescfield"></a>SQLGetDescField  
 Les champs de descripteur disponibles avec les nouveaux types sont décrits dans les sections « Champs de descripteur pour les paramètres » et « Champs de descripteur pour les résultats », plus haut dans cette rubrique.  
  
### <a name="sqlgetdescrec"></a>SQLGetDescRec  
 Les valeurs retournées pour les UDT sont les suivantes :  
  
|Type de données SQL|Type|SubType|Longueur|Précision|Échelle|  
|-------------------|----------|-------------|------------|---------------|-----------|  
|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT|0|*n*|n|0|  
|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|SQL_SS_UDT|0|SQL_SS_LENGTH_UNLIMITED (0)|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlgettypeinfo"></a>SQLGetTypeInfo  
 Les valeurs retournées pour les UDT sont celles décrites dans la section « Métadonnées de colonne retournées par SQLColumns et SQLProcedureColumns (métadonnées de catalogue) », plus haut dans cette rubrique.  
  
### <a name="sqlprocedurecolumns"></a>SQLProcedureColumns  
 Les valeurs retournées pour les UDT sont celles décrites dans la section « Métadonnées de colonne retournées par SQLColumns et SQLProcedureColumns (métadonnées de catalogue) », plus haut dans cette rubrique.  
  
### <a name="sqlputdata"></a>SQLPutData  
 Les valeurs de paramètre UDT sont converties du type de données C en type de données SQL, comme décrit dans la section « Liaisons et conversions » de cette rubrique.  
  
### <a name="sqlsetdescfield"></a>SQLSetDescField  
 Les champs de descripteur disponibles avec les nouveaux types sont décrits dans les sections « Champs de descripteur pour les paramètres » et « Champs de descripteur pour els résultats », plus haut dans cette rubrique.  
  
### <a name="sqlsetdescrec"></a>SQLSetDescRec  
 Les valeurs autorisées pour les UDT sont les suivantes :  
  
|Type de données SQL|Type|SubType|Longueur|Précision|Échelle|  
|-------------------|----------|-------------|------------|---------------|-----------|  
|SQL_SS_UDT<br /><br /> (longueur inférieure ou égale à 8 000 octets)|SQL_SS_UDT|0|*n*|*n*|0|  
|SQL_SS_UDT<br /><br /> (longueur supérieure à 8 000 octets)|SQL_SS_UDT|0|SQL_SS_LENGTH_UNLIMITED (0)|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlspecialcolumns"></a>SQLSpecialColumns  
 Les valeurs retournées pour les UDT des colonnes DATA_TYPE, TYPE_NAME, COLUMN_SIZE, BUFFER_LENGTH et DECIMAL_DIGTS sont celles décrites dans « Métadonnées retournés par SQLColumns et SQLProcedureColumns (métadonnées de catalogue) », plus haut dans cette rubrique.  
  
## <a name="see-also"></a>Voir aussi  
 [Types CLR volumineux définis par l’utilisateur](../../../relational-databases/native-client/features/large-clr-user-defined-types.md)  
  
  
