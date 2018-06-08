---
title: Comportement avec les versions SQL Server antérieures (ODBC) de Type améliorées de Date et d’heure | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-date-time
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- date/time [ODBC], enhanced behavior with earlier SQL Server versions
ms.assetid: cd4e137f-dc5e-4df7-bc95-51fe18c587e0
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e63cefdf59a3198ff1f71b96ce36ebc11dae592b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enhanced-date-and-time-type-behavior-with-previous-sql-server-versions-odbc"></a>Comportement des types de date et d'heure améliorés avec les versions SQL Server antérieures (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Cette rubrique décrit le comportement attendu lorsqu'une application cliente qui utilise les fonctionnalités améliorées de date et d'heure communique avec une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], et lorsqu'une application cliente qui utilise Microsoft Data Access Components, Windows Data Access Components ou une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] envoie des commandes à un serveur qui prend en charge les fonctionnalités améliorées de date et d'heure.  
  
## <a name="down-level-client-behavior"></a>Comportement de client de bas niveau  
 Les applications clientes compilées à l'aide d'une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] voient les nouveaux types de date et d'heure sous la forme de colonnes nvarchar. Le contenu de la colonne est les représentations sous forme de littéral, comme décrit dans la section « Formats de données : chaînes et littéraux » de [prise en charge du Type de données de Date ODBC et les améliorations apportées au](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md). La taille de colonne correspond à la longueur littérale maximale pour la précision en fractions de seconde spécifiée pour la colonne.  
  
 Les API du catalogue retournent des métadonnées conformes au code du type de données de bas niveau retourné au client (nvarchar, par exemple) et à la représentation de bas niveau associée (le format littéral approprié, par exemple). Toutefois, le nom du type de données retourné est le nom de type [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] réel.  
  
 Métadonnées d’instruction retournée par SQLDescribeCol, SQLDescribeParam, SQGetDescField et SQLColAttribute retournent des métadonnées qui sont cohérente avec le type de bas niveau à tous les égards, y compris le nom de type. Est un exemple d’un tel type de bas niveau **nvarchar**.  
  
 Lorsqu’une application cliente de bas niveau s’exécute sur un [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (ou version ultérieure) sur les modifications de schéma à la date/heure ont été apportées des types de serveur, le comportement attendu est comme suit :  
  
|Type SQL Server 2005|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (ou version ultérieure) Type|Type de client ODBC|Conversion des résultats (de SQL vers C)|Conversion des paramètres (de C vers SQL)|  
|--------------------------|----------------------------------------------|----------------------|------------------------------------|---------------------------------------|  
|DateTime|Date|SQL_C_TYPE_DATE|OK|OK (1)|  
|||SQL_C_TYPE_TIMESTAMP|Champs d'heure définis à zéro.|OK (2)<br /><br /> Échoue si le champ d'heure n'est pas nul. Fonctionne avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Time(0)|SQL_C_TYPE_TIME|OK|OK (1)|  
|||SQL_C_TYPE_TIMESTAMP|Champs de date définis à la date actuelle.|OK (2)<br /><br /> La date est ignorée. Échoue si les fractions de seconde sont différentes de zéro. Fonctionne avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Time(7)|SQL_C_TIME|Échec : littéral d'heure non valide.|OK (1)|  
|||SQL_C_TYPE_TIMESTAMP|Échec : littéral d'heure non valide.|OK (1)|  
||Datetime2(3)|SQL_C_TYPE_TIMESTAMP|OK|OK (1)|  
||Datetime2 (7)|SQL_C_TYPE_TIMESTAMP|OK|La valeur sera arrondie à 1/300e de seconde par la conversion cliente.|  
|Smalldatetime|Date|SQL_C_TYPE_DATE|OK|OK|  
|||SQL_C_TYPE_TIMESTAMP|Champs d'heure définis à zéro.|OK (2)<br /><br /> Échoue si le champ d'heure n'est pas nul. Fonctionne avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Time(0)|SQL_C_TYPE_TIME|OK|OK|  
|||SQL_C_TYPE_TIMESTAMP|Champs de date définis à la date actuelle.|OK (2)<br /><br /> La date est ignorée. Échoue si les fractions de seconde ne sont pas nulles.<br /><br /> Fonctionne avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].|  
||Datetime2(0)|SQL_C_TYPE_TIMESTAMP|OK|OK|  
  
## <a name="key-to-symbols"></a>Liste des symboles  
  
|Symbole|Signification|  
|------------|-------------|  
|1|Si cela a fonctionné avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] cela doit continuer à fonctionner avec une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|2|Une application qui fonctionnait avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] pourrait échouer avec une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 Notez que seules les modifications de schéma courantes ont été considérées. Les modifications courantes sont les suivantes :  
  
-   Utilisation d'un nouveau type alors qu'en toute logique une application requiert uniquement une valeur de date ou d'heure. Toutefois, l'application a été forcée d'utiliser des données datetime ou smalldatetime en raison du manque de types de date et d'heure distincts.  
  
-   Utilisation d'un nouveau type pour gagner en précision sur les fractions de seconde.  
  
-   Passage à datetime2, car il représente le type de données de date et d'heure préféré.  
  
### <a name="column-metadata-returned-by-sqlcolumns-sqlprocedurecolumns-and-sqlspecialcolumns"></a>Métadonnées de colonne retournées par SQLColumns, SQLProcedureColumns et SQLSpecialColumns  
 Les valeurs de colonnes suivantes sont retournées pour les types date/heure :  
  
|Type de colonne|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|-----------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_WVARCHAR|SQL_WVARCHAR|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|COLUMN_SIZE|10|8,10..16|16|23|19, 21..27|26, 28..34|  
|BUFFER_LENGTH|20|16, 20..32|16|16|38, 42..54|52, 56..68|  
|DECIMAL_DIGITS|NULL|NULL|0|3|NULL|NULL|  
|SQL_DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_DATETIME|SQL_DATETIME|SQL_WVARCHAR|SQL_WVARCHAR|  
|SQL_DATETIME_SUB|NULL|NULL|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|NULL|NULL|  
|CHAR_OCTET_LENGTH|NULL|NULL|NULL|NULL|NULL|NULL|  
|SS_DATA_TYPE|0|0|111|111|0|0|  
  
 SQLSpecialColumns ne retourne pas SQL_DATA_TYPE, SQL_DATETIME_SUB, CHAR_OCTET_LENGTH ni SS_DATA_TYPE.  
  
### <a name="data-type-metadata-returned-by-sqlgettypeinfo"></a>Métadonnées de type de données retournées par SQLGetTypeInfo  
 Les valeurs de colonnes suivantes sont retournées pour les types date/heure :  
  
|Type de colonne|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|-----------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_WVARCHAR|SQL_WVARCHAR|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|‘|‘|‘|‘|‘|‘|  
|LITERAL_SUFFIX|‘|‘|‘|‘|‘|‘|  
|CREATE_PARAMS|NULL|NULL|NULL|NULL|NULL|NULL|  
|NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|  
|CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|NULL|NULL|NULL|NULL|NULL|NULL|  
|FXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|AUTO_UNIQUE_VALUE|NULL|NULL|NULL|NULL|NULL|NULL|  
|LOCAL_TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|MINIMUM_SCALE|NULL|NULL|0|3|NULL|NULL|  
|MAXIMUM_SCALE|NULL|NULL|0|3|NULL|NULL|  
|SQL_DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_DATETIME|SQL_DATETIME|SQL_WVARCHAR|SQL_WVARCHAR|  
|SQL_DATETIME_SUB|NULL|NULL|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|NULL|NULL|  
|NUM_PREC_RADIX|NULL|NULL|NULL|NULL|NULL|NULL|  
|INTERVAL_PRECISION|NULL|NULL|NULL|NULL|NULL|NULL|  
|USERTYPE|0|0|12|22|0|0|  
  
## <a name="down-level-server-behavior"></a>Comportement de serveur de bas niveau  
 En cas de connexion à une instance de serveur d'une version antérieure à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], toute tentative d'utiliser les nouveaux types de serveur ou les codes de métadonnées et les champs de descripteur associés provoquera le renvoi d'une erreur SQL_ERROR. Un enregistrement de diagnostic sera généré avec SQLSTATE HY004 et le message « Type de données SQL non valide pour la version du serveur lors de la connexion », ou avec 07006 et « Violation de l'attribut de type de données restreint ».  
  
## <a name="see-also"></a>Voir aussi  
 [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
  
  
