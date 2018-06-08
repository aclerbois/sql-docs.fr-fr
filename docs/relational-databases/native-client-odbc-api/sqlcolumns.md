---
title: SQLColumns | Documents Microsoft
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
- SQLColumns function
ms.assetid: 69d3af44-8196-43ab-8037-cdd06207b171
caps.latest.revision: 62
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e735efbd7aa155d8f4618afffc203cf4558fe9ee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcolumns"></a>SQLColumns
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  **SQLColumns** retourne SQL_SUCCESS qu’il existe des valeurs ou non le *CatalogName*, *TableName*, ou *ColumnName* paramètres. **SQLFetch** retourne SQL_NO_DATA lorsque des valeurs non valides sont utilisées dans ces paramètres.  
  
> [!NOTE]  
>  Pour les types de valeur de grande taille, tous les paramètres « length » sont retournés avec la valeur SQL_SS_LENGTH_UNLIMITED.  
  
 **SQLColumns** peut être exécutée sur un curseur côté serveur statique. Toute tentative d’exécution **SQLColumns** sur un curseur de mettre à jour (dynamique ou jeu de clés) retourne SQL_SUCCESS_WITH_INFO, indiquant que le type de curseur a été modifié.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client prend en charge les informations de rapport pour les tables des serveurs liés en acceptant un nom en deux parties pour le *CatalogName* paramètre : *nom_serveur_lié.Nom_Catalogue*.  
  
 Pour ODBC 2. *x* ne pas à l’aide de caractères génériques dans les applications *TableName*, **SQLColumns** renvoie des informations sur les tables dont les noms correspondent à *TableName* et sont détenues par l’utilisateur actuel. Si l’utilisateur actuel ne possède aucune table dont le nom correspond à la *TableName* paramètre, **SQLColumns** retourne des informations sur toutes les tables possédées par d’autres utilisateurs où le nom de la table correspond à la *TableName* paramètre. Pour ODBC 2. *x* applications à l’aide de caractères génériques, **SQLColumns** retourne toutes les tables dont les noms correspondent à *TableName*. Pour ODBC 3. *x* applications **SQLColumns** retourne toutes les tables dont les noms correspondent à *TableName* quel que soit le propriétaire ou si les caractères génériques sont utilisés.  
  
 Le tableau ci-dessous dresse la liste des colonnes renvoyées par le jeu de résultats :  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|DATA_TYPE|Retourne SQL_VARCHAR, SQL_VARBINARY ou SQL_WVARCHAR pour les **varchar (max)** des types de données.|  
|TYPE_NAME|Retourne « varchar », « varbinary » ou « nvarchar » pour le **varchar (max)**, **varbinary (max)**, et **nvarchar (max)** des types de données.|  
|COLUMN_SIZE|Retourne SQL_SS_LENGTH_UNLIMITED pour **varchar (max)** les types de données indiquant que la taille de la colonne est illimitée.|  
|BUFFER_LENGTH|Retourne SQL_SS_LENGTH_UNLIMITED pour **varchar (max)** les types de données indiquant que la taille de la mémoire tampon est illimitée.|  
|SQL_DATA_TYPE|Retourne SQL_VARCHAR, SQL_VARBINARY ou SQL_WVARCHAR pour les **varchar (max)** des types de données.|  
|CHAR_OCTET_LENGTH|Retourne la longueur maximale d'une colonne de type char ou binary. Retourne 0 pour indiquer que la taille est illimitée.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|Retourne le nom du catalogue dans lequel un nom de collection de schémas XML est défini. Si le nom du catalogue est introuvable, cette variable contient une chaîne vide.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|Retourne le nom du schéma dans lequel un nom de collection de schémas XML est défini. Si le nom du schéma est introuvable, cette variable contient une chaîne vide.|  
|SS_XML_SCHEMACOLLECTION_NAME|Retourne le nom d'une collection de schémas XML. Si le nom est introuvable, cette variable contient une chaîne vide.|  
|SS_UDT_CATALOG_NAME|Le nom du catalogue contenant l’UDT (type défini par l’utilisateur).|  
|SS_UDT_SCHEMA_NAME|Le nom du schéma contenant l’UDT.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|Nom qualifié de l'assembly du type défini par l'utilisateur (UDT).|  
  
 Pour les UDT, la colonne TYPE_NAME existante est utilisée pour indiquer le nom de l’UDT ; Par conséquent, aucune colonne supplémentaire ne doit être ajouté au jeu de résultats de **SQLColumns** ou [SQLProcedureColumns](../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md). Le champ DATA_TYPE pour un paramètre ou une colonne de type UDT est SQL_SS_UDT.  
  
 Pour le type UDT des paramètres, vous pouvez utiliser les nouveaux descripteurs spécifiques au pilote définis ci-dessus pour obtenir ou définir les propriétés de métadonnées supplémentaires d'un UDT, si le serveur retourne ou nécessite ces informations.  
  
 Lorsqu’un client se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et appelle SQLColumns, à l’aide des valeurs NULL ou le caractère générique pour le paramètre d’entrée de catalogue ne retourne pas d’informations à partir d’autres catalogues. Seules les informations sur le catalogue actuel sont retournées. Le client peut appeler tout d’abord SQLTables pour déterminer dans quel catalogue se trouve la table souhaitée. Le client peut ensuite utiliser cette valeur de catalogue pour le paramètre d’entrée de catalogue dans son appel à SQLColumns pour récupérer des informations sur les colonnes de cette table.  
  
## <a name="sqlcolumns-and-table-valued-parameters"></a>SQLColumns et paramètres table  
 Le jeu de résultats retourné par SQLColumns dépend de la valeur de SQL_SOPT_SS_NAME_SCOPE. Pour plus d’informations, consultez [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). Les colonnes suivantes ont été ajoutées pour les paramètres table :  
  
|Nom de colonne|Type de données|Sommaire|  
|-----------------|---------------|--------------|  
|SS_IS_COMPUTED|Smallint|Pour une colonne d'un TABLE_TYPE, SQL_TRUE si la colonne est une colonne calculée ; sinon, SQL_FALSE.|  
|SS_IS_IDENTITY|Smallint|SQL_TRUE si la colonne est une colonne d'identité ; sinon, SQL_FALSE.|  
  
 Pour plus d’informations sur les paramètres table, consultez [paramètres table & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlcolumns-support-for-enhanced-date-and-time-features"></a>Prise en charge par SQLColumns des fonctionnalités de date et heure améliorées  
 Pour plus d’informations sur les valeurs retournées pour les types date/heure, consultez [métadonnées de catalogue](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md).  
  
 Pour plus d’informations, consultez [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlcolumns-support-for-large-clr-udts"></a>Prise en charge SQLColumns pour les types CLR volumineux définis par l'utilisateur  
 **SQLColumns** prend en charge les types CLR volumineux définis par l’utilisateur (UDT). Pour plus d’informations, consultez [Large CLR User-Defined Types & #40 ; ODBC & #41 ;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlcolumns-support-for-sparse-columns"></a>Prise en charge SQLColumns pour les colonnes éparses  
 Deux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des colonnes spécifiques qui ont été ajoutés au jeu de résultats pour SQLColumns :  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|SS_IS_SPARSE|**smallint**|Si la colonne est une colonne éparse, SQL_TRUE ; sinon, SQL_FALSE.|  
|SS_IS_COLUMN_SET|**smallint**|Si la colonne est la **column_set** colonne, sql_true ; sinon, SQL_FALSE.|  
  
 Conformément à la spécification ODBC, SS_IS_SPARSE et SS_IS_COLUMN_SET apparaissent avant toutes les colonnes spécifiques au pilote qui ont été ajoutés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les versions antérieures à [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]et après toutes les colonnes mandatées par ODBC lui-même.  
  
 Le jeu de résultats retourné par SQLColumns dépend de la valeur de SQL_SOPT_SS_NAME_SCOPE. Pour plus d’informations, consultez [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Pour plus d’informations sur les colonnes éparses dans ODBC, consultez [prise en charge des colonnes éparses &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLColumns (fonction)](http://go.microsoft.com/fwlink/?LinkId=59336)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
